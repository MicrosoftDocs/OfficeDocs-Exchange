---
title: 'Setup failure occurred while uninstalling a server role'
TOCTitle: Setup failure occurred while uninstalling a server role_InterruptedUninstallNotContinued
ms:assetid: 187967b2-cb28-45d7-8858-2a083c1ebe58
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.interrupteduninstallnotcontinued(v=EXCHG.150)
ms:contentKeyID: 46628824
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Setup failure occurred while uninstalling a server role\_InterruptedUninstallNotContinued

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Exchange Server 2010 setup cannot continue because a previous setup failure occurred while uninstalling a server role.

Exchange 2010 setup requires that a failed server role uninstall be successfully uninstalled before any other setup task can continue.

To address this issue, uninstall the failed server role(s), and then rerun setup.

For more information about how to remove a server role, see the Exchange 2007 product documentation topic, [How to Remove Exchange 2007 Server Roles](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb124115(v=exchg.80)).
