---
localization_priority: Normal
description: You can configure a Voice Mail Preview partner on a Unified Messaging (UM) mailbox policy. After you've configured Voice Mail Preview partner settings, such as the Voice Mail Preview partner ID and Voice Mail Preview partner address, on a UM mailbox policy, the settings you configure will apply to all UM-enabled users who are linked with that mailbox policy.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7bb914ca-5502-4e64-bae5-555034138d8a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure Voice Mail Preview partner services for users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure Voice Mail Preview partner services for users in Exchange Online

You can configure a Voice Mail Preview partner on a Unified Messaging (UM) mailbox policy. After you've configured Voice Mail Preview partner settings, such as the Voice Mail Preview partner ID and Voice Mail Preview partner address, on a UM mailbox policy, the settings you configure will apply to all UM-enabled users who are linked with that mailbox policy.

> [!NOTE]
> You must use Exchange Online PowerShell to configure a Voice Mail Preview partner.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Sign up with a partner service

To find the list of certified partners and detailed instructions for how to sign up, see [Voice Mail Preview advisor](voice-mail-preview-advisor.md) or see the [Microsoft PinPoint](https://go.microsoft.com/fwlink/p/?LinkId=281966) website. After you've signed up, the Voice Mail Preview partner will provide you a partner ID and the SMTP address to use to forward the voice messages.

In Step 2, you'll apply the Partner ID and SMTP address you acquired in Step 1 to the required UM mailbox policies.

## Step 2: Set the Voice Mail Preview partner address and ID

This example sets the Voice Mail Preview partner address to exumvmp@fabrikam.com and the Voice Mail Preview partner ID to CON123-2010 on a UM mailbox policy named _MyUMMailboxPolicy_.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -VoiceMailPreviewPartnerAddress exumvmp@fabrikam.com
-VoiceMailPreviewPartnerAssignedID CON123-2010
```

## Step 3: Configure advanced Voice Mail Preview partner settings

If the partner requires custom settings, you may want to set two additional parameters for a Voice Mail Preview partner as follows:

- _VoiceMailPreviewPartnerMaxMessageDuration_

- _VoiceMailPreviewPartnerMaxDeliveryDelay_

This example sets the maximum message duration to 300 seconds (5 minutes) and the maximum delivery delay to 600 seconds (10 minutes) on a UM mailbox policy named _MyUMMailboxPolicy_.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -VoiceMailPreviewPartnerMaxMessageDuration 300 -VoiceMailPreviewPartnerMaxDeliveryDelay 600
```

## Step 4: Assign a UM-enabled user to the UM mailbox policy for a Voice Mail Preview partner

If you want to configure the Voice Mail Preview partner service for some, but not all, UM-enabled users in a UM dial plan, you must create a new UM mailbox policy and configure the partner settings. When you've finished, you can apply the new policy to selected UM-enabled users. For more information about how to assign a UM-enabled user to a UM mailbox policy, see the following topics:

- [Assign a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/assign-um-mailbox-policy.md)

- [Set-UMMailbox](https://docs.microsoft.com/powershell/module/exchange/set-ummailbox)

For more information about the Voice Mail Preview partner program, see [Voice Mail Preview advisor](voice-mail-preview-advisor.md).
