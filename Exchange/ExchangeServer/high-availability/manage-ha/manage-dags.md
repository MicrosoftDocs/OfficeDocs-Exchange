---
localization_priority: Normal
description: 'Summary: Learn how to create, configure, and manage database availability group (DAG) in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 74be3f97-ec0f-4d2a-b5d8-7770cc489919
ms.reviewer:
title: Manage database availability groups
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage database availability groups

A database availability group (DAG) is a set of up to 16 Exchange Mailbox servers that provides automatic, database-level recovery from a database, server, or network failure. DAGs use continuous replication and a subset of Windows failover clustering technologies to provide high availability and site resilience. Mailbox servers in a DAG monitor each other for failures. When a Mailbox server is added to a DAG, that server works with the other servers in the DAG to provide automatic, database-level recovery from database failures.

When you create a DAG, it's initially empty. When you add the first server to a DAG, a failover cluster is automatically created for the DAG. In addition, the infrastructure that monitors the servers for network or server failures is initiated. The failover cluster heartbeat mechanism and cluster database are then used to track and manage information about the DAG that can change quickly, such as database mount status, replication status, and last mounted location.

## Creating DAGs

A DAG can be created using the New Database Availability Group wizard in the Exchange admin center (EAC), or by running the **New-DatabaseAvailabilityGroup** cmdlet in the Exchange Management Shell. When creating a DAG, you provide a name for the DAG, and optional witness server and witness directory settings. In addition, you can assign one or more IP addresses to the DAG, either by using static IP addresses or by allowing the DAG to be automatically assigned the necessary IP addresses using Dynamic Host Configuration Protocol (DHCP). You can manually assign IP addresses to the DAG by using the _DatabaseAvailabilityGroupIpAddresses_ parameter. If you omit this parameter, the DAG attempts to obtain an IP address by using a DHCP server on your network.

If you're creating a DAG that will contain Mailbox servers that are running Windows Server 2012 R2, you also have the option of creating a DAG without a cluster administrative access point. In that case, the cluster will not have a cluster name object (CNO) in Active Directory, and the cluster core resource group will not contain a network name resource or an IP address resource.

For detailed steps about how to create a DAG, see [Create a database availability group](create-dags.md).

When you create a DAG, an empty object representing the DAG with the name you specified and an object class of **msExchMDBAvailabilityGroup** is created in Active Directory.

DAGs use a subset of Windows failover clustering technologies in Windows Server 2008 R2 or later, such as the cluster heartbeat, cluster networks, and cluster database (for storing data that changes or can change quickly, such as database state changes from active to passive or the reverse, or from mounted to dismounted or the reverse). Therefore you can create DAGs only on Exchange Mailbox servers installed on supported versions of Windows that include Windows failover clustering.

> [!NOTE]
> The failover cluster created and used by the DAG must be dedicated to the DAG. The cluster can't be used for any other high availability solution or for any other purpose. For example, the failover cluster can't be used to cluster other applications or services. Using a DAG's underlying failover cluster for purposes other than the DAG isn't supported.

### DAG witness server and witness directory

When creating a DAG, you need to specify a name for the DAG no longer than 15 characters that's unique within the Active Directory forest. In addition, each DAG is configured with a witness server and witness directory. The witness server and its directory are used only when there's an even number of members in the DAG and then only for quorum purposes. You don't need to create the witness directory in advance. Exchange automatically creates and secures the directory for you on the witness server. The witness directory shouldn't be used for any purpose other than for the DAG witness server.

> [!NOTE]
> In the database mirroring topology, you can have a third server called the witness. The witness server enables automatic failover from principal to mirror server or vice-versa. Unlike principal and mirror servers, the witness server does not serve the database. The role of the witness is to verify whether a given partner server is up and functioning. Supporting automatic failover is the only function for witness server, and it identifies which server holds the principal copy and which server holds the mirror copy of the database.

The requirements for the witness server are as follows:

- The witness server can't be a member of the DAG.

- The witness server must be in the same Active Directory forest as the DAG.

- The witness server must be running Windows Server 2008 or later.

- A single server can serve as a witness for multiple DAGs. However, each DAG requires its own witness directory.

Regardless of what server is used as the witness server, if the Windows Firewall is enabled on the intended witness server, you must enable the Windows Firewall exception for File and Printer Sharing. The witness server uses SMB port 445.

> [!IMPORTANT]
> If the witness server you specify isn't an Exchange 2010 or later server, you must add the Exchange Trusted Subsystem universal security group (USG) to the local Administrators group on the witness server prior to creating the DAG. These security permissions are necessary to ensure that Exchange can create a directory and share on the witness server as needed.

Neither the witness server nor the witness directory needs to be fault tolerant or use any form of redundancy or high availability. There's no need to use a clustered file server for the witness server or employ any other form of resiliency for the witness server. There are several reasons for this. With larger DAGs (for example, six members or more), several failures are required before the witness server is needed. Because a six-member DAG can tolerate as many as two voter failures without losing quorum, it would take as many as three voters failing before the witness server would be needed to maintain a quorum. Also, if there's a failure that affects your current witness server (for example, you lose the witness server because of a hardware failure), you can use the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/set-databaseavailabilitygroup) cmdlet to configure a new witness server and witness directory (provided you have a quorum).

> [!NOTE]
> You can also use the **Set-DatabaseAvailabilityGroup** cmdlet to configure the witness server and witness directory in the original location if the witness server lost its storage or if someone changed the witness directory or share permissions.

#### Witness server placement considerations

The placement of a DAG's witness server will depend on your business requirements and the options available to your organization. Exchange now includes support for new DAG configuration options that aren't recommended or aren't possible in Exchange 2010. These options include using a third location, such as a third datacenter, a branch office, or a Microsoft Azure virtual network.

The following table lists general witness server placement recommendations for different deployment scenarios.

|**Deployment Scenario**|**Recommendations**|
|:-----|:-----|
|Single DAG deployed in a single datacenter|Locate witness server in the same datacenter as DAG members|
|Single DAG deployed across two datacenters; no additional locations available|Locate witness server on a Microsoft Azure virtual network to enable automatic datacenter failover, or <br/> Locate witness server in primary datacenter|
|Multiple DAGs deployed in a single datacenter|Locate witness server in the same datacenter as DAG members. Additional options include: <br/> • Using the same witness server for multiple DAGs <br/> • Using a DAG member to act as a witness server for a different DAG|
|Multiple DAGs deployed across two datacenters|Locate witness server on a Microsoft Azure virtual network to enable automatic datacenter failover, or <br/> Locate witness server in the datacenter that is considered primary for each DAG. Additional options include: <br/> • Using the same witness server for multiple DAGs <br/> • Using a DAG member to act as a witness server for a different DAG|
|Single or Multiple DAGs deployed across more than two datacenters|In this configuration, the witness server should be located in the datacenter where you want the majority of quorum votes to exist.|

When a DAG has been deployed across two datacenters, you can now use a third location for hosting the witness server. If your organization has a third location with a network infrastructure that is isolated from network failures that affect the two datacenters in which your DAG is deployed, then you can deploy the DAG's witness server in that third location, thereby configuring your DAG with the ability automatically failover databases to the other datacenter in response to a datacenter-level failure event. If your organization only has two physical locations, you can use a Microsoft Azure virtual network as a third location to place your witness server.

#### Specifying a witness server and witness directory during DAG creation

When creating a DAG, you must provide a name for the DAG. You can optionally also specify a witness server and witness directory.

When creating a DAG, the following combinations of options and behaviors are available:

- You can specify only a name for the DAG, and leave the **Witness server** and **Witness directory** fields blank. In this scenario, the wizard searches the local Active Directory site for a Client Access server that doesn't have the Mailbox server installed, and it automatically creates the default directory (%SystemDrive%:\DAGFileShareWitnesses\\< _DAGFQDN_\>) and default share (\< _DAGFQDN_\>) on that server and uses that Client Access server as the witness server. For example, consider the witness server CAS3 on which the operating system has been installed onto drive C. A DAG named DAG1 in the contoso.com domain would use a default witness directory of C:\DAGFileShareWitnesses\DAG1.contoso.com, which would be shared as \\CAS3\DAG1.contoso.com.

- You can specify a name for the DAG, the witness server that you want to use, and the directory you want created and shared on the witness server.

- You can specify a name for the DAG and the witness server that you want to use, and leave the **Witness directory** field blank. In this scenario, the wizard creates the default directory on the specified witness server.

- You can specify a name for the DAG, leave the **Witness server** field blank, and specify the directory you want created and shared on the witness server. In this scenario, the wizard searches for a Client Access server that doesn't have the Mailbox server installed, and it automatically creates the specified DAG on that server, shares the directory, and uses that Client Access server as the witness server.

When a DAG is formed, it initially uses the Node Majority quorum model. When the second Mailbox server is added to the DAG, the quorum is automatically changed to a Node and File Share Majority quorum model. When this change occurs, the DAG's cluster begins using the witness server for maintaining quorum. If the witness directory doesn't exist, Exchange automatically creates it, shares it, and provisions the share with full control permissions for the CNO computer account for the DAG.

> [!NOTE]
> Using a file share that's part of a Distributed File System (DFS) namespace isn't supported.

If Windows Firewall is enabled on the witness server before the DAG is created, it may block the creation of the DAG. Exchange uses Windows Management Instrumentation (WMI) to create the directory and file share on the witness server. If Windows Firewall is enabled on the witness server and there are no firewall exceptions configured for WMI, the **New-DatabaseAvailabilityGroup** cmdlet fails with an error. If you specify a witness server, but not a witness directory, you receive the following error message.

 `The task was unable to create the default witness directory on server <Server Name>. Please manually specify a witness directory.`

If you specify a witness server and witness directory, you receive the following warning message.

 `Unable to access file shares on witness server '<ServerName>'. Until this problem is corrected, the database availability group may be more vulnerable to failures. You can use the Set-DatabaseAvailabilityGroup cmdlet to try the operation again. Error: The network path was not found.`

If Windows Firewall is enabled on the witness server after the DAG is created but before servers are added, it may block the addition or removal of DAG members. If Windows Firewall is enabled on the witness server and there are no firewall exceptions configured for WMI, the **Add-DatabaseAvailabilityGroupServer** cmdlet displays the following warning message.

 `Failed to create file share witness directory 'C:\DAGFileShareWitnesses\DAG_FQDN' on witness server '<ServerName>'. Until this problem is corrected, the database availability group may be more vulnerable to failures. You can use the Set-DatabaseAvailabilityGroup cmdlet to try the operation again. Error: WMI exception occurred on server '<ServerName>': The RPC server is unavailable. (Exception from HRESULT: 0x800706BA)`

To resolve the preceding error and warnings, do one of the following:

- Manually create the witness directory and share on the witness server, and assign the CNO for the DAG full control for the directory and share.

- Enable the WMI exception in Windows Firewall.

- Disable Windows Firewall.

## DAG membership

After a DAG has been created, you can add servers to or remove servers from the DAG using the Manage Database Availability Group wizard in the Exchange admin center (EAC), or using the **Add-DatabaseAvailabilityGroupServer** or **Remove-DatabaseAvailabilityGroupServer** cmdlets in the Exchange Management Shell. For detailed steps about how to manage DAG membership, see [Manage database availability group membership](dag-memberships.md).

> [!NOTE]
> Each Mailbox server that's a member of a DAG is also a node in the underlying cluster used by the DAG. As a result, at any one time, a Mailbox server can be a member of only one DAG.

If the Mailbox server being added to a DAG doesn't have the failover clustering component installed, the method used to add the server (for example, the **Add-DatabaseAvailabilityGroupServer** cmdlet or the Manage Database Availability Group wizard) installs the failover clustering feature.

When the first Mailbox server is added to a DAG, the following occurs:

- The Windows failover clustering component is installed, if it isn't already installed.

- A failover cluster is created using the name of the DAG. This failover cluster is used exclusively by the DAG, and the cluster must be dedicated to the DAG. Use of the cluster for any other purpose isn't supported.

- A CNO is created in the default computers container.

- The name and IP address of the DAG is registered as a Host (A) record in Domain Name System (DNS).

- The server is added to the DAG object in Active Directory.

- The cluster database is updated with information on the databases mounted on the added server.

In a large or multiple site environment, especially those in which the DAG is extended to multiple Active Directory sites, you must wait for Active Directory replication of the DAG object containing the first DAG member to complete. If this Active Directory object isn't replicated throughout your environment, adding the second server may cause a new cluster (and new CNO) to be created for the DAG. This is because the DAG object appears empty from the perspective of the second member being added, thereby causing the **Add-DatabaseAvailabilityGroupServer** cmdlet to create a cluster and CNO for the DAG, even though these objects already exist. To verify that the DAG object containing the first DAG server has been replicated, use the **Get-DatabaseAvailabilityGroup** cmdlet on the second server being added to verify that the first server you added is listed as a member of the DAG.

When the second and subsequent servers are added to the DAG, the following occurs:

- The server is joined to the Windows failover cluster for the DAG.

- The quorum model is automatically adjusted:

  - A Node Majority quorum model is used for DAGs with an odd number of members.

  - A Node and File Share Majority quorum model is used for DAGs with an even number of members.

- The witness directory and share are automatically created by Exchange when needed.

- The server is added to the DAG object in Active Directory.

- The cluster database is updated with information about mounted databases.

> [!NOTE]
> The quorum model change should happen automatically. However, if the quorum model doesn't automatically change to the proper model, you can run the **Set-DatabaseAvailabilityGroup** cmdlet with only the _Identity_ parameter to correct the quorum settings for the DAG.

### Pre-staging the cluster name object for a DAG

The CNO is a computer account created in Active Directory and associated with the cluster's Name resource. The cluster's Name resource is tied to the CNO, which is a Kerberos-enabled object that acts as the cluster's identity and provides the cluster's security context. The formation of the DAG's underlying cluster and the CNO for that cluster is performed when the first member is added to the DAG. When the first server is added to the DAG, remote PowerShell contacts the Microsoft Exchange Replication service on the Mailbox server being added. The Microsoft Exchange Replication service installs the failover clustering feature (if it isn't already installed) and begins the cluster creation process. The Microsoft Exchange Replication service runs under the LOCAL SYSTEM security context, and it's under this context in which cluster creation is performed.

> [!CAUTION]
> If your DAG members are running Windows Server 2012, you must pre-stage the CNO prior to adding the first server to the DAG. If your DAG members are running Windows Server 2012 R2, and you create a DAG without a cluster administrative access point, then a CNO will not be created, and you do not need to create a CNO for the DAG.

In environments where computer account creation is restricted, or where computer accounts are created in a container other than the default computers container, you can pre-stage and provision the CNO. You create and disable a computer account for the CNO, and then either:

- Assign full control of the computer account to the computer account of the first Mailbox server you're adding to the DAG.

- Assign full control of the computer account to the Exchange Trusted Subsystem USG.

Assigning full control of the computer account to the computer account of the first Mailbox server you're adding to the DAG ensures that the LOCAL SYSTEM security context will be able to manage the pre-staged computer account. Assigning full control of the computer account to the Exchange Trusted Subsystem USG can be used instead because the Exchange Trusted Subsystem USG contains the machine accounts of all Exchange servers in the domain.

For detailed steps about how to pre-stage and provision the CNO for a DAG, see [Pre-stage the cluster name object for a database availability group](pre-stage-dag-cnos.md).

### Removing servers from a DAG

Mailbox servers can be removed from a DAG by using the Manage Database Availability Group wizard in the EAC or the **Remove-DatabaseAvailabilityGroupServer** cmdlet in the Exchange Management Shell. Before a Mailbox server can be removed from a DAG, all replicated mailbox databases must first be removed from the server. If you attempt to remove a Mailbox server with replicated mailbox databases from a DAG, the task fails.

There are scenarios in which you must remove a Mailbox server from a DAG before performing certain operations. These scenarios include:

- **Performing a server recovery operation**: If a Mailbox server that's a member of a DAG is lost, or otherwise fails and is unrecoverable and needs replacement, you can perform a server recovery operation using the **Setup /m:RecoverServer** switch. However, before you can perform the recovery operation, you must first remove the server from the DAG using the [Remove-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/remove-databaseavailabilitygroupserver) cmdlet with the _ConfigurationOnly_ parameter.

- **Removing the database availability group**: There may be situations in which you need to remove a DAG (for example, when disabling third-party replication mode). If you need to remove a DAG, you must first remove all servers from the DAG. If you attempt to remove a DAG that contains any members, the task fails.

## Configuring DAG properties

After servers have been added to the DAG, you can use the EAC or the Exchange Management Shell to configure the properties of a DAG, including the witness server and witness directory used by the DAG, and the IP addresses assigned to the DAG.

Configurable properties include:

- **Witness server**: The name of the server that you want to host the file share for the file share witness. We recommend that you specify a Client Access server as the witness server. This enables the system to automatically configure, secure, and use the share, as needed, and enables the messaging administrator to be aware of the availability of the witness server.

- **Witness directory**: The name of a directory that will be used to store file share witness data. This directory will automatically be created by the system on the specified witness server.

- **Database availability group IP addresses**: One or more IP addresses must be assigned to the DAG, unless the DAG members are running Windows Server 2012 R2 and you're creating a DAG without an IP address. Otherwise, the DAG's IP addresses can be configured using manually assigned static IP addresses, or they can be automatically assigned to the DAG using a DHCP server in your organization.

The Exchange Management Shell enables you to configure DAG properties that aren't available in the EAC, such as DAG IP addresses, network encryption and compression settings, network discovery, the TCP port used for replication, and alternate witness server and witness directory settings, and to enable Datacenter Activation Coordination mode.

For detailed steps about how to configure DAG properties, see [Configure database availability group properties](configure-dag-properties.md).

### DAG network encryption

DAGs support the use of encryption by leveraging the encryption capabilities of the Windows Server operating system. DAGs use Kerberos authentication between Exchange servers. Microsoft Kerberos security support provider (SSP) EncryptMessage and DecryptMessage APIs handle encryption of DAG network traffic. Microsoft Kerberos SSP supports multiple encryption algorithms. (For the complete list, see section 3.1.5.2, "Encryption Types" of [Kerberos Protocol Extensions](https://docs.microsoft.com/openspecs/windows_protocols/ms-kile/2a32282e-dd48-4ad9-a542-609804b02cc9)). The Kerberos authentication handshake selects the strongest encryption protocol supported in the list: typically Advanced Encryption Standard (AES) 256-bit, potentially with a SHA Hash-based Message Authentication Code (HMAC) to maintain integrity of the data. For details, see [HMAC](https://en.wikipedia.org/wiki/HMAC).

Network encryption is a property of the DAG and not a DAG network. You can configure DAG network encryption using the **Set-DatabaseAvailabilityGroup** cmdlet in the Exchange Management Shell. The possible encryption settings for DAG network communications are shown in the following table.

|**Setting**|**Description**|
|:-----|:-----|
|Disabled|Network encryption isn't used.|
|Enabled|Network encryption is used on all DAG networks for replication and seeding.|
|InterSubnetOnly|Network encryption is used on DAG networks when replicating across different subnets. This is the default setting.|
|SeedOnly|Network encryption is used on all DAG networks for seeding only.|

### DAG network compression

DAGs support built-in compression. When compression is enabled, DAG network communication uses XPRESS, which is the Microsoft implementation of the LZ77 algorithm. This is the same type of compression used in many Microsoft protocols, in particular, MAPI RPC compression between Microsoft Outlook and Exchange.

As with network encryption, network compression is also a property of the DAG and not a DAG network. You configure DAG network compression by using the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/set-databaseavailabilitygroup) cmdlet in the Exchange Management Shell. The possible compression settings for DAG network communications are shown in the following table.

|**Setting**|**Description**|
|:-----|:-----|
|Disabled|Network compression isn't used.|
|Enabled|Network compression is used on all DAG networks for replication and seeding.|
|InterSubnetOnly|Network compression is used on DAG networks when replicating across different subnets. This is the default setting.|
|SeedOnly|Network compression is used on all DAG networks for seeding only.|

## DAG networks

A DAG network is a collection of one or more subnets used for either replication traffic or MAPI traffic. Each DAG contains a maximum of one MAPI network and zero or more replication networks. In a single network adapter configuration, the network is used for both MAPI and replication traffic. Although a single network adapter and path is supported, we recommend that each DAG have a minimum of two DAG networks. In a two-network configuration, one network is typically dedicated for replication traffic, and the other network is used primarily for MAPI traffic. You can also add network adapters to each DAG member and configure additional DAG networks as replication networks.

> [!NOTE]
> When using multiple replication networks, there's no way to specify an order of precedence for network use. Exchange randomly selects a replication network from the group of replication networks to use for log shipping.

In Exchange 2010, manual configuration of DAG networks was necessary in many scenarios. By default, in later versions of Exchange, DAG networks are automatically configured by the system. Before you can create or modify DAG networks, you must first enable manual DAG network control by running the following command:

```powershell
Set-DatabaseAvailabilityGroup <DAGName> -ManualDagNetworkConfiguration $true
```

After you've enabled manual DAG network configuration, you can use the **New-DatabaseAvailabilityGroupNetwork** cmdlet in the Exchange Management Shell to create a DAG network. For detailed steps about how to create a DAG network, see [Create a database availability group network](create-dag-networks.md).

You can use the **Set-DatabaseAvailabilityGroupNetwork** cmdlet in the Exchange Management Shell to configure DAG network properties. For detailed steps about how to configure DAG network properties, see [Configure database availability group network properties](configure-dag-network-properties.md). Each DAG network has required and optional parameters to configure:

- **Network name**: A unique name for the DAG network of up to 128 characters.

- **Network description**: An optional description for the DAG network of up to 256 characters.

- **Network subnets**: One or more subnets entered using a format of _IPAddress/Bitmask_ (for example, 192.168.1.0/24 for Internet Protocol version 4 (IPv4) subnets; 2001:DB8:0:C000::/64 for Internet Protocol version 6 (IPv6) subnets).

- **Enable replication**: In the EAC, select the check box to dedicate the DAG network to replication traffic and block MAPI traffic. Clear the check box to prevent replication from using the DAG network and to enable MAPI traffic. In the Exchange Management Shell, use the _ReplicationEnabled_ parameter in the [Set-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/set-databaseavailabilitygroupnetwork) cmdlet to enable and disable replication.

> [!NOTE]
> Disabling replication for the MAPI network doesn't guarantee that the system won't use the MAPI network for replication. When all configured replication networks are offline, failed, or otherwise unavailable, and only the MAPI network remains (which is configured as disabled for replication), the system uses the MAPI network for replication.

The initial DAG networks (for example, MapiDagNetwork and ReplicationDagNetwork01) created by the system are based on the subnets enumerated by the Cluster service. Each DAG member must have the same number of network adapters, and each network adapter must have an IPv4 address (and optionally, an IPv6 address as well) on a unique subnet. Multiple DAG members can have IPv4 addresses on the same subnet, but each network adapter and IP address pair in a specific DAG member must be on a unique subnet. In addition, only the adapter used for the MAPI network should be configured with a default gateway. Replication networks shouldn't be configured with a default gateway.

For example, consider DAG1, a two-member DAG where each member has two network adapters (one dedicated for the MAPI network and the other for a replication network). Example IP address configuration settings are shown in the following table.

**Example network adapter settings**

|**Server-network adapter**|**IP address/subnet mask**|**Default gateway**|
|:-----|:-----|:-----|
|EX1-MAPI|192.168.1.15/24|192.168.1.1|
|EX1-Replication|10.0.0.15/24|Not applicable|
|EX2-MAPI|192.168.1.16|192.168.1.1|
|EX2-Replication|10.0.0.16|Not applicable|

In the following configuration, there are two subnets configured in the DAG: 192.168.1.0 and 10.0.0.0. When EX1 and EX2 are added to the DAG, two subnets will be enumerated and two DAG networks will be created: MapiDagNetwork (192.168.1.0) and ReplicationDagNetwork01 (10.0.0.0). These networks will be configured as shown in the following table.

**Enumerated DAG network settings for a single-subnet DAG**

|**Name**|**Subnets**|**Interfaces**|**MAPI access enabled**|**Replication enabled**|
|:-----|:-----|:-----|:-----|:-----|
|MapiDagNetwork|192.168.1.0/24|EX1 (192.168.1.15) <br/> EX2 (192.168.1.16)|True|True|
|ReplicationDagNetwork01|10.0.0.0/24|EX1 (10.0.0.15) <br/> EX2 (10.0.0.16)|False|True|

To complete the configuration of ReplicationDagNetwork01 as the dedicated replication network, disable replication for MapiDagNetwork by running the following command.

```powershell
Set-DatabaseAvailabilityGroupNetwork -Identity DAG1\MapiDagNetwork -ReplicationEnabled:$false
```

After replication is disabled for MapiDagNetwork, the Microsoft Exchange Replication service uses ReplicationDagNetwork01 for continuous replication. If ReplicationDagNetwork01 experiences a failure, the Microsoft Exchange Replication service reverts to using MapiDagNetwork for continuous replication. This is done intentionally by the system to maintain high availability.

### DAG networks and multiple subnet deployments

In the preceding example, even though there are two different subnets in use by the DAG (192.168.1.0 and 10.0.0.0), the DAG is considered a single-subnet DAG because each member uses the same subnet to form the MAPI network. When DAG members use different subnets for the MAPI network, the DAG is referred to as a *multi-subnet DAG*. In a multi-subnet DAG, the proper subnets are automatically associated with each DAG network.

For example, consider DAG2, a two-member DAG where each member has two network adapters (one dedicated for the MAPI network and the other for a replication network), and each DAG member is located in a separate Active Directory site, with its MAPI network on a different subnet. Example IP address configuration settings are shown in the following table.

**Example network adapter settings for a multi-subnet DAG**

|**Server-network adapter**|**IP address/subnet mask**|**Default gateway**|
|:-----|:-----|:-----|
|EX1-MAPI|192.168.0.15/24|192.168.0.1|
|EX1-Replication|10.0.0.15/24|Not applicable|
|EX2-MAPI|192.168.1.15|192.168.1.1|
|EX2-Replication|10.0.1.15|Not applicable|

In the following configuration, there are four subnets configured in the DAG: 192.168.0.0, 192.168.1.0, 10.0.0.0, and 10.0.1.0. When EX1 and EX2 are added to the DAG, four subnets will be enumerated, but only two DAG networks will be created: MapiDagNetwork (192.168.0.0, 192.168.1.0) and ReplicationDagNetwork01 (10.0.0.0, 10.0.1.0). These networks will be configured as shown in the following table.

**Enumerated DAG network settings for a multi-subnet DAG**

|**Name**|**Subnets**|**Interfaces**|**MAPI access enabled**|**Replication enabled**|
|:-----|:-----|:-----|:-----|:-----|
|MapiDagNetwork|192.168.0.0/24 <br/> 192.168.1.0/24|EX1 (192.168.0.15) <br/> EX2 (192.168.1.15)|True|True|
|ReplicationDagNetwork01|10.0.0.0/24 <br/> 10.0.1.0/24|EX1 (10.0.0.15) <br/> EX2 (10.0.1.15)|False|True|

### DAG networks and iSCSI networks

By default, DAGs perform discovery of all networks detected and configured for use by the underlying cluster. This includes any Internet SCSI (iSCSI) networks in use as a result of using iSCSI storage for one or more DAG members. As a best practice, iSCSI storage should use dedicated networks and network adapters. These networks shouldn't be managed by the DAG or its cluster, or used as DAG networks (MAPI or replication). Instead, these networks should be manually disabled from use by the DAG, so they can be dedicated to iSCSI storage traffic. To disable iSCSI networks from being detected and used as DAG networks, configure the DAG to ignore any currently detected iSCSI networks using the [Set-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/set-databaseavailabilitygroupnetwork) cmdlet, as shown in this example:

```powershell
Set-DatabaseAvailabilityGroupNetwork -Identity DAG2\DAGNetwork02 -ReplicationEnabled:$false -IgnoreNetwork:$true
```

This command will also disable the network for use by the cluster. Although the iSCSI networks will continue to appear as DAG networks, they won't be used for MAPI or replication traffic after running the above command.

## Configuring DAG members

Mailbox servers that are members of a DAG have some properties specific to high availability that should be configured as described in the following sections:

- [Automatic database mount dial](#automatic-database-mount-dial)

- [Database copy automatic activation policy](#database-copy-automatic-activation-policy)

- [Maximum active databases](#maximum-active-databases)

### Automatic database mount dial

The _AutoDatabaseMountDial_ parameter specifies the automatic database mount behavior after a database failover. You can use the [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/mailbox-databases-and-servers/set-mailboxserver) cmdlet to configure the _AutoDatabaseMountDial_ parameter with any of the following values:

- `BestAvailability`: If you specify this value, the database automatically mounts immediately after a failover if the copy queue length is less than or equal to 12. The copy queue length is the number of logs recognized by the passive copy that needs to be replicated. If the copy queue length is more than 12, the database doesn't automatically mount. When the copy queue length is less than or equal to 12, Exchange attempts to replicate the remaining logs to the passive copy and mounts the database.

- `GoodAvailability`: If you specify this value, the database automatically mounts immediately after a failover if the copy queue length is less than or equal to six. The copy queue length is the number of logs recognized by the passive copy that needs to be replicated. If the copy queue length is more than six, the database doesn't automatically mount. When the copy queue length is less than or equal to six, Exchange attempts to replicate the remaining logs to the passive copy and mounts the database.

- `Lossless`: If you specify this value, the database doesn't automatically mount until all logs generated on the active copy have been copied to the passive copy. This setting also causes the Active Manager best copy selection algorithm to sort potential candidates for activation based on the database copy's activation preference value and not its copy queue length.

The default value is `GoodAvailability`. If you specify either `BestAvailability` or `GoodAvailability`, and all the logs from the active copy can't be copied to the passive copy being activated, you may lose some mailbox data. However, the Safety Net feature (which is enabled by default) helps protect against most data loss by resubmitting messages that are in the Safety Net queue.

#### Example: configuring automatic database mount dial

The following example configures a Mailbox server with an _AutoDatabaseMountDial_ setting of `GoodAvailability`.

```powershell
Set-MailboxServer -Identity EX1 -AutoDatabaseMountDial GoodAvailability
```

### Database copy automatic activation policy

The _DatabaseCopyAutoActivationPolicy_ parameter specifies the type of automatic activation available for mailbox database copies on the selected Mailbox servers. You can use the [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/mailbox-databases-and-servers/set-mailboxserver) cmdlet to configure the _DatabaseCopyAutoActivationPolicy_ parameter with any of the following values:

- `Blocked`: If you specify this value, databases can't be automatically activated on the selected Mailbox servers.

- `IntrasiteOnly`: If you specify this value, the database copy is allowed to be activated on servers in the same Active Directory site. This prevents cross-site failover or activation. This property is for incoming mailbox database copies (for example, a passive copy being made an active copy). Databases can't be activated on this Mailbox server for database copies that are active in another Active Directory site.

- `Unrestricted`: If you specify this value, there are no special restrictions on activating mailbox database copies on the selected Mailbox servers.

#### Example: configuring database copy automatic activation policy

The following example configures a Mailbox server with a _DatabaseCopyAutoActivationPolicy_ setting of `Blocked`.

```powershell
Set-MailboxServer -Identity EX1 -DatabaseCopyAutoActivationPolicy Blocked
```

### Maximum active databases

The _MaximumActiveDatabases_ parameter (also used with the [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/mailbox-databases-and-servers/set-mailboxserver) cmdlet) specifies the number of databases that can be mounted on a Mailbox server. You can configure Mailbox servers to meet your deployment requirements by ensuring that an individual Mailbox server doesn't become overloaded.

The _MaximumActiveDatabases_ parameter is configured with a whole number numeric value. When the maximum number is reached, the database copies on the server won't be activated if a failover or switchover occurs. If the copies are already active on a server, the server won't allow databases to be mounted.

#### Example: configuring maximum active databases

The following example configures a Mailbox server to support a maximum of 20 active databases.

```powershell
Set-MailboxServer -Identity EX1 -MaximumActiveDatabases 20
```

## Performing maintenance on DAG members

Before performing any type of software or hardware maintenance on a DAG member, you should first put the DAG member in maintenance mode. The following scripts are provided with Exchange Server to assist with DAG maintenance procedures.

- **StartDagServerMaintenance.ps1**: Assists with moving all active databases off the server. It also moves all critical DAG support functionality, such as the Primary Active Manager (PAM) role, and blocks them from moving back to the server before maintenance is complete.

- **StopDagServerMaintenance.ps1**: Assists with taking the DAG member out of maintenance mode, and making it an active target for all databases and all critical DAG support functionality.

Both of the above scripts accept the _ServerName_ parameter (which can be either the host name or the fully qualified domain name (FQDN) of the DAG member) and the _WhatIf_ parameter. Both scripts can be run locally or remotely. The server on which the scripts are executed must have the Windows Failover Cluster Management tools installed (RSAT-Clustering).

> [!NOTE]
> The **RedistributeActiveDatabases.ps1** script is also avaialble, which assists with mounting mailbox databases on specific DAG members as inidicated by the Activation Preference number on each database. However, in Exchange 2016 CU2 or later, the new DAG property named _PreferenceMoveFrequency_ automatically balances database copies across a DAG. Therefore, you'll only need to use **RedistributeActiveDatabases.ps1** if you've disabled this functionality or if you want to balance database copies manually.

The StartDagServerMaintenance.ps1 script performs the following tasks:

- Sets the value of the _DatabaseCopyAutoActivationPolicy_ parameter on the DAG member to `Blocked`, which prevents any database copies from being activated on the server.

- Pauses the node in the cluster, which prevents the node from being and becoming the PAM.

- Moves all active databases currently hosted on the DAG member to other DAG members.

- If the DAG member currently owns the default cluster group, the script moves the default cluster group (and therefore the PAM role) to another DAG member.

If any of the preceding tasks fails, all operations, except for successful database moves, are undone by the script.

To begin maintenance procedures on a DAG member, including flushing the transport queues and suspending client connectivity, perform the following tasks:

1. To empty the transport queues, run the following command:

   ```powershell
   Set-ServerComponentState <ServerName> -Component HubTransport -State Draining -Requester Maintenance
   ```

2. To initiate the draining of the transport queues, run the following command:

   ```powershell
   Restart-Service MSExchangeTransport
   ```

3. To begin the process of draining all Unified Messaging calls (in Exchange 2016 only), run the following command:

   ```powershell
   Set-ServerComponentState <ServerName> -Component UMCallRouter -State Draining -Requester Maintenance
   ```

4. To access the DAG maintenance scripts, run the following command:

   ```powershell
   CD $ExScripts
   ```

5. To run the StartDagServerMaintenance.ps1 script, run the following command:

   ```powershell
   .\StartDagServerMaintenance.ps1 -ServerName <ServerName> -MoveComment Maintenance -PauseClusterNode
   ```

   For the value of the _MoveComment_ parameter, you can make any notation you want. The above example uses "Maintenance."

   > [!NOTE]
   > This script can take some time to execute, and during this time you may not see any activity on your screen.

6. To redirect messages pending delivery in the local queues to the Exchange server specified by the Target parameter, run

   ```powershell
   Redirect-Message -Server <ServerName> -Target <Server FQDN>
   ```

7. To place the server into maintenance mode, run:

   ```powershell
   Set-ServerComponentState <ServerName> -Component ServerWideOffline -State Inactive -Requester Maintenance
   ```

To verify that a server is ready for maintenance, perform the following tasks:

1. To verify the server has been placed into maintenance mode, confirm that only `Monitoring` and `RecoveryActionsEnabled` are in an Active state when you run the following command:

   ```powershell
   Get-ServerComponentState <ServerName> | Format-Table Component,State -Autosize
   ```

2. To verify the server is not hosting any active database copies, run:

   ```powershell
   Get-MailboxServer <ServerName> | Format-List DatabaseCopyAutoActivationPolicy
   ```

3. To verify that the cluster node is paused, run:

   ```powershell
   Get-ClusterNode <ServerName> | Format-List
   ```

4. To verify that all transport queues have been emptied, run:

   ```powershell
   Get-Queue
   ```

After the maintenance is complete and the DAG member is ready to return to service, the StopDagServerMaintenance.ps1 script helps takes the DAG member out of maintenance mode and put it back into production. The StopDagServerMaintenance.ps1 script performs the following tasks:

- Resumes the node in the cluster, which enables full cluster functionality for the DAG member.

- Sets the value of the _DatabaseCopyAutoActivationPolicy_ parameter on the DAG member to `Unrestricted`.

- Runs the [Resume-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/database-availability-groups/resume-mailboxdatabasecopy) cmdlet for each database copy hosted on the DAG member.

When you're ready to restore the DAG member to full production status, including resuming the transport queues and client connectivity, perform the following tasks:

1. To configure the server as out of maintenance mode and ready to accept client connections, run:

   ```powershell
   Set-ServerComponentState <ServerName> -Component ServerWideOffline -State Active -Requester Maintenance
   ```

2. To allow the server to accept Unified Messaging calls (in Exchange 2016 only), run:

   ```powershell
   Set-ServerComponentState <ServerName> -Component UMCallRouter -State Active -Requester Maintenance
   ```

3. To access the DAG maintenance scripts, run the following command:

   ```powershell
   CD $ExScripts
   ```

4. To execute the StopDagServerMaintenance.ps1 script, run:

   ```powershell
   .\StopDagServerMaintenance.ps1 -serverName <ServerName>
   ```

5. To enable the transport queues to resume accepting and processing messages, run:

   ```powershell
   Set-ServerComponentState <ServerName> -Component HubTransport -State Active -Requester Maintenance
   ```

6. To resume transport activity, run:

   ```powershell
   Restart-Service MSExchangeTransport
   ```

To verify that a server is ready for production use, perform the following tasks:

1. To verify the server is not in maintenance mode, run

   ```powershell
   Get-ServerComponentState <ServerName> | Format-Table Component,State -Autosize
   ```

If you're installing an Exchange update, and the update process fails, it can leave some server components in an inactive state, which will be displayed in the output of the above `Get-ServerComponentState` cmdlet. To resolve this, run the following commands:

1. `Set-ServerComponentState <ServerName> -Component ServerWideOffline -State Active -Requester Functional`

2. `Set-ServerComponentState <ServerName> -Component Monitoring -State Active -Requester Functional`

3. `Set-ServerComponentState <ServerName> -Component RecoveryActionsEnabled -State Active -Requester Functional`

## Shutting down DAG members

Exchange high availability solution is integrated with the Windows shutdown process. If an administrator or application initiates a shutdown of a Windows server in a DAG that has a mounted database that's replicated to one or more DAG members, the system attempts to activate another copy of the mounted database prior to allowing the shutdown process to complete.

However, this new behavior doesn't guarantee that all of the databases on the server being shut down will experience a `lossless` activation. As a result, it's a best practice to perform a server switchover prior to shutting down a server that's a member of a DAG.

## Installing updates on DAG members

Installing Exchange updates on a server that's a member of a DAG is a relatively straightforward process. When you install an update on a server that's a member of a DAG, several services are stopped during the installation, including all Exchange services and the Cluster service. The general process for applying updates to a DAG member is as follows:

1. Use the steps described above to put the DAG member in maintenance mode.

2. Install the update.

3. Use the steps described above to take the DAG member out of maintenance mode and put it back into production.

4. Optionally, use the RedistributeActiveDatabases.ps1 script to rebalance the active database copies across the DAG.

For more information about the latest Exchange updates, see [Exchange Server build numbers and release dates](../../new-features/build-numbers-and-release-dates.md).
