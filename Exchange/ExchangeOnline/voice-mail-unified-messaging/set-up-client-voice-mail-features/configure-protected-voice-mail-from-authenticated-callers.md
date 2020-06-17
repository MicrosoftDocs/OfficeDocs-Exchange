---
localization_priority: Normal
description: 'You can configure Unified Messaging to answer an incoming call, and then determine whether it will apply protection to voice mail messages by using encryption. When a voice message is protected:'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f69e94a7-9768-4445-9ded-e78d732bd623
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure Protected Voice Mail from authenticated callers in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure Protected Voice Mail from authenticated callers in Exchange Online

You can configure Unified Messaging to answer an incoming call, and then determine whether it will apply protection to voice mail messages by using encryption. When a voice message is protected:

- The message is marked as Private in Microsoft Outlook and Outlook on the web (formerly known as Outlook Web App).

- The voice message can be opened only by the intended recipient of the voice message.

- The recipient can reply to the voice message, but can't forward it to someone who wasn't included on the original voice message.

This setting applies to voice messages sent to UM-enabled users when they don't answer their phone. This setting also applies when callers sign in to their mailbox using Outlook Voice Access, and then create and send a voice message.

For additional management tasks related to Protected Voice Mail procedures, see [Protected Voice Mail procedures](protected-voice-mail-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure Protected Voice Mail from authenticated callers

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Protected voice mail**, under **Protect voice message from authenticated callers**, select one of the following options:

   - **None**: Use this setting when you don't want protection applied to any voice messages sent to UM-enabled users.

   - **Private**: Use this setting when you want Unified Messaging to apply protection only to voice messages that have been marked as private by the caller.

   - **All**: Use this setting when you want Unified Messaging to apply protection to all voice messages, including those not marked as private.

4. Click **Save**.

## Use Exchange Online PowerShell to configure Protected Voice Mail from authenticated callers

This example protects voice messages from all authenticated callers on the UM mailbox policy `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy ProtectAuthenticatedVoiceMail -All
```
