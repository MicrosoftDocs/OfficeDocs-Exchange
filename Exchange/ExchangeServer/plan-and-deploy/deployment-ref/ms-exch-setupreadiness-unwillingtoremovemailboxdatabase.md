---
localization_priority: Normal
description: Exchange Server 2016 or Exchange 2019 Setup can't remove the Mailbox server role from the server because the server contains active mailboxes.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.UnwillingToRemoveMailboxDatabase
ms.author: chrisda
ms.assetid: 5881e4c0-c2e2-48db-84b4-7f9ce3cf46a7
ms.date: 8/2/2018
ms.reviewer: 
title: Cannot remove mailbox database [UnwillingToRemoveMailboxDatabase]
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Cannot remove mailbox database [UnwillingToRemoveMailboxDatabase]

Exchange Setup can't continue because it can't remove a user mailbox database from the local server without incurring potential data loss.

Before Exchange Setup removes the Mailbox server role from a server, Setup confirms that alll mailbox databases have been removed from the server, or that the mailboxes on the server don't contain active mailboxes. However, user mailboxes might still remain on the server.

To resolve this issue do either of these steps:

- To preserve the mailboxes and their content, move the mailboxes to another server. For instructions, see [Mailbox moves in Exchange Server](../../recipients/mailbox-moves.md).

- Disable the mailboxes if they're no longer required. For more information, see [Disable-Mailbox](http://technet.microsoft.com/library/33be55a3-1880-437d-a631-c1cca1736421.aspx).

- Remove the mailbox databases if they're no longer required. For instructions, see [Manage mailbox databases in Exchange Server](../../architecture/mailbox-servers/manage-databases.md).

After you deal with the mailbox databases on the server, run Exchange Setup again.

- For more information about how to identify a mailbox in the database, see [Get-Mailbox](http://technet.microsoft.com/library/8a5a6eb9-4a75-47f9-ae3b-a3ba251cf9a8.aspx).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
