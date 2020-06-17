---
localization_priority: Normal
description: You can use Exchange Online PowerShell to remove one or more call answering rules for a user. You can also use the Remove-UMCallAnsweringRule cmdlet in a PowerShell script to remove one or more call answering rules for multiple users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 1da3c5bc-7227-4b37-96f6-67ceefc084d5
ms.reviewer: 
f1.keywords:
- NOCSH
title: Remove a call answering rule for a user in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Remove a call answering rule for a user in Exchange Online

You can use Exchange Online PowerShell to remove one or more call answering rules for a user. You can also use the **Remove-UMCallAnsweringRule** cmdlet in a PowerShell script to remove one or more call answering rules for multiple users.

Call answering rules are applied to incoming calls similar to the way Inbox rules are applied to incoming email messages. By default, when a user is enabled for Unified Messaging (UM), no call answering rules are configured. Even so, incoming calls are answered by the mail system and callers are prompted to leave a voice message.

> [!NOTE]
> Users that are UM-enabled can sign in to Outlook on the web (formerly known as Outlook Web App) to create, manage, and remove call answering rules.

For additional management tasks related to Call Answering Rules, see [Forwarding calls procedures](forwarding-calls-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- You can only use Exchange Online PowerShell to perform this procedure. To learn how to connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554)..

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to remove a call answering rule

This example removes the call answering rule `MyUMCallAnsweringRule` from a user's mailbox. The user's mailbox is the mailbox of the user running the cmdlet.

```PowerShell
Remove-UMCallAnsweringRule -Identity MyUMCallAnsweringRule
```

This example removes the call answering rule `MyUMCallAnsweringRule` from the mailbox of Tony Smith.

```PowerShell
Remove-UMCallAnsweringRule -Identity MyUMCallAnsweringRule -Mailbox tonysmith
```
