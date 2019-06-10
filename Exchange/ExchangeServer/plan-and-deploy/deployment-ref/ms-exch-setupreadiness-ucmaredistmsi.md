---
localization_priority: Normal
description: Exchange Server 2016 Setup can't continue because the Unified Communications Managed API 4.0 Runtime update is required on servers before you install the Mailbox server role.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.UcmaRedistMsi
ms.author: chrisda
ms.assetid: b26b628b-116d-4f13-ab86-bac80e2a2e1f
ms.date: 8/2/2018
ms.reviewer: 
title: UCMA 4.0, Core Runtime not installed [UcmaRedistMsi]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# UCMA 4.0, Core Runtime not installed [UcmaRedistMsi]

Exchange 2016 Setup requires the Unified Communications Managed API 4.0 Runtime for Unified Messaging (UM) services on the Mailbox server role. You need to install this update before Exchange 2016 Setup can continue.

Download and install the 64-bit update from [Unified Communications Managed API 4.0 Runtime](https://go.microsoft.com/fwlink/p/?linkId=258269), and then click **Retry** on the **Readiness Checks** page in the Exchange 2016 Setup wizard.

> [!NOTE]
> If the installation of this update requires a reboot, you'll need to exit Exchange 2016 Setup, reboot, and then start Setup again.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
