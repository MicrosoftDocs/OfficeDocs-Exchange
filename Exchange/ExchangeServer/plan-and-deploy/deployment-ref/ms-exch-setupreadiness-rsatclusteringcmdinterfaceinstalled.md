---
localization_priority: Normal
description: Microsoft Exchange Server 2016 Setup can't continue because the local computer is missing a required Windows feature. You'll need to install this Windows feature before Exchange 2016 can continue.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.RsatClusteringCmdInterfaceInstalled
ms.author: chrisda
ms.assetid: 0d839514-5ab7-497d-8945-41392b4c3980
ms.date: 7/22/2015
ms.reviewer: 
title: Failover Cluster Command Interface Windows feature not installed [RsatClusteringCmdInterfaceInstalled]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Failover Cluster Command Interface Windows feature not installed [RsatClusteringCmdInterfaceInstalled]

Microsoft Exchange Server 2016 Setup can't continue because the local computer is missing a required Windows feature. You'll need to install this Windows feature before Exchange 2016 can continue.

Exchange 2016 Setup requires that the **Failover Cluster Command Interface** Windows feature be installed on the computer before installation can continue.

Do the following to install the Windows feature on this computer. If the feature requires a reboot to complete installation, you'll need to exit Exchange 2016 Setup, reboot, and then start Setup again.

> [!NOTE]
> Additional Windows features or updates might need to be installed before Exchange 2016 Setup can continue. For a complete list of required Windows features and updates, check out [Exchange Server prerequisites](../../plan-and-deploy/prerequisites.md).

1. Open Windows PowerShell on the local computer.

2. Run the following command to install the required Windows feature.

  ```
  Install-WindowsFeature RSAT-Clustering-CmdInterface
  ```

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
