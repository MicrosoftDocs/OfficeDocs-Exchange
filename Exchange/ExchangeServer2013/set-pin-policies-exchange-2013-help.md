---
title: 'Set Outlook Voice Access PIN policies: Exchange 2013 Help'
TOCTitle: Set Outlook Voice Access PIN policies
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5b2800b7-bfa6-4282-975c-0706ae25ad64
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set Outlook Voice Access PIN policies in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

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

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to set PIN policies for Outlook Voice Access users

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, click the UM dial plan you want to edit, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to edit, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. Click **Properties**.

4. On the **UM mailbox policy** page, click **PIN policies**.

5. On the **PIN Policies** page, configure the PIN settings for the Outlook Voice Access users associated with this UM mailbox policy, and then click **Save**.

## Use the Shell to set PIN policies for Outlook Voice Access users

This example sets the PIN settings for users associated with the UM mailbox policy `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 8 -MaxLogonAttempts 12 -MinPINLength 8 -PINHistoryCount 10 -PINLifetime 60 -ResetPINText "The PIN used to allow you access to your mailbox using Outlook Voice Access has been reset."
```
