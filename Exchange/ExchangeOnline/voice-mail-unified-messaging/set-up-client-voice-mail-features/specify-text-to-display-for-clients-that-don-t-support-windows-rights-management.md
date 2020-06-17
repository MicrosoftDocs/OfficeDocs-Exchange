---
localization_priority: Normal
description: You can specify the text that will be sent to a user when they receive a protected voice message but their email client doesn't support Information Rights Management (IRM) or Windows Rights Management.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a9b2238a-b534-469c-a0c3-2768bc3d005b
ms.reviewer: 
f1.keywords:
- NOCSH
title: Specify the text to display for email clients that don't support Windows Rights Management in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Specify the text to display for email clients that don't support Windows Rights Management in Exchange Online

You can specify the text that will be sent to a user when they receive a protected voice message but their email client doesn't support Information Rights Management (IRM) or Windows Rights Management.

Protected Voice Mail can be accessed only by email clients that support Windows Rights Management or when a UM-enabled user uses Outlook Voice Access to access a protected voice message.

Protected Voice Mail is encrypted. When a voice message is protected:

- The message is marked as Private in Microsoft Outlook and Outlook on the web (formerly known as Outlook Web App).

- The voice message can be opened only by the intended recipient of the voice message.

- The recipient can reply to the voice message, but can't forward it to someone who wasn't included on the original voice message.

If a protected voice message is sent to someone whose email client doesn't support Windows Rights Management and isn't accessing the message using Outlook Voice Access, an email message will be sent to them that includes the text you specify. This text should include instructions about what the called party should do to be able to receive the protected voice message.

For additional management tasks related to Protected Voice Mail procedures, see [Protected Voice Mail procedures](protected-voice-mail-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use EAC to specify the text to display for email clients that don't support Windows Rights Management

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Protected voice mail**, under **Message to send to users who don't have Windows Rights Management support**, type the message text in the text box.

4. Click **Save**.

## Use Exchange Online PowerShell to specify the text to display for email clients that don't support Windows Rights Management

This example specifies the text to display to users associated with the UM mailbox policy named `MyUMMailboxPolicy` who have email clients that don't support Windows Rights Management.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -ProtectedVoiceMailText "Your email client software does not support Protected Voice Mail. Please contact the Help Desk."
```
