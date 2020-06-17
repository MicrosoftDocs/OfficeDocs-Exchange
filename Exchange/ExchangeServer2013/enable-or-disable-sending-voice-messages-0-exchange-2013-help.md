---
title: 'Enable or disable sending voice messages to users: Exchange 2013 Help'
TOCTitle: Enable or disable sending voice messages to users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: faa300d8-2534-40db-8ef9-428be8bb7934
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable sending voice messages to users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable callers to send voice messages to users from a Unified Messaging (UM) auto attendant, or prevent them from doing so. By default, this option is enabled and lets callers send voice messages to users in the UM dial plan that's associated with the UM auto attendant. If you disable this option, the auto attendant won't invite callers to send a voice message during a system prompt.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable callers to send voice messages or prevent them from doing so

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Address book and operator access**, under **Options for contacting users**, select the check box next to **Allow callers to leave voice messages for users** to enable callers to leave voice messages. To prevent callers from leaving voice messages, clear the check box.

4. Click **Save**.

> [!NOTE]
> If you disable this option and also disable the **Allow callers to dial users** option, the **Options for searching the address book** are also disabled.

## Use the Shell to enable callers to send voice messages or prevent them from doing so

This example prevents callers who call in to a UM auto attendant named `MyUMAutoAttendant` from sending voice messages.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -SendVoiceMsgEnabled $false
```

This example enables callers who call in to a UM auto attendant named `MyUMAutoAttendant` to send voice messages.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -SendVoiceMsgEnabled $true
```
