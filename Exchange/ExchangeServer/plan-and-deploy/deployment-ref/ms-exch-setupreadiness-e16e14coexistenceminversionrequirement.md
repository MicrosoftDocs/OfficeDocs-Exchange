---
localization_priority: Normal
monikerRange: exchserver-2016
description: Setup can't continue because the organization contains one or more Exchange 2010 servers that aren't running the minimum required version of Exchange.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.E16E14CoexistenceMinVersionRequirement
ms.author: v-mapenn
ms.assetid: 24d16ace-249f-4c74-b617-3b0242e5aeca
ms.reviewer: 
title: Exchange 2010 SP3 RU11 or later is required for coexistence with Exchange 2016 [E16E14CoexistenceMinVersionRequirement]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange 2010 SP3 RU11 or later is required for coexistence with Exchange 2016 [E16E14CoexistenceMinVersionRequirement]

The installation of Exchange Server 2016 can't continue because Setup found one or more Exchange 2010 servers that aren't running the minimum required version of Exchange 2010. Before you can install Exchange 2016 in your organization, all Exchange 2010 servers in the forest need to be running Exchange 2010 Service Pack 3 (SP3) and Update Rollup 11 (RU11) or later. This requirement includes Exchange 2010 Edge Transport servers.

> [!IMPORTANT]
> After you upgrade your Exchange 2010 Edge Transport servers to Exchange 2010 SP3 RU11 or later, you need to **re-create** the Edge subscription between your Exchange organization and each Edge Transport server (to update the Edge Transport server's Exchange version in Active Directory). For more information about re-creating Edge subscriptions in Exchange 2010, see [Managing Edge Subscriptions](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124096(v=exchg.141)).
