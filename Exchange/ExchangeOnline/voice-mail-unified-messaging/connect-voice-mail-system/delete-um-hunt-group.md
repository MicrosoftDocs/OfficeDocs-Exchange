---
localization_priority: Normal
description: After you delete a Unified Messaging (UM) hunt group, the UM IP gateway associated with the UM hunt group will no longer service or answer incoming calls. If deleting the UM hunt group leaves the UM IP gateway without any remaining configured hunt groups, the UM IP gateway can't handle or process UM calls.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 11ac102d-b58d-486c-85b6-e096428e556d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Delete a UM hunt group in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Delete a UM hunt group in Exchange Online

After you delete a Unified Messaging (UM) hunt group, the UM IP gateway associated with the UM hunt group will no longer service or answer incoming calls. If deleting the UM hunt group leaves the UM IP gateway without any remaining configured hunt groups, the UM IP gateway can't handle or process UM calls.

For additional tasks related to UM hunt groups, see [UM hunt group procedures](um-hunt-group-procedures.md).

> [!CAUTION]
> If you want to change the UM hunt group settings, you must delete the hunt group and then create another hunt group that has the appropriate settings.

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM IP gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md).

- Before you perform these procedures, confirm that a UM hunt group has been created. For detailed steps, see [Create a UM hunt group](create-um-hunt-group.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to delete a UM hunt group

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, click the UM dial plan you want to change, and on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Hunt Groups**, select the hunt group you want to delete, and on the toolbar, click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

3. On the **Warning** page, click **Yes**.

## Use Exchange Online PowerShell to delete a UM hunt group

This example deletes a UM hunt group named `MyUMHuntGroup`.

```PowerShell
Remove-UMHuntGroup -identity MyUMHuntGroup
```
