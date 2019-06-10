---
localization_priority: Normal
description: Microsoft Exchange Server 2016 Setup displayed this warning because there are servers running Exchange 2016 or later in this organization and MAPI over HTTP isn't enabled.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.WarnMapiHttpNotEnabled
ms.author: chrisda
ms.assetid: 8c7e2b60-2973-47b1-ac1c-3c3e96824aee
ms.date: 7/22/2015
ms.reviewer: 
title: MAPI over HTTP isn't enabled [WarnMapiHttpNotEnabled]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# MAPI over HTTP isn't enabled [WarnMapiHttpNotEnabled]

Microsoft Exchange Server 2016 Setup displayed this warning because there are servers running Exchange 2016 or later in this organization and MAPI over HTTP isn't enabled.

MAPI over HTTP is the preferred Outlook connectivity method when connecting to servers running Exchange 2016 or later. MAPI over HTTP improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function. This enables supported clients to change networks or resume from hibernation while maintaining the same server context.

Exchange Setup won't automatically enable MAPI over HTTP to avoid making unexpected changes to client connectivity. However, we recommend that you enable MAPI over HTTP as soon as possible to receive the benefits it provides.

For more information about MAPI over HTTP and how to enable it, see [MAPI over HTTP in Exchange Server](../../clients/mapi-over-http/mapi-over-http.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
