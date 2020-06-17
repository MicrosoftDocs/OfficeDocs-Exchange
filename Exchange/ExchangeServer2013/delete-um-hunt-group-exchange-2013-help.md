---
title: 'Delete a UM hunt group: Exchange 2013 Help'
TOCTitle: Delete a UM hunt group
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 11ac102d-b58d-486c-85b6-e096428e556d
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delete a UM hunt group in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

After you delete a Unified Messaging (UM) hunt group, the UM IP gateway associated with the UM hunt group will no longer service or answer incoming calls. If deleting the UM hunt group leaves the UM IP gateway without any remaining configured hunt groups, the UM IP gateway can't handle or process UM calls.

For additional tasks related to UM hunt groups, see [UM hunt group procedures](um-hunt-group-procedures-exchange-2013-help.md).

> [!CAUTION]
> If you want to change the UM hunt group settings, you must delete the hunt group and then create another hunt group that has the appropriate settings.

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM hunt groups" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM hunt group has been created. For detailed steps, see [Create a UM hunt group](create-um-hunt-group-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to delete a UM hunt group

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, click the UM dial plan you want to change, and on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Hunt Groups**, select the hunt group you want to delete, and on the toolbar, click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

3. On the **Warning** page, click **Yes**.

## Use the Shell to delete a UM hunt group

This example deletes a UM hunt group named `MyUMHuntGroup`.

```powershell
Remove-UMHuntGroup -identity MyUMHuntGroup
```
