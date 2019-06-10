---
localization_priority: Normal
ms.topic: article
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.IsServerCoreInstalled
ms.author: chrisda
ms.assetid: 3d297c4f-7b5a-4faa-bf5e-320fe0529dfe
monikerRange: exchserver-2016
description: Exchange Server 2016 Setup can't continue because it detected that the local computer is running Windows Server Core or Windows Nano Server.
title: Windows Server Core or Windows Nano Server is installed [IsServerCoreInstalled]
ms.collection: exchange-server
audience: Developer
ms.date: 6/12/2018
ms.reviewer: 
manager: dansimp
ms.prod: exchange-server-it-pro

---

# Windows Server Core or Windows Nano Server is installed [IsServerCoreInstalled]

Microsoft Exchange Server 2016 Setup can't continue because it detected that the local computer is running Windows Server Core or Windows Nano Server. Exchange 2016 requires that **Windows Server with Desktop Experience** (Windows Server 2016) or **Windows Server with a GUI** (Windows Server 2012 and 2012R2) is installed on the local computer. Before you can install Exchange 2016, you need to do one of the following depending on the version of Windows Server you have installed:

- **Windows Server 2012 and Windows Server 2012 R2**: Run the following command in Windows PowerShell:

  ```
  Install-WindowsFeature Server-Gui-Mgmt-Infra, Server-Gui-Shell -Restart
  ```

- **Windows Server 2016**: Install Windows Server 2016 and choose the **Desktop Experience** installation option. If a computer is running Windows Server 2016 Core or Nano and you want to install Exchange 2016 on it, you'll need to reinstall the operating system and choose the **Desktop Experience** installation option.

For more information, see [Exchange Server system requirements](../../plan-and-deploy/system-requirements.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
