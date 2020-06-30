---
title: 'OAB server has been deleted_OffLineABServerDeleted: Exchange 2013 Help'
TOCTitle: OAB server has been deleted_OffLineABServerDeleted
ms:assetid: 38b5dacf-ef65-4b25-97f6-d8dec956d7d5
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.offlineabserverdeleted(v=EXCHG.150)
ms:contentKeyID: 46628863
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# OAB server has been deleted\_OffLineABServerDeleted

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because its attempt to install the Client Access server role failed. The Exchange Mailbox server designated to host the Offline Address Book (OAB) has been deleted.

Exchange 2007 setup requires that there be a valid Exchange Mailbox server hosting OAB generation for the Client Access server role installation to continue.

To address this issue, designate a valid Exchange Mailbox server as OAB generation host, and then rerun Exchange 2007 setup.

For information about how to designate an Exchange server to be the host for Offline Address Book generation, see "How to Create an Offline Address Book" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb124270(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb124270(v=exchg.80))) in the Exchange 2007 product documentation.
