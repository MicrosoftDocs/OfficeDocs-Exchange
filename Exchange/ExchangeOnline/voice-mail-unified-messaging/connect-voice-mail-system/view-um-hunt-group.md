---
localization_priority: Normal
description: When you view the properties for a Unified Messaging (UM) hunt group, you can view the properties associated with a single UM hunt group or with all UM hunt groups associated with a single UM IP gateway. If neither parameter is specified, all UM hunt groups will be returned. You can't use the EAC to view UM hunt group properties; you must use Exchange Online PowerShell.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f038f7b4-4de9-4373-bd58-09d49e37a3ed
ms.reviewer: 
f1.keywords:
- NOCSH
title: View a UM hunt group in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# View a UM hunt group in Exchange Online

When you view the properties for a Unified Messaging (UM) hunt group, you can view the properties associated with a single UM hunt group or with all UM hunt groups associated with a single UM IP gateway. If neither parameter is specified, all UM hunt groups will be returned. You can't use the EAC to view UM hunt group properties; you must use Exchange Online PowerShell.

After a UM hunt group has been created, the configured settings can't be changed. If you want to change a configuration setting such as the pilot identifier on a UM hunt group, you must delete the existing UM hunt group and create a new UM hunt group that has the correct settings.

For additional tasks related to UM hunt groups, see [UM hunt group procedures](um-hunt-group-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM gateway has been created. For detailed steps, see [Create a UM IP gateway](create-um-ip-gateway.md).

- Before you perform this procedure, confirm that a UM hunt group has been created. For detailed steps, see [Create a UM hunt group](create-um-hunt-group.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to view the properties of a UM hunt group

This example displays all the UM hunt groups in the Active Directory forest.

```PowerShell
Get-UMHuntGroup
```

This example displays the details of a UM hunt group named `MyUMHuntGroup` in a formatted list.

```PowerShell
Get-UMHuntGroup -identity MyUMIPGateway\MyUMHuntGroup | Format-List
```

> [!NOTE]
> When you're using the **Get-UMHuntGroup** cmdlet, you can't enter only the name of the UM hunt group. You must also include the name of the UM IP gateway that's associated with the UM hunt group.
