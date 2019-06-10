---
localization_priority: Normal
description: 'Summary: Learn how connectors are used in Exchange Server 2016 or Exchange Server 2019 for incoming and outgoing mail flow in your organization.'
ms.topic: hub-page
author: chrisda
ms.author: chrisda
ms.assetid: 73559b0c-fc0e-41fd-84df-d07442137a0c
ms.date:
ms.reviewer: 
title: Connectors, Exchange connector, Exchange send connector, Exchange receive connector
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Connectors

Exchange uses connectors to enable incoming and outgoing mail flow on Exchange servers, and also between services in the transport pipeline on the local Exchange server.

These are the types of connectors that are available in Exchange.

****

|**Connector**|**Description**|
|:-----|:-----|
|Receive connectors|Receive connectors control incoming SMTP mail flow. They listen for incoming connections that match the configuration of the connector. Multiple default Receive connectors are created when you install Exchange.  <br/> For more information, see [Receive connectors](receive-connectors.md).|
|Send connectors|Send connectors control outgoing SMTP mail flow. A Send connector is chosen based on the message recipients and the configuration of the connector. No default Send connectors for external mail flow are created when you install Exchange, but implicit and invisible Send connectors exist, and are used to route mail between internal Exchange servers.  <br/> For more information, see [Receive connectors](receive-connectors.md).|
|Delivery agents and Delivery Agent Connectors|Delivery agents and Delivery Agent connectors control outgoing mail flow to non-SMTP systems. Outgoing messages are put into message queues for delivery to the non-SMTP system. Delivery agents and Delivery agent connectors are preferred over Foreign connectors due to their improved performance and management.  <br/> For more information, see [Delivery Agents and Delivery Agent Connectors](http://technet.microsoft.com/library/38c942ee-b59d-47ec-87eb-bebad441ada5.aspx).|
|Foreign connectors|Foreign connectors control outgoing mail flow to non-SMTP systems. Outgoing messages are written to files in a location called the Drop directory to be picked up by the non-SMTP system.  <br/> For information, see [Foreign Connectors](http://technet.microsoft.com/library/21c6a7a9-f4d2-4359-9ac9-930701b63a4e.aspx).|
