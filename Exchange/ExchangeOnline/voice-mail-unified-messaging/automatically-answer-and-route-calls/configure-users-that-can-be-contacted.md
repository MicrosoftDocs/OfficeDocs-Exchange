---
localization_priority: Normal
description: You can specify the group of users that callers can contact when calling into a Unified Messaging (UM) auto attendant. By default, callers can contact users within the same dial plan that's associated with the UM auto attendant. However, you can change the grouping of users to allow callers to transfer calls or send voice messages to users who are located in the organization's address book or to a specific set of users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 45d9d6d5-c9d6-4b73-8aa2-a23599a4381c
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure the group of users that can be contacted in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure the group of users that can be contacted in Exchange Online

You can specify the group of users that callers can contact when calling into a Unified Messaging (UM) auto attendant. By default, callers can contact users within the same dial plan that's associated with the UM auto attendant. However, you can change the grouping of users to allow callers to transfer calls or send voice messages to users who are located in the organization's address book or to a specific set of users.

For additional management tasks related to UM auto attendants, see [Manage a UM auto attendant](manage-um-auto-attendant.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM auto attendant has been created. For detailed steps, see [Create a UM auto attendant](create-a-um-auto-attendant.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure the group of users that callers can contact

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, select the UM auto attendant you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Auto Attendant** page \> **Address book and operator access**, under **Options for searching the address book**, choose from the following options:

   - **In this dial plan only**: Select this option to allow callers who connect to the UM auto attendant to locate and contact users who are in the dial plan associated with the UM auto attendant.

   - **In the entire organization**: Select this option to allow callers who connect to the UM auto attendant to locate and contact anyone listed in the organization's address book. This includes all users who are mailbox-enabled.

4. Click **Save**.

## Use Exchange Online PowerShell to configure the group of users that callers can contact

This example sets the scope of the users that callers can contact to all users in the organization's address book on a UM auto attendant named `MyUMAutoAttendant`.

```PowerShell
Set-UMAutoAttendant -Identity MyUMAutoAttendant -ContactScope GlobalAddressList
```
