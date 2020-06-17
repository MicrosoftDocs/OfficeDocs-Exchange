---
localization_priority: Normal
description: The Limit on personal greetings (minutes) setting enables you to enter the maximum number of minutes that users associated with the Unified Messaging (UM) mailbox policy can use to record their voice mail greetings. This setting applies to both their standard voice mail and their Out of Office voice mail greetings. By default, the maximum greeting duration is set to 5 minutes. However, you can configure the maximum greeting duration to any setting between 1 and 10 minutes.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d400f250-0f55-45f5-9918-5f1d7819fbdf
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the limit on personal greetings for Outlook Voice Access users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the limit on personal greetings for Outlook Voice Access users in Exchange Online

The **Limit on personal greetings (minutes)** setting enables you to enter the maximum number of minutes that users associated with the Unified Messaging (UM) mailbox policy can use to record their voice mail greetings. This setting applies to both their standard voice mail and their Out of Office voice mail greetings. By default, the maximum greeting duration is set to 5 minutes. However, you can configure the maximum greeting duration to any setting between 1 and 10 minutes.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to change the maximum greeting duration

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM mailbox policy** page \> **General**, under **Limit on personal greetings (minutes)**, enter the length of time, in minutes, allowed for personal greetings for voice mail users.

4. Click **Save**.

## Use Exchange Online PowerShell to change the maximum greeting duration

This example configures the maximum greeting duration on the UM mailbox policy `MyUMMailboxPolicy` to 3 minutes.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy MaxGreetingDuration 3
```
