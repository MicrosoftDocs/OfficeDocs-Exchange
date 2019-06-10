---
localization_priority: Normal
description: 'Summary: Learn about the elements of high availability and site resilience to incorporate in your Exchange Server 2016 or Exchange Server 2019 deployment plan.'
ms.topic: conceptual
author: msdmaguire
ms.author: dmaguire
ms.assetid: 29bb0358-fc8e-4437-8feb-d2959ed0f102
ms.date: 7/9/2018
ms.reviewer: 
title: Plan for high availability and site resilience
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Plan for high availability and site resilience

During the planning phase, the system architects, administrators, and other key stakeholders should identify the business requirements and the architectural requirements for the deployment; in particular, the requirements about high availability and site resilience.

There are general requirements that must be met for deploying these features, as well as hardware, software, and networking requirements that must also be met.

## General requirements
<a name="GR"> </a>

Before deploying a database availability group (DAG) and creating mailbox database copies, make sure that the following system-wide recommendations are met:

- Domain Name System (DNS) must be running. Ideally, the DNS server should accept dynamic updates. If the DNS server doesn't accept dynamic updates, you must create a DNS host (A) record for each Exchange server. Otherwise, Exchange won't function properly.

- Each Mailbox server in a DAG must be a member server in the same domain.

- Adding an Exchange Mailbox server that's also a directory server to a DAG isn't supported.

- The name you assign to the DAG must be a valid, available, and unique computer name of 15 characters or less.

## Hardware requirements
<a name="HR"> </a>

Generally, there are no special hardware requirements specific to DAGs or mailbox database copies. The servers used must meet all of the requirements set forth in [Exchange Server prerequisites](../plan-and-deploy/prerequisites.md).

## Storage requirements
<a name="StoreReq"> </a>

Generally, there are no special storage requirements specific to DAGs or mailbox database copies. DAGs don't require or use cluster-managed shared storage. Cluster-managed shared storage is supported for use in a DAG only when the DAG is configured to use a solution that leverages the Third Party Replication API built into Exchange Server.

## Software requirements
<a name="SoftReq"> </a>

Each member of a DAG must be running the same operating system. Exchange Server 2016 is supported on the Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016. Exchange Server 2019 is supported on the Windows Server 2019 operating system. Within a specific DAG, all members must be running the same supported operating system.

In addition to meeting the prerequisites for installing Exchange Server, there are operating system requirements that must be met. DAGs use Windows Failover Clustering technology, and as a result, they require the Standard or Datacenter version of the Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, or Windows Server 2019 operating systems.

## Network requirements
<a name="NR"> </a>

There are specific networking requirements that must be met for each DAG and for each DAG member. Each DAG must have a single *MAPI network*, which is used by a DAG member to communicate with other servers (for example, other Exchange servers or directory servers), and zero or more *Replication networks*, which are networks dedicated to log shipping and seeding.

In previous versions of Exchange, we recommended at least two networks (one MAPI network and one Replication network) for DAGs. In Exchange 2016 and Exchange 2019, multiple networks are supported, but our recommendation depends on your physical network topology. If you have multiple physical networks between DAG members that are physically separate from one another, then using a separate MAPI and Replication network provides additional redundancy. If you have multiple networks that are partially physically separate but converge into a single physical network (for example, a single WAN link), then using a single network (preferably 10 gigabit Ethernet) for both MAPI and Replication traffic is recommended. This provides simplicity for the network and the network path.

Consider the following when designing the network infrastructure for your DAG:

- Each member of the DAG must have at least one network adapter that's able to communicate with all other DAG members. If you're using a single network path, we recommend that you use a minimum of 1 gigabit Ethernet, but preferably 10 gigabit Ethernet. In addition, when using a single network adapter in each DAG member, we recommend that you design the overall solution with the single network adapter and path in mind.

- Using two network adapters in each DAG member provides you with one MAPI network and one Replication network, with redundancy for the Replication network and the following recovery behaviors:

  - In the event of a failure affecting the MAPI network, a server failover will occur (assuming there are healthy mailbox database copies that can be activated).

  - In the event of a failure affecting the Replication network, if the MAPI network is unaffected by the failure, log shipping and seeding operations will revert to use the MAPI network, even if the MAPI network has it's _ReplicationEnabled_ property set to False. When the failed Replication network is restored to health and ready to resume log shipping and seeding operations, you must manually switch over to the Replication network. To change replication from the MAPI network to a restored Replication network, you can either suspend and resume continuous replication by using the **Suspend-MailboxDatabaseCopy** and **Resume-MailboxDatabaseCopy** cmdlets, or restart the Microsoft Exchange Replication service. We recommend using suspend and resume operations to avoid the brief outage caused by restarting the Microsoft Exchange Replication service.

- Each DAG member must have the same number of networks. For example, if you plan on using a single network adapter in one DAG member, all members of the DAG must also use a single network adapter.

- Each DAG must have no more than one MAPI network. The MAPI network must provide connectivity to other Exchange servers and other services, such as Active Directory and DNS.

- Additional Replication networks can be added, as needed. You can also prevent an individual network adapter from being a single point of failure by using network adapter teaming or similar technology. However, even when using teaming, this doesn't prevent the network itself from being a single point of failure. Moreover, teaming adds unnecessary complexity to the DAG.

- Each network in each DAG member server must be on its own network subnet. Each server in the DAG can be on a different subnet, but the MAPI and Replication networks must be routable and provide connectivity, such that:

  - Each network in each DAG member server is on its own network subnet that's separate from the subnet used by each other network in the server.

  - Each DAG member server's MAPI network can communicate with each other DAG member's MAPI network.

  - Each DAG member server's Replication network can communicate with each other DAG member's Replication network.

  - There is no direct routing that allows heartbeat traffic from the Replication network on one DAG member server to the MAPI network on another DAG member server, or vice versa, or between multiple Replication networks in the DAG.

- Regardless of their geographic location relative to other DAG members, each member of the DAG must have round trip network latency no greater than 500 milliseconds between each other member. As the round trip latency between two Mailbox servers hosting copies of a database increases, the potential for replication not being up to date also increases. Regardless of the latency of the solution, customers should validate that the networks between all DAG members is capable of satisfying the data protection and availability goals of the deployment. Configurations with higher latency values may require special tuning of DAG, replication, and network parameters, such as increasing the number of databases or decreasing the number of mailboxes per database, to achieve the desired goals.

- Round trip latency requirements may not be the most stringent network bandwidth and latency requirement for a multi-datacenter configuration. You must evaluate the total network load, which includes client access, Active Directory, transport, continuous replication, and other application traffic, to determine the necessary network requirements for your environment.

- DAG networks support Internet Protocol version 4 (IPv4) and IPv6. IPv6 is supported only when IPv4 is also used; a pure IPv6 environment isn't supported. Using IPv6 addresses and IP address ranges is supported only when both IPv6 and IPv4 are enabled on that computer, and the network supports both IP address versions. If Exchange Server is deployed in this configuration, all server roles can send data to and receive data from devices, servers, and clients that use IPv6 addresses.

- Automatic Private IP Addressing (APIPA) is a feature of Windows that automatically assigns IP addresses when no Dynamic Host Configuration Protocol (DHCP) server is available on the network. APIPA addresses (including manually assigned addresses from the APIPA address range) aren't supported for use by DAGs or by Exchange Server.

### DAG name and IP address requirements

During creation, each DAG is given a unique name, and either assigned one or more static IP addresses, or configured to use DHCP. Regardless of whether you use static or dynamically assigned addresses, any IP address assigned to the DAG must be on the MAPI network.

Each DAG running on Windows Server 2012 requires a minimum of one IP address on the MAPI network. A DAG requires additional IP addresses when the MAPI network is extended across multiple subnets. DAGs running on Windows Server 2012 R2, Windows Server 2016 or Windows Server 2019 that are created without a cluster administrative access point do not require an IP address.

The following figure illustrates a DAG where all nodes in the DAG have the MAPI network on the same subnet.

 **DAG with MAPI network on same subnet**

![DAG on single subnet](../media/ITPro_Mailbox_DAGSingleSubnet.gif)

In this example, the MAPI network in each DAG member is on the 172.19.18. _x_ subnet. As a result, the DAG requires a single IP address on that subnet.

The next figure illustrates a DAG that has a MAPI network that extends across two subnets: 172.19.18. _x_ and 172.19.19. _x_.

 **DAG with MAPI network on multiple subnets**

![DAG extended across multiple subnets](../media/ITPro_Mailbox_DAGMultiSubnet.gif)

In this example, the MAPI network in each DAG member is on a separate subnet. As a result, the DAG requires two IP addresses, one for each subnet on the MAPI network.

Each time the DAG's MAPI network is extended across an additional subnet, an additional IP address for that subnet must be configured for the DAG. Each IP address that's configured for the DAG is assigned to and used by the DAG's underlying failover cluster. The name of the DAG is also used as the name for the underlying failover cluster.

At any specific time, the cluster for the DAG will use only one of the assigned IP addresses. Windows Failover Clustering registers this IP address in DNS when the cluster IP address and Network Name resources are brought online. In addition to using an IP address and network name, a cluster name object (CNO) is created in Active Directory. The name, IP address, and CNO for the cluster are used internally by the system to secure the DAG and for internal communication purposes. Administrators and end users don't need to interface with or connect to the DAG name or IP address.

> [!NOTE]
> Although the cluster's IP address and network name are used internally by the system, there is no hard dependency in Exchange Server that these resources be available. Even if the underlying cluster's administrative access point (for example, its IP address and Network Name resources) is offline, internal communication still occurs within the DAG by using the DAG member server names. However, we recommend that you periodically monitor the availability of these resources to ensure that they aren't offline for more than 30 days. If the underlying cluster is offline for more than 30 days, the cluster CNO account may be invalidated by the garbage collection mechanism in Active Directory.

### Network adapter configuration for DAGs

Each network adapter must be configured properly based on its intended use. A network adapter that's used for a MAPI network is configured differently from a network adapter that's used for a Replication network. In addition to configuring each network adapter correctly, you must also configure the network connection order in Windows so that the MAPI network is at the top of the connection order. For detailed steps about how to modify the network connection order, see [Modify the protocol bindings and network provider order](https://go.microsoft.com/fwlink/p/?linkId=179138).

#### MAPI network adapter configuration

A network adapter intended for use by a MAPI network should be configured as described in the following table.

|**Networking features**|**Settings**|
|:-----|:-----|
|Client for Microsoft Networks|Enabled|
|QoS Packet Scheduler|Optionally enabled|
|File and Printer Sharing for Microsoft Networks|Enabled|
|Internet Protocol version 6 (TCP/IP v6)|Enabled|
|Internet Protocol version 4 (TCP/IP v4)|Enabled|
|Link-Layer Topology Discovery Mapper I/O Driver|Enabled|
|Link-Layer Topology Discovery Responder|Enabled|

The TCP/IP v4 properties for a MAPI network adapter are configured as follows:

- The IP address for a DAG member's MAPI network can be manually assigned or configured to use DHCP. If DHCP is used, we recommend using persistent reservations for the server's IP address.

- The MAPI network typically uses a default gateway, although one isn't required.

- At least one DNS server address must be configured. Using multiple DNS servers is recommended for redundancy.

- The **Register this connection's addresses in DNS** check box should be selected.

#### Replication network adapter configuration

A network adapter intended for use by a Replication network should be configured as described in the following table.

|**Networking features**|**Settings**|
|:-----|:-----|
|Client for Microsoft Networks|Disabled|
|QoS Packet Scheduler|Optionally enabled|
|File and Printer Sharing for Microsoft Networks|Disabled|
|Internet Protocol version 6 (TCP/IP v6)|Enabled|
|Internet Protocol version 4 (TCP/IP v4)|Enabled|
|Link-Layer Topology Discovery Mapper I/O Driver|Enabled|
|Link-Layer Topology Discovery Responder|Enabled|

The TCP/IP v4 properties for a Replication network adapter are configured as follows:

- The IP address for a DAG member's Replication network can be manually assigned or configured to use DHCP. If DHCP is used, we recommend using persistent reservations for the server's IP address.

- Replication networks typically don't have default gateways, and if the MAPI network has a default gateway, no other networks should have default gateways. Routing of network traffic on a Replication network can be configured by using persistent, static routes to the corresponding network on other DAG members using gateway addresses that have the ability to route between the Replication networks. All other traffic not matching this route will be handled by the default gateway that's configured on the adapter for the MAPI network.

- DNS server addresses shouldn't be configured.

- The **Register this connection's addresses in DNS** check box shouldn't be selected.

## Witness server requirements
<a name="WSR"> </a>

A *witness server* is a server outside a DAG that's used to achieve and maintain quorum when the DAG has an even number of members. DAGs with an odd number of members don't use a witness server. All DAGs with an even number of members must use a witness server. The witness server can be any computer running Windows Server. There is no requirement that the version of the Windows Server operating system of the witness server matches the operating system used by the DAG members.

Quorum is maintained at the cluster level, underneath the DAG. A DAG has quorum when the majority of its members are online and can communicate with the other online members of the DAG. This notion of quorum is one aspect of the concept of quorum in Windows failover clustering. A related and necessary aspect to quorum in failover clusters is the *quorum resource*. The quorum resource is a resource inside a failover cluster that provides a means for arbitration leading to cluster state and membership decisions. The quorum resource also provides persistent storage for storing configuration information. A companion to the quorum resource is the *quorum log*, which is a configuration database for the cluster. The quorum log contains information such as which servers are members of the cluster, what resources are installed in the cluster, and the state of those resources (for example, online or offline).

It's critical that each DAG member have a consistent view of how the DAG's underlying cluster is configured. The quorum acts as the definitive repository for all configuration information relating to the cluster. The quorum is also used as a tie-breaker to avoid *split-brain* syndrome. Split brain syndrome is a condition that occurs when DAG members can't communicate with each other but are running. Split brain syndrome is prevented by always requiring a majority of the DAG members (and in the case of DAGs with an even number of member, the DAG witness server) to be available and interacting for the DAG to be operational.

## Planning for site resilience
<a name="PSR"> </a>

Every day, more businesses recognize that access to a reliable and available messaging system is fundamental to their success. For many organizations, the messaging system is part of the business continuity plans, and their messaging service deployment is designed with site resilience in mind. Fundamentally, many site resilient solutions involve the deployment of hardware in a second datacenter.

Ultimately, the overall design of a DAG, including the number of DAG members and the number of mailbox database copies, will depend on each organization's recovery service level agreements (SLAs) that cover various failure scenarios. During the planning stage, the solution's architects and administrators identify the requirements for the deployment, including in particular the requirements for site resilience. They identify the locations to be used and the required recovery SLA targets. The SLA will identify two specific elements that should be the basis for the design of a solution that provides high availability and site resilience: the recovery time objective and the recovery point objective. Both of these values are measured in minutes. The recovery time objective is how long it takes to restore service. The recovery point objective refers to how current the data is after the recovery operation has completed. An SLA may also be defined for restoring the primary datacenter to full service after its problems are corrected.

The solution's architects and administrators will also identify which set of users require site resilience protection, and determine if the multiple site solution will be an active/passive or active/active configuration. In an active/passive configuration, no users are normally hosted in the standby datacenter. In an active/active configuration, users are hosted in both locations, and some percentage of the total number of databases within the solution has a preferred active location in a second datacenter. When service for the users of one datacenter fails, those users are activated in the other datacenter.

Constructing the appropriate SLAs often requires answering the following basic questions:

- What level of service is required after the primary datacenter fails?

- Do users need their data or just messaging services?

- How rapidly is data required?

- How many users must be supported?

- How will users access their data?

- What is the standby datacenter activation SLA?

- How is service moved back to the primary datacenter?

- Are the resources dedicated to the site resilience solution?

By answering these questions, you begin to shape a site resilient design for your messaging solution. A core requirement of recovery from site failure is to create a solution that gets the necessary data to the backup datacenter that hosts the backup messaging service.

### Certificate planning

There are no unique or special design considerations for certificates when deploying a DAG in a single datacenter. However, when extending a DAG across multiple datacenters in a site resilient configuration, there are some specific considerations with respect to certificates. Generally, your certificate design will depend on the clients in use, as well as the certificate requirements by other applications that use certificates. But there are some specific recommendations and best practices you should follow with respect to the type and number of certificates.

As a best practice, you should minimize the number of certificates you use for your Exchange servers and reverse proxy servers. We recommend using a single certificate for all of these service endpoints in each datacenter. This approach minimizes the number of certificates that are needed, which reduces both cost and complexity for the solution.

For Outlook Anywhere clients, we recommend that you use a single subject alternative name (SAN) certificate for each datacenter, and include multiple host names in the certificate. To ensure Outlook Anywhere connectivity after a database, server, or datacenter switchover, you must use the same Certificate Principal Name on each certificate, and configure the Outlook Provider Configuration object in Active Directory with the same Principal Name in Microsoft-Standard Form (msstd). For example, if you use a Certificate Principal Name of mail.contoso.com, you would configure the attribute as follows.

```
Set-OutlookProvider EXPR -CertPrincipalName "msstd:mail.contoso.com"
```

Some applications that integrate with Exchange have specific certificate requirements that may require using additional certificates. Exchange Server can co-exist with Office Communications Server (OCS). OCS requires certificates with 1024-bit or greater certificates that use the OCS server name for the Certificate Principal Name. Because using an OCS server name for the Certificate Principal Name would prevent Outlook Anywhere from working properly, you would need to use an additional and separate certificate for the OCS environment.

### Network planning

In addition to the specific networking requirements that must be met for each DAG, as well as for each server that's a member of a DAG, there are some requirements and recommendations that are specific to site resilience configurations. As with all DAGs, whether the DAG members are deployed in a single site or in multiple sites, the round-trip return network latency between DAG members must be no greater than 500 milliseconds. In addition, there are specific configuration settings that are recommended for DAGs that are extended across multiple sites:

- **MAPI networks should be isolated from Replication networks**: Windows network policies, Windows firewall policies, or router access control lists (ACLs) should be used to block traffic between the MAPI network and the Replication networks. This configuration is necessary to prevent network heartbeat cross talk.

- **Client-facing DNS records should have a Time to Live (TTL) value of 5 minutes**: The amount of downtime that clients experience is dependent not just on how quickly a switchover can occur, but also on how quickly DNS replication occurs and the clients query for updated DNS information. DNS records for all Exchange client services, including Outlook on the web (formerly known as Outlook Web App), Exchange ActiveSync, Exchange Web Services, Outlook Anywhere, SMTP, POP3, and IMAP4 in both the internal and external DNS servers should be set with a TTL of 5 minutes.

- **Use static routes to configure connectivity across Replication networks**: To provide network connectivity between each of the Replication network adapters, use persistent static routes. This is a quick and one-time configuration that's performed on each DAG member when using static IP addresses. If you're using DHCP to obtain IP addresses for your Replication networks, you can also use it to assign static routes for the replication, thereby simplifying the configuration process.

### General site resilience planning

In addition to the requirements listed above for high availability, there are other recommendations for deploying Exchange Server in a site resilient configuration (for example, extending a DAG across multiple datacenters). What you do during the planning phase will directly affect the success of your site resilience solution. For example, poor namespace design can cause difficulties with certificates, and an incorrect certificate configuration can prevent users from accessing services.

To minimize the time it takes to activate a second datacenter, and allow the second datacenter to host the service endpoints of a failed datacenter, the appropriate planning must be completed. The following are examples:

- The SLA goals for the site resilience solution must be well understood and documented.

- The servers in the second datacenter must have sufficient capacity to host the combined user population of both datacenters.

- The second datacenter must have all services enabled that are provided in the primary datacenter (unless the service isn't included as part of the site resilience SLA). This includes Active Directory, networking infrastructure (for example, DNS or TCP/IP), telephony services (if Unified Messaging in Exchange 2016 is in use), and site infrastructure (such as power or cooling).

- For some services to be able to service users from the failed datacenter, they must have the proper server certificates configured. Some services don't allow instancing (for example, POP3 and IMAP4) and only allow the use of a single certificate. In these cases, either the certificate must be a SAN certificate that includes multiple names, or the multiple names must be similar enough so that a wildcard certificate can be used (assuming the security policies of the organization allows the use of wildcard certificates).

- The necessary services must be defined in the second datacenter. For example, if the first datacenter has three different SMTP URLs on different transport servers, the appropriate configuration must be defined in the second datacenter to enable at least one (if not all three) transport server to host the workload.

- The necessary network configuration must be in place to support the datacenter switchover. This might mean making sure that the load balancing configurations are in place, that global DNS is configured, and that the Internet connection is enabled with the appropriate routing configured.

- The strategy for enabling the DNS changes necessary for a datacenter switchover must be understood. The specific DNS changes, including their TTL settings, must be defined and documented to support the SLA in effect.

- A strategy for testing the solution must also be established and factored into the SLA. Periodic validation of the deployment is the only way to guarantee that the quality and viability of the deployment doesn't degrade over time. After the deployment is validated, we recommend that the part of the configuration that directly affects the success of the solution be explicitly documented. In addition, we recommend that you enhance your change management processes around those segments of the deployment.
