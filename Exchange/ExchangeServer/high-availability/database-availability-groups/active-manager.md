---
localization_priority: Normal
description: 'Summary: Learn about Active Manager in Exchange Server 2016 and Exchange Server 2019 and how best copy and server selection (BCSS) works.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: f4be27b7-1d7c-47b4-87ac-bfdfcc046f00
ms.date: 7/9/2018
ms.reviewer: 
title: Active Manager
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Active Manager

Microsoft Exchange Server includes a component called *Active Manager* that manages the high availability platform that includes the database availability group (DAG) and mailbox database copies. Active Manager runs inside the Microsoft Exchange Replication service (MSExchangeRepl.exe) on all Mailbox servers. On Mailbox servers that aren't members of a DAG, there is a single Active Manager role: *Standalone Active Manager*.

On servers that are members of a DAG, there are two Active Manager roles: *Primary Active Manager* (PAM) and *Standby Active Manager* (SAM). PAM is the Active Manager role in a DAG that decides which copies will be active and passive. PAM is responsible for getting topology change notifications and reacting to server failures. The DAG member that holds the PAM role is always the member that currently owns the cluster quorum resource (default cluster group). If the server that owns the cluster quorum resource fails, the PAM role automatically moves to a surviving server that takes ownership of the cluster quorum resource. In addition, if you need to take the server that hosts the cluster quorum resource offline for maintenance or an upgrade, you must first move the PAM to another server in the DAG. The PAM controls all movement of the active designations between a database's copies. (Only one copy can be active at any specified time, and that copy may be mounted or dismounted.) The PAM also performs the functions of the SAM role on the local system (detecting local database and local Information Store failures).

The SAM provides information on which server hosts the active copy of a mailbox database to other components of Exchange that are running an Active Manager client component (for example, Client Access or Transport services). The SAM detects failures of local databases and the local Information Store. It reacts to failures by asking the PAM to initiate a failover (if the database is replicated). A SAM doesn't determine the target of failover, nor does it update a database's location state in the PAM. It will access the active database copy location state to answer queries for the active copy of the database that it receives.

> [!NOTE]
> Exchange Server isn't a clustered application. Instead, it uses the cluster library functions implemented in clusapi.dll for cluster, group, cluster network (heartbeating), node management, cluster registry, and a few control code functions. In addition, Active Manager stores current mailbox database information (for example, active and passive data, and mounted data) in the cluster database (also known as the cluster registry). Although the information is stored directly in the cluster database, it isn't accessed directly by any other components.

In Exchange Server, the Microsoft Exchange Replication service periodically monitors the health of all mounted databases. In addition, it also monitors the Extensible Storage Engine (ESE) for any I/O errors or failures. When the service detects a failure, it notifies Active Manager. Active Manager then determines which database copy should be mounted and what it requires to mount that database. In addition, it tracks the active copy of a mailbox database (based on the last mounted copy of the database) and provides the tracking results information to Client Access services on the Mailbox server to which the client is connected.

## Best Copy Selection

When a failure occurs that prevents access to the active copy of a replicated mailbox database, Active Manager selects the best possible passive copy of the affected database to activate. This process was known as best copy selection (BCS) in earlier versions of Exchange, and in Exchange 2016 and Exchange 2019 it's known as best copy and server selection (BCSS). The general process occurs in the following order:

1. Managed availability or Active Manager detects a failure, or an administrator initiates a targetless switchover.

2. The PAM runs the BCSS internal algorithm.

3. A process called *attempt copy last logs* (ACLL) occurs, which tries to copy any missing log files from the server that hosted the active database copy prior to the failure or switchover.

4. After the ACLL process has completed, the value of the _AutoDatabaseMountDial_ for the Mailbox servers hosting copies of the database is compared with the copy queue length of the database being activated. At this point, either:

  - The number of missing log files is equal to or less than the value of _AutoDatabaseMountDial_, in which case Step 5 occurs.

  - The number of missing log files is greater than the value of _AutoDatabaseMountDial_, in which case Active Manager will try to activate next best available copy, if there is one.

5. The PAM issues a mount request to the Microsoft Exchange Information Store via remote procedure call (RPC). At this point, either:

  - The database mounts and is made available to clients.

  - The database doesn't mount, and PAM performs steps 3 and 4 on the next best copy (if one is available).

In earlier versions of Exchange, the BCS process evaluated several aspects of each database copy to determine the best copy to activate. These included:

- Copy queue length

- Replay queue length

- Database status

- Content index status

In Exchange Server, Active Manager runs through all of the same BCS checks and phases, but now it also includes the use of a constraint of the decreasing order of health states. Specifically, BCSS includes several new health checks that are part of the built in managed availability monitoring components in Exchange Server. There are four additional checks performed by Active Manager (listed in the order in which they are performed):

1. **All Healthy**: Checks for a server hosting a copy of the affected database that has all monitoring components in a healthy state.

2. **Up to Normal Healthy**: Checks for a server hosting a copy of the affected database that has all monitoring components with Normal priority in a healthy state.

3. **All Better than Source**: Checks for a server hosting a copy of the affected database that has monitoring components in a state that's better than the current server hosting the affected copy.

4. **Same as Source**: Checks for a server hosting a copy of the affected database that has monitoring components in a state that's the same as the current server hosting the affected copy.

If BCSS is invoked as a result of a failover that's triggered by a monitoring component (for example, via a Failover responder), an additional mandatory constraint is enforced where the target server's component health must be better than the server on which the failover occurred. For example, if a failure of Outlook on the web triggers a failover via a Failover responder, BCSS must select a server hosting a copy of the affected database on which Outlook on the web is healthy.

### Best copy selection process

With respect to database failures (not protocol failures), Active Manager begins the best copy selection process by creating a list of database copies that are potential candidates for activation. Any database copies that are unreachable or are administratively blocked from activation are ignored and not used during the selection process. The order of the list depends on the value of the _AutoDatabaseMountDial_:

- If the _AutoDatabaseMountDial_ is configured with any value other than `Lossless` on all servers that host a copy of the database, Active Manager sorts the resulting list using the copy queue length as the primary key. The calculation is based on LastLogInspected (from the copy's point of view), so the list of potential copies is sorted by the highest value for LastLogInspected (which will be the copy with the lowest copy queue length). If necessary, Active Manager sorts the list a second time, using the value for activation preference as a secondary key to break any tie conditions where two or more passive copies have the same copy queue length. The copy with the lowest activation preference value has the higher priority on the list.

- If the _AutoDatabaseMountDial_ is configured with a value of `Lossless` on any server that hosts a copy of the database, Active Manager sorts the resulting list in ascending order by using the value for activation preference as the primary key. In addition, when an administrator performs a lossless server or database switchover without specifying a target, Active Manager also sorts the resulting list in ascending order by using the value for activation preference as the primary key.

Next, Active Manager attempts to locate a mailbox database copy on the list that has a status of Healthy, DisconnectedAndHealthy, DisconnectedAndResynchronizing, or SeedingSource, and then evaluates the activation potential of each of the copies on the list by using an order set of ten criteria. Active Manager determines if any of the candidates for activation meet the first set of criteria:

- It has a content index with a status of Healthy.

- It has a copy queue length less than 10 log files.

- It has a replay queue length less than 50 log files.

If none of the database copies meets the first set of criteria, Active Manager tries to locate a database copy that meets the second set of criteria:

- It has a content index with a status of Crawling.

- It has a copy queue length less than 10 log files.

- It has a replay queue length less than 50 log files.

If none of the database copies meets the second set of criteria, Active Manager tries to locate a database copy that meets the third set of criteria:

- It has a content index with a status of Healthy.

- It has a replay queue length less than 50 log files.

If none of the database copies meets the third set of criteria, Active Manager tries to locate a database copy that meets the fourth set of criteria:

- It has a content index with a status of Crawling.

- It has a replay queue length less than 50 log files.

If none of the database copies meets the fourth set of criteria, Active Manager tries to locate a database copy that meets the fifth set of criteria:

- It has a replay queue length less than 50 log files.

If none of the database copies meets the fifth set of criteria, Active Manager tries to locate a database copy that meets the sixth set of criteria:

- It has a content index with a status of Healthy.

- It has a copy queue length less than 10 log files.

If none of the database copies meets the sixth criteria, Active Manager tries to locate a database copy that meets the seventh set of criteria:

- It has a content index with a status of Crawling.

- It has a copy queue length less than 10 log files.

If none of the database copies meets the seventh set of criteria, Active Manager tries to locate a database copy that meets the eighth set of criteria:

- It has a content index with a status of Healthy.

If none of the database copies meets all of the eighth set of criteria, Active Manager tries to locate a database copy that meets the ninth set of criteria:

- It has a content index with a status of Crawling.

If none of the database copies meets the ninth set of criteria, Active Manager tries to activate any database copy with a status of Healthy, DisconnectedAndHealthy, DisconnectedAndResynchronizing, or SeedingSource (the tenth set of criteria). If it can't find any database copies that meet the tenth set of criteria, it isn't able to automatically activate a database copy.

After one or more copies are located that meet one or more sets of criteria, the ACLL process copies any log files from the original source to the potential new active copy. After the ACLL process has completed, the PAM issues a mount request and either the database mounts and is made available to clients, or the database doesn't mount and the PAM searches for the next best copy (if one is available).
