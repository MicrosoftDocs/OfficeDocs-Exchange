---
localization_priority: Normal
description: Learn about the benefits and requirements for MAPI over HTTP in Exchange Server 2016 and Exchange Server 2019.
ms.topic: conceptual
author: msdmaguire
ms.author: dmaguire
ms.assetid: 4663b5db-5b30-4a5a-a302-be6fef7fe5da
ms.date: 7/10/2018
ms.reviewer: 
title: MAPI over HTTP in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# MAPI over HTTP in Exchange Server

Messaging Application Programming Interface (MAPI) over HTTP is a transport protocol that improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function. This enables supported clients to change networks or resume from hibernation while maintaining the same server context.

Implementing MAPI over HTTP does not mean that it is the only protocol that can be used for Outlook to access Exchange. Outlook clients that are not MAPI over HTTP capable can still use Outlook Anywhere (RPC over HTTP) to access Exchange through a MAPI-enabled Client Access server.

In Exchange 2016 and Exchange 2019, MAPI over HTTP can be applied across your entire organization, or at the individual mailbox level.

## Benefits of MAPI over HTTP
MAPI over HTTP offers the following benefits to the clients that support it:

- Enables future innovation in authentication by using an HTTP based protocol.

- Provides faster reconnection times after a communications break because only TCP connections (not RPC connections) need to be rebuilt. Examples of a communication break include:

  - Device hibernation

  - Changing from a wired network to a wireless or cellular network

- Offers a session context that is not dependent on the connection. The server maintains the session context for a configurable period of time, even if the user changes networks.

## MAPI over HTTP when upgrading Exchange
In Exchange 2016 or later, MAPI over HTTP is enabled by default at the organization level, although you still need to configure the virtual directories as described in [Configure MAPI over HTTP](configure-mapi-over-http.md) for users to take advantage of it.

The scenarios where MAPI over HTTP is enabled or disabled by default at the organization level are described in the following table:

||**Exchange 2019**|**Exchange 2016**|
|:-----|:-----|:-----|
|**Upgrading from an Exchange 2016 environment**|MAPI over HTTP is enabled by default|n/a|
|**Upgrading from an environment that contains any Exchange 2013 servers**|MAPI over HTTP is disabled by default|MAPI over HTTP is disabled by default|
|**Upgrading from an Exchange 2010 environment**|n/a|MAPI over HTTP is enabled by default|

During the upgrade from an organization that contains Exchange 2013 servers, administrators will receive the [MAPI over HTTP isn't enabled [WarnMapiHttpNotEnabled]](../../plan-and-deploy/deployment-ref/ms-exch-setupreadiness-warnmapihttpnotenabled.md) readiness check warning, and enabling MAPI over HTTP post-installation is recommended. In any organization that contains Exchange 2013 servers, MAPI over HTTP won't be enabled by default, and administrators will need to follow the steps in [Configure MAPI over HTTP](configure-mapi-over-http.md) to enable it.

## Supportability and Prerequisites
Consider the following requirements to enable MAPI over HTTP.

### Supportability
Use the following matrix to verify that your clients and servers support MAPI over HTTP.

|**Product**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1**|**Exchange 2013 RTM**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|Outlook 2013 SP1 and all later versions of Outlook|MAPI over HTTP  <br/> Outlook Anywhere|MAPI over HTTP <br/> Outlook Anywhere|MAPI over HTTP  <br/> Outlook Anywhere|Outlook Anywhere|RPC <br/> Outlook Anywhere|
|Outlook 2010 SP2 with updates <br/> KB2956191 and KB2965295 (April 14, 2015)|MAPI over HTTP <br/> Outlook Anywhere|MAPI over HTTP <br/> Outlook Anywhere|MAPI over HTTP <br/> Outlook Anywhere|Outlook Anywhere|RPC <br/> Outlook Anywhere|
|Outlook 2013 RTM|Outlook Anywhere|Outlook Anywhere|Outlook Anywhere|Outlook Anywhere|RPC <br/> Outlook Anywhere|
|All earlier versions of Outlook|Outlook Anywhere|Outlook Anywhere|Outlook Anywhere|Outlook Anywhere|RPC <br/> Outlook Anywhere|

### Prerequisites

The following conditions are required for clients and servers to support MAPI over HTTP with Exchange Server. Once the following prerequisites are in place, see [Configure MAPI over HTTP](configure-mapi-over-http.md) to enable it in your organization.

- Supported Outlook clients (see the table in the previous section).

- .NET Framework 4.5.2 or later. Note that this is no longer an issue for Exchange 2016 CU5 or later. For more information about the .NET Framework requirements for Exchange 2016, see [.NET Framework](../../plan-and-deploy/system-requirements.md#net-framework).
