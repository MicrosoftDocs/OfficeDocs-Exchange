---
title: 'Set the number of sign-in failures before a voice mail PIN is reset: Exchange 2013 Help'
TOCTitle: Set the number of sign-in failures before a voice mail PIN is reset
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 4de38499-0a6f-4f00-8697-eeff805d7266
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the number of sign-in failures before a voice mail PIN is reset in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure the number of sign-in failures allowed before the PIN is reset for an Outlook Voice Access user to a value from 1 through 998. The default is 5. The number of sign-in failures allowed before a PIN is reset is configured on a Unified Messaging (UM) mailbox policy and applies to all Outlook Voice Access users associated with the UM mailbox policy.

> [!NOTE]
> You can increase security by configuring the **Number of sign-in failures before PIN reset** setting to a number less than 5. You decrease security if you configure it to a number more than 5.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure the number of sign-in failures before a PIN is reset

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. Click **PIN policies**, and next to **Number of sign-in failures before PIN reset**, enter a value between 0 and 999.

5. Click **Save**.

## Use the Shell to configure the number of sign-in failures before a PIN is reset

This example sets the number of sign-in failures before the user's PIN is reset to 3 for UM-enabled users who are associated with a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 3
```

This example sets the number of sign-in failures before the user's PIN is reset to 3, the maximum number of sign-in attempts to 5, and the minimum PIN length to 9 for UM-enabled users who are associated with a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 3 -MaxLogonAttempts 5 -MinPINLength 9
```
