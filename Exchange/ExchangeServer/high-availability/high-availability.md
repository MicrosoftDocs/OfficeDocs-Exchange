---
localization_priority: Normal
description: 'Summary: Learn about the high availability and site resilience capabilities available in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b1704c87-5f5c-464e-9dc5-1bb1bdf9ac2a
ms.reviewer: 
title: High availability and site resilience
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# High availability and site resilience

You can protect your Exchange Server mailbox databases and the data they contain by configuring your Exchange servers and databases for high availability and site resilience. Exchange Server minimizes the cost and complexity of deploying a highly available and resilient messaging solution while providing high levels of service and data availability and support for very large mailboxes.

Exchange Server enables customers of all sizes and in all segments to economically deploy a messaging continuity service in their organization by building on the native replication capabilities and high availability architecture introduced in Exchange 2010. For a list of changes since Exchange 2010, see [Changes to high availability and site resilience over previous versions](ha-changes.md).

## Key terminology
<a name="KT"> </a>

The following key terms are important to understand high availability or site resilience:

 *Active Manager*

> An internal Exchange component which runs inside the Microsoft Exchange Replication service that's responsible for failure monitoring and corrective action through failover within a database availability group (DAG).

 *AutoDatabaseMountDial*

> A property setting of a Mailbox server that determines whether a passive database copy will automatically mount as the new active copy, based on the number of log files missing by the copy being mounted.

 *Continuous replication - block mode*

> In block mode, as each update is written to the active database copy's active log buffer, it's also shipped to a log buffer on each of the passive mailbox copies in block mode. When the log buffer is full, each database copy builds, inspects, and creates the next log file in the generation sequence.

 *Continuous replication - file mode*

> In file mode, closed transaction log files are pushed from the active database copy to one or more passive database copies.

 *Database availability group*

> A group of up to 16 Exchange servers that hosts a set of replicated databases.

 *Database mobility*

> The ability of an Exchange Server mailbox database to be replicated to and mounted on other Exchange servers.

 *Datacenter*

> Typically this refers to an Active Directory site; however, it can also refer to a physical site. In the context of this documentation, datacenter equals Active Directory site.

 *Datacenter Activation Coordination mode*

> A property of the DAG setting that, when enabled, forces the Microsoft Exchange Replication service to acquire permission to mount databases at startup.

 *Disaster recovery*

> Any process used to manually recover from a failure. This can be a failure that affects a single item, or it can be a failure that affects an entire physical location.

 *Exchange third-party replication API*

> An Exchange-provided API that enables use of third-party synchronous replication for a DAG instead of continuous replication.

 *High availability*

> A solution that provides service availability, data availability, and automatic recovery from failures that affect the service or data (such as a network, storage, or server failure).

 *Incremental deployment*

> The ability to deploy high availability and site resilience after Exchange Server is installed.

 *Lagged mailbox database copy*

> A passive mailbox database copy that has a log replay lag time greater than zero.

 *Mailbox database copy*

> A mailbox database (.edb file and logs), which is either active or passive.

 *Mailbox resiliency*

> The name of a unified high availability and site resilience solution in Exchange Server.

 *Managed availability*

> A set of internal processes made up of probes, monitors, and responders that incorporate monitoring and high availability across all server roles and all protocols.

 *\*over* (pronounced "star over")

> Short for *switchovers* and *failovers*. A switchover is a manual activation of one or more database copies. A failover is an automatic activation of one or more database copies after a failure.

 *Safety Net*

> Formerly known as transport dumpster, this is a feature of the transport service that stores a copy of all messages for _X_ days. The default setting is 2 days.

 *Shadow redundancy*

> A transport server feature that provides redundancy for messages for the entire time they're in transit.

 *Site resilience*

> A configuration that extends the messaging infrastructure to multiple Active Directory sites to provide operational continuity for the messaging system in the event of a failure affecting one of the sites.

## Database availability groups
<a name="DA"> </a>

A DAG is the base component of the high availability and site resilience framework built into Exchange Server. A DAG is a group of up to 16 Exchange servers that hosts a set of databases and provides automatic, database-level recovery from failures that affect individual databases, networks, or servers. Any server in a DAG can host a copy of a mailbox database from any other server in the DAG. When a server is added to a DAG, it works with the other servers in the DAG to provide automatic recovery from failures that affect mailbox databases, such as a disk failure or server failure. For more information about DAGs, see [Database availability groups](database-availability-groups/database-availability-groups.md).

## Mailbox database copies
<a name="MD"> </a>

The high availability and site resilience features used first introduced in Exchange 2010 are used in Exchange Server to create and maintain database copies. Exchange Server also leverages the concept of database mobility, which is Exchange-managed database-level failovers.

Database mobility disconnects databases from servers and adds support for up to 16 copies of a single database. It also provides a native experience for creating copies of a database.

Setting a database copy as the active mailbox database is known as a *switchover*. When a failure affecting a database or access to a database occurs and a new database becomes the active copy, this process is known as a *failover*. This process also refers to a server failure in which one or more servers bring online the databases previously online on the failed server. When either a switchover or failover occurs, other Exchange servers become aware of the switchover almost immediately and redirect client and messaging traffic to the new active database.

For example, if an active database in a DAG fails because of an underlying storage failure, Active Manager will automatically recover by failing over to a database copy on another server in the DAG. In Exchange Server, managed availability provides behaviors to recover from loss of protocol access to a database, including recycling application worker pools, restarting services and servers, and initiating database failovers.

For more information about mailbox database copies, see [Mailbox database copies](database-availability-groups/database-copies.md).

## Active Manager
<a name="AM"> </a>

Exchange Server leverages Active Manager to manage the database and database copy health, status, continuous replication, and other aspects of high availability. For more information about Active Manager, see [Active Manager](database-availability-groups/active-manager.md).

## Site resilience
<a name="Site"> </a>

In Exchange 2010, you could deploy a DAG across two datacenters and host the witness in a third datacenter and enable failover for the Mailbox server role for either datacenter. But you didn't get failover for the solution itself because the namespace still needed to be manually changed for the non-Mailbox server roles.

In Exchange 2016 and Exchange 2019, the namespace doesn't need to move with the DAG. Exchange leverages fault tolerance built into the namespace through multiple IP addresses, load balancing (and if need be, the ability to take servers in and out of service). Modern HTTP clients work with this redundancy automatically. The HTTP stack can accept multiple IP addresses for a fully qualified domain name (FQDN), and if the first IP address it tries fails hard (that is, it can't connect), it will try the next IP address in the list. In a soft failure (connection is lost after the session is established, perhaps due to an intermittent failure in the service where, for example, a device is dropping packets and needs to be taken out of service), the user might need to refresh their browser.

This means the namespace is no longer a single point of failure as it was in Exchange 2010. In Exchange 2010, perhaps the biggest single point of failure in the messaging system is the FQDN that you give to users because it tells the user where to go. In the Exchange 2010 paradigm, changing where that FQDN goes isn't easy because you have to change DNS, and then handle DNS latency, which in some parts of the world is challenging. And you have name caches in browsers that are typically about 30 minutes or more that also have to be handled.

In Exchange Server, clients have more than one place to go. Almost all the client access protocols in Exchange Server are HTTP based. Examples include Outlook, EAS, EWS, Outlook on the web, and EAC). All supported HTTP clients have the ability to use multiple IP addresses, thereby providing failover on the client side. You can configure DNS to hand multiple IP addresses to a client during name resolution. The client asks for mail.contoso.com and gets back two IP addresses, or four IP addresses, for example. However many IP addresses the client gets back will be used reliably by the client. This makes the client a lot better off because if one of the IP addresses fails, the client has one or more alternative IP addresses to try to connect to. If a client tries one and it fails, it waits about 20 seconds and then tries the next one in the list. Thus, if you lose the VIP for the Client Access service array, recovery for the clients happens automatically, and in about 21 seconds.

The benefits include the following:

- In Exchange Server, if you lose the load balancer in your primary site, you simply turn it off (or maybe turn off the VIP) and repair or replace it. Clients that aren't already using the VIP in the secondary datacenter will automatically fail over to the secondary VIP without any change of namespace, and without any change in DNS. Not only does that mean you no longer have to perform a switchover, but it also means that all of the time normally associated with a datacenter switchover recovery isn't spent. In Exchange 2010, you had to handle DNS latency (hence, the recommendation to set the Time to Live (TTL) to 5 minutes, and the introduction of the failback URL). In Exchange 2016 and Exchange 2019, you don't need to do that because you get fast failover (20 seconds) of the namespace between VIPs (datacenters).

- Because you can fail over the namespace between datacenters, all that's needed to achieve a datacenter failover is a mechanism for failover of the Mailbox server role across datacenters. To get automatic failover for the DAG, you simply architect a solution where the DAG is evenly split between two datacenters, and then place the witness server in a third location so that it can be arbitrated by DAG members in either datacenter, regardless of the state of the network between the datacenters that contain the DAG members. If you only have two datacenters and a third physical location isn't available, you can place the witness server on a Microsoft Azure virtual machine. See [Using a Microsoft Azure VM as a DAG witness server](manage-ha/azure-vms-as-dag-witness-servers.md) for more information.

- In this scenario, the administrator's efforts are geared toward simply fixing the problem, and not spent restoring service. You simply fix the thing that failed; while service has been running and data integrity has been maintained. The urgency and stress level you feel when fixing a broken device is nothing like the urgency and stress you feel when you're working to restore service. It's better for the end user, and less stressful for the administrator.

You can allow failover to occur without having to perform switchbacks (sometimes mistakenly referred to as failbacks). If you lose servers in your primary datacenter, resulting in a 20 second interruption for clients, you might not even care about failing back. At this point, your primary concern would be fixing the core issue (for example, replacing the failed load balancer). After it's back online and functioning, some clients will start using it, and other clients might remain operational through the second datacenter.

Exchange Server also provides functionality that enables administrators to deal with intermittent failures. An intermittent failure is where, for example, the initial TCP connection can be made, but nothing happens afterward. An intermittent failure requires some sort of extra administrative action to be taken because it might be the result of a replacement device being put into service. While this repair process is occurring, the device might be powered on and accepting some requests, but not really ready to service clients until the necessary configuration steps are performed. In this scenario, the administrator can perform a namespace switchover by simply removing the VIP for the device being replaced from DNS. Then during that service period, no clients will be trying to connect to it. After the replacement process has completed, the administrator can add the VIP back to DNS, and clients will eventually start using it.

For details about planning and deploying site resilience, see [Plan for high availability and site resilience](plan-ha.md) and [Deploying high availability and site resilience](deploy-ha.md).

## Third-party replication API
<a name="API"> </a>

Exchange Server includes a third-party replication API that enables organizations to use third-party synchronous replication solutions instead of the built-in continuous replication feature. Microsoft supports third-party solutions that use this API, provided that the solution provides the necessary functionality to replace all native continuous replication functionality that's disabled as a result of using the API. Solutions are supported only when the API is used within a DAG to manage and activate mailbox database copies. Use of the API outside of these boundaries isn't supported. In addition, the solution must meet the applicable Windows hardware support requirements. (Test validation isn't required for support.)

When deploying a solution that uses the built-in third-party replication API, be aware that the solution vendor is responsible for primary support of the solution. Microsoft supports Exchange data for both replicated and non-replicated solutions. Solutions that use data replication must adhere to the Microsoft support policy for data replication. In addition, solutions that utilize the Windows Failover Cluster resource model must meet Windows cluster supportability requirements as described in Microsoft Knowledge Base article 943984, [The Microsoft Support Policy for Windows Server 2008 or Windows Server 2008 R2 Failover Clusters](https://support.microsoft.com/help/943984) or [The Microsoft Support Policy for Windows Server 2012 Failover Clusters](https://support.microsoft.com/help/2775067).

Microsoft's backup and restore support policy for deployments that use third-party replication API-based solutions is the same as for native continuous replication deployments.

If you're a partner seeking information about the third-party API, contact your Microsoft representative.

## High availability and site resilience documentation
<a name="docs"> </a>

The following table contains links to topics that will help you learn about and manage DAGs, mailbox database copies, and backup and restore for Exchange Server.

|**Topic**|**Description**|
|:-----|:-----|
|[Database availability groups](database-availability-groups/database-availability-groups.md)|Learn about DAGs, Active Manager, Datacenter Activation Coordination (DAC) mode, and mailbox database copies.|
|[Plan for high availability and site resilience](plan-ha.md)|Learn about the general, hardware, network, software, witness server, and other requirements and best practices for DAGs.|
|[Deploying high availability and site resilience](deploy-ha.md)|Explore an example deployment scenario for deploying and configuring DAGs.|
|[Managing high availability and site resilience](manage-ha/manage-ha.md)|Learn about DAG management tasks, switchovers and failovers, and maintenance mode.|
|[Monitor database availability groups](manage-ha/monitor-dags.md)|Learn about the built-in cmdlets and scripts for monitoring DAGs and database copies.|
|[Backup, restore, and disaster recovery](disaster-recovery/disaster-recovery.md)|Learn about backing up and restoring Exchange databases, recovery databases, and server recovery.|
