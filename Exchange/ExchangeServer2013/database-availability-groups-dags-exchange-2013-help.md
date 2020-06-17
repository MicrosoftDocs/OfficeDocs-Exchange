---
title: 'Database availability groups (DAGs): Exchange 2013 Help'
TOCTitle: Database availability groups (DAGs)
ms:assetid: ab9b88ce-2f44-4334-96ad-a666b95888a0
ms:mtpsurl: https://technet.microsoft.com/library/Dd979799(v=EXCHG.150)
ms:contentKeyID: 48385432
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Database availability groups (DAGs)

_**Applies to:** Exchange Server 2013_

Learn about Exchange DAG in Exchange Server 2013. This article discusses the database availability group (DAG) lifecycle, as well as using a DAG for high availability and for site resilience.

A database availability group (DAG) is the base component of the Mailbox server high availability and site resilience framework built into Microsoft Exchange Server 2013. A DAG is a group of up to 16 Mailbox servers that hosts a set of databases and provides automatic database-level recovery from failures that affect individual servers or databases.

A DAG is a boundary for mailbox database replication, database and server switchovers and failovers, and an internal component called *Active Manager*. Active Manager, which runs on every Mailbox server, manages switchovers and failovers within DAGs. For more information about Active Manager, see [Active Manager](active-manager-exchange-2013-help.md).

Any server in a DAG can host a copy of a mailbox database from any other server in the DAG. When a server is added to a DAG, it works with the other servers in the DAG to provide automatic recovery from failures that affect mailbox databases, such as a disk, server, or network failure.

## Database availability group (DAG) lifecycle

DAGs leverage the concept of *incremental deployment*, which is the ability to deploy service and data availability for all Mailbox servers and databases after Exchange is installed. After you deploy Exchange 2013 Mailbox servers, you can create a DAG, add Mailbox servers to the DAG, and then replicate mailbox databases between the DAG members.

> [!NOTE]
> It's supported to create a DAG that contains a combination of physical Mailbox servers and virtualized Mailbox servers, provided that the servers and solution comply with the <A href="exchange-2013-system-requirements-exchange-2013-help.md">Exchange 2013 system requirements</A> and the requirements set forth in <A href="exchange-2013-virtualization-exchange-2013-help.md">Exchange 2013 virtualization</A>. As with all Exchange high availability configurations, you must ensure that all Mailbox servers in the DAG are sized appropriately to handle the necessary workload during scheduled and unscheduled outages.

A DAG is created by using the [New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroup) cmdlet. A DAG is initially created as an empty object in Active Directory. This directory object is used to store relevant information about the DAG, such as server membership information and some DAG configuration settings. When you add the first server to a DAG, a failover cluster is automatically created for the DAG. This failover cluster is used exclusively by the DAG, and the cluster must be dedicated to the DAG. Use of the cluster for any other purpose isn't supported.

In addition to a failover cluster being created, the infrastructure that monitors the servers for network or server failures is initiated. The failover cluster heartbeat mechanism and cluster database are then used to track and manage information about the DAG that can change quickly, such as database mount status, replication status, and last mounted location.

During creation, the DAG is given a unique name, and either assigned one or more static IP addresses or configured to use Dynamic Host Configuration Protocol (DHCP), or created without a cluster administrative access point. DAGs without an administrative access point can be created only on servers running Exchange 2013 Service Pack 1 or later on Windows Server 2012 R2 Standard or Datacenter edition. DAGs without cluster administrative access points have the following characteristics:

- There is no IP address assigned to the cluster/DAG, and therefore no IP Address Resource in the cluster core resource group.

- There is no network name assigned to the cluster, and therefore no Network Name Resource in the cluster core resource group

- The name of the cluster/DAG is not registered in DNS, and it is not resolvable on the network.

- A cluster name object (CNO) is not created in Active Directory.

- The cluster cannot be managed using the Failover Cluster Management tool. It must be managed using Windows PowerShell, and the PowerShell cmdlets must be run against individual cluster members.

This example shows how to use the Shell to create a DAG with a cluster administrative access point that will have three servers. Two servers (EX1 and EX2) are on the same subnet (10.0.0.0), and the third server (EX3) is on a different subnet (192.168.0.0).

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

The cluster for DAG1 is created when EX1 is added to the DAG. During cluster creation, the **Add-DatabaseAvailabilityGroupServer** cmdlet retrieves the IP addresses configured for the DAG and ignores the ones that don't match any of the subnets found on EX1. In the first example above, the cluster for DAG1 is created with an IP address of 10.0.0.5, and 192.168.0.5 is ignored. In the second example above, the value of the *DatabaseAvailabilityGroupIPAddresses* parameter instructs the task to create a failover cluster for the DAG that does not have an administrative access point. Thus, the cluster is created with an IP address or network name resource in the core cluster resource group.

Then, EX2 is added, and the **Add-DatabaseAvailabilityGroupServer** cmdlet again retrieves the IP addresses configured for the DAG. There are no changes to the cluster's IP addresses because in EX2 is on the same subnet as EX1.

Then, EX3 is added, and the **Add-DatabaseAvailabilityGroupServer** cmdlet again retrieves the IP addresses configured for the DAG. Because a subnet matching 192.168.0.5 is present on EX3, the 192.168.0.5 address is added as an IP address resource in the cluster group. In addition, an **OR** dependency for the Network Name resource for each IP address resource is automatically configured. The 192.168.0.5 address will be used by the cluster when the cluster core resource group moves to EX3.

For DAGs with cluster administrative access points, Windows failover clustering registers the IP addresses for the cluster in the Domain Name System (DNS) when the Network Name resource is brought online. In addition, when EX1 is added to the cluster, a cluster name object (CNO) is created in Active Directory. The network name, IP address(es), and CNO for the cluster are not used for DAG functions. Administrators and end users don't need to interface with or connect to the cluster/DAG name or IP address for any reason. Some third-party applications connect to the cluster administrative access point to perform management tasks, such as backup or monitoring. If you do not use any third-party applications that require a cluster administrative access point, and your DAG is running Exchange 2013 SP1 or later on Windows Server 2012 R2, then we recommend creating a DAG without an administrative access point. This simplifies DAG configuration, eliminates the need for one or more IP addresses, and reduces the attack surface of a DAG.

DAGs are also configured to use a witness server and a witness directory. The witness server and witness directory are either automatically configured by the system, or they can be manually configured by the administrator. In the examples above, EX4 (a server that is not and will not be a member of the DAG) is being manually configured as the DAG's witness server.

By default, a DAG is designed to use the built-in continuous replication feature to replicate mailbox databases among servers in the DAG. If you're using third-party data replication that supports the Third Party Replication API in Exchange 2013, you must create the DAG in third-party replication mode by using the [New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroup) cmdlet with the *ThirdPartyReplication* parameter. After this mode is enabled, it can't be disabled.

After the DAG is created, Mailbox servers can be added to the DAG. When the first server is added to the DAG, a cluster is formed for use by the DAG. DAGs make use of Windows failover clustering technology, such as the cluster heartbeat, cluster networks, and the cluster database (for storing data that changes, such as database state changes from active to passive or vice versa, or from mounted to dismounted and vice versa). As each subsequent server is added to the DAG, it's joined to the underlying cluster, the cluster's quorum model is automatically adjusted by Exchange, and the server is added to the DAG object in Active Directory.

After Mailbox servers are added to a DAG, you can configure a variety of DAG properties, such as whether to use network encryption or network compression for database replication within the DAG. You can also configure DAG networks and create additional DAG networks.

After you add members to a DAG and configure the DAG, the active mailbox databases on each server can be replicated to the other DAG members. After you create mailbox database copies, you can monitor the health and status of the copies using a variety of built-in monitoring tools. In addition, you can perform database and server switchovers.

For more information about creating DAGs, managing DAG membership, configuring DAG properties, creating and monitoring mailbox database copies, and performing switchovers, see [Managing high availability and site resilience](managing-high-availability-and-site-resilience-exchange-2013-help.md).

## Database availability group (DAG) quorum models

Underneath every DAG is a Windows failover cluster. Failover clusters use the concept of quorum, which uses a consensus of voters to ensure that only one subset of the cluster members (which could mean all members or a majority of members) is functioning at one time. Quorum isn't a new concept for Exchange 2013. Highly available Mailbox servers in previous versions of Exchange also use failover clustering and its concept of quorum. Quorum represents a shared view of members and resources, and the term quorum is also used to describe the physical data that represents the configuration within the cluster that's shared between all cluster members. As a result, all DAGs require their underlying failover cluster to have quorum. If the cluster loses quorum, all DAG operations terminate and all mounted databases hosted in the DAG dismount. In this event, administrator intervention is required to correct the quorum problem and restore DAG operations.

Quorum is important to ensure consistency, to act as a tie-breaker to avoid partitioning, and to ensure cluster responsiveness:

- **Ensuring consistency**: A primary requirement for a Windows failover cluster is that each of the members always has a view of the cluster that's consistent with the other members. The cluster hive acts as the definitive repository for all configuration information relating to the cluster. If the cluster hive can't be loaded locally on a DAG member, the Cluster service doesn't start, because it isn't able to guarantee that the member meets the requirement of having a view of the cluster that's consistent with the other members.

- **Acting as a tie-breaker**: A quorum witness resource is used in DAGs with an even number of members to avoid split brain syndrome scenarios and to make sure that only one collection of the members in the DAG is considered official. When the witness server is needed for quorum, any member of the DAG that can communicate with the witness server can place a Server Message Block (SMB) lock on the witness server's witness.log file. The DAG member that locks the witness server (referred to as the *locking node*) retains an additional vote for quorum purposes. The DAG members in contact with the locking node are in the majority and maintain quorum. Any DAG members that can't contact the locking node are in the minority and therefore lose quorum.

- **Ensuring responsiveness**: To ensure responsiveness, the quorum model makes sure that, whenever the cluster is running, enough members of the distributed system are operational and communicative, and at least one replica of the cluster's current state can be guaranteed. No additional time is required to bring members into communication or to determine whether a specific replica is guaranteed.

DAGs with an even number of members use the failover cluster's Node and File Share Majority quorum mode, which employs an external witness server that acts as a tie-breaker. In this quorum mode, each DAG member gets a vote. In addition, the witness server is used to provide one DAG member with a weighted vote (for example, it gets two votes instead of one). The cluster quorum data is stored by default on the system disk of each member of the DAG, and is kept consistent across those disks. However, a copy of the quorum data isn't stored on the witness server. A file on the witness server is used to keep track of which member has the most updated copy of the data, but the witness server doesn't have a copy of the cluster quorum data. In this mode, a majority of the voters (the DAG members plus the witness server) must be operational and able to communicate with each other to maintain quorum. If a majority of the voters can't communicate with each other, the DAG's underlying cluster loses quorum, and the DAG will require administrator intervention to become operational again.

DAGs with an odd number of members use the failover cluster's Node Majority quorum mode. In this mode, each member gets a vote, and each member's local system disk is used to store the cluster quorum data. If the configuration of the DAG changes, that change is reflected across the different disks. The change is only considered to have been committed and made persistent if that change is made to the disks on half the members (rounding down) plus one. For example, in a five-member DAG, the change must be made on two plus one members, or three members total.

Quorum requires a majority of voters to be able to communicate with each other. Consider a DAG that has four members. Because this DAG has an even number of members, an external witness server is used to provide one of the cluster members with a fifth, tie-breaking vote. To maintain a majority of voters (and therefore quorum), at least three voters must be able to communicate with each other. At any time, a maximum of two voters can be offline without disrupting service and data access. If three or more voters are offline, the DAG loses quorum, and service and data access will be disrupted until you resolve the problem.

## Using a database availability group (DAG) for high availability

To illustrate how a DAG can provide high availability for your mailbox databases, consider the following example, which uses a DAG with five members. This DAG is illustrated in the following figure.

**DAG with five members**

![Database Availability Group (DAG)](images/Dd979799.21fcbf7b-cb10-49c0-8e32-bdf3c03f825d(EXCHG.150).gif "Database Availability Group (DAG)")

In the preceding figure, the green databases are active mailbox database copies and the blue databases are passive mailbox database copies. In this example, the database copies aren't mirrored across each server, but rather spread across multiple servers. This ensures that no two servers in the DAG have the same set of database copies, providing the DAG with greater resilience to failures, including failures that occur while other components are unavailable as a result of regular maintenance.

Consider the following scenario, using the preceding example DAG, which illustrates resilience to multiple database and server failures.

Initially, all databases and servers are healthy. You need to install some operating system updates on EX2, so you put the server into maintenance mode. This causes a server switchover, which activates the copy of DB4 on another Mailbox server. A server switchover moves all active mailbox database copies from their current server to one or more other Mailbox servers in the DAG in preparation for a scheduled outage for the current server. In this example, there's only one active mailbox database on EX2 (DB4), so only one active mailbox database copy is moved.

**DAG with a server offline for maintenance**

![Database Availability Group (DAG) with a Server Offline](images/Dd979799.f48f0e77-80e1-4f14-8c36-112393895bdc(EXCHG.150).gif "Database Availability Group  (DAG) with a Server Offline")

While you perform maintenance on EX2, EX3 experiences a catastrophic hardware failure and goes offline. Prior to going offline, EX3 hosted the active copy of DB2. To recover from the failure, the system automatically activates the copy of DB2 that's hosted on EX1 within 30 seconds. This is illustrated in the following figure.

**DAG with a server offline for maintenance and a failed server**

![DAG with a server offline and a failed server](images/Dd979799.9bbfd9e7-3881-4957-ae8d-32318cbc208b(EXCHG.150).gif "DAG with a server offline and a failed server")

After the scheduled maintenance is completed for EX2, you bring the server online and take it out of maintenance mode. As soon as EX2 is available, the other members of the DAG are notified, and the copies of DB1, DB4, and DB5 hosted on EX2 are automatically synchronized with the active copy of each database. This is illustrated in the following figure.

**DAG with a restored server synchronizing its database copies**

![DAG with restored server resynchronizing databases](images/Dd979799.58601531-e078-41d3-9287-e8e470ef7f41(EXCHG.150).gif "DAG with restored server resynchronizing databases")

After the failed hardware component in EX3 is replaced with a new component, EX3 is brought online. After EX3 is available, the other members of the DAG are notified, and the copies of DB2, DB3, and DB4 hosted on EX3 are automatically synchronized with the active copy of each database. This is illustrated in the following figure.

**DAG with a repaired server synchronizing its database copies**

![DAG with Member Resynchronizing Database Copies](images/Dd979799.56259671-e840-4cf0-9ea2-3657dc36c035(EXCHG.150).gif "DAG with Member Resynchronizing Database Copies")

## Using a database availability group (DAG) for site resilience

In addition to providing high availability within a datacenter, a DAG can also be extended to one or more datacenters in a configuration that provides site resilience for one or multiple datacenters. In the preceding example figures, the DAG is located in a single datacenter and single Active Directory site. Incremental deployment can be used to extend this DAG to a second datacenter (and a second Active Directory site) by deploying a Mailbox server and the necessary supporting resources (one or more Active Directory servers, and DNS services). The Mailbox server is then added to the DAG, as illustrated in the following figure.

**DAG extended across two Active Directory sites**

![DAG extended across two Active Directory sites](images/Dd979799.28e96e9d-d7d6-451a-b7b8-c06122c81dc9(EXCHG.150).gif "DAG extended across two Active Directory sites")

In this example, a passive copy of each active database in the Redmond datacenter is configured on EX6 in the Dublin datacenter. However, there are many other examples of DAG configurations that provide site resilience. For example:

- Instead of hosting only passive database copies, EX6 could host all active copies, or it could host a mixture of active and passive copies.

- In addition to EX6, multiple DAG members could be deployed in the Dublin datacenter, providing protection against additional failures. This configuration also provides additional capacity, so that if the Redmond datacenter fails, the Dublin datacenter can support a much larger user population.

## Using multiple database availability groups (DAGs) for site resilience

In the preceding example, a single DAG extends across multiple datacenters, providing site resilience for either or both datacenters. When using a single DAG to provide site resilience in an environment where each datacenter to which you extend the DAG has an active user population, there is a single point of failure in the wide area network (WAN) connection. This is because quorum requires a majority of the voters to be active and able to communicate with each other.

In the preceding example, the majority of voters are located in the Redmond datacenter. If the Dublin datacenter hosts active mailbox databases, and it has a local user population, a WAN outage would result in a messaging service outage for the Dublin users. When WAN connectivity breaks, only the DAG members in the Redmond datacenter retain quorum and continue providing messaging service.

To eliminate the WAN as a single point of failure when you need to provide site resilience for multiple datacenters that each have an active user population, you should deploy multiple DAGs, where each DAG has a majority of voters in a separate datacenter. When a WAN outage occurs, replication will be blocked until connectivity is restored. Users will have messaging service, because each DAG continues to service its local user population.
