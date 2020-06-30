---
title: 'Insufficient permissions to remove all server roles'
TOCTitle: Insufficient permissions to remove all server roles_CannotUninstallDelegatedServer
ms:assetid: 214ae6f3-15e7-4337-99e8-40f9547c8e0c
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.cannotuninstalldelegatedserver(v=EXCHG.150)
ms:contentKeyID: 46628832
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Insufficient permissions to remove all server roles\_CannotUninstallDelegatedServer

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because the attempt to uninstall all server roles failed.

Exchange 2007 setup requires that the user who is logged on when uninstalling all server roles be a member of the Exchange Organization Administrators groups, or the Enterprise Admins group.

To resolve this issue, grant the logged-on user Exchange Organization Administrator permissions or enroll them in the Enterprise Admins group, or log on with an account that has those permissions and rerun Exchange 2007 setup.

For more information about Active Directory permissions needed with Microsoft Exchange, see "Working with Active Directory Permissions in Exchange Server" ([https://docs.microsoft.com/previous-versions/tn-archive/bb124223(v=exchg.65)](https://docs.microsoft.com/previous-versions/tn-archive/bb124223(v=exchg.65))).
