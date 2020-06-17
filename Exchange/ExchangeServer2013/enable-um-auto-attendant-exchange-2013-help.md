---
title: 'Enable a UM auto attendant: Exchange 2013 Help'
TOCTitle: Enable a UM auto attendant
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 16667a8f-50ab-4bb8-9a05-0389511974b1
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable a UM auto attendant in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

By default, when a Unified Messaging (UM) auto attendant is created, its status is set to disabled. After you create the UM auto attendant, you can change its status to enable it to answer incoming calls.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM auto attendants" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change and click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to enable. On the toolbar, click the **Up arrow** ![Up Arrow Icon](images/ITPro_EAC_UpArrowIcon.gif).

3. On the **Warning** page, click **Yes**.

## Use the Shell to enable a UM auto attendant

This example enables the UM auto attendant named `MyUMAutoAttendant` to answer incoming calls.

```powershell
Enable-UMAutoAttendant -Identity MyUMAutoAttendant
```
