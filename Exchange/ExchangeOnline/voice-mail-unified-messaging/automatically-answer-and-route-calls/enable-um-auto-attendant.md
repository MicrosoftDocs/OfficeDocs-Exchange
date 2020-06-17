---
localization_priority: Normal
description: By default, when a Unified Messaging (UM) auto attendant is created, its status is set to disabled. After you create the UM auto attendant, you can change its status to enable it to answer incoming calls.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 16667a8f-50ab-4bb8-9a05-0389511974b1
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable a UM auto attendant in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable a UM auto attendant in Exchange Online

By default, when a Unified Messaging (UM) auto attendant is created, its status is set to disabled. After you create the UM auto attendant, you can change its status to enable it to answer incoming calls.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to enable. On the toolbar, click the **Up arrow** ![Up Arrow Icon](../../media/ITPro_EAC_UpArrowIcon.gif).

3. On the **Warning** page, click **Yes**.

## Use Exchange Online PowerShell to enable a UM auto attendant

This example enables the UM auto attendant named `MyUMAutoAttendant` to answer incoming calls.

```PowerShell
Enable-UMAutoAttendant -Identity MyUMAutoAttendant
```
