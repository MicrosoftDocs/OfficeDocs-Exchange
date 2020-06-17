---
localization_priority: Normal
description: You can use Exchange Online PowerShell to create one or more call answering rules for a user. You can also use the New-UMCallAnsweringRule cmdlet in a PowerShell script to create call answering rules for multiple users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 0976f8f2-3449-44f1-b0d1-20c91622e827
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create a call answering rule in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create a call answering rule in Exchange Online

You can use Exchange Online PowerShell to create one or more call answering rules for a user. You can also use the **New-UMCallAnsweringRule** cmdlet in a PowerShell script to create call answering rules for multiple users.

Call answering rules are applied to incoming calls similar to the way Inbox rules are applied to incoming email messages. By default, when a user is enabled for Unified Messaging (UM), no call answering rules are configured. Even so, incoming calls are answered by the mail system and callers are prompted to leave a voice message.

> [!NOTE]
> Users that are UM-enabled can sign in to Outlook on the web (formerly known as Outlook Web App)p to create, manage, and remove call answering rules.

For additional management tasks related to Call Answering Rules, see [Forwarding calls procedures](forwarding-calls-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- You can only use Exchange Online PowerShell to perform this procedure. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to create a call answering rule

This example creates the call answering rule `MyCallAnsweringRule` in the mailbox for Tony Smith with the priority of 2.

```PowerShell
New-UMCallAnsweringRule -Name MyCallAnsweringRule -Priority 2 -Mailbox tonysmith
```

This example creates the call answering rule `MyCallAnsweringRule` in the mailbox for Tony Smith and performs the following actions:

- Sets the call answering rule to two caller IDs.

- Sets the priority of the call answering rule to 2.

- Sets the call answering rule to allow callers to interrupt the greeting.

```PowerShell
New-UMCallAnsweringRule -Name MyCallAnsweringRule -CallerIds "1,4255550100,,","1,4255550123,," -Priority 2 -CallersCanInterruptGreeting $true -Mailbox tonysmith
```

This example creates the call answering rule `MyCallAnsweringRule` in the mailbox for Tony Smith and performs the following actions:

> Sets the priority of the call answering rule to 2.

> Creates key mappings for the call answering rule.

> If the caller reaches the voice mail for the user and the status of the user is set to Busy, the caller can:

    - Press the 1 key and be transferred to a receptionist at extension 45678.

    - Press the 2 key so the Find Me feature will be used for urgent issues, ring extension 23456 first, and then ring extension 45671.

```PowerShell
New-UMCallAnsweringRule -Name MyCallAnsweringRule -Priority 2 -Mailbox tonysmith -ScheduleStatus 0x4 - -KeyMappings "1,1,Receptionist,,,,,45678,","5,2,Urgent Issues,23456,23,45671,50,,"
```
