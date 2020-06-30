---
title: 'Route mail between Active Directory sites: Exchange 2013 Help'
TOCTitle: Route mail between Active Directory sites
ms:assetid: 86b423e3-7bec-4430-9a5a-4f84ce9d82ea
ms:mtpsurl: https://technet.microsoft.com/library/JJ916681(v=EXCHG.150)
ms:contentKeyID: 50934220
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Route mail between Active Directory sites

_**Applies to:** Exchange Server 2013_

An Active Directory site is a logical configuration component that's based on the physical aspects of the network. The primary purpose for creating an Active Directory site is to define which subnets in the network are connected in a way that optimizes control of Active Directory replication traffic. Microsoft Exchange Server 2013 recognizes both database availability groups (DAGs) and Active Directory sites as routing boundaries, and Exchange 2013 servers make routing decisions based on the Active Directory site topology.

By default, an Active Directory forest contains only one Active Directory site. The default name for this Active Directory site is `Default-First-Site-Name`. If no other Active Directory sites are created, all domain member computers in the forest are members of `Default-First-Site-Name`. You don't have to configure a subnet-to-site association. If additional Active Directory sites are created, you need specify the subnets that are assigned to that Active Directory site.

## Determining site membership

Each Active Directory site is associated with one or more IP subnets. An administrator assigns Active Directory site membership to computers that are configured as domain controllers and global catalog servers. Other domain member computers, such as Exchange servers, are assigned Active Directory site membership automatically when they're configured to use an IP address that's in an IP subnet that's associated with an Active Directory site. Computers that have the same Active Directory site membership are presumed to have good network connectivity. A server is always a member of a single Active Directory site.

When an application can determine the Active Directory site membership of the computer where it's installed and of other computers in the forest, and then use that information to control communication flow, it's a site-aware application. When site-aware applications must use the services of another server, such as a domain controller or global catalog server, priority is given to the servers that have the same Active Directory site membership as the computer that's requesting those services.

Exchange 2013 is a site-aware application and uses the Active Directory topology for message routing and to communicate with the services that are running on other Exchange 2013 computers. The Active Directory site isn't only a routing boundary. It's also a service discovery boundary.

Determining site membership for a domain member computer depends on a series of DNS queries to compare the local IP address to defined subnets and then determine the appropriate site membership association. To reduce the overhead that's associated with DNS queries, the Exchange 2013 Active Directory schema additions include the **msExchServerSite** attribute for the Exchange server object. The value of this attribute is the distinguished name of the Active Directory site of an Exchange server. This attribute is a property of each Exchange server object. When site membership affinity is stored as an attribute of the server object, the current topology can be read directly from Active Directory instead of relying on DNS queries and a site membership association is enabled for a non-domain computer, such as a subscribed Edge Transport server.

The value for the **msExchServerSite** attribute is populated and kept up to date by the Microsoft Exchange Active Directory Topology service. When a Windows-based computer starts, the Net Logon service determines site membership for the computer. The Net Logon service uses that information to locate domain controllers that are located in the same Active Directory site as the local computer and then directs authorization and authentication requests to those servers. The Microsoft Exchange Active Directory Topology service uses the **DsGetSiteName** API call to retrieve the site membership value from the Net Logon service and writes the Active Directory site's distinguished name to the **msExchServerSite** attribute for the Exchange server object in Active Directory.

The following table shows how an organization might define Active Directory sites. In this example, three Active Directory sites are defined, and each Active Directory site is associated with more than one IP subnet.

**Example of an Active Directory site-to-subnet association**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Active Directory site name</th>
<th>Associated IP subnets</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Site A</p></td>
<td><p>192.168.1.0/24</p>
<p>192.168.2.0/24</p></td>
</tr>
<tr class="even">
<td><p>Site B</p></td>
<td><p>192.168.3.0/24</p>
<p>192.168.4.0/24</p></td>
</tr>
<tr class="odd">
<td><p>Site C</p></td>
<td><p>192.168.5.0/24</p>
<p>192.168.6.0/24</p></td>
</tr>
</tbody>
</table>

If a server named Mailbox01 has the IP address of 192.168.1.1, it's a member of Site A. By changing the IP address of a server, you may change its site membership. If you change the IP address of Mailbox01 to 192.168.2.1, it won't change the server's Active Directory site membership because that subnet is also associated with Site A. However, if you move the server and the IP address changes to 192.168.3.1, the server would be considered a member of Site B.

A change in site membership can also occur if you change the association of subnets to Active Directory sites. For example, if you remove the subnet 192.168.3.0 from association with Site B and associate it with Site A, the site membership of a server that has the IP address of 192.168.3.1 also changes to Site A. Whenever a change in site membership occurs, Exchange must update its configuration data so that the change is considered when Exchange makes routing decisions. Some latency occurs between the time that a change in an Active Directory site membership occurs and the topology change is fully propagated.

## IP site links

Site links are logical paths between Active Directory sites. A site link object represents a set of sites that can communicate at a uniform cost. Site links don't correspond to the actual path taken by network packets on the physical network. However, the cost assigned to the site link by the administrator typically relates to the underlying network reliability, speed, and available bandwidth. For example, the Active Directory administrator would assign a lower cost to a network connection with a speed of 100 megabits per second (Mbps) than to a network connection with a speed of 10 Mbps.

By default, all site links are transitive. This means that if Site A has a link to Site B, and Site B has a link to Site C, Site A is transitively linked to Site C. The transitive link between Site A and Site C is also known as a *site-link bridge*.

Exchange uses only IP site links to determine its Active Directory site routing topology. The cost that's assigned to the IP site link will be considered by the routing component of Exchange when calculating a routing table. These costs are used to calculate the least-cost routing path to the ultimate destination for a message.

Every Active Directory site must be associated with at least one IP site link. There is a single default IP site link named `DEFAULTIPSITELINK`. When you create an Active Directory site, you need to associate that site to an IP site link. You can create additional IP site links to implement the desired topology, or you can associate every Active Directory site to the `DEFAULTIPSITELINK`. Each Active Directory site that's part of an IP site link can communicate directly with every other site in that link at a uniform cost.

In the following figure, four Active Directory sites are configured in the forest. Every site has been associated with the `DEFAULTIPSITELINK`. Therefore, each Active Directory site communicates directly with every other site by using the same cost metric. More than one communication path is indicated, but only a single IP site link is defined.

**Full mesh topology with a single IP site link**

![Full mesh topology with a single IP site link](images/JJ916681.af38d41d-e768-4221-ab4b-b782aa388b73(EXCHG.150).gif "Full mesh topology with a single IP site link")

In the following figure, four Active Directory sites are configured in the forest. In this topology, the administrator has configured IP site links to create a *hub-and-spoke topology* of Active Directory sites. Each spoke site can communicate directly with the central site, and the spoke sites can communicate with one another by using the transitive IP site links.

**Hub-and-spoke topology of Active Directory IP site links**

![Hub and spoke topology of IP site links](images/JJ916681.eca6cd51-8c2e-4996-81ea-070cd9766ef8(EXCHG.150).gif "Hub and spoke topology of IP site links")

It's important to note that Exchange uses site links when determining the least-cost path, but will always try to deliver messages directly to the destination Exchange server. For example, if a user in Site B in the topology shown in the preceding figure sends a message to another user in Site C, the Mailbox server in Site B will connect directly to the Mailbox server in Site C. If you want to force messages to go through Site A, you must enable that site as a hub site. For more information about hub sites, see "Implementing Hub Sites" later in this topic.

An Active Directory administrator implements the topology that best represents the connectivity and communication requirements of the forest. Because the same topology is used by Exchange, you need to make sure that the current topology supports efficient messaging communication.

The default cost for a site link is 100. A valid site link cost can be any number from 1 through 99,999. If you specify redundant links, the link with the lowest cost assignment is always preferred. An Exchange organization administrator can assign an Exchange-specific cost to an IP site link. If an Exchange cost is assigned to an IP site link, it will be used by Exchange. Otherwise, the Active Directory cost is used. For more information about how to set an Exchange cost on an IP site link, see "Controlling IP Site Link Costs" later in this topic. An administrator who has membership in the Enterprise Administrators group can create additional IP site links.

For more information about Active Directory site configuration, see [Designing the Site Topology](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc787284(v=ws.10)).

## Controlling IP site link costs

Active Directory IP site links costs are based on relative network speed compared to all network connections in the WAN and are designed to produce a reliable and efficient replication topology. Therefore, in most cases, the existing IP site link costs should work well for Exchange message routing. However, if after documenting the existing Active Directory site and IP site link topology, you verify that the Active Directory IP site link costs and traffic flow patterns aren't optimal for Exchange, you can make adjustments to the costs evaluated by Exchange. Changing the cost assigned to the IP site link by using Active Directory tools would impact the entire environment. Instead, use the **Set-AdSiteLink** cmdlet in the Exchange Management Shell to assign an Exchange-specific cost to the IP site link. For example, to configure the Exchange-specific cost value of 25 on the IP site link named SITELINKAB, run the following command in the Shell: `Set-AdSiteLink SITELINKAB -ExchangeCost 25`.

When an Exchange-specific cost is assigned to an IP site link, the Exchange cost overrides the Active Directory cost for message routing purposes, and routing only considers the Exchange cost when it evaluates the least-cost routing path.

Adjusting IP site link costs can be useful when the message routing topology has to diverge from the Active Directory replication topology. Exchange costs can be used to force all message routes to use a hub site. Exchange costs can also be used to control where messages are queued when communication to an Active Directory site fails. The following figure shows an Active Directory topology with four sites.

**Topology with Exchange costs configured on IP site links**

![Topology with Exchange costs on IP site links](images/JJ916681.56ac2bab-99de-4ddf-b968-80cd34ab8c21(EXCHG.150).gif "Topology with Exchange costs on IP site links")

In the preceding figure, the network connection between Site C and Site D is a low bandwidth connection that's only used for Active Directory replication and shouldn't be used for message routing. However, the Active Directory IP site link costs cause that link to be included in the least-cost routing path from any other Active Directory site to Site D. Therefore, messages are delivered to the Site D queue in Site C. The Exchange administrator prefers that the least-cost routing path include Site B instead so that if Site D is unavailable, the messages will queue at Site B. Configuring a high Exchange cost on the IP site link between Site C and Site D prevents that IP site link from being included in the least-cost routing path to Site D.

Exchange provides support for configuration of a maximum message size limit on an Active Directory IP site link. By default, Exchange doesn't impose a maximum message size limit on messages that are relayed between Exchange servers in different Active Directory sites. If you use the **Set-AdSiteLink** cmdlet to configure a maximum message size on an Active Directory IP site link, routing generates a non-delivery report (NDR) for any message that has a size larger than the maximum message size limit that's configured on any Active Directory site link in the least-cost routing path. This configuration is useful for restricting the size of messages that are sent to remote Active Directory sites that must communicate over low-bandwidth connections. For more information, see [Message size limits](message-size-limits-exchange-2013-help.md).

## Implementing hub sites

In your Exchange organization, you may want to force all message delivery through a specific Active Directory site. You can use the Shell to designate an Active Directory site as a hub site. When you do this, you cause additional overall overhead because more servers are involved in message delivery. For example, consider a message that's sent from Site A to Site E. If the least-cost routing path is Site A-Site B-Site C-Site D-Site E, and you designate Site C as a hub site, the message is relayed from Site A to Site C and then relayed from Site C to Site E.

You use the **Set-AdSite** cmdlet to specify an Active Directory site as a hub site. Whenever a hub site exists along the least-cost routing path for message delivery, the messages are queued and are processed by the Transport service on Mailbox servers in the hub site before they're relayed to their ultimate destination.

After the least-cost routing path is chosen, routing determines whether there's a hub site along that routing path. If a hub site is configured, messages stop at a Mailbox server in the hub site before they're relayed to the target destination. If there's more than one hub site along the least-cost routing path, messages stop at each hub site along the routing path.

This variation to direct relay routing only is in effect when the hub site is located along the least-cost routing path. The following figure shows the correct use of a hub site. In this diagram, Site B is configured as a hub site. Messages that are relayed from Site A to Site D are relayed through Site B before they're delivered to Site D.

**Message delivery with a hub site**

![Message delivery with a hub site](images/JJ916681.93238bcb-6bbc-4a48-aeb0-09342a421b5b(EXCHG.150).gif "Message delivery with a hub site")

The following figure shows how IP site link costs affect routing to a hub site. In this scenario, Site B has been designated as a hub site. However, Site B doesn't exist along the least-cost routing path between any other sites. Therefore, messages that are relayed from Site A to Site D are never relayed through Site B. An Active Directory site is never used as a hub site if it isn't on the least-cost routing path between two other sites.

**Misconfigured hub site**

![Misconfigured hub site](images/JJ916681.c010d4d8-5cd3-4b79-b7db-0367ba3cc287(EXCHG.150).gif "Misconfigured hub site")

You can configure any Active Directory site as a hub site. However, for this configuration to work correctly, you must have at least one Mailbox server in the hub site.

## Topology discovery

The Active Directory topology is made available to Exchange by the following required elements:

- The Microsoft Exchange Active Directory Topology service.

- The topology discovery module inside the Microsoft Exchange Transport service.

The Microsoft Exchange Active Directory Topology service runs on all Exchange 2013 Client Access servers and Mailbox servers. These servers use the Microsoft Exchange Active Directory Topology service to discover the domain controllers and global catalog servers that can be used by the Exchange servers to read and write Active Directory data. Exchange 2013 binds to the identified directory servers whenever Exchange has to read from or write to Active Directory.

The topology discovery module is part of the Microsoft Exchange Transport service and provides information about the Active Directory topology to Exchange servers. This API discovers the Exchange servers and roles in the organization and determines their relationship to the Active Directory configuration objects. Configuration data is retrieved from Active Directory and then cached so that it can be accessed by the Exchange services that are running on that computer.

The topology discovery module performs the following steps to generate an Exchange routing topology:

1. Data is read from Active Directory. All the following objects are retrieved:

   - Active Directory sites.

   - IP site links.

   - All Exchange servers.

2. The data that's retrieved in step 1 is used to create the initial topology and to begin linking and mapping the related configuration objects.

3. Exchange servers are matched to Active Directory sites by retrieving the site attribute value from the Exchange server object that's stored in Active Directory.

4. Routing tables are updated with the collection of information retrieved.

This process makes every Exchange 2013 server aware of the other Exchange servers in the organization and of how close the Exchange servers are to one another.
