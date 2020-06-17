---
localization_priority: Normal
description: Setup can't continue because the organization contains one or more Exchange 2013 servers that aren't running the minimum required version of Exchange.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.E16E15CoexistenceMinVersionRequirement
ms.author: v-mapenn
ms.assetid: 81658d0d-f132-4dbf-9cb6-467e3e3536df
ms.reviewer: 
title: Exchange 2013 CU10 or later is required for coexistence with Exchange 2016 or later [E16E15CoexistenceMinVersionRequirement]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange 2013 CU10 or later is required for coexistence with Exchange 2016 or later [E16E15CoexistenceMinVersionRequirement]

The installation of Exchange Server 2016 or later can't continue because Setup found one or more Exchange 2013 servers that aren't running the minimum required version of Exchange 2013. Before you can install Exchange 2016 or later in your organization, all Exchange 2013 servers in the forest need to be running Exchange 2013 Cumulative Update 10 (CU10) or later. This requirement includes Exchange 2013 Edge Transport servers.

> [!IMPORTANT]
> After you upgrade your Exchange 2013 Edge Transport servers to Exchange 2013 CU10 or later, you need to **re-create** the Edge subscription between your Exchange organization and each Edge Transport server (to update the Edge Transport server's Exchange version in Active Directory). For more information about re-creating Edge subscriptions in Exchange 2013, see [Manage Edge Subscriptions](https://docs.microsoft.com/Exchange/manage-edge-subscriptions-exchange-2013-help).
