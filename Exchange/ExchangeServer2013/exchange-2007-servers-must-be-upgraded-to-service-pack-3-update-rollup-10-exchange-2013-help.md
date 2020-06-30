---
title: 'Exchange 2007 servers must be upgraded to Service Pack 3, Update Rollup 10'
TOCTitle: Exchange 2007 servers must be upgraded to Service Pack 3, Update Rollup 10
ms:assetid: b8028a00-c451-412e-86f2-1669f6eee8fc
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.e15e12coexistenceminversionrequirement(v=EXCHG.150)
ms:contentKeyID: 47899847
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2007 servers must be upgraded to Service Pack 3, Update Rollup 10

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it detected one or more Exchange Server 2007 servers haven't been upgraded to Exchange Server 2007 Service Pack 3 (SP3) Roll Up 10 (RU10). Before you can install Exchange 2013, all Exchange 2007 servers in your organization must be upgraded to Exchange 2007 SP3 RU10. This requirement includes Exchange 2007 Edge Transport servers. For more information, see [Upgrade from Exchange 2007 to Exchange 2013](upgrade-from-exchange-2007-to-exchange-2013-exchange-2013-help.md).

> [!IMPORTANT]
> After you upgrade your Exchange 2007 Edge Transport servers to Exchange 2007 SP3 RU10, you must re-create the Edge subscription between your Exchange organization and each Edge Transport server to update their server version in Active Directory. For more information about re-creating Edge subscriptions in Exchange 2007, see <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125236(v=exchg.80)">Subscribing the Edge Transport Server to the Exchange Organization</A>.
