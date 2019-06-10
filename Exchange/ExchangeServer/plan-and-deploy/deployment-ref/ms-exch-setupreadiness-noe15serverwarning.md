---
localization_priority: Normal
description: Microsoft Exchange Server 2016 Setup displayed this warning because no Exchange Server 2013 server roles exist in the organization.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.NoE15ServerWarning
ms.author: chrisda
ms.assetid: 55274feb-e683-4447-a053-8650ef174667
ms.date: 6/12/2018
ms.reviewer: 
title: No Exchange 2013 servers detected [NoE15ServerWarning]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# No Exchange 2013 servers detected [NoE15ServerWarning]

Microsoft Exchange Server 2016 Setup displayed this warning because no Exchange Server 2013 server roles exist in the organization.

> [!CAUTION]
> If you continue with Exchange Server 2016 installation, you won't be able to add Exchange 2013 servers to the organization at a future date.

Before deploying Exchange 2016, consider the following factors that may require you to deploy Exchange 2013 servers prior to deploying Exchange 2016:

- **Third-party or in-house developed applications**: Applications developed for earlier versions of Exchange may not be compatible with Exchange 2016. You may need to maintain Exchange 2013 servers to support these applications.

- **Coexistence or migration requirements**: If you plan on migrating mailboxes into your organization, some solutions may require the use of Exchange 2013 servers.

If you decide that you need to deploy Exchange 2013 servers, you must do so before you deploy Exchange 2016. Active Directory must be prepared for each Exchange version in the following order:

1. Exchange 2013

2. Exchange 2016

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
