---
localization_priority: Normal
description: Exchange Server 2016 or Exchange Server 2019 Setup can't continue because another Microsoft Exchange System Object container exists in Active Directory.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.AdInitErrorRule
ms.author: chrisda
ms.assetid: cd0f45ab-89de-4653-b50d-c1157c2329d5
ms.date: 8/2/2018
ms.reviewer: 
title: Duplicate Microsoft Exchange System Objects container exists in Active Directory [AdInitErrorRule]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Duplicate Microsoft Exchange System Objects container exists in Active Directory [AdInitErrorRule]

Exchange Setup can't continue because it found a duplicate Microsoft Exchange System Objects container in Active Directory Domain Naming context. When Setup finds a duplicate Microsoft Exchange System Objects container, you need to delete the duplicate container before Setup can continue. Note that running **DomainPrep** again won't fix the problem. You need to find and delete the duplicate Microsoft Exchange System Objects container.

To resolve this issue, do the following steps:

1. Open **Active Directory Users and Computers**. For example:

  - Press Windows key + R, enter **dsc.msc**, and then click **OK**.

  - In **Administrative Tools** \> **Active Directory Users and Computers**.

2. In the **Active Directory Users and Computers**, click **View** \> **Advanced Features**.

4. Locate the duplicate **Microsoft Exchange System Objects** container.

5. Verify that the duplicate **Microsoft Exchange System Objects** container doesn't contain valid Active Directory objects.

6. Right-click the duplicate **Microsoft Exchange System Objects container**, click **Delete**, and then click **Yes** in the confirmation dialog box.

> [!NOTE]
> To immediately replicate the change, you need to manually initiate replication between domain controllers.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
