---
localization_priority: Normal
description: You can force users who receive protected voice mail messages to use the Play on Phone feature to listen to their messages. Or, if the client software doesn't support rights management, users must use Outlook Voice Access to listen to messages.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3c33370c-4262-42b1-8d83-d61fc7c426cd
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable multimedia playback of protected voice messages in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable multimedia playback of protected voice messages in Exchange Online

You can force users who receive protected voice mail messages to use the Play on Phone feature to listen to their messages. Or, if the client software doesn't support rights management, users must use Outlook Voice Access to listen to messages.

To listen to voice messages, Unified Messaging (UM)-enabled users can use the Play on Phone feature or use multimedia software on a computer or mobile device. Multimedia playback allows a UM-enabled user to use a media player over computer speakers or use a media player on a mobile device to hear the voice message.

> [!NOTE]
> Protected voice mail is available only on clients that are using a version of Outlook that supports rights management. If the client software doesn't support rights management, users must use Outlook Voice Access to listen to their calls.

By default, the value of the **RequireProtectedPlayOnPhone** property on a UM mailbox policy is set to false. This means that UM-enabled users that are associated with that UM mailbox policy can listen to protected voice messages by:

- Using Outlook Voice Access.

- Using the built-in media player or the Play on Phone button in Outlook 2010 or a later version.

- Using the built-in media player or the Play on Phone button in Outlook on the web (formerly known as Outlook Web App).

If this value is set to true, multimedia playback of protected voice mail isn't allowed. UM-enabled users associated with a UM mailbox policy on which this value is set to true can listen to protected voice messages only by:

- Using Outlook Voice Access.

- Using the Play on Phone button in Outlook 2010 or a later version.

- Using the Play on Phone button in Outlook on the web.

This setting is especially useful when UM-enabled users use public computers, laptops in public places, or their mobile device's media player to listen to protected voice mail that can contain private information.

For additional management tasks related to Protected Voice Mail procedures, see [Protected Voice Mail procedures](protected-voice-mail-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or disable multimedia playback of protected voice messages

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. Under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Protected voice mail**, select the check box next to **Require Play on Phone for protected voice messages** to enable this setting. Clear the check box to disable this setting.

4. Click **Save**.

## Use Exchange Online PowerShell to enable or disable multimedia playback of protected voice messages

This example allows users who are associated with the UM mailbox policy named `MyUMMailboxPolicy` to play back protected voice messages using a media player.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -RequireProtectedPlayOnPhone $false
```

This example prevents users who are associated with the UM mailbox policy named `MyUMMailboxPolicy` from playing back protected voice messages using a media player.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -RequireProtectedPlayOnPhone $true
```
