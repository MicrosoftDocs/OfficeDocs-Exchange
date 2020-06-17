---
localization_priority: Normal
description: Learn how to use Exchange Online PowerShell to view or configure one or more call answering rules for a user.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: de6d9fa1-7878-49a9-bddb-e3317d94f4d8
ms.reviewer: 
f1.keywords:
- NOCSH
title: View and manage a call answering rule in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# View and manage a call answering rule in Exchange Online

You can use Exchange Online PowerShell to view or configure one or more call answering rules for a user. You can also use the **Get-UMCallAnsweringRule** or **Set-UMCallAnsweringRule** cmdlets in a PowerShell script to view or manage call answering rules for multiple users.

Call answering rules are applied to incoming calls similar to the way Inbox rules are applied to incoming email messages. By default, when a user is enabled for Unified Messaging (UM), no call answering rules are configured. Even so, incoming calls are answered by the mail system and callers are prompted to leave a voice message.

> [!IMPORTANT]
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

## Use Exchange Online PowerShell to view a call answering rule

You can retrieve the properties for a single call answering rule or a list of call answering rules in a UM-enabled user's mailbox.

This example returns a formatted list of call answering rules in a user's UM-enabled mailbox.

```PowerShell
Get-UMCallAnsweringRule-Mailbox tonysmith | Format-List
```

This example displays the properties of the call answering rule `MyUMCallAnsweringRule`.

```PowerShell
Get-UMCallAnsweringRule -Identity MyUMCallAnsweringRule
```

## Use Exchange Online PowerShell to configure a call answering rule

You can configure or change a call answering rule that's stored in a user's mailbox. You can specify the following conditions:

- Who the incoming call is from

- Time of day

- Calendar free/busy status

- Whether automatic replies are turned on for email

You can also specify the following actions:

- Find me

- Transfer the caller to someone else

- Leave a voice message

This example sets the priority to 2 on the call answering rule `MyCallAnsweringRule` that exists in the mailbox for Tony Smith.

```PowerShell
Set-UMCallAnsweringRule -Mailbox tonysmith -Name MyCallAnsweringRule -Priority 2
```

This example performs the following actions on the call answering rule `MyCallAnsweringRule` in the mailbox for Tony Smith:

- Sets the call answering rule to two caller IDs.

- Sets the priority of the call answering rule to 2.

- Sets the call answering rule to allow callers to interrupt the greeting.

```PowerShell
Set-UMCallAnsweringRule -Name MyCallAnsweringRule -CallerIds "1,4255550100,,","1,4255550123,," -Priority 2 -CallersCanInterruptGreeting $true -Mailbox tonysmith
```

This example changes the free/busy status to Away on the call answering rule `MyCallAnsweringRule` in the mailbox for Tony Smith and sets the priority to 2.

```PowerShell
Set-UMCallAnsweringRule -Name MyCallAnsweringRule -Priority 2 -Mailbox tonysmith@contoso.com -ScheduleStatus 0x8
```
