---
localization_priority: Normal
description: 'Summary: Learn how Exchange Server selects connectors (Send connectors, Delivery Agent connectors, or Foreign connectors) to deliver messages to external recipients.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 579c6dc1-ece3-442a-bb8c-f55bcb543119
ms.date: 7/6/2018
ms.reviewer: 
title: Connector selection in external message routing
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Connector selection in external message routing

Like previous versions of Exchange, Exchange Server 2016 and Exchange Server 2019 use connectors to deliver messages to external recipients (recipients that don't exist in the Exchange organization). Exchange uses Send connectors to route messages to external SMTP domains. If the external recipient isn't on an SMTP messaging system, Exchange uses Delivery Agent connectors or Foreign connectors.

 For more information about the different types of connectors, see [Connectors](../../mail-flow/connectors/connectors.md). For more information about how Exchange makes routing decisions, see [Mail routing](mail-routing.md).

## Connector considerations in message routing

The settings that are configured on connectors might eliminate an otherwise available connector from routing consideration. These settings are described in the following table:

|**Connector setting**|**Comments**|
|:-----|:-----|
|State (enabled or disabled)|Only enabled connectors are used in routing decisions. If a connector is disabled, it's not considered when routing messages.|
|Address space|The address spaces defines the destination domains or other address spaces that are serviced by the connector. When Exchange selects a connector for routing a message, it only considers connectors that have a matching address space. If more than one connector matches the destination address space, the connector with the more precise address match is selected. <br/><br/> For example, suppose the recipient is julia@marketing.contoso.com, and separate Send connectors are configured for \*, \*.contoso.com and marketing.contoso.com. The order of connector preference based solely on the address space is: <br/>1: marketing.contoso.com <br/>2: \*.contoso.com <br/>3: \*|
|Address space type|By default, the address space type on a new Send connector is SMTP. If you specify a non-SMTP address space, the messages are still sent to the destination (a smart host) by using SMTP. You need to create a Delivery Agent connector or a Foreign connector to route non-SMTP messages to non-SMTP messaging servers without using SMTP.|
|Address space cost|You use the cost value on the address space for mail flow optimization and fault tolerance when the same address space is configured on multiple connectors. A lower cost value indicates a preferred connector.|
|Source server|At least one Mailbox server or Edge Transport server must be configured to host the connector. You can configure multiple source servers to provide load balancing and fault tolerance for the address spaces that are defined on the connector.|
|Scope|The connector's scope controls its visibility within the Exchange organization. <br/><br/> By default, connectors are visible to all the Exchange servers in the entire Active Directory forest. However, you can limit the scope of a connector so that it's only visible to other Exchange servers in the local Active Directory site. The connector is invisible to Exchange servers in other Active Directory sites, and isn't used in their routing decisions. A connector that's restricted in this way is said to be *scoped*.|
|Message size limits|A message size restriction on a connector can eliminate the connector from selection if the message is larger than the maximum size that's allowed on the connector. <br/><br/> For more information about message size limits on connectors, see [Connector limits](../message-size-limits.md#connector-limits).|

## Selecting the connector for an external recipient

For messages that are sent to external recipients, Exchange must select the best connector to route the message through. The decisions that are required to select this connector are described in the following list:

1. Exchange eliminates all connectors that have a message size limit that's smaller than the size of the message.

2. Exchange narrows the list of remaining connectors to those that satisfy all of the following criteria:

   - The connector is scoped to another Exchange server in the local Active Directory site, or isn't scoped at all (is available to all Exchange in the Active Directory forest).

   - The connector is enabled.

   - The connector is configured with an address space that matches the recipient's email address.

3. From the resulting list of connectors, Exchange selects the connector with the most specific address space match. If multiple connectors have the same address space specificity, Exchange uses the following criteria to select a connector:

   1. **Aggregate cost**: This is the sum of the cost that's assigned to all the IP site links between the source Active Directory site and the Active Directory site that contains the source servers for the connector, and the cost that's assigned to the address space on the connector (IP site link costs + connector cost). The connector with the lowest aggregate cost is selected. If multiple connectors have the same aggregate cost, the selection process continues to the next step.

   2. **Hop count**: The source server for the connector that can be reached in the least number of hops is selected. Typically, this means the general order of preference is:

      1. The local Exchange server.

      2. An Exchange server in the same Active Directory site.

      3. An Exchange server in a remote Active Directory site.

      If multiple connectors have the same hop count, the selection process continues to the next step.

   3. **Connector name**: If more than one routing path has the same aggregate cost and hop count, the connector with the name that has the lowest alphanumeric value is selected.

## Handling messages that can't be routed

If no connector satisfies all of the selection criteria, one of the following actions occurs:

- If there is no matching connector for an SMTP address space, the recipient is marked as unreachable and the message is routed to the Unreachable queue. For more information about the Unreachable queue, see [Types of queues](../queues/queues.md#types-of-queues).

- If there is no matching connector for a non-SMTP address space, a non-delivery report (also known as an NDR or bounce message) is returned to the sender.

- If the message size exceeds the connector size restriction for all connectors, an NDR is returned to the sender.
