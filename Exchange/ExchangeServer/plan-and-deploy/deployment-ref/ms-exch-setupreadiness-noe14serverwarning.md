---
localization_priority: Normal
monikerRange: exchserver-2016
description: Microsoft Exchange Server 2016 Setup displayed this warning because no Exchange 2010 servers exist in the organization.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.NoE14ServerWarning
ms.author: chrisda
ms.assetid: 789cabab-c769-4a16-a6c8-3db82cff8861
ms.date: 6/12/2018
ms.reviewer: 
title: No Exchange 2010 servers detected [NoE14ServerWarning]
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# No Exchange 2010 servers detected [NoE14ServerWarning]

Microsoft Exchange Server 2016 Setup displayed this warning because no Exchange 2010 servers exist in the organization.

> [!CAUTION]
> If you continue with Exchange Server 2016 installation, you won't be able to add Exchange 2010 servers to the organization in the future.

Before deploying Exchange 2016, consider the following factors that may require you to deploy Exchange 2010 servers prior to deploying Exchange 2016:

- **Third-party or in-house developed applications**: Applications developed for earlier versions of Exchange may not be compatible with Exchange 2016. You may need to maintain Exchange 2010 servers to support these applications.

- **Coexistence or migration requirements**: If you plan on migrating mailboxes into your organization, some solutions may require the use of Exchange 2010 servers.

If you decide that you need to deploy Exchange 2010 servers, you need to do so before you deploy Exchange 2016. You need to prepare Active Directory for each Exchange version in the following order:

1. Exchange 2010

2. Exchange 2013 (only required if you're planning to deploy Exchange 2013 at a later date)

3. Exchange 2016

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
