---
localization_priority: Normal
description: You can include additional text in the email message that's sent when a voice mail message is received by a user who is enabled for Unified Messaging (UM) voice mail. By default, the text that's included with a voice message indicates only that the user has received a voice message. However, you can create a custom message by adding text in the When a user receives a voice message box on a UM mailbox policy. For example, the text can include information about system security policies and describe the correct way to handle voice messages in your organization. After you add the text, it will be included in each email message that's sent when UM-enabled users associated with the UM mailbox policy receive a voice message.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b2eec29c-e5eb-4263-80d8-0b9813dd56dc
ms.reviewer: 
f1.keywords:
- NOCSH
title: Include text with the email message sent when a voice message Is received in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Include text with the email message sent when a voice message Is received in Exchange Online

You can include additional text in the email message that's sent when a voice mail message is received by a user who is enabled for Unified Messaging (UM) voice mail. By default, the text that's included with a voice message indicates only that the user has received a voice message. However, you can create a custom message by adding text in the **When a user receives a voice message** box on a UM mailbox policy. For example, the text can include information about system security policies and describe the correct way to handle voice messages in your organization. After you add the text, it will be included in each email message that's sent when UM-enabled users associated with the UM mailbox policy receive a voice message.

> [!NOTE]
> The custom text that accompanies a voice message is limited to 512 characters, and can include simple HTML text.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to change the text included with a voice message

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Message text**, in the text box for **When a user receives a voice message**, enter the text you want to include in the email message that's sent when users receive a voice message.

4. Click **Save**.

## Use Exchange Online PowerShell to change the text included with a voice message

This example includes the additional text, "Do not forward voice messages to users outside this organization", with voice messages sent to users who are associated with the UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -VoiceMailText "Do not forward voice messages to users outside this organization."
```
