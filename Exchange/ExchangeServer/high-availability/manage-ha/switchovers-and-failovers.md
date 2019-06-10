---
localization_priority: Normal
description: 'Summary: An overview of switchovers and failovers in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 75388645-cae1-402e-bf02-c4949d3e2c31
ms.date: 7/9/2018
ms.reviewer: 
title: Switchovers and failovers
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Switchovers and failovers

Switchovers and failovers are the two forms of outages in Microsoft Exchange Server:

- A *switchover* is a scheduled outage of a database or server that's explicitly initiated by a cmdlet or by the managed availability system in Exchange Server. Switchovers are typically done to prepare for performing a maintenance operation. Switchovers involve moving the active mailbox database copy to another server in the database availability group (DAG). If no healthy target is found during a switchover, administrators will receive an error and the mailbox database will remain up, or mounted.

- A *failover* refers to unexpected events that result in the unavailability of services, data, or both. A failover involves the system automatically recovering from the failure by activating a passive mailbox database copy to make it the active mailbox database copy. If no healthy target is found during a failover, the mailbox database will be dismounted.

Exchange Server is specifically designed to handle both switchovers and failovers.

Looking for management tasks related to high availability and site resilience? See [Managing high availability and site resilience](manage-ha.md).

## Switchovers

There are three types of switchovers in Exchange Server:

- Database switchovers

- Server switchovers

- Datacenter switchovers

### Database Switchovers

A *database switchover* is the process by which an individual active database is switched over to another database copy (a passive copy), and that database copy is made the new active database copy. Database switchovers can happen both within and across datacenters. A database switchover can be performed by using the Exchange Admin Center (EAC) or the Exchange Management Shell. Regardless of which interface is used, the switchover process is as follows:

1. The administrator initiates a database switchover to move the current active mailbox database copy to another server.

2. The client used for the task makes an RPC call to the Microsoft Exchange Replication service on a DAG member.

3. If the DAG member doesn't hold the Primary Active Manager (PAM) role, the DAG member refers the task to the server that holds the PAM role.

4. The task makes an RPC call to the Microsoft Exchange Replication service on the server that holds the PAM role.

5. The PAM reads and updates the database location information that's stored in the cluster database for the DAG.

6. The PAM contacts the Microsoft Exchange Replication service on the DAG member whose passive copy is being activated as the new active mailbox database copy.

7. The Microsoft Exchange Replication service on the target server queries the Microsoft Exchange Replication services on all other DAG members to determine the best log source for the database copy.

8. The database is dismounted from the current server and the Microsoft Exchange Replication service on the target server copies the remaining logs to the target server.

9. The Microsoft Exchange Replication service on the target server requests a database mount.

10. The Microsoft Exchange Information Store service on the target server replays the log files and mounts the database.

11. Any error codes are returned to the Microsoft Exchange Replication service on the target server.

12. The PAM updates the database copy state information in the cluster database for the DAG.

13. Any error codes are returned by the Microsoft Exchange Replication service on the target server to the Microsoft Exchange Replication service on the PAM.

14. The Microsoft Exchange Replication service on the PAM returns any errors to the administrative interface where the task was called.

15. Remote PowerShell returns the results of the operation to the calling administrative interface.

For detailed steps about how to perform a database switchover, see [Activate a mailbox database copy](activate-db-copies.md).

### Server Switchovers

A server switchover is the process by which all active databases on a DAG member are activated on one or more other DAG members. Like database switchovers, a server switchover can occur both within a datacenter and across datacenters, and it can be initiated by using both the EAC and the Exchange Management Shell. Regardless of which interface is used, the server switchover process is as follows:

1. The administrator initiates a server switchover to move all current active mailbox database copies to one or more other servers.

2. The task performs the same steps described earlier in this topic for database switchovers (Steps 2 through 4) for each of the active databases on the current server.

3. The PAM reads and updates the database location information that's stored in the cluster database for the DAG.

4. The PAM contacts the Microsoft Exchange Replication service on each DAG member that has a passive copy being activated.

5. The Microsoft Exchange Replication service on the target servers query the Microsoft Exchange Replication services on all other DAG members to determine the best log source for the database copy.

6. The database is dismounted from the current server and the Microsoft Exchange Replication service on each target server copies the remaining logs.

7. The Microsoft Exchange Replication service on each target server requests a database mount.

8. The Microsoft Exchange Information Store service on each target server replays the log files and mounts the database.

9. Any error codes are returned to the Microsoft Exchange Replication service on the target server.

10. The PAM updates the database copy state information in the cluster database for the DAG.

11. Any error codes are returned by the Microsoft Exchange Replication service on the target server to the Microsoft Exchange Replication service on the PAM.

12. The Microsoft Exchange Replication service on the PAM returns any errors to the administrative interface where the task was called.

13. Remote PowerShell returns the results of the operation to the calling administrative interface.

For detailed steps about how to perform a server switchover, see [Perform a server switchover](server-switchovers.md).

### Datacenter Switchovers

In a site resilient configuration, automatic recovery in response to a site-level failure can occur within a DAG, allowing the messaging system to remain in a functional state. This configuration requires at least three locations, as it requires deploying DAG members in two locations and the DAG's witness server in a third location.

If you don't have three locations, or even if you do have three locations but you want to control datacenter-level recovery actions, you can configure a DAG for manual recovery in the event of a site-level failure. In that event, you would perform a process called a *datacenter switchover*. As with many disaster recovery scenarios, prior planning and preparation for a datacenter switchover can simplify your recovery process and reduce the duration of your outage. For detailed steps to performing a datacenter switchover, see [Datacenter switchovers](datacenter-switchovers.md)

## Failovers

A failover is an automatic activation process that can occur at the database, server, or datacenter level. Failovers occur in response to a failure that affects an individual database (for example, an isolated storage loss) an entire server (for example, a motherboard failure or a loss of power), or an entire site (for example, the loss of all DAG members in a site).

DAGs and mailbox database copies provide full redundancy and rapid recovery of both the data and the services that provide access to the data. The following table lists the expected recovery actions for a variety of failures. Some failures require the administrator to initiate the recovery, and other failures are automatically handled by the system.

|**Description**|**Automatic activation**|**Automatic repair action**|**State during repair: Active**|**State during repair: Passive**|**Repair actions**|**Comments**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Extensible Storage Engine (ESE) soft database failure: The drives storing the database are returning errors on some reads (for example, a -1018 error).|Possible short outage.  <br/> Possible automatic failover.|Automatic patching of bad page.|Manual switchover, automatic failover, or online repair.|Failed|RAID rebuild, database and database copy repair, restore and run recovery then page patching, or page patching from copy.|There may be other soft database failure codes.  <br/> Doesn't include NTFS file system block failures.  <br/> If failover or switchover is performed, host server is updated.|
|ESE " *semi-soft"* database failure: The drives storing the database are returning errors on some writes.|Short outage during automatic failover.|Automatic volume/disk rebuilt after possible drive replacement.|Dismounted if can't be recovered.|Failed|RAID rebuild may solve the problem.  <br/> Copy and repair, restore and run recovery, or volume/disk rebuilt after possible replacement.|An ESE semi-soft write error means some writes are successful.  <br/> Doesn't include an NTFS block failure.|
|ESE "semi-soft" log failure: The drives storing the log data are returning non-recovered errors on some reads or writes.|Short outage during automatic failover.|Automatic volume/disk rebuilt after possible drive replacement.|Dismounted if can't be recovered.|Failed|RAID rebuild may solve the problem.  <br/> Copy and repair, restore and run recovery, or volume/disk rebuilt after possible replacement.|An ESE semi-soft read/write error means some reads/writes are successful.  <br/> If the database fails, automated recovery will occur before log data recovery processing starts.|
|ESE software error or resource exhaustion: An error where ESE terminates instance (for example, Event ID 1022, checkpoint depth too deep).|Short outage during automatic failover.|None.|Dismounted if can't be recovered.|Failed|Fix underlying resource issue.|This failure could be the surfaced error of other cases.|
|NTFS block failures: The drives storing the database or logs experiences a read or write error to an NTFS control structure.|Short outage during automatic failover.|Volume completely rebuilt after possible drive replacement.|Dismounted if can't be recovered.|Failed|RAID rebuild may solve the problem. NTFS utilities may solve the NTFS problems. Exchange recovery may be required.|This is more likely to occur when RAID isn't in use. If this impacts the active log volume, some recent log files will be lost.  <br/> Doesn't include errors automatically corrected by NTFS or its underlying software or hardware stack.|
|Database or log drive failure: A drive storing the database or logs has completely failed and is inaccessible.|Short outage during automatic failover.|Drive reformatted or replaced, followed by complete volume rebuild.|Dismounted if can't be recovered.|Failed|Drive replacement followed by possible RAID rebuild.  <br/> Drive replacement followed by complete volume rebuild.  <br/> Complete volume rebuild.|Not applicable.|
|Database or log volume failure: The volume fails due to NTFS or lower level volume issues.|Short outage during automatic failover.|Drive reformatted or replaced.|Dismounted if can't be recovered.|Failed|Drive replacement followed by possible RAID rebuild.  <br/> Drive replacement followed by complete volume rebuild.  <br/> Complete volume rebuild.|Not applicable.|
|Database or log volume out of space: The NTFS file system with the database or log files is out of space.|Automatic failover if other copy isn't in similar state.|None.|Dismounted.|Failed|Run full or incremental backups, manually delete logs, let time pass, resume database copy, or repair failed database copy.|Not applicable.|
|Administrator dismounts the wrong database.|If automatic failover isn't blocked by the administrator, there will be a short outage.  <br/> If automatic failover is prevented, there will be an outage until the database is mounted.|None.|Dismounted.|Not applicable|Administrator corrects the error.|Not applicable.|
|Administrator suspends the wrong database copy.|Depending on configuration and impacted copy, auto recovery may be prevented.|None.|Not applicable.|Suspended|Administrator corrects the error.|Not applicable.|
|Administrator dismounts a database for storage, NTFS, or volume maintenance.|If automatic failover isn't blocked by the administrator, there will be a short outage.  <br/> If automatic failover is blocked, there will be an outage until the administrator completes the task.|None.|Dismounted.|Not applicable|Administrator completes the task.|Not applicable.|
|Administrator suspends a database copy for storage, NTFS, or volume maintenance.|Depending on configuration and impacted copy, auto recovery may be prevented.|None.|Not applicable.|Suspended|Administrator completes the actions.|Not applicable.|
|Administrator dismounts a database for offline database maintenance.|Outage until repaired.|None.|Dismounted.|Suspended|Administrator completes the actions.|Active and passive database copies are diverged.  <br/> Administrator must suspend copies.|
|Storage area network (SAN), disk, or storage controller failure.|Short outage during automatic failover.|None.|Dismounted.|Any|Repair hardware.|A passive database copy will be in the state that existed at the time when the system failed.|
|Server hardware maintenance.|Short outage during automatic failover (unless blocked by an administrator).|None.|Dismounted.|Any|Complete actions.|A passive database copy will be in the state that existed at the time when the system was shut down.|
|Server software maintenance.|Short outage during automatic failover (unless blocked by an administrator).|None.|Dismounted.|Any|Complete actions.|A passive database copy will be in the state that existed at the time when the system was shut down.|
|Microsoft Exchange Information Store service is stopped or paused by an administrator.|Short outage during automatic failover.|None.|Dismounted.|Any|Restart the Microsoft Exchange Information Store service.|Not applicable.|
|Microsoft Exchange Information Store service fails; operating system is still running.|Short outage during automatic failover.|Service Control Manager restarts the Microsoft Exchange Information Store service.|Dismounted.|Any|Manually or automatically restart the Microsoft Exchange Information Store service.|A passive database copy will be in the state that existed when the Microsoft Exchange Information Store service failed.|
|Partial Microsoft Exchange Information Store service failure; some part of the Exchange store stops functioning, but it's not identified as completely failed.|Possible short outage during automatic failover.|None.|Mounted and partially functional.|Any, but may be only partially functional|Restart server, operating system, or Microsoft Exchange Information Store service.|Not applicable.|
| Server failure: The server fails for one of the following reasons:  <br/>  Complete power failure  <br/>  Unrecovered failure of the processor chip, motherboard, or backplane  <br/>  Operating system stop error  <br/>  Operating system stops responding  <br/>  Complete communication failure|Short outage during automatic failover.|Restart computer.|Dismounted.|Any|Restore power, change operating system settings, change hardware settings, replace hardware, restart operating system, service operating system, service hardware, or repair communication problems.|Not applicable.|
|DAG experiences a quorum failure.|Outage until repaired.|None.|Dismounted.|Any|Repair failed quorum, assign new quorum, or restore the network that's causing quorum failure.|A passive database copy will be in the state that existed at the time when the system failed.|
|MAPI network communication failure: The server is no longer available on the MAPI network.|Short outage during automatic failover; must be lossless.|None. Communication continues to be attempted.|Dismounted.|Any|Fix communication problem by correcting hardware or software issues.|Not applicable.|
|Replication network communication failure: The server can't receive heartbeats, log copies, or seed through the failed replication network.|Possible short copying or seeding outage while the workload is switched to other network.|None. Communication continues to be attempted.|None.|Any|Fix communication problem by correcting hardware or software issues.|Resiliency impacted by failure.|
|Multiple network communication failure: The server can't receive heartbeats, log copies, or seed through multiple networks.|Short outage during automatic failover; must be lossless.|None. Communication continues to be attempted.|Dismounted.|Any|Fix communication problem by correcting hardware or software issues.|At least one network is still functional.|
|Partial failure of one or more networks: Networks experience high error rates.|Failure not detected; no action.|None.|Mounted, but possible performance issues.|Any|Fix communication problem by correcting hardware or software issues.|Network experiences higher than normal error rates.|
|Undetected operating system hang: Operating system stops responding but it's not detected by monitoring or clustering.|None.|None.|Any.|Any|Restart or terminate the resources that aren't responding.|Hang isn't detected so no action is taken.  <br/> Some functionality may be operational.|
|Operating system drive experiences a failure.|Short outage during automatic failover.|None.|Dismounted.|Any|Replace drive and rebuild server or rebuild volume by using RAID.|Not applicable.|
|Operating system drive out of space.|Short outage during automatic failover.|None.|Dismounted.|Any|Manually free space on the volume.|Not applicable.|
|Drive containing Exchange binaries experiences a volume or drive failure.|Short outage during automatic failover.|None.|Dismounted.|Any|Replace drive and reinstall application or rebuild volume by using RAID.|Not applicable.|
|Drive containing the Exchange binaries is out of space.|Short outage during automatic failover.|None.|Dismounted.|Any|Manually free space on the volume.|Not applicable.|
|Invalid new log detected: The log sequence is disrupted by an existing file.|Short outage during automatic failover; assume other copies don't have the same problem.|None.|Dismounted.|Failed|Remove disruptive logs after determining source.|The disruptive logs shouldn't replicate.|
|Continuous replication detects invalid log: Replay detects an inappropriate log during copy or replay.|Not applicable.|Discard log.|Not applicable.|Failed|Discard invalid log; move impacting log stream.|Not applicable.|

### Database Failovers

A database failover occurs when a database copy that was active is no longer able to remain active. The following occurs as part of a database failover:

1. The database failure is detected by the Microsoft Exchange Information Store service.

2. The Microsoft Exchange Information Store service writes failure events to the crimson channel event log.

3. The Active Manager on the server that contains the failed database detects the failure events.

4. The Active Manager requests the database copy status from the other servers that hold a copy of the database.

5. The other servers return the requested database copy status to the requesting Active Manager.

6. The PAM initiates a move of the active database to another server in the DAG using a best copy selection algorithm.

7. The PAM updates the database mount location in the cluster database to refer to the selected server.

8. The PAM sends a request to the Active Manager on the selected server to become the database master.

9. The Active Manager on the selected server requests that the Microsoft Exchange Replication service attempt to copy the last logs from the previous server and set the mountable flag for the database.

10. The Microsoft Exchange Replication service copies the logs from the server that previously had the active copy of the database.

11. The Active Manager reads the maximum log generation number from the cluster database.

12. The Microsoft Exchange Information Store service mounts the new active database copy.

### Server Failovers

A server failover occurs when the DAG member is no longer able to service the MAPI network, or when the Cluster service on a DAG member is no longer able to contact the remaining DAG members. The following occurs as part of a server failover:

1. The Cluster service on the PAM sends a notification to the PAM for one of two conditions:

1. **Node Down**: The server is reachable but is unable to participate in DAG operations.

2. **MAPI Network Down**: The server can't be contacted over the MAPI network and therefore can't participate in DAG operations.

2. If the server is reachable, the PAM contacts the Active Manager on the affected server and requests that all databases be immediately dismounted.

3. For each affected database copy:

1. The PAM requests the database copy status from all servers in the DAG.

2. The PAM receives a response from all reachable and active DAG members.

3. The PAM tries to determine the best log source among all responding servers by querying the most recent log generation number from each of the responders.

4. Each of the servers responds with the log generation number.

4. The PAM retrieves the current search index catalog status from the cluster database.

5. Based on the log generation number and catalog health of each database copy, the PAM selects the best copies to activate.

6. The PAM updates the mounted location of the database in the cluster database.

7. The PAM initiates database failover by communicating with the Active Manager on one or more other servers.

8. The Active Manager on the selected servers requests that the Microsoft Exchange Replication service attempt to copy the last logs from the previous server and set the mountable flag.

9. When the database is mountable, the Active Manager on the servers mounts the databases.

For more information about Active Manager's best copy selection process, see [Active Manager](../../high-availability/database-availability-groups/active-manager.md).

### Datacenter Failovers

Significant changes have been made since Exchange 2010 regarding site resilience configuration. With namespace simplification, consolidation of server roles, separation of Client Access services and DAG recovery (in Exchange Server, the namespace does not need to move with the DAG), and changes around load balancing, Exchange Server provides site resilience options like the ability to use a single global namespace. If you have more than two locations in which to deploy messaging service components, Exchange Server also provides the ability to configure the messaging service for automatic failover in response to failures that required manual intervention in previous versions.

Exchange leverages fault tolerance built into the namespace through multiple IP addresses, load balancing, and, if necessary, the ability to take servers in and out of service. Exchange Server makes it possible to leverage the clients' ability to cache multiple IP addresses returned from a DNS server in response to a name resolution request. Clients with the ability to cache multiple IP addresses (which includes almost all HTTP-based clients in Exchange Server, such as Outlook, Outlook Anywhere, EAS, EWS, Outlook on the web, EAC, RPS, etc.), all have the ability to use those multiple IP addresses, and this provides failover on the client side. You can configure DNS to hand multiple IP addresses to a client during name resolution. The client asks for mail.contoso.com and gets back two IP addresses, or four IP addresses, for example. However many IP addresses the client gets back will be used reliably by the client. This makes the client a lot better off because if one of the IP addresses fails, the client has one or more others to try to connect to. If a client tries one and it fails, it waits around 20 seconds and then tries the next one in the list. Thus, if you lose connectivity to your primary Client Access services (CAS) array, and you have a second published IP address for a second CAS array, recovery for the clients happens automatically (and in about 21 seconds).

Modern HTTP clients (operating systems and Web browsers that are ten years old or less) simply work with this redundancy automatically. The HTTP stack can accept multiple IP addresses for an FQDN, and if the first IP it tries fails hard (e.g., cannot connect), it will try the next IP in the list. In a soft failure (connect lost after session established, perhaps due to an intermittent failure in the service where, for example, a device is dropping packets and needs to be taken out of service), the user might need to refresh their browser.

With the proper configuration, failover can happen at the client level and clients will be automatically redirected to a second datacenter where Client Access services is running, and the servers that are running Client Access services will proxy the communication back to the user's Mailbox server, which remains unaffected by the outage (because you don't do a switchover). Instead of working to recover service, the service recovers itself and you can focus on fixing the core issue (e.g., replacing a failed load balancer).

Since you can failover the namespace between datacenters, all that is needed to achieve a datacenter failover is a mechanism for failover of the Mailbox role across datacenters. To get automatic failover for the DAG, you simply architect a solution where the DAG is evenly split between two datacenters, and then place the witness server in a third location so that it can be arbitrated by DAG members in either datacenter, regardless of the state of the network between the datacenters that contain the DAG members. The key is that third location is isolated from network failures that affect the locations containing the DAG members.

If you only have two datacenters and would like to be able to configure automatic failover, you can utilize Microsoft Azure as your third location. You will need to create an Azure virtual network and connect it to your two datacenters using a multi-point VPN. You will then be able to place your witness server on a Microsoft Azure virtual machine. For more information, see [Using a Microsoft Azure VM as a DAG witness server](azure-vms-as-dag-witness-servers.md).
