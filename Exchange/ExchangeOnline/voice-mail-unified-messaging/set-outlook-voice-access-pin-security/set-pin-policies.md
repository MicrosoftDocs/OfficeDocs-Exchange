---
localization_priority: Normal
description: You can set PIN policies on a Unified Messaging (UM) mailbox policy. UM mailbox policies can be configured to increase the level of security for UM-enabled users that use Outlook Voice Access by requiring users to comply with the predefined PIN policies for your organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5b2800b7-bfa6-4282-975c-0706ae25ad64
ms.reviewer: 
f1.keywords:
- NOCSH
title: Set Outlook Voice Access PIN policies in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Set Outlook Voice Access PIN policies in Exchange Online

You can set PIN policies on a Unified Messaging (UM) mailbox policy. UM mailbox policies can be configured to increase the level of security for UM-enabled users that use Outlook Voice Access by requiring users to comply with the predefined PIN policies for your organization.

To set PIN policies for Outlook Voice Access users, you can either create a new UM mailbox policy or modify an existing UM mailbox policy. After a new UM mailbox policy is created, you can then configure the UM mailbox policy by configuring the following PIN settings:

- `MinPasswordLength`

- `PINLifetime`

- `LogonFailuresBeforePINReset`

- `MaxLogonAttempts`

- `AllowCommonPatterns`

- `PINHistoryCount`

It's a security best practice to implement strong PIN requirements for UM users. This can be enforced by creating UM PIN policies that require 6 or more digits for PINs and increase the level of security for your network.

When you change the PIN policy, the new PIN setting is applied to users who are currently associated with the UM mailbox policy. For example, if you modify the UM mailbox policy and change the minimum PIN length from 7 to 10 digits, the next time users log on they'll be forced to change their PIN to comply with the changed PIN requirement.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to set PIN policies for Outlook Voice Access users

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, click the UM dial plan you want to edit, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to edit, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. Click **Properties**.

4. On the **UM mailbox policy** page, click **PIN policies**.

5. On the **PIN Policies** page, configure the PIN settings for the Outlook Voice Access users associated with this UM mailbox policy, and then click **Save**.

## Use Exchange Online PowerShell to set PIN policies for Outlook Voice Access users

This example sets the PIN settings for users associated with the UM mailbox policy `MyUMMailboxPolicy`.

```PowerShell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 8 -MaxLogonAttempts 12 -MinPINLength 8 -PINHistoryCount 10 -PINLifetime 60 -ResetPINText "The PIN used to allow you access to your mailbox using Outlook Voice Access has been reset."
```
