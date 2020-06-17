---
title: 'Datacenter Switchovers: Exchange 2013 Help'
TOCTitle: Datacenter Switchovers
ms:assetid: ac208c12-04d0-4809-bacd-72478ff14983
ms:mtpsurl: https://technet.microsoft.com/library/Dd351049(v=EXCHG.150)
ms:contentKeyID: 62549506
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Datacenter Switchovers

_**Applies to:** Exchange Server 2013 SP1_

In a site resilient configuration, automatic recovery in response to a site-level failure can occur within a DAG, allowing the messaging system to remain in a functional state. This configuration requires at least three locations, as it requires deploying DAG members in two locations and the DAG's witness server in a third location.

If you don't have three locations, or even if you do have three locations, but you want to control datacenter-level recovery actions, you can configure a DAG for manual recovery in the event of a site-level failure. In that event, you would perform a process called a *datacenter switchover*. As with many disaster recovery scenarios, prior planning and preparation for a datacenter switchover can simplify your recovery process and reduce the duration of your outage.

There are four basic steps that you complete to perform a datacenter switchover, after making the initial decision to activate the second datacenter:

1. **Terminate a partially running datacenter**: This step involves terminating Exchange services in the primary datacenter, if any services are still running. This is particularly important for the Mailbox server role because it uses an active/passive high availability model. If services in a partially failed datacenter aren't stopped, it's possible for problems from the partially failed datacenter to negatively affect the services during a switchover back to the primary datacenter.

   > [!IMPORTANT]
   > If network or Active Directory infrastructure reliability has been compromised as a result of the primary datacenter failure, we recommend that all messaging services be off until these dependencies are restored to healthy service.

2. **Validate and confirm the prerequisites for the second datacenter**: This step can be performed in parallel with step 1 because validation of the health of the infrastructure dependences in the second datacenter is largely independent of the first datacenter services. Each organization typically requires its own method for performing this step. For example, you may decide to complete this step by reviewing health information collected and filtered by an infrastructure monitoring application, or by using a tool that's unique to your organization's infrastructure. This is a critical step, because activating the second datacenter when its infrastructure is unhealthy and unstable is likely to yield poor results.

3. **Activate the Mailbox servers**: This step begins the process of activating the second datacenter. This step can be performed in parallel with step 4 because the Microsoft Exchange services can handle database outages and recover. Activating the Mailbox servers involves a process of marking the failed servers from the primary datacenter as unavailable followed by activation of the servers in the second datacenter. The activation process for Mailbox servers depends on whether the DAG is in database activation coordination (DAC) mode. For more information about database activation coordination mode, see [Datacenter Activation Coordination mode](datacenter-activation-coordination-mode-exchange-2013-help.md).

   If the DAG is in DAC mode, you can use the Exchange site resilience cmdlets to terminate a partially failed datacenter (if necessary) and activate the Mailbox servers. For example, in DAC mode, this step is performed by using the [Stop-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Stop-DatabaseAvailabilityGroup) cmdlet. In some cases, the servers must be marked as unavailable twice (once in each datacenter). Next, the [Restore-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Restore-DatabaseAvailabilityGroup) cmdlet is run to restore the remaining members of the database availability group (DAG) in the second datacenter by reducing the DAG members to those that are still operational, thereby reestablishing quorum. If the DAG isn't in DAC mode, you must use the Windows Failover Cluster tools to activate the Mailbox servers. After either process is complete, the database copies that were previously passive in the second datacenter can become active and be mounted. At this point, Mailbox server recovery is complete.

4. **Activate the Client Access servers**: This involves using the URL mapping information and the Domain Name System (DNS) change methodology to perform all required DNS updates. The mapping information describes what DNS changes to perform. The amount of time required to complete the update depends on the methodology used and the Time to Live (TTL) settings on the DNS record (and whether the deployment's infrastructure honors the TTL).

Users should start to have access to messaging services sometime after steps 3 and 4 are completed. Steps 3 and 4 are described in greater detail later in this topic.

## Terminating a Partially Failed Datacenter

If any DAG members in the failed datacenter are still running, they should be terminated.

When the DAG is in DAC mode, the specific actions to terminate any surviving DAG members in the primary datacenter are as follows:

1. The DAG members in the primary datacenter must be marked as stopped in the primary datacenter. *Stopped* is a state of Active Manager that prevents databases from mounting, and Active Manager on each server in the failed datacenter is put into this state by using the [Stop-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Stop-DatabaseAvailabilityGroup) cmdlet. The *ActiveDirectorySite* parameter of this cmdlet can be used to mark all of the servers in the primary datacenter as stopped with a single command. This step may not be possible depending on the failure. This step should be taken if the state of the datacenter permits it. The **Stop-DatabaseAvailabilityGroup** cmdlet should be run against all servers in the primary datacenter. If the Mailbox server is unavailable but Active Directory is operating in the primary datacenter, the **Stop-DatabaseAvailabilityGroup** command with the *ConfigurationOnly* parameter must be run against all servers in this state in the primary datacenter, or the Mailbox server must be turned off. Failure to either turn off the Mailbox servers in the failed datacenter or to successfully perform the **Stop-DatabaseAvailabilityGroup** command against the servers will create the potential for split-brain syndrome to occur across the two datacenters. You may need to individually turn off computers through power management devices to satisfy this requirement.

2. The second datacenter must now be updated to represent which primary datacenter servers are stopped. This is done by running the same **Stop-DatabaseAvailabilityGroup** command with the *ConfigurationOnly* parameter using the same *ActiveDirectorySite* parameter and specifying the name of the Active Directory site in the failed primary datacenter. The purpose of this step is to inform the servers in the second datacenter about which mailbox servers are available to use when restoring service.

When the DAG isn't in DAC mode, the specific actions to terminate any surviving DAG members in the primary datacenter are as follows:

1. The DAG members in the primary datacenter must be forcibly evicted from the DAG's underlying cluster by running the following commands on each member:

   ```powershell
   net stop clussvc
   ```

   ```powershell
   cluster <DAGName> node <DAGMemberName> /forcecleanup
   ```

2. The DAG members in the second datacenter must now be restarted and then used to complete the eviction process from the second datacenter. Stop the Cluster service on each DAG member in the second datacenter by running the following command on each member:

   ```powershell
   net stop clussvc
   ```

3. On a DAG member in the second datacenter, force a quorum start of the Cluster service by running the following command:

   ```powershell
   net start clussvc /forcequorum
   ```

4. Open the Failover Cluster Management tool and connect to the DAG's underlying cluster. Expand the cluster, and then expand **Nodes**. Right-click each node in the primary datacenter, select **More Actions**, and then select **Evict**. When you're done evicting the DAG members in the primary datacenter, close the Failover Cluster Management tool.

## Activating Mailbox Servers

The steps needed to activate Mailbox servers during a datacenter switchover also depend on whether the DAG is in DAC mode. Before activating the DAG members in the second datacenter, we recommend that you validate that the infrastructure services in the second datacenter are ready for messaging service activation.

When the DAG is in DAC mode, the steps to complete activation of the mailbox servers in the second datacenter are as follows:

1. The Cluster service must be stopped on each DAG member in the second datacenter. You can use the **Stop-Service** cmdlet to stop the service (for example, `Stop-Service ClusSvc`), or use `net stop clussvc` from an elevated command prompt.

2. The Mailbox servers in the standby datacenter are then activated by using the [Restore-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Restore-DatabaseAvailabilityGroup) cmdlet. The Active Directory site of the standby datacenter is passed to the **Restore-DatabaseAvailabilityGroup** cmdlet to identify which servers to use to restore service and to configure the DAG to use an alternate witness server. If the alternate witness server wasn't previously configured, you can configure it by using the *AlternateWitnessServer* and *AlternateWitnessDirectory* parameters of the [Restore-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Restore-DatabaseAvailabilityGroup) cmdlet. If this command succeeds, the quorum criteria are shrunk to the servers in the standby datacenter. If the number of servers in that datacenter is an even number, the DAG will switch to using the alternate witness server as identified by the setting on the DAG object.

3. The databases can now be activated. Depending on the specific configuration used by the organization, this may not be automatic. If the servers in the standby datacenter have an activation blocked setting, the system won't do an automatic failover from the primary datacenter to the standby datacenter of any database. If no failover restrictions are present for any of the database copies in the standby datacenter, the system will activate copies in the second datacenter assuming they are healthy. If databases are configured with an activation blocked setting that requires explicit manual action, there are two choices for action:

   1. Clear the setting that blocks activation. This will make the system return to its default behavior, which is to activate any available copy.

   2. Leave the setting unchanged and use the [Move-ActiveMailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Move-ActiveMailboxDatabase) cmdlet to complete the database activation in the second datacenter. To complete this step using the **Move-ActiveMailboxDatabase** cmdlet when activation blocked is set, you must explicitly identify the target of the move.

4. The last step is to review all error and warning messages from the tasks. Any indicated warnings should be followed up and corrected. The task design model for these commands is to only fail if they can't achieve the fundamental goal of their design. For example, the **Restore-DatabaseAvailabilityGroup** cmdlet will fail if it can't shrink the quorum of the DAG to allow a server in the second datacenter to be restarted for servicing without causing a quorum outage. However, each task's output is also used to identify the issues that require administrator follow-up. You're strongly encouraged to save all task output and review it for follow-up actions.

When the DAG isn't in DAC mode, the steps to complete activation of the mailbox servers in the second datacenter are as follows:

1. The quorum must be modified based on the number of DAG members in the second datacenter.

   1. If there's an odd number of DAG members, change the DAG quorum model from a Node a File Share Majority to a Node Majority quorum by running the following command:

      ```powershell
      cluster <DAGName> /quorum /nodemajority
      ```

   2. If there's an even number of DAG members, reconfigure the witness server and directory by running the following command in the Exchange Management Shell:

      ```powershell
      Set-DatabaseAvailabilityGroup <DAGName> -WitnessServer <ServerName>
      ```

2. Start the Cluster service on any remaining DAG members in the second datacenter by running the following command:

   ```powershell
   net start clussvc
   ```

3. Perform server switchovers to activate the mailbox databases in the DAG by running the following command for each DAG member:

   ```powershell
   Move-ActiveMailboxDatabase -Server <DAGMemberinPrimarySite> -ActivateOnServer <DAGMemberinSecondSite>
   ```

4. Mount the mailbox databases on each DAG member in the second site by running the following command:

   ```powershell
   Get-MailboxDatabase <DAGMemberinSecondSite> | Mount-Database
   ```

## Activating Client Access Servers

Clients connect to service endpoints (for example Outlook Web App, Autodiscover, Exchange ActiveSync, Outlook Anywhere, POP3, IMAP4, and the RPC Client Access array) to access Exchange services and data. Therefore, activating Client Access servers involves changing the mapping of the DNS records for these service endpoints from IP addresses in the primary datacenter to the IP addresses in the second datacenter that are configured as the new service endpoints. Depending on your DNS configuration, the DNS records that need to be modified may or may not be in the same DNS zone.

## Activating Client Access Servers

Clients will then automatically connect to the new service endpoints in one of two ways:

- Clients will continue to try to connect, and should automatically connect after the TTL has expired for the original DNS entry, and after the entry is expired from the client's DNS cache. Users can also run the `ipconfig /flushdns` command from a command prompt to manually clear their DNS cache.

- Clients starting or restarting will perform a DNS lookup on startup and will get the new IP address for the service endpoint, which will be a Client Access server or array in the second datacenter.

Assuming that all appropriate configuration changes have been completed to define and configure the services in the second datacenter to function as they were in the primary datacenter, and assuming that the established DNS configuration is correct, no further changes should be needed to activate Client Access servers.

## Activating Transport Services

Clients and other servers that submit messages typically identify those servers using DNS. Activating transport services in the second datacenter involves changing DNS records to point to the IP addresses of the Mailbox servers in the second datacenter. Clients and sending servers will then automatically connect to the servers in the second datacenter in one of two ways:

- Clients will continue to try to connect, and should automatically connect after the TTL has expired for the original DNS entry, and after the entry is expired from the client's DNS cache. Users can also run the `ipconfig /flushdns` command from a command prompt to manually clear their DNS cache.

- Clients starting or restarting will perform a DNS lookup on startup and will get the new IP address for the SMTP endpoint, which will be a Mailbox server in the second datacenter.

Assuming that all appropriate configuration changes have been completed to define and configure the services in the second datacenter to function as they were in the primary datacenter, and assuming that the established DNS configuration is correct, no further changes should be needed to activate transport services.

## Activating Unified Messaging Services

Unified Messaging (UM) services connect to an organization's PBX system and phone lines. The logical connection between the PBX system and the Unified Messaging service is provided by an IP gateway. IP gateways include high availability functionality and are able to switch between multiple Unified Messaging services when a failure is detected.

If there are Unified Messaging services in the second datacenter that were in a disabled state because they are dedicated to the site resilience solution, they can be enabled by using the [Enable-UMService](https://docs.microsoft.com/powershell/module/exchange/Enable-UMService) cmdlet (for example, `Enable-UMService EX4`).

Assuming the IP gateways are associated with Unified Messaging services by using DNS servers, activating Unified Messaging services therefore involves changing DNS records to point to the new IP addresses that will be configured for the Unified Messaging service in the second datacenter. Assuming that all appropriate configuration changes have been completed to define and configure the services in the second datacenter to function as they were in the primary datacenter, and assuming that the established DNS configuration is correct, no further changes should be needed to activate Unified Messaging services.

If the IP gateway in use doesn't support the use of DNS names to resolve the Unified Messaging services, additional configuration steps will be necessary to manually point the IP gateway to the IP addresses of the Unified Messaging services in the second datacenter.

## Activating Edge Transport Servers

The steps to activate the Edge Transport server role will vary, depending on the specific configuration. Edge Transport servers in two datacenters can be configured in either an active/passive or an active/active configuration. In an active/passive configuration, the Edge Transport server in the second datacenter is idle until the second datacenter is activated. In an active/active configuration, Edge Transport servers in both datacenters are delivering mail at all times.

In an active/active configuration, no steps are necessary to activate the second datacenter's Edge Transport servers because they are already running. In an active/passive configuration, the DNS MX resource record for each SMTP domain needs to be updated as part of the switchover from the primary datacenter to the standby datacenter. Although the active/active configuration provides a simple datacenter switchover solution, it has the drawback of requiring careful load monitoring to make sure that after the datacenter switchover, the Edge Transport servers in the second datacenter can provide sufficient capacity to support the increased load now flowing through it, as a result of the Edge Transport servers in the primary datacenter being unavailable.

Even with an active/active configuration, it may be appropriate to update the MX resource records for your Edge Transport servers during a datacenter switchover. Allowing the MX resource record for the failed datacenter to continue to point at the failed datacenter means that when the datacenter starts recovering, it could start experiencing connection attempts to its Edge Transport servers. This could happen while the Edge Transport services are in an unstable state (for example, because dependent services in the datacenter are being restored).

Assuming the DNS records are under the control of the organization, activating Edge Transport servers involves updating the MX resource record for each SMTP domain hosted by the server.

> [!NOTE]
> If the MX resource record used by your organization isn't hosted by a DNS server under your organization's control, you might consider referencing a CNAME record in the MX resource record and using a CNAME record under the organization's control that can then be updated.

DNS updates enable incoming traffic, and outgoing traffic is handled by the activation of the mailbox databases in a site that has functioning Edge Transport servers:

- When incoming SMTP connections are initiated using the updated name resolution information, SMTP clients will connect to the Edge Transport servers in the second datacenter. Traffic will be appropriately routed by the Edge Transport server, and no further changes are required.

- When outgoing SMTP connections are initiated, they will try the locally available Edge Transport server, and those messages will be queued or immediately sent based on the status of the receiving server.

## Restoring Service to the Primary Datacenter

Generally, datacenter failures are either temporary or permanent. With a permanent failure, such as an event that has caused the permanent destruction of a primary datacenter, there's no expectation that the primary datacenter will be activated. However, with a temporary failure (for example, an extended power loss or extensive but repairable damage), there's an expectation that the primary datacenter will eventually be restored to full service.

The process of restoring service to a previously failed datacenter is referred to as a *switchback*. The steps used to perform a datacenter switchback are similar to the steps used to perform a datacenter switchover. A significant distinction is that datacenter switchbacks are scheduled, and the duration of the outage is often much shorter.

It's important that switchback not be performed until the infrastructure dependencies for Exchange have been reactivated, are functioning and stable, and have been validated. If these dependencies aren't available or healthy, it's likely that the switchback process will cause a longer than necessary outage, and the process could fail altogether.

## Mailbox Server Role Switchback

The Mailbox server role should be the first role that's switched back to the primary datacenter. The following steps detail the Mailbox server role switchback process:

1. As part of the datacenter switchover process, the Mailbox servers in the primary datacenter were put into a stopped state. When the environment (such as primary datacenter, Exchange dependencies, and wide area network (WAN) connectivity) is ready, the first step is to put the Mailbox servers in the restored primary datacenter into a started state and incorporate them into the DAG. The way in which this is done depends on whether the DAG is in DAC mode.

   1. If the DAG is in DAC mode, you can reincorporate the DAG members in the primary site by using the [Start-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Start-DatabaseAvailabilityGroup) cmdlet. Then, to make sure that the proper quorum model is being used by the DAG, run the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup) cmdlet against the DAG without specifying any parameters.

   2. If the DAG isn't in DAC mode, you can reincorporate the DAG members by using the [Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Add-DatabaseAvailabilityGroupServer) cmdlet.

2. After the Mailbox servers in the primary datacenter have been incorporated into the DAG, they will need some time to synchronize their database copies. Depending on the nature of the failure, the length of the outage, and actions taken by an administrator during the outage, this may require reseeding the database copies. For example, if during the outage, you remove the database copies from the failed primary datacenter to allow log file truncation to occur for the surviving active copies in the second datacenter, reseeding will be required. Each database can individually proceed from this point forward. After a replicated database copy in the primary datacenter is healthy, it can proceed to the next step.

    > [!NOTE]
    > This process doesn't require that all databases be moved at the same time. You are encouraged to move the majority of your organization's databases at one time, but some databases many linger in the second datacenter if there are issues associated with the database copies in the primary datacenter.

3. After a majority of the databases are in a healthy state in the primary datacenter, the switchback outage can be scheduled. When the scheduled time arrives, the following steps must be taken:

   1. During the datacenter switchover process, the DAG was configured to use an alternate witness server. The DAG must be reconfigured to use a witness server in the primary datacenter. If you are using the same witness server and witness directory that was used prior to the primary datacenter outage, you can run the `Set-DatabaseAvailabilityGroup -Identity DAGName` command. If you plan on using a witness server or witness directory that is different from the original witness server and directory, use the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup) command to configure the witness server and witness directory parameters with the appropriate values.

   2. The databases being reactivated in the primary datacenter should be dismounted in the second datacenter. You can use the [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/Dismount-Database) cmdlet to dismount the databases.

   3. After the databases have been dismounted, the Client Access server URLs should be moved from the second datacenter to the primary datacenter. This is accomplished by changing the DNS record for the URLs to point to the Client Access server or array in the primary datacenter. This will result in the system acting as though a database failover has occurred for each database being moved.

      > [!IMPORTANT]
      > Don't proceed to the next step until the Client Access server URLs have been moved and the DNS TTL and cache entries have expired. Activating the databases in the primary datacenter prior to moving the Client Access server URLs to the primary datacenter will result in an invalid configuration (for example, a mounted database that has no Client Access servers in its Active Directory site).

   4. Because each database in the primary datacenter is in a healthy state, it can be activated in the primary datacenter by performing database switchovers. This is accomplished by using the [Move-ActiveMailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Move-ActiveMailboxDatabase) cmdlet for each database that will be activated.

   5. After each database is moved to the primary datacenter, it can be mounted by using the [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/Mount-Database) cmdlet.

After one or more databases are active and mounted in the primary datacenter, switchback procedures for the other server roles can be performed.

## Client Access Server Switchback

As part of the switchover process, the internal and external DNS records used by clients, other servers, and IP gateways to resolve the service endpoints for Client Access servers, Transport and Unified Messaging services, and Edge Transport servers were modified to point to the corresponding endpoints in the second datacenter. The switchback process for the other server roles involves modifying those records to point to the restored service endpoints in the primary datacenter.

As with the DNS changes that were made during the switchover to the second datacenter, clients, servers, and IP gateways will continue to try to connect, and should automatically connect after the TTL has expired for the original DNS entry, and after the entry is expired from their DNS cache.

## Reestablishing Site Resilience

After switchback to the primary datacenter is completed successfully, you can reestablish site resilience for the primary datacenter by verifying the health and status of each mailbox database copy in the second datacenter. In addition, if any database copies in the second datacenter were originally blocked for activation, you can reconfigure those settings at this time.
