---
title: 'Remove a call answering rule for a user: Exchange 2013 Help'
TOCTitle: Remove a call answering rule for a user
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 1da3c5bc-7227-4b37-96f6-67ceefc084d5
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a call answering rule for a user in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can use the Shell to remove one or more call answering rules for a user. You can also use the **Remove-UMCallAnsweringRule** cmdlet in an Exchange Management Shell script to remove one or more call answering rules for multiple users.

Call answering rules are applied to incoming calls similar to the way Inbox rules are applied to incoming email messages. By default, when a user is enabled for Unified Messaging (UM), no call answering rules are configured. Even so, incoming calls are answered by the mail system and callers are prompted to leave a voice message.

> [!NOTE]
> Users that are UM-enabled can sign in to Outlook Web App to create, manage, and remove call answering rules.

For additional management tasks related to Call Answering Rules, see [Forwarding calls procedures](forwarding-calls-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM call answering rules" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- You can only use the Shell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to remove a call answering rule

This example removes the call answering rule `MyUMCallAnsweringRule` from a user's mailbox. The user's mailbox is the mailbox of the user running the cmdlet.

```powershell
Remove-UMCallAnsweringRule -Identity MyUMCallAnsweringRule
```

This example removes the call answering rule `MyUMCallAnsweringRule` from the mailbox of Tony Smith.

```powershell
Remove-UMCallAnsweringRule -Identity MyUMCallAnsweringRule -Mailbox tonysmith
```
