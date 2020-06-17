---
localization_priority: Normal
description: You can enter the name of your business in the Business name box on a UM auto attendant. By default, no business name is entered. If you enter a business name, a default greeting prompt with the business name will be played to callers when they call in to the Unified Messaging (UM) auto attendant.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a0e7cb24-0f55-442d-8ae2-21b177940b78
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enter a business name in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enter a business name in Exchange Online

You can enter the name of your business in the **Business name** box on a UM auto attendant. By default, no business name is entered. If you enter a business name, a default greeting prompt with the business name will be played to callers when they call in to the Unified Messaging (UM) auto attendant.

For additional tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure a business name

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to set a business name, and then, on the toolbar, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **General**, under **Business name**, type the name of the business.

4. Click **Save**.

## Use Exchange Online PowerShell to configure a business name

This example sets the business name on a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -BusinessName "Northwind Traders"
```
