---
localization_priority: Normal
description: You can disable the Voice Mail Preview feature for users associated with a Unified Messaging (UM) mailbox policy. Disabling this setting prevents users from receiving the text of a voice mail message in the message body of an email or text message. The default setting is enabled.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 362fed13-3a9c-4111-bfa4-8c45ab6a3a01
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable Voice Mail Preview for users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable Voice Mail Preview for users in Exchange Online

You can disable the Voice Mail Preview feature for users associated with a Unified Messaging (UM) mailbox policy. Disabling this setting prevents users from receiving the text of a voice mail message in the message body of an email or text message. The default setting is enabled.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to disable Voice Mail Preview

1. In the EAC, navigate to **Unified Messaging** \> **UM Dial plans**, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **General**, clear the check box next to **Allow voice mail preview**.

4. Click **Save**.

## Use Exchange Online PowerShell to disable Voice Mail Preview

This example prevents users who are associated with the UM mailbox policy `MyUMMailboxPolicy` from using the Voice Mail Preview feature.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy - AllowVoiceMailPreview $false
```
