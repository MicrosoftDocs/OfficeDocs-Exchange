---
title: 'Set the number of sign-in failures before a voice mail user Is locked out: Exchange 2013 Help'
TOCTitle: Set the number of sign-in failures before a voice mail user Is locked out
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 855e1980-2868-4983-b097-0b5f63f202b8
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the number of sign-in failures before a voice mail user Is locked out in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can configure the number of sign-in failures allowed before an Outlook Voice Access user is locked out of their mailbox. The number of sign-in failures allowed before a voice mail user is locked out is configured on a Unified Messaging (UM) mailbox policy, and applies to all UM-enabled users associated with the UM mailbox policy. By default it is set to 15.

To increase security, decrease the maximum number of failed attempts. However, remember that if you decrease it to a number much lower than the default, users may be locked out unnecessarily. Unified Messaging will generate warning events you can view using Event Viewer if PIN authentication fails for UM-enabled users or if users are unsuccessful when they try to sign in to the system. This setting must be larger than the setting for the number of sign-in failures before the PIN is reset.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure the number of sign-in failures before a voice mail user is locked out

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. Click **PIN policies**, and next to **Number of sign-in failures before lockout**, enter a value between 1 and 999.

5. Click **Save**.

## Use the Shell to configure the number of sign-in failures before a voice mail user is locked out

This example sets the maximum number sign-in attempts to 10 for UM-enabled users who are associated with a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -MaxLogonAttempts 10
```

This example sets the number of sign-in failures before the Outlook Voice Access user's PIN is reset to 3, the maximum number of sign-in attempts to 5, and a minimum PIN length to 9 for UM-enabled users who are associated with a UM mailbox policy named `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -LogonFailuresBeforePINReset 3
-MaxLogonAttempts 5 -MinPINLength 9
```
