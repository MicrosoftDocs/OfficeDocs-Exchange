---
localization_priority: Normal
description: When a user's mailbox is enabled for Unified Messaging (UM) voice mail, an email message is sent that welcomes the user to Unified Messaging. This message contains the PIN information the user will use to first access the voice mail system.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3e8292fb-0cdb-445d-8048-a59af7c38d63
ms.reviewer: 
f1.keywords:
- NOCSH
title: Include text with the email message sent when a user Is enabled for voice mail in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Include text with the email message sent when a user Is enabled for voice mail in Exchange Online

When a user's mailbox is enabled for Unified Messaging (UM) voice mail, an email message is sent that welcomes the user to Unified Messaging. This message contains the PIN information the user will use to first access the voice mail system.

You can customize the text that's sent in the welcome email message by adding text in the **When a user is enabled for Unified Messaging** box on a UM mailbox policy. You can include such information as the UM technical support telephone numbers or additional Outlook Voice Access numbers. After you add the text, it will be included in the email message sent when users associated with the UM mailbox policy are enabled for Unified Messaging.

> [!NOTE]
> The custom text you add to the welcome message is limited to 512 characters, and it can include simple HTML text.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to customize the text sent when a mailbox is enabled for Unified Messaging

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Message text**, in the text box for **When a user is enabled for Unified Messaging**, enter the text you want to include in the email message that's sent when users are enabled for Unified Messaging voice mail.

4. Click **Save**.

## Use Exchange Online PowerShell to customize the text sent when a mailbox is enabled for Unified Messaging

This example enables UM-enabled users who are associated with a UM mailbox policy to receive additional instructions about UM and the Outlook Voice Access number that they can use to access their mailbox over a phone.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -UMEnabledText "You've been enabled for Unified Messaging voice mail. To access your Exchange mailbox, call your internal telephone extension number. From outside your office, call 425-555-1234."
```
