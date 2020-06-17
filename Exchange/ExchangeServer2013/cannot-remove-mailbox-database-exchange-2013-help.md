---
title: 'Cannot remove mailbox database: Exchange 2013 Help'
TOCTitle: Cannot remove mailbox database
ms:assetid: 5881e4c0-c2e2-48db-84b4-7f9ce3cf46a7
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.unwillingtoremovemailboxdatabase(v=EXCHG.150)
ms:contentKeyID: 46628948
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Cannot remove mailbox database

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 Setup can't continue because it can't remove a user mailbox database from the local server without incurring potential data loss.

Exchange 2013 Setup determines whether all mailbox databases have been removed from the server before the Mailbox server role is removed. However, user mailboxes might still remain on the server.

To resolve this issue, move any mailboxes on the server to another Exchange server or, if the mailboxes and the data contained within them are no longer required, disable the mailboxes. Then run Exchange 2013 Setup again.

  - For more information about how to identify a mailbox in the database, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox).

  - For more information about how to move a mailbox, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

  - For more information about how to disable a mailbox, see [Disable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Disable-Mailbox).

  - For more information about how to remove a mailbox database, see [Manage mailbox databases in Exchange 2013](manage-mailbox-databases-in-exchange-2013-exchange-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
