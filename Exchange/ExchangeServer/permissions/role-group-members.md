---
localization_priority: Normal
description: 'Summary: Learn how to add, remove and view members of a management role group in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c064729d-7cda-47fc-b105-acf4b300d430
ms.reviewer:
title: Manage role group members
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage role group members

 To learn about role groups in Exchange Server, see [Understanding Management Role Groups](https://docs.microsoft.com/exchange/understanding-management-role-groups-exchange-2013-help).

For additional management tasks related to role groups, see [Permissions](permissions.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- To open the EAC, see [Exchange admin center in Exchange Server](../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](feature-permissions/rbac-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Add members to a role group
<a name="add"> </a>

To give a user the permissions that are granted by a role group, you need to add the user, or a universal security group (USG), or another role group that the user is a member of, as a member of the role group.

### Use the EAC to add members to a role group

1. In the Exchange admin center (EAC), navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to add members to, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. In the **Members** section, click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png).

4. Select the users, USGs, or other role groups you want to add to the role group, click **Add**, and then click **OK**.

5. Click **Save** to save the changes to the role group.

### Use the Exchange Management Shell to add members to a role group

To add a role group member, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember#examples) section in [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember).

To add multiple role group members or to replace the role group membership entirely, see the [Examples]https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember#examples section in [Update-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember).

### How do you know this worked?

To verify that you have successfully added one or more members to a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you added members to.

3. In the role group details pane, verify that the members you added are listed.

## Remove members from a role group
<a name="remove"> </a>

To remove the permissions granted by a role group from a user, you need to remove the user, or the universal security group (USG) the user is a member of, from the role group's membership.

### Use the EAC to remove members from a role group

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to remove members from, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. In the **Members** section, select the members you want to remove, click **Remove** ![Remove icon](../media/ITPro_EAC_RemoveIcon.png), and then click **Save**.

### Use the Exchange Management Shell to remove members from a role group

To remove a role group member, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroupMember#Examples) section in [Remove-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroupMember).

To remove multiple role group members or to replace the role group membership entirely, see the [Examples]https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember#examples section in [Update-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember).

### How do you know this worked?

To verify that you have successfully removed one or more members to a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you removed members from.

3. In the role group details pane, verify that the members you removed are no longer listed.

## View the members of a role group
<a name="view"> </a>

The members of a role group are granted the permissions provided by the management roles assigned to the role group. You can view the members of a role group to see which users, universal security groups (USG), or other role groups are granted permissions by the role group you specify.

### Use the EAC to view the members of a role group

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to view the members of.

3. In the role group details pane, view the members in the role group details pane.

### Use the Exchange Management Shell to view the members of a role group

To view the members of a role group, see the "Examples" section in [Get-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/get-rolegroupmember).
