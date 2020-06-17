---
localization_priority: Normal
description: Exchange Server 2016 or Exchange 2019 Setup can't remove the Mailbox server role from the server because the server contains active mailboxes.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.UnwillingToRemoveMailboxDatabase
ms.author: v-mapenn
ms.assetid: 5881e4c0-c2e2-48db-84b4-7f9ce3cf46a7
ms.reviewer:
title: Cannot remove mailbox database [UnwillingToRemoveMailboxDatabase]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Cannot remove mailbox database [UnwillingToRemoveMailboxDatabase]

Exchange Setup can't continue because it can't remove a user mailbox database from the local server without incurring potential data loss.

Before Exchange Setup removes the Mailbox server role from a server, Setup confirms that all mailbox databases have been removed from the server, or that the mailboxes on the server don't contain active mailboxes. However, user mailboxes might still remain on the server.

To resolve this issue do either of these steps:

- To preserve the mailboxes and their content, move the mailboxes to another server. For instructions, see [Mailbox moves in Exchange Server](../../recipients/mailbox-moves.md).

- Disable the mailboxes if they're no longer required. For more information, see [Disable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/disable-mailbox).

- Remove the mailbox databases if they're no longer required. For instructions, see [Manage mailbox databases in Exchange Server](../../architecture/mailbox-servers/manage-databases.md).

After you deal with the mailbox databases on the server, run Exchange Setup again.

- For more information about how to identify a mailbox in the database, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
