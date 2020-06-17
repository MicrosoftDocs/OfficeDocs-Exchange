---
localization_priority: Normal
description: Microsoft Exchange Server 2016 Setup has detected that the computer you're attempting to install Exchange 2016 on doesn't have a recommended Windows update installed. We strongly recommend that you install this Windows update before installing Exchange 2016 to avoid any issues resolved by the update.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.Win2k12RefsUpdateNotInstalled
ms.author: v-mapenn
ms.assetid: 50b23c15-53b8-46ee-a334-94534bd76e2c
ms.reviewer: 
title: Running 'dir' on an ReFS-formatted disk could cause the computer to freeze [Win2k12RefsUpdateNotInstalled]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Running "dir" on an ReFS-formatted disk could cause the computer to freeze [Win2k12RefsUpdateNotInstalled]

Microsoft Exchange Server 2016 Setup has detected that the computer you're attempting to install Exchange 2016 on doesn't have a recommended Windows update installed. We strongly recommend that you install this Windows update before installing Exchange 2016 to avoid any issues resolved by the update.

Computers running Windows Server 2012 and later support the Resilient File System (ReFS). An issue exists that could cause computers to freeze when the "**dir**" command is run on disks formatted with ReFS.

Download and install the 64-bit update from the following URL, and then click **retry** on the **Readiness Checks** page.

> [!NOTE]
> If this update requires a reboot to complete installation, you'll need to exit Exchange 2016 Setup, reboot, and then start Setup again.

Microsoft Knowledge Base article KB2894875, [Windows 8-based or Windows Server 2012-based computer freezes when you run the "dir" command on an ReFS volume](https://support.microsoft.com/help/2894875).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
