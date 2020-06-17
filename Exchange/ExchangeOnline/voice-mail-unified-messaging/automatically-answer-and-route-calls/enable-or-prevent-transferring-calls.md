---
localization_priority: Normal
description: You can enable callers to transfer calls to users through an auto attendant, or prevent them from doing so. By default this option is enabled, and lets callers transfer calls to UM-enabled users in the Unified Messaging (UM) dial plan that's associated with the UM auto attendant.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ca961cc8-cc24-4e05-b72d-79979c155cf9
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or prevent transferring calls from an auto attendant in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or prevent transferring calls from an auto attendant in Exchange Online

You can enable callers to transfer calls to users through an auto attendant, or prevent them from doing so. By default this option is enabled, and lets callers transfer calls to UM-enabled users in the Unified Messaging (UM) dial plan that's associated with the UM auto attendant.

For additional management tasks related to UM auto attendants, see [UM auto attendant procedures](um-auto-attendant-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable or prevent call transfers to users from a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant for which you want to configure call transfer, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Address book and operator access**, under **Options for contacting users**, select the check box next to **Allow callers to dial users** to enable calls to be transferred. To prevent call transfers, clear the check box.

4. Click **Save**.

> [!NOTE]
> If you clear this check box and also clear the **Allow callers to leave voice messages for users** check box, the **Options for searching the address book** are disabled.

## Use Exchange Online PowerShell to enable or prevent call transfers to users from a UM auto attendant

This example prevents call transfers on a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -AllowDialPlanSubscribers $false
```

This example enables call transfers on a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -AllowDialPlanSubscribers $true
```
