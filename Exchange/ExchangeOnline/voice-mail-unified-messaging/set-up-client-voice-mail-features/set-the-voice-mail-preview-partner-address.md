---
localization_priority: Normal
description: You can set a Voice Mail Preview partner address on a Unified Messaging (UM) mailbox policy. After you've set the Voice Mail Preview partner address on a UM mailbox policy, the setting will apply to all UM-enabled users who are linked with that mailbox policy.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 57fbed1e-1b14-4939-95e6-ef7c072f32a9
ms.reviewer: 
f1.keywords:
- NOCSH
title: Set the Voice Mail Preview partner address in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Set the Voice Mail Preview partner address in Exchange Online

You can set a Voice Mail Preview partner address on a Unified Messaging (UM) mailbox policy. After you've set the Voice Mail Preview partner address on a UM mailbox policy, the setting will apply to all UM-enabled users who are linked with that mailbox policy.

> [!NOTE]
> You must use Exchange Online PowerShell to set a Voice Mail Preview partner address.

For more information about the Voice Mail Preview partner program, see [Voice Mail Preview advisor](voice-mail-preview-advisor.md).

For additional management tasks related to Voice Mail Preview, see [Voice Mail Preview procedures](voice-mail-preview-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to set the Voice Mail Preview partner address on a UM mailbox policy

This example sets the Voice Mail Preview partner address to exumvmp@fabrikam.com on a UM mailbox policy named _MyUMMailboxPolicy_.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -VoiceMailPreviewPartnerAddress exumvmp@fabrikam.com
```
