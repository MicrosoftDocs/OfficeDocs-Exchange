---
localization_priority: Normal
description: 'Summary: Learn about database availability groups (DAGs) in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cdaf6096-f23e-4e18-8130-40127e4a97be
ms.reviewer:
title: Database availability groups
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Database availability groups

A database availability group (DAG) is the base component of the Mailbox server high availability and site resilience framework built into Microsoft Exchange Server. A DAG is a group of up to 16 Mailbox servers that hosts a set of databases and provides automatic database-level recovery from failures that affect individual servers or databases.

> [!IMPORTANT]
> All servers within a DAG must be running the same version of Exchange. For example, you can't mix Exchange 2013 servers and Exchange 2016 servers in the same DAG.

A DAG is a boundary for mailbox database replication, database and server switchovers and failovers, and an internal component called *Active Manager*. Active Manager, which runs on every Mailbox server, manages switchovers and failovers within DAGs. For more information about Active Manager, see [Active Manager](active-manager.md).

Any server in a DAG can host a copy of a mailbox database from any other server in the DAG. When a server is added to a DAG, it works with the other servers in the DAG to provide automatic recovery from failures that affect mailbox databases, such as a disk, server, or network failure.

> [!NOTE]
> For more information about creating DAGs, managing DAG membership, configuring DAG properties, creating and monitoring mailbox database copies, and performing switchovers, see [Managing high availability and site resilience](../../high-availability/manage-ha/manage-ha.md).

## Database availability group lifecycle
<a name="DAG_Lifecycle"> </a>

DAGs leverage the concept of *incremental deployment*, which is the ability to deploy service and data availability for all Mailbox servers and databases after Exchange is installed. After you deploy Exchange Server Mailbox servers, you can create a DAG, add Mailbox servers to the DAG, and then replicate mailbox databases between the DAG members.

> [!NOTE]
> It's supported to create a DAG that contains a combination of physical Mailbox servers and virtualized Mailbox servers, provided that the servers and solution comply with the [Exchange Server system requirements](../../plan-and-deploy/system-requirements.md) and the requirements set forth in [Exchange Server virtualization](../../plan-and-deploy/virtualization.md). As with all Exchange high availability configurations, you must ensure that all Mailbox servers in the DAG are sized appropriately to handle the necessary workload during scheduled and unscheduled outages.

A DAG is created by using the [New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/new-databaseavailabilitygroup) cmdlet. A DAG is initially created as an empty object in Active Directory. This directory object is used to store relevant information about the DAG, such as server membership information and some DAG configuration settings. When you add the first server to a DAG, a failover cluster is automatically created for the DAG. This failover cluster is used exclusively by the DAG, and the cluster must be dedicated to the DAG. Use of the cluster for any other purpose isn't supported.

In addition to a failover cluster being created, the infrastructure that monitors the servers for network or server failures is initiated. The failover cluster heartbeat mechanism and cluster database are then used to track and manage information about the DAG that can change quickly, such as database mount status, replication status, and last mounted location.

During creation, the DAG is given a unique name, and either assigned one or more static IP addresses or configured to use Dynamic Host Configuration Protocol (DHCP), or created without a cluster administrative access point. DAGs without an administrative access point can be created only on servers running Exchange 2019, Exchange 2016, or Exchange 2013 Service Pack 1 or later, with Windows Server 2012 R2 Standard or Datacenter edition. DAGs without cluster administrative access points have the following characteristics:

- There is no IP address assigned to the cluster/DAG, and therefore no IP Address Resource in the cluster core resource group.

- There is no network name assigned to the cluster, and therefore no Network Name Resource in the cluster core resource group

- The name of the cluster/DAG is not registered in DNS, and it is not resolvable on the network.

- A cluster name object (CNO) is not created in Active Directory.

- The cluster cannot be managed using the Failover Cluster Management tool. It must be managed using Windows PowerShell, and the PowerShell cmdlets must be run against individual cluster members.

This example shows you how to use the Exchange Management Shell to create a DAG with a cluster administrative access point that will have three servers. Two servers (EX1 and EX2) are on the same subnet (10.0.0.0), and the third server (EX3) is on a different subnet (192.168.0.0).

```powershell
New-DatabaseAvailabilityGroup -Name DAG1 -WitnessServer EX4 -DatabaseAvailabilityGroupIPAddresses 10.0.0.5,192.168.0.5
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX1
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX2
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX3
```

The commands to create a DAG without a cluster administrative access point are very similar:

```powershell
New-DatabaseAvailabilityGroup -Name DAG1 -WitnessServer EX4 -DatabaseAvailabilityGroupIPAddresses ([System.Net.IPAddress])::None
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX1
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX2
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer EX3
```

The cluster for DAG1 is created when EX1 is added to the DAG. During cluster creation, the **Add-DatabaseAvailabilityGroupServer** cmdlet retrieves the IP addresses configured for the DAG and ignores the ones that don't match any of the subnets found on EX1. In the first example above, the cluster for DAG1 is created with an IP address of 10.0.0.5, and 192.168.0.5 is ignored. In the second example above, the value of the _DatabaseAvailabilityGroupIPAddresses_ parameter instructs the task to create a failover cluster for the DAG that does not have an administrative access point. Thus, the cluster is created with an IP address or network name resource in the core cluster resource group.

Then, EX2 is added, and the **Add-DatabaseAvailabilityGroupServer** cmdlet again retrieves the IP addresses configured for the DAG. There are no changes to the cluster's IP addresses because in EX2 is on the same subnet as EX1.

Then, EX3 is added, and the **Add-DatabaseAvailabilityGroupServer** cmdlet again retrieves the IP addresses configured for the DAG. Because a subnet matching 192.168.0.5 is present on EX3, the 192.168.0.5 address is added as an IP address resource in the cluster group. In addition, an **OR** dependency for the Network Name resource for each IP address resource is automatically configured. The 192.168.0.5 address will be used by the cluster when the cluster core resource group moves to EX3.

For DAGs with cluster administrative access points, Windows failover clustering registers the IP addresses for the cluster in the Domain Name System (DNS) when the Network Name resource is brought online. In addition, when EX1 is added to the cluster, a cluster name object (CNO) is created in Active Directory. The network name, IP address(es), and CNO for the cluster are not used for DAG functions. Administrators and end users don't need to interface with or connect to the cluster/DAG name or IP address for any reason. Some third-party applications connect to the cluster administrative access point to perform management tasks, such as backup or monitoring. If you do not use any third-party applications that require a cluster administrative access point, and your DAG is running Exchange 2016 or Exchange 2019 on Windows Server 2012 R2, then we recommend creating a DAG without an administrative access point. This simplifies DAG configuration, eliminates the need for one or more IP addresses, and reduces the attack surface of a DAG.

DAGs are also configured to use a witness server and a witness directory. The witness server and witness directory are either automatically configured by the system, or they can be manually configured by the administrator. In the examples above, EX4 (a server that is not and will not be a member of the DAG) is being manually configured as the DAG's witness server.

By default, a DAG is designed to use the built-in continuous replication feature to replicate mailbox databases among servers in the DAG. If you're using third-party data replication that supports the Third Party Replication API in Exchange Server, you must create the DAG in third-party replication mode by using the [New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/new-databaseavailabilitygroup) cmdlet with the _ThirdPartyReplication_ parameter. After this mode is enabled, it can't be disabled.

After the DAG is created, Mailbox servers can be added to the DAG. When the first server is added to the DAG, a cluster is formed for use by the DAG. DAGs make use of Windows failover clustering technology, such as the cluster heartbeat, cluster networks, and the cluster database (for storing data that changes, such as database state changes from active to passive or vice versa, or from mounted to dismounted and vice versa). As each subsequent server is added to the DAG, it's joined to the underlying cluster, the cluster's quorum model is automatically adjusted by Exchange, and the server is added to the DAG object in Active Directory.

After Mailbox servers are added to a DAG, you can configure a variety of DAG properties, such as whether to use network encryption or network compression for database replication within the DAG. You can also configure DAG networks and create additional DAG networks.

After you add members to a DAG and configure the DAG, the active mailbox databases on each server can be replicated to the other DAG members. After you create mailbox database copies, you can monitor the health and status of the copies using a variety of built-in monitoring tools. In addition, you can perform database and server switchovers.

### Database availability group quorum models
<a name="rtt"> </a>

Underneath every DAG is a Windows failover cluster. Failover clusters use the concept of quorum, which uses a consensus of voters to ensure that only one subset of the cluster members (which could mean all members or a majority of members) is functioning at one time. Quorum isn't a new concept for Exchange Server. Highly available Mailbox servers in previous versions of Exchange also use failover clustering and its concept of quorum. Quorum represents a shared view of members and resources, and the term quorum is also used to describe the physical data that represents the configuration within the cluster that's shared between all cluster members. As a result, all DAGs require their underlying failover cluster to have quorum. If the cluster loses quorum, all DAG operations terminate and all mounted databases hosted in the DAG dismount. In this event, administrator intervention is required to correct the quorum problem and restore DAG operations.

Quorum is important to ensure consistency, to act as a tie-breaker to avoid partitioning, and to ensure cluster responsiveness:

- **Ensuring consistency**: A primary requirement for a Windows failover cluster is that each of the members always has a view of the cluster that's consistent with the other members. The cluster hive acts as the definitive repository for all configuration information relating to the cluster. If the cluster hive can't be loaded locally on a DAG member, the Cluster service doesn't start, because it isn't able to guarantee that the member meets the requirement of having a view of the cluster that's consistent with the other members.

- **Acting as a tie-breaker**: A quorum witness resource is used in DAGs with an even number of members to avoid split brain syndrome scenarios and to make sure that only one collection of the members in the DAG is considered official. When the witness server is needed for quorum, any member of the DAG that can communicate with the witness server can place a Server Message Block (SMB) lock on the witness server's witness.log file. The DAG member that locks the witness server (referred to as the *locking node*) retains an additional vote for quorum purposes. The DAG members in contact with the locking node are in the majority and maintain quorum. Any DAG members that can't contact the locking node are in the minority and therefore lose quorum.

- **Ensuring responsiveness**: To ensure responsiveness, the quorum model makes sure that, whenever the cluster is running, enough members of the distributed system are operational and communicative, and at least one replica of the cluster's current state can be guaranteed. No additional time is required to bring members into communication or to determine whether a specific replica is guaranteed.

DAGs with an even number of members use the failover cluster's Node and File Share Majority quorum mode, which employs an external witness server that acts as a tie-breaker. In this quorum mode, each DAG member gets a vote. In addition, the witness server is used to provide one DAG member with a weighted vote (for example, it gets two votes instead of one). The cluster quorum data is stored by default on the system disk of each member of the DAG, and is kept consistent across those disks. However, a copy of the quorum data isn't stored on the witness server. A file on the witness server is used to keep track of which member has the most updated copy of the data, but the witness server doesn't have a copy of the cluster quorum data. In this mode, a majority of the voters (the DAG members plus the witness server) must be operational and able to communicate with each other to maintain quorum. If a majority of the voters can't communicate with each other, the DAG's underlying cluster loses quorum, and the DAG will require administrator intervention to become operational again.

DAGs with an odd number of members use the failover cluster's Node Majority quorum mode. In this mode, each member gets a vote, and each member's local system disk is used to store the cluster quorum data. If the configuration of the DAG changes, that change is reflected across the different disks. The change is only considered to have been committed and made persistent if that change is made to the disks on half the members (rounding down) plus one. For example, in a five-member DAG, the change must be made on two plus one members, or three members total.

Quorum requires a majority of voters to be able to communicate with each other. Consider a DAG that has four members. Because this DAG has an even number of members, an external witness server is used to provide one of the cluster members with a fifth, tie-breaking vote. To maintain a majority of voters (and therefore quorum), at least three voters must be able to communicate with each other. At any time, a maximum of two voters can be offline without disrupting service and data access. If three or more voters are offline, the DAG loses quorum, and service and data access will be disrupted until you resolve the problem.
