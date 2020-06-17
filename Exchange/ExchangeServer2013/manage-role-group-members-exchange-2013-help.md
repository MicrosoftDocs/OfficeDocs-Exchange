---
title: 'Manage role group members: Exchange 2013 Help'
TOCTitle: Manage role group members
ms:assetid: c064729d-7cda-47fc-b105-acf4b300d430
ms:mtpsurl: https://technet.microsoft.com/library/JJ657492(v=EXCHG.150)
ms:contentKeyID: 49289402
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage role group members

_**Applies to:** Exchange Server 2013_

This topic shows you how to add, remove and view members of a management role group in Microsoft Exchange Server 2013. For more information about role groups in Exchange 2013, see [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md).

For additional management tasks related to role groups, see [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Add members to a role group

To give a user the permissions that are granted by a role group, you need to add the user, or a universal security group (USG), or another role group that the user is a member of, as a member of the role group.

## Use the EAC to add members to a role group

1. In the Exchange admin center (EAC), navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to add members to, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In the **Members** section, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

4. Select the users, USGs, or other role groups you want to add to the role group, click **Add**, and then click **OK**.

5. Click **Save** to save the changes to the role group.

## Use the Shell to add members to a role group

To add a role group member, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember#examples) section in [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember).

To add multiple role group members or to replace the role group membership entirely, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember#examples) section in [Update-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember).

## How do you know this worked?

To verify that you have successfully added one or more members to a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you added members to.

3. In the role group details pane, verify that the members you added are listed.

## Remove members from a role group

To remove the permissions granted by a role group from a user, you need to remove the user, or the universal security group (USG) the user is a member of, from the role group's membership.

## Use the EAC to remove members from a role group

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to remove members from, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In the **Members** section, select the members you want to remove, click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon"), and then click **Save**.

## Use the Shell to remove members from a role group

To remove a role group member, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroupMember#examples) section in [Remove-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroupMember).

To remove multiple role group members or to replace the role group membership entirely, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember#examples) section in [Update-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember).

## How do you know this worked?

To verify that you have successfully removed one or more members to a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you removed members from.

3. In the role group details pane, verify that the members you removed are no longer listed.

## View the members of a role group

The members of a role group are granted the permissions provided by the management roles assigned to the role group. You can view the members of a role group to see which users, universal security groups (USG), or other role groups are granted permissions by the role group you specify.

## Use the EAC to view the members of a role group

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to view the members of.

3. In the role group details pane, view the members in the role group details pane.

## Use the Shell to view the members of a role group

To view the members of a role group, see the "Examples" section in [Get-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroupMember).
