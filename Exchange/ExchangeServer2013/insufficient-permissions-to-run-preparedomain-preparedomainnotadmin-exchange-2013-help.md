---
title: 'Insufficient permissions to run /PrepareDomain'
TOCTitle: Insufficient permissions to run /PrepareDomain_PrepareDomainNotAdmin
ms:assetid: c33a2bc0-5b07-49b8-a1c1-53baa4933d44
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.preparedomainnotadmin(v=EXCHG.150)
ms:contentKeyID: 46629108
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Insufficient permissions to run /PrepareDomain\_PrepareDomainNotAdmin

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because the attempt to run the **/PrepareDomain** process failed. The logged on user has insufficient permissions to perform the **/PrepareDomain** process.

Exchange 2007 setup requires that the user who is logged on when running the **/PrepareDomain** process be a member of the Domain Admins group for the domain to be prepared, and a member of the Enterprise Admins group.

To resolve this issue, grant the logged-on user Domain Admins group permissions for the domain being prepared and enroll them in the Enterprise Admins groups, or log on with an account that has those permissions and rerun Exchange 2007 setup.

For more information about Active Directory permissions that are needed with Microsoft Exchange, see "Working with Active Directory Permissions in Exchange Server" ([https://docs.microsoft.com/previous-versions/tn-archive/bb124223(v=exchg.65)](https://docs.microsoft.com/previous-versions/tn-archive/bb124223(v=exchg.65))).
