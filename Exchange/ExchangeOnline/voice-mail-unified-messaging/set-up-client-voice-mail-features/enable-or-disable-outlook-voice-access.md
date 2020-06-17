---
localization_priority: Normal
description: You can enable or disable access to Outlook Voice Access for UM-enabled users who are associated with a Unified Messaging (UM) mailbox policy. Outlook Voice Access is a feature used by UM-enabled users to access their mailbox over a phone. By default, this setting is enabled.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c0c244a0-ad2f-4adf-bc1f-1d55fd7ea2d5
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable Outlook Voice Access for users in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable Outlook Voice Access for users in Exchange Online

You can enable or disable access to Outlook Voice Access for UM-enabled users who are associated with a Unified Messaging (UM) mailbox policy. Outlook Voice Access is a feature used by UM-enabled users to access their mailbox over a phone. By default, this setting is enabled.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or disable Outlook Voice Access

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. Under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, select or clear the check box next to **Allow Outlook Voice Access**.

4. Click **Save**.

## Use Exchange Online PowerShell to enable or disable Outlook Voice Access

This example allows users who are associated with the UM mailbox policy `MyUMMailboxPolicy` to use Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowSubscriberAccess $true
```

This example prevents users who are associated with the UM mailbox policy `MyUMMailboxPolicy` from using Outlook Voice Access.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowSubscriberAccess $false
```
