---
title: 'Exchange 2010 servers must be upgraded to Service Pack 3: Exchange 2013 Help'
TOCTitle: Exchange 2010 servers must be upgraded to Service Pack 3
ms:assetid: b4f74863-1567-4d6d-ae21-b0af495a1d82
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.e15e14coexistenceminversionrequirement(v=EXCHG.150)
ms:contentKeyID: 47899865
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2010 servers must be upgraded to Service Pack 3

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it detected one or more Microsoft Exchange Server 2010 servers haven't been upgraded to Service Pack 3 (SP3) for Exchange Server 2010. Before you can install Exchange 2013, all Exchange 2010 servers in your organization must be upgraded to Exchange 2010 SP3. This requirement includes Exchange 2010 Edge Transport servers. For more information, see [Upgrade from Exchange 2010 to Exchange 2013](upgrade-from-exchange-2010-to-exchange-2013-exchange-2013-help.md).

> [!IMPORTANT]
> After you upgrade your Exchange 2010 Edge Transport servers to Exchange 2010 SP3, you must re-create the Edge subscription between your Exchange organization and each Edge Transport server to update their server version in Active Directory. For more information about re-creating Edge subscriptions in Exchange 2010, see <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124096(v=exchg.141)">Managing Edge Subscriptions</A>.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
