---
localization_priority: Normal
description: Learn how mail is routed between Exchange servers in an Exchange 2016 or Exchange 2019 organization.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6fd39079-9655-4fd9-9269-c7462c76e0a7
ms.reviewer:
title: Mail routing in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mail routing in Exchange Server

The primary task of the Transport service that exists on Mailbox servers in your Exchange organization is to route messages received from users and external sources to their ultimate destinations. Routing decisions are made during message categorization. The categorizer is a component of the Transport service that processes all incoming messages and determines what to do with the message based on information about their destinations.

Routing in Exchange 2016 and Exchange 2019 is virtually unchanged from Exchange 2013. These are the notable changes to routing compared to Exchange 2010:

- Routing is fully aware of database availability groups (DAGs), and is able to use DAG membership in routing decisions, even when the DAG members are in different Active Directory sites. For Mailbox servers that don't belong to DAGs and for interoperability with previous versions of Exchange, Active Directory site membership is still used in routing decisions.

- The Transport service never communicates directly with a mailbox database. Instead, the Transport service communicates with the Mailbox Transport service locally or on remote Mailbox servers. Only the Mailbox Transport service communicates with the local mailbox database. When the Mailbox server is a member of a DAG, only the Mailbox Transport service on the Mailbox server that holds the active copy of the mailbox database accepts messages for the destination recipient.

- Remote procedure calls (RPCs) are used only by the Mailbox Transport service to send messages to or receive messages from the local mailbox database. When the Mailbox server is a member of a DAG, the Mailbox Transport service only uses RPCs to communicate locally with the active copies of the mailbox databases. In other words, RPC is never used for cross-server or cross-service communication. Instead, the Mailbox Transport service and the Transport service always communicate using SMTP.

- Exchange now uses more precise queuing for remote destinations. Instead of using one queue for all destinations in a remote Active Directory site, Exchange now queues messages for specific destinations within the Active Directory site, such as individual Send connectors.

- Linked connectors are no longer available. A linked connector was a Receive connector that was linked to a Send connector. All messages received by the Receive connector were automatically forwarded to the Send connector.

## Routing components

When a message is received by the Transport service on a Mailbox server, the message must be categorized. The first phase of message categorization is recipient resolution. After the recipient has been resolved, the ultimate destination can be determined. The next phase, routing, determines how to best reach that destination. Routing in Exchange is generalized for increased flexibility and decreased complexity by using the concepts of *routing destinations* and *delivery groups*.

### Routing destinations

The ultimate destination for a message is called a *routing destination*. Regardless of the complexity of an Exchange organization, there are surprisingly few routing destinations. They are:

- **A mailbox database**: This is the routing destination for any recipient with a mailbox in the Exchange organization. In Exchange 2013 or later, public folders are a type of mailbox, so routing messages to public folder recipients is the same as routing messages to mailbox recipients.

- **A connector**: A Send connector is used as a routing destination for SMTP messages based on the configuration of the Send connector (address spaces, scoped or not, etc.). Similarly, a Delivery Agent connector or Foreign connector is used as a routing destination for non-SMTP messages.

- **A distribution group expansion server**: This is the routing destination when a distribution group has a designated expansion server (a server that's responsible for expanding the membership list of the group). A distribution group expansion server is an Exchange 2013 or later Mailbox server or an Exchange 2010 Hub Transport server.

Note that these routing destinations existed in previous versions of Exchange, but they weren't called routing destinations.

### Delivery groups

A collection of one or more transport servers is responsible for delivering mail to each routing destination. This collection of transport servers is called a *delivery group*. The term *transport servers* is used because the servers could be a mixture of Exchange 2013 or later Mailbox servers (the Transport service) or Exchange 2010 Hub Transport servers. The relationship between routing destinations and delivery groups is explained in the following table:

|**Routing destination**|**Delivery group**|
|:-----|:-----|
|Exchange 2013 or later mailbox databases|Exchange 2013 or later Mailbox servers.|
|Exchange 2010 mailbox databases in Exchange 2016 organizations |Only Exchange 2010 Hub Transport servers.|
|Connectors|Exchange 2013 or later Mailbox servers or Exchange 2010 Hub Transport servers.|
|Distribution group expansion servers|Exchange 2013 or later Mailbox servers or Exchange 2010 Hub Transport servers.|

 How the message is routed depends on the relationship between the source delivery group and the destination delivery group:

- If the source and destination delivery group are the same, no routing decisions are required. The routing destination is the next hop for the message.

- If the source delivery group is outside the destination delivery group, routing decisions are required. The message is relayed along the least-cost routing path to the destination delivery group. Depending on the size and complexity of the Exchange environment, the message might be relayed through many transport servers to reach the destination delivery group for delivery to the routing destination.

The different types of delivery groups that exist in Exchange 2016 are summarized in the following table.

|**Delivery group type**|**Delivery group**|**Routing destination**|**Comments**|
|:-----|:-----|:-----|:-----|
|Routable DAG|• Exchange 2019 Mailbox servers that belong to the Exchange 2019 DAG. <br/> • Exchange 2016 Mailbox servers that belong to the Exchange 2016 DAG. <br/> • Exchange 2013 Mailbox servers that belong to the Exchange 2013 DAG.|Mailbox databases in the DAG|After the message arrives at a Mailbox server in the DAG, the Transport service routes the message to the Mailbox Transport Delivery service on the DAG member that holds the active copy of the destination mailbox database. The Mailbox Transport Delivery service then delivers the message to the local mailbox database. Although a DAG might contain Mailbox servers located in different Active Directory sites, the DAG defines the delivery group, not the Active Directory site.|
|Mailbox delivery group (Exchange 2013 or later)|Exchange 2013 or later Mailbox servers in the Active Directory site.|Mailbox databases on Exchange 2013 or later servers in the Active Directory site that don't belong to a DAG.|Mailbox databases located on servers that don't belong to a DAG are serviced by the Transport service on Mailbox servers in the same Active Directory site. <br/> After the message arrives on an Mailbox server in the Active Directory site, the Transport service uses SMTP to transfer the message to the Mailbox Transport Delivery service on the Mailbox server that holds the mailbox database. The Mailbox Transport Delivery service then delivers the message to the local mailbox database using RPC. <br/> In other words, the following mail delivery paths are supported between the different versions of Exchange: <br/> • Exchange 2019 Transport service to Exchange 2016 Mailbox Transport Delivery service to Exchange 2016 mailbox database. <br/> • Exchange 2019 Transport service to Exchange 2013 Mailbox Transport Delivery service to Exchange 2013 mailbox database.    <br/> • Exchange 2016 Transport service to Exchange 2019 Mailbox Transport Delivery service to Exchange 2019 mailbox database. <br/> • Exchange 2016 Transport service to Exchange 2013 Mailbox Transport Delivery service to Exchange 2013 mailbox database.  <br/> • Exchange 2013 Transport service to Exchange 2019 Mailbox Transport Delivery service to Exchange 2019 mailbox database. <br/> • Exchange 2013 Transport service to Exchange 2016 Mailbox Transport Delivery service to Exchange 2016 mailbox database.|
|Mailbox delivery group (Exchange 2010)|Exchange 2010 Hub Transport servers in the Active Directory site.|Mailbox databases on Exchange 2010 Mailbox servers in the Active Directory site.|Mailbox databases located on Exchange 2010 Mailbox servers are serviced by the Exchange 2010 Hub Transport servers in the same Active Directory site. <br/> After the message arrives at a random Exchange 2010 Hub Transport server in the Active Directory site, the store driver on the Hub Transport server uses RPC to write the message to the mailbox database.|
|Connector source server|A mixture of any Exchange 2013 or later Mailbox servers or Exchange 2010 Hub Transport servers that are defined as source transport servers for the connector.|A Send connector, Delivery Agent connector, or Foreign connector.|If the connector is scoped (that is, restricted to transport servers in the same Active Directory site), then only other transport servers in that site are aware of the connector, and can use the connector to route mail. <br/> If the connector isn't scoped, then all transport servers in the entire Active Directory forest are aware of the connector, and can use the connector to route mail.|
|Server list|The Exchange 2013 or later Mailbox server or Exchange 2010 Hub Transport server that's defined as the expansion server for the distribution group.|The distribution group expansion server.|none|
|AD site| Any mixture of Exchange 2013 or later Mailbox servers or Exchange 2010 Hub Transport servers that exist in: <br/>  • Active Directory sites that are configured as hub sites. <br/>  • Active Directory sites that have subscribed Edge Transport servers.|None. The message must travel through the Active Directory site on the way to the actual routing destination.|This delivery group type is the only routing scenario in Exchange 2013 or later where *delayed fan-out* is still used. Delayed fan-out attempts to reduce the number of message transmissions when multiple routing destinations share part of the least-cost routing path. <br/> Hub sites are used only if the Active Directory site exists along the least-cost routing path for the message.  br/> For Edge Transport servers, the Transport service on any Mailbox server in the subscribed Active Directory site is able to send messages to the Edge Transport server, regardless of whether that server participates in EdgeSync synchronization. For more information, see [Edge Transport servers](../../architecture/edge-transport-servers/edge-transport-servers.md).|

> [!NOTE]
> Delivery group membership isn't mutually exclusive. For example, a Mailbox server that's a member of a DAG can also be the source transport server of a Send connector. The Mailbox server belongs to the routable DAG delivery group for the mailbox databases in the DAG, and the connector source server delivery group for the Send connector.

### Queues

From the perspective of the sending transport server, each message delivery queue represents the destination for a particular message. When the Transport service selects the destination for a message, the destination is stamped on the recipient as the **NextHopSolutionKey** attribute. If a single message is sent to more than one recipient, each recipient has the **NextHopSolutionKey** attribute. The receiving transport server also performs message categorization and queues the message for delivery. After a message is queued, you can examine the delivery type for a particular queue to determine whether a message will be relayed again when it reaches the next hop destination. Every unique value of the **NextHopSolutionKey** attribute corresponds to a separate message delivery queue.

For more information, see [NextHopSolutionKey](../queues/queues.md#nexthopsolutionkey).

## Routing messages

When a message needs to be delivered to a remote delivery group, a routing path must be determined for the message. Exchange uses the following logic to select the routing path for a message. This logic is basically unchanged from Exchange 2010:

1. Calculate the least-cost routing path by adding the cost of the IP site links that must be traversed to reach the destination. If the destination is a connector, the cost assigned to the address space is added to the cost to reach the selected connector. If multiple routing paths are possible, the routing path with the lowest aggregate cost is used.

    **Note**: Size limits on connectors are a factor here. Connectors that are configured with message sizes limits smaller than the size of the message are eliminated from consideration. For more information, see [Connector selection in external message routing](connector-selection.md).

2. If more than one routing path has the same aggregate cost, the number of hops in each path is evaluated and the routing path with the least number of hops is used.

3. If more than one routing path is still available, the name assigned to the Active Directory sites before the destination is considered. The routing path where the Active Directory site nearest the destination is lowest in alphanumeric order is used. If the site nearest the destination is the same for all routing paths being evaluated, an earlier site name is considered.

In Exchange 2010, each message recipient is associated with only one Active Directory site, and there is only one least cost routing from the source Active Directory site to the destination site. In Exchange 2013 or later, a delivery group might span multiple Active Directory sites, and there might be multiple least-cost routing paths to those sites. Exchange designates a single Active Directory site in the destination delivery group as the *primary site*. The primary site is closest Active Directory site based on the routing logic described earlier. To successfully route messages between delivery groups, Exchange takes the following issues into consideration:

- **The presence of one or more hub sites along the least-cost routing path**: If the least-cost routing path to the primary site contains any hub sites, the message must be routed through the hub sites. The closest hub site along the least-cost routing path is selected as a new delivery group of the type **AD site**, which includes all transport servers in the hub site. After the message traverses the hub site, routing of the message along the least-cost routing path continues. If the primary site happens to be a hub site, the primary site is still considered a hub site for the following reasons:

  - If the destination delivery group spans multiple Active Directory sites, the source server should only attempt to connect to the servers in the hub site.

  - The servers in the hub site that belong to the target delivery group are preferred.

    As in previous version of Exchange, hub sites that aren't in the least-cost routing path to the primary site are ignored.

- **The target Exchange server to select in the destination routing group**: When the destination delivery group spans multiple Active Directory sites, the routing path to specific servers within the delivery group might have different costs. Servers located in the closest Active Directory site are selected as the target servers for the delivery group based on the least-cost routing path, and the Active Directory site those servers are in is selected as the primary site.

- **Fallback options when connection attempts to all servers in the destination routing group fail**: If the destination delivery group spans multiple Active Directory sites, the first fallback option is all other servers in the destination delivery group in other Active Directory sites that aren't selected as target servers. Server selection is based on the least-cost routing path to the other Active Directory sites. If the destination delivery group has any servers in the local Active Directory site, there are no other fallback options because the message is already as close to the target routing destination as possible. If the destination delivery group has servers in remote Active Directory sites, the option is to try to connect to all other servers in the primary site. If that fails, a backoff path in the least-cost routing path to the primary site is used. Exchange tries to deliver the message as close to the destination as possible by backing off, hop by hop, along the least-cost routing path until a connection is made.

### Routing messages between Active Directory sites

The way that Exchange routes messages between Active Directory sites is virtually the same as Exchange 2010. For more information, see [Route Mail Between Active Directory Sites](https://docs.microsoft.com/exchange/route-mail-between-active-directory-sites-exchange-2013-help).

### Routing in the Front End Transport service on Mailbox servers

The Front End Transport service acts as a stateless proxy for all inbound and (optionally) outbound external SMTP traffic for the Exchange organization. For outgoing messages, the Transport service communicates with the Front End Transport service only when it's specifically configured to do so. For more information, see [Configure Send connectors to proxy outbound mail](../../mail-flow/connectors/proxy-outbound-mail.md).

For incoming messages, the Front End Transport service must quickly find a single, healthy Transport service to receive the message transmission, regardless of the number or type of recipients. Failure to do so results in the email service being perceived as unavailable by the sending server. Like the Transport service, the Front End Transport service loads routing tables based on information from Active Directory, and uses delivery groups to determine how to route messages. However, the routing tables used by the Front End Transport service have the following unique characteristics:

- The Front End Transport service is never considered a member of a delivery group, even when the Mailbox server and the Client access server are installed on the same physical server (which is always the case in Exchange 2016 or later). This forces the Front End Transport service to communicate only with the Transport service.

- The routing tables don't contain any Send connector routes.

- The routing tables contain a special list of Mailbox servers in the local Active Directory site for fast fail-over purposes.

Routing in the Front End Transport service resolves message recipients to mailbox databases. The list of Mailbox servers used by the Front End Transport service is based on the mailbox databases of the message recipients. Note that it's possible that none of the recipients have mailboxes, for example, if the recipient is a distribution group or a mail user. For each mailbox database, the Front End Transport service looks up the delivery group and the associated routing information. The delivery groups used by the Front End Transport service are:

- Routable DAG

- Mailbox delivery group

- AD site

Depending on the number and type of recipients, the Front End Transport service performs one of the following actions:

- For messages with a single mailbox recipient, select a Mailbox server in the target delivery group, and give preference to the Mailbox server based on the proximity of the Active Directory site. Routing the message to the recipient might involve routing the message through a hub site.

- For messages with multiple mailbox recipients, use the first 20 recipients to select a Mailbox server in the closest delivery group, based on the proximity of the Active Directory site. Note that message bifurcation doesn't occur in Front End Transport, so only one Mailbox server is ultimately selected, regardless of number of recipients in a message.

- If the message has no mailbox recipients, select a random Mailbox server in the local Active Directory site.

### Routing in the Mailbox Transport service on Mailbox servers

The Mailbox Transport service consists of two separate services: the Mailbox Transport Submission service and Mailbox Transport Delivery service. The Mailbox Transport Delivery service receives SMTP messages from the Transport service, and connects to the local mailbox database by using RPC to deliver the message. The Mailbox Transport Submission service connects to the local mailbox database by using RPC to retrieve messages, and submits the messages over SMTP to the Transport service. The Mailbox Transport service is stateless, and doesn't use message delivery queues.

Like the Transport service, the Mailbox Transport service loads routing tables based on information from Active Directory, and uses delivery groups to determine how to route messages. However, there are routing aspects that are unique to the Mailbox Transport service:

- Because the Transport service and the Mailbox Transport service exist on the same Mailbox server, the Mailbox Transport service always belongs to the same delivery group as the Mailbox server. This delivery group is referred to as the *local delivery group*.

- The Mailbox Transport Submission service doesn't automatically send messages to the Transport service on the local Mailbox server or on other Mailbox servers in its own local delivery group. The Mailbox Transport Submission service has access to the same routing topology information as the Transport service, so the Mailbox Transport submission service can send messages to the Transport service on Mailbox servers outside the delivery group. The Mailbox servers in the local delivery group are used as fallback options, and for delivery to non-mailbox recipients.

- The Mailbox Transport service only communicates with the Transport service on Mailbox servers.

- The Mailbox Transport service only communicates with local mailbox databases. The Mailbox Transport service never communicates with mailbox databases on other Mailbox servers.

When a user sends a message from their mailbox, the Mailbox Transport Submission service resolves the message recipients to mailbox databases. The list of Mailbox servers used by the Mailbox Transport Submission service is based on the mailbox databases of the message recipients. Note that it's possible that none of the recipients have mailboxes, for example, if the recipient is a distribution group or a mail user. For each mailbox database, the Mailbox Transport Submission service looks up the delivery group and the associated routing information. The delivery groups used by the Mailbox Transport Submission service are:

- Routable DAG

- Mailbox delivery group

- AD site

Depending on the number and type of recipients, the Mailbox Transport Submission service performs one of the following actions:

- For messages with a single mailbox recipient, select a Mailbox server in the target delivery group, and give preference to the Mailbox server based on the proximity of the Active Directory site. Routing the message to the recipient might involve routing the message through a hub site.

- For messages with multiple mailbox recipients, use the first 20 recipients to select a Mailbox server in the closest delivery group, based on the proximity of the Active Directory site.

- If the message has no mailbox recipients, select a Mailbox server in the local delivery group.

When the Mailbox Transport Delivery service receives a message from the Transport service, it accepts or rejects the message for delivery to a local mailbox database. The Mailbox Transport Delivery service can deliver the message if the recipient resides in an active copy of a local mailbox database. But, if the recipient doesn't reside in an active copy of a local mailbox database, the Mailbox Transport Delivery service can't deliver the message, and must provide a non-delivery response to the Transport service. For example, if the active copy of the mailbox database recently moved to another server, the Transport service might erroneously transmit a message to a Mailbox server that now holds an inactive copy of the mailbox database. The non-delivery responses that the Mailbox Transport Delivery service returns to the Transport service include:

- Retry delivery

- Generate an NDR (also known as a non-delivery report, delivery status notification, DSN, or bounce message)

- Reroute the message

### Routing in the Transport service on Edge Transport servers

The Transport service on Edge Transport servers provides SMTP relay and smart host services for all Internet mail flow. Messages that come and go from the Internet are stored in message delivery queues on the Edge Transport server. The queues correspond to external domains or Send connectors. For more information, see [NextHopSolutionKey](../queues/queues.md#nexthopsolutionkey).

Typically, when you install an Edge Transport server in your perimeter network, you subscribe the Edge Transport server to an Active Directory site. The Active Directory site contains the Mailbox servers that relay messages to and from the Edge Transport server. The Edge Subscription process creates an Active Directory site membership affiliation for the Edge Transport server. The site affiliation enables the Mailbox servers in the Active Directory site to relay messages to the Edge Transport server without having to configure explicit Send connectors.

In organizations that have Exchange servers in multiple Active Directory sites, outbound mail from internal recipients to external recipients is first routed to the subscribed Active Directory site. Transport servers in the target Active Directory site are the delivery group. The routing destination is the intra-organization Send connector in the Transport service on any of the Mailbox servers in the subscribed Active Directory site. The *intra-organization Send connector* is special Send connector that exists in the Transport service on every Mailbox server. This Send connector is implicitly created, invisible, requires no management, and is used to relay messages between Exchange servers.

For more information about how mail is routed to and from Edge Transport servers, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).
