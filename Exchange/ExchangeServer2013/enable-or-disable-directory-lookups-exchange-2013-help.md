---
title: 'Enable or disable directory lookups: Exchange 2013 Help'
TOCTitle: Enable or disable directory lookups
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c0768815-8578-4385-8d4c-7d1e40304cec
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable directory lookups in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable directory lookups so that callers who call in to a Unified Messaging (UM) auto attendant can look up names in the directory using their telephone keypad but not be able to search the directory using voice inputs. This setting is enabled by default. If this setting is disabled, callers won't be able to search the directory for a specific person using touchtone or voice commands.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

> [!NOTE]
> Outlook Voice Access users can't use Automatic Speech Recognition (ASR) or speech inputs to locate users in the directory, they can only use DTMF or touchtone inputs.

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable directory lookups

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to enable or disable directory lookups, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Address book and operator access**, under **Options for searching the address book**, select the check box next to **Allow callers to search for users by name or alias** to enable callers to search for users. To disable callers from searching for users, clear this check box.

4. Click **Save**.

## Use the Shell to enable or disable directory lookups

This example disables directory lookups on a UM auto attendant named `MyUMAutoAttendant`.

```powershell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -NameLookupEnabled $false
```
