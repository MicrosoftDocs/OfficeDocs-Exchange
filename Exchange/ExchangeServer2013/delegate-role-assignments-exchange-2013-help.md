---
title: 'Delegate role assignments: Exchange 2013 Help'
TOCTitle: Delegate role assignments
ms:assetid: ed2d00d9-90c9-49dc-ab8a-cd791569aeed
ms:mtpsurl: https://technet.microsoft.com/library/Dd351237(v=EXCHG.150)
ms:contentKeyID: 49289453
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Delegate role assignments

_**Applies to:** Exchange Server 2013_

Management role delegation enables role assignees to assign a specified management role to other management role groups, management role assignment policies, users, or universal security groups (USG). By default, only members of the Organization Management management role group can delegate role assignments. When a new installation of Microsoft Exchange Server 2013 is deployed, only the user account that installed Exchange 2013 is a member of the Organization Management role group.

If you assign a delegating role assignment to a role group, any member of the role group can delegate the associated management role to other role assignees.

> [!IMPORTANT]
> Delegating role assignments doesn't give the role assignee the permissions granted by the role, only the ability to assign the role to others. If you want to also give the permissions granted by the role to the role assignee, you must also create a regular role assignment. To create a regular role assignment, see the following topics:<BR><A href="manage-role-groups-exchange-2013-help.md">Manage role groups</A><BR><A href="manage-role-assignment-policies-exchange-2013-help.md">Manage role assignment policies</A><BR><A href="add-a-role-to-a-user-or-usg-exchange-2013-help.md">Add a role to a user or USG</A>

> [!NOTE]
> This topic discusses management role assignment delegation. If you want to delegate who can add members to or remove members from role groups, which is the recommended method of delegation, see <A href="manage-role-groups-exchange-2013-help.md">Manage role groups</A>.

For more information about regular role assignments and delegating management role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

Looking for other management tasks related to managing permissions? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role assignments" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to delegate a management role

You can create delegating role assignments using the same predefined scopes, recipient filter or server-filter-based scopes, server list-based scopes, and organizational unit (OU) scopes that can be used to create regular or exclusive scopes. The only difference between creating a regular role assignment and a delegating role assignment is the addition of the *Delegating* switch to the command. For more information about how to create role assignments, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

> [!NOTE]
> You can't create a delegating role assignment to a management role assignment policy.

This example creates a delegating role assignment to enable members of the Senior Admins role group to assign the Mail Recipients role to any role assignee in the Exchange organization.

```powershell
New-ManagementRoleAssignment -Role "Mail Recipients" -SecurityGroup "Senior Admins" -Name "Mail Recipients_Senior Admin - Delegate" -Delegating
```

This example creates a delegating role assignment to enable members of the Senior Admins role group to assign the Mail Recipients role only to users in the Sales/Users OU in the contoso.com domain.

```powershell
New-ManagementRoleAssignment -Role "Mail Recipients" -SecurityGroup "Senior Admins" -Name "Mail Recipients_Senior Admins - Delegate" -RecipientOrganizationalUnitScope contoso.com/sales/users -Delegating
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).
