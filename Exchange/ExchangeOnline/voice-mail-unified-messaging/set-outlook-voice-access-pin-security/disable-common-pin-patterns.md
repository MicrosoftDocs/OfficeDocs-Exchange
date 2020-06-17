---
localization_priority: Normal
description: You can enable or disable common Unified Messaging (UM) PIN patterns for Outlook Voice Access users. If you enable or disable the common PIN patterns setting on a UM mailbox policy, the setting will apply to all UM-enabled users associated with the UM mailbox policy. By default, UM-enabled users can't use common patterns when they create a PIN.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: eecc40ae-fac7-41e4-a1e1-16330f4462a3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Disable common PIN patterns for voice mail in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Disable common PIN patterns for voice mail in Exchange Online

You can enable or disable common Unified Messaging (UM) PIN patterns for Outlook Voice Access users. If you enable or disable the common PIN patterns setting on a UM mailbox policy, the setting will apply to all UM-enabled users associated with the UM mailbox policy. By default, UM-enabled users can't use common patterns when they create a PIN.

You can configure several PIN-related settings on a UM mailbox policy. The **Allow Common PIN Patterns** setting is used to allow or prevent the use of common number patterns when users create a PIN. By default, this setting is disabled and prevents users from using the following number patterns:

- **Sequential numbers**: These are PIN values that include only consecutive numbers. Examples of consecutive numbers for a PIN are 1234 and 65432.

- **Repeated numbers**: These are PIN values that include only repeated numbers. Examples of repeated numbers are 11111 and 22222.

- **Suffix of mailbox extension**: These are PIN values that include the suffix of a user's mailbox extension. For example, if a user's mailbox extension is 36697, the user's PIN cannot be 3669712.

> [!NOTE]
> If the **Allow Common PIN Patterns** setting is enabled, only the suffix of the mailbox extension will be rejected.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to disable common PIN patterns

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, under **PIN polices**, clear the check box next to **Allow common PIN patterns**.

4. Click **Save**.

## Use Exchange Online PowerShell to disable common PIN patterns

This example prevents users associated with the UM mailbox policy named `MyUMMailboxPolicy` from using PINs that contain common patterns.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowCommonPatterns $false
```
