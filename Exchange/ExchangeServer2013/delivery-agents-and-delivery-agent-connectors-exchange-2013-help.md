---
title: 'Delivery agents and Delivery Agent connectors: Exchange 2013 Help'
TOCTitle: Delivery agents and Delivery Agent connectors
ms:assetid: 38c942ee-b59d-47ec-87eb-bebad441ada5
ms:mtpsurl: https://technet.microsoft.com/library/Dd638118(v=EXCHG.150)
ms:contentKeyID: 49300477
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delivery agents and Delivery Agent connectors

_**Applies to:** Exchange Server 2013_

A delivery agent can deliver messages from your SMTP Exchange Server environment to a system that doesn't use the SMTP protocol. Each delivery agent is associated with a Delivery Agent connector, which queues messages routed to the delivery agent for processing and delivery to the non-SMTP device or system.

> [!TIP]
> While the Foreign connector architecture remains in Microsoft Exchange 2013, we recommend using delivery agents for routing messages to non-SMTP systems whenever possible. The primary reasons for this are that you can use queue management for messages, there is no need to manage file transfer to a Drop directory, and you can verify message delivery.

## Function and benefits of Delivery Agents

A delivery agent is a component installed in the Transport service of a Mailbox server that can perform the following tasks:

- Establish a connection to the foreign system for message delivery.

- Retrieve messages from the delivery queues on Mailbox servers.

- Deliver messages to the foreign system.

- Provide acknowledgement for each successful message delivery.

While the Foreign connector architecture remains in Microsoft Exchange Server 2013, we recommend using delivery agents for routing messages to non-SMTP systems whenever possible. Delivery agents provide the following benefits:

- They allow queue management of messages routed to foreign systems.

- Because the messages no longer need to be written to and read from the file system, message delivery performance is improved.

- They provide access to message properties with rich events for agent developers.

- Development time for a delivery agent is faster than implementing a Foreign connector because the delivery agent can use the message representation and management features of Exchange.

- You can verify that the messages are delivered to the foreign system, rather than simply written to the Drop directory.

- The use of Delivery Agent connectors allows service level agreement (SLA) analysis because it's possible to track the latency of message delivery to the foreign system.

## Adding Delivery Agents to your organization

To use a delivery agent in your organization, you have to complete the following:

1. Acquire the delivery agent. Typically, delivery agents are written by third parties. Exchange 2013 comes with only one Delivery Agent connector by default: the Text Messaging Delivery Agent connector.

2. Install the delivery agent in the Transport service of your Mailbox servers that will act as source servers for the Delivery Agent connectors.

3. Create a Delivery Agent connector for the specific protocol.

When all of these steps are completed, messages to the foreign systems will be routed through the Delivery Agent connectors and processed by the delivery agent.

[Microsoft.Exchange.Data.Transport.Delivery Namespace](https://docs.microsoft.com/previous-versions/office/exchange-server-api/aa564119(v=exchg.150)) provides more information about developing a delivery agent.

## Delivery Agent connectors

A Delivery Agent connector in Exchange 2013 is similar to the Delivery Agent connector introduced in Exchange 2010. They route messages addressed to foreign systems that do not use the SMTP protocol. When a message is routed to a Delivery Agent connector, the associated delivery agent performs the content conversion and message delivery. Typically, delivery agents are created by a third-party and configured to work with a Delivery Agent connector in your organization.

A Delivery Agent connector cannot be created in the Exchange admin center. Rather, you create a Delivery Agent connector in the Exchange Management Shell with the [New-DeliveryAgentConnector](https://docs.microsoft.com/powershell/module/exchange/New-DeliveryAgentConnector) cmdlet and edit the Delivery Agent connector's properties with [Set-DeliveryAgentConnector](https://docs.microsoft.com/powershell/module/exchange/Set-DeliveryAgentConnector). You can specify one or more host Mailbox servers for the connector, by using the optional *SourceTransportServers* parameter.

## Default text messaging Delivery Agent connector

You can use the Text Messaging Delivery Agent connector to route messages to mobile devices. On your Exchange server, run `Get-DeliveryAgentConnector | fl` to view the connector and all of its parameters. Note that the *DeliveryProtocol* is set to `MOBILE`.
