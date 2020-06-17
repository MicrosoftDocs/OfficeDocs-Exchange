---
localization_priority: Normal
description: 'Summary: The operational tasks of managing DAGs, mailbox database copies, and other high availability elements of Exchange Server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f9677392-88d2-457f-a488-245771a8c1f2
ms.reviewer:
title: Managing high availability and site resilience
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Managing high availability and site resilience

After you build, validate, and deploy a Microsoft Exchange Server high availability or site resilience solution, the solution transitions from the deployment phase to the operational phase of the overall solution lifecycle. The operational phase consists of several tasks, and all tasks are related to one of the following areas: database availability groups (DAGs), mailbox database copies, performing proactive monitoring, and managing switchovers and failovers.

## Database availability group management
<a name="Da"> </a>

The operational management tasks associated with DAGs include:

- **Creating one or more DAGs**: Creating a DAG is typically a one-time procedure performed during the deployment phase of the solution lifecycle. However, there may be reasons for creating DAGs that occur during the operational phase, for example:

  - The DAG is configured for third-party replication mode, and you want to revert to using continuous replication. You can't convert a DAG back to continuous replication; you need to create a DAG.

  - You have servers in multiple domains. All members of the same DAG must also be members of the same domain.

- **Managing DAG membership**: Managing DAG members is an infrequent task typically performed during the deployment phase of the solution lifecycle. However, because of the flexibility provided by incremental deployment, managing DAG membership may also be performed throughout the solution lifecycle.

- **Configuring DAG properties**: Each DAG has various properties that can be configured as needed. These properties include:

  - **Witness server and witness directory**: The witness server is a server outside the DAG that acts as a quorum voter when the DAG contains an even number of members. The witness directory is a directory created and shared on the witness server for use by the system in maintaining a quorum.

  - **IP addresses**: Each DAG will have one or more IPv4 addresses, and optionally, one or more IPv6 addresses. The IP addresses assigned to the DAG are used by the DAG's underlying cluster. The number of IPv4 addresses assigned to the DAG equals the number of subnets that comprise the MAPI network used by the DAG. You can configure the DAG to use static IP addresses or to obtain addresses automatically by using Dynamic Host Configuration Protocol (DHCP).

  - **Datacenter Activation Coordination mode**: Datacenter Activation Coordination mode is a property setting on a DAG that's designed to prevent split-brain conditions at the database level, in a scenario in which you're restoring service to a primary datacenter after a datacenter switchover has been performed. For more information about Datacenter Activation Coordination mode, see [Datacenter Activation Coordination mode](../../high-availability/database-availability-groups/dac-mode.md).

  - **Alternate witness server and alternate witness directory**: The alternate witness server and alternate witness directory are values that you can preconfigure as part of the planning process for a datacenter switchover. These refer to the witness server and witness directory that will be used when a datacenter switchover has been performed.

  - **Replication port**: By default, all DAGs use TCP port 64327 for continuous replication. You can modify the DAG to use a different TCP port for replication by using the _ReplicationPort_ parameter of the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup) cmdlet.

  - **Network discovery**: You can force the DAG to rediscover networks and network interfaces. This operation is used when you add or remove networks or introduce new subnets. Rediscovery of all DAG networks can be forced by using the _DiscoverNetworks_ parameter of the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup) cmdlet.

  - **Network compression**: By default, DAGs use compression only between DAG networks on different subnets. You can enable compression for all DAG networks or for seeding operations only, or you can disable compression for all DAG networks.

  - **Network encryption**: By default, DAGs use encryption only between DAG networks on different subnets. You can enable encryption for all DAG networks or for seeding operations only, or you can disable encryption for all DAG networks.

- **Shutting down DAG members**: The Exchange Server high availability solution is integrated with the Windows shutdown process. If an administrator or application initiates a shutdown of a Windows server in a DAG that has a mounted database that's replicated to one or more DAG members, the system will try to activate another copy of the mounted databases prior to allowing the shutdown process to complete. However, this new behavior doesn't guarantee that all of the databases on the server being shut down will experience a lossless activation. As a result, it's a best practice to perform a server switchover prior to shutting down a server that's a member of a DAG.

For detailed steps about how to create a DAG, see [Create a database availability group](create-dags.md). For detailed steps about how to configure DAGs and DAG properties, see [Configure database availability group properties](configure-dag-properties.md). For more information about each of the preceding management tasks, and about managing DAGs in general, see [Manage database availability groups](manage-dags.md).

## Mailbox database copy management
<a name="Ma"> </a>

The operational management tasks associated with mailbox database copies include:

- **Adding mailbox database copies**: When you add a copy of a mailbox database, continuous replication is automatically enabled between the existing database and the database copy.

- **Configuring mailbox database copy properties**: You can configure a variety of properties, such as the database activation policy, the amount of time, if any, for replay lag and truncation lag, and the activation preference for the database copy.

- **Suspending or resuming a mailbox database copy**: You can suspend a mailbox database copy in preparation for seeding, or for other forms of maintenance. You can also suspend a mailbox database copy for activation only. This configuration prevents the system from automatically activating the copy as a result of a failure, but it still allows the system to keep the database copy up to date with log shipping and replay.

- **Updating a mailbox database copy**: Updating, also known as *seeding*, is the process in which a copy of a mailbox database is added to another Mailbox server. This becomes the baseline database for the copy. After the initial first seed of the baseline database copy, only in rare circumstances will the database need to be seeded again.

- **Activating a mailbox database copy**: Activating is the process of designating a specific passive copy as the new active copy of a mailbox database. This process is referred to as a *switchover*. For more information, see "Switchovers and Failovers" later in this topic.

- **Removing a mailbox database copy**: Occasionally, it may be necessary to remove a mailbox database copy. For example, you can't remove a Mailbox server from a DAG until all mailbox database copies are removed from the server. In addition, you must remove all copies of a mailbox database before you can change the path for a mailbox database.

For detailed steps about how to add a mailbox database copy, see [Add a mailbox database copy](add-db-copies.md). For detailed steps about how to configure mailbox database copies, see [Configure mailbox database copy properties](configure-db-properties.md). For more information about each of the preceding management tasks, and about managing mailbox database copies in general, see [Manage mailbox database copies](manage-database-copies.md). For detailed steps about how to remove a mailbox database copy, see [Remove a mailbox database copy](remove-db-copies.md).

## Proactive monitoring
<a name="Pr"> </a>

Making sure that your servers are operating reliably and that your database copies are healthy are key objectives for daily messaging operations. Exchange Server includes a number of features that can be used to perform a variety of health monitoring tasks for DAGs and mailbox database copies, including:

- [Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabasecopystatus)

- [Test-ReplicationHealth](https://docs.microsoft.com/powershell/module/exchange/test-replicationhealth)

- Crimson channel event logging

In addition to monitoring the health and status, it's also critical to monitor for situations that can compromise availability. For example, we recommend that you monitor the redundancy of your replicated databases. It's critical to avoid situations where you're down to a single copy of a database. This scenario should be treated with the highest priority and resolved as soon as possible.

For more detailed information about monitoring the health and status of DAGs and mailbox database copies, see [Monitor database availability groups](monitor-dags.md).

## Switchovers and failovers
<a name="Sw"> </a>

A *switchover* is a manual process in which an administrator manually activates one or more mailbox database copies. Switchovers, which can occur at the database or server level, are typically performed as part of preparation for maintenance activities. Switchover management involves performing database or server switchovers as needed. For example, if you need to perform maintenance on a Mailbox server in a DAG, you would first perform a server switchover so that the server didn't host any active mailbox database copies. For detailed steps about how to perform a database switchover, see [Activate a mailbox database copy](activate-db-copies.md). Switchovers can also be performed at the datacenter level.

A *failover* is the automatic activation by the system of one or more database copies in reaction to a failure. For example, the loss of a disk drive in a RAID-less environment will trigger a database failover. The loss of the MAPI network or a power failure will trigger a server failover.
