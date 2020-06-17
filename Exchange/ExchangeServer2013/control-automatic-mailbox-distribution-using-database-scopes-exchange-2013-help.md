---
title: 'Control automatic mailbox distribution using database scopes'
TOCTitle: Control automatic mailbox distribution using database scopes
ms:assetid: 8eaff177-2251-4c8b-8570-c91a77d0a6fc
ms:mtpsurl: https://technet.microsoft.com/library/Ff628332(v=EXCHG.150)
ms:contentKeyID: 49289347
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Control automatic mailbox distribution using database scopes

_**Applies to:** Exchange Server 2013_

Automatic mailbox distribution is a feature in Microsoft Exchange Server 2013 that randomly selects a mailbox database to store a new or moved mailbox when you don't specify a database explicitly. This feature can be helpful when you want to allow junior administrators or help desk staff to create mailboxes without needing to know which mailbox databases mailboxes should be created on.

You can use database management scopes to control which mailbox databases can be selected by automatic mailbox distribution. When you apply database scopes to an administrator, only the databases that match the defined database scope are available to the administrator. Because automatic mailbox distribution uses the context of the current user, it's also constrained by the database scopes applied to the administrator.

For more information about automatic mailbox distribution, database scopes, and role assignments, see the following topics:

- [Automatic mailbox distribution](automatic-mailbox-distribution-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for other management tasks related to scopes? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management scopes" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create a database scope

In this step, decide which databases you want to include in the database scope. Also, decide whether you want to specify a static list of databases, or whether you want to create a database filter that includes only the databases that match the criteria you specify.

> [!IMPORTANT]
> Role assignments associated with database scopes are applied only to users who connect to servers running Microsoft Exchange Server 2010 Service Pack&nbsp;1 (SP1) or later or Exchange 2013. If a user assigned a role assignment associated with a database scope connects to a pre-Exchange 2010 SP1 server, the role assignment isn't applied to the user, and the user won't be granted any permissions provided by the role assignment.

### Use a database list scope

Use a database list if you want to define a static list of mailbox databases that should be included in this scope. Use the following syntax to create a database list scope.

```powershell
New-ManagementScope -Name <scope name> -DatabaseList <database 1>, <database 2...>
```

This example creates a scope that applies only to the databases Database 1, Database 2, and Database 3.

```powershell
New-ManagementScope -Name "Accounting databases" -DatabaseList "Database 1", "Database 2", "Database 3"
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Use a database filter scope

Use a database filter if you want to create a dynamic database scope that includes only the databases that match the criteria you specify. This can be useful if you don't want to manage the database scope after it's created and you've defined standard values for your organization that can identify specific sets of mailbox databases.

For a list of filterable database properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

Use the following syntax to create a database filter scope.

```powershell
New-ManagementScope -Name <scope name> -DatabaseRestrictionFilter <filter query>
```

This example creates a scope that includes all the databases that contain the string "ACCT" in the **Name** property of the database.

```powershell
New-ManagementScope -Name "Accounting Databases" -DatabaseRestrictionFilter "Name -Like '*ACCT*'"
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

## Step 2: Add the database scope to a management role assignment

After you create the scope, you must add it to a new or existing management role assignment. We recommend that you use management role groups to control administrative permissions, so the examples in this step use an example role group called Accounting Administrators. For more information about how to create a role group, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

After you assign the role to a role group with the database scope, the members of the role group will only be able to create mailboxes on, and move mailboxes to, the databases included in the scope.

For a list of built-in roles that you can assign to role groups, see [Built-in management roles](built-in-management-roles-exchange-2013-help.md).

### Add a new role assignment

Use this procedure if you've just created a role group and you need to add roles to it.

Use the following syntax to create a role assignment between the management role you want to assign and the new role group, with the new database scope.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name> -CustomConfigWriteScope <database scope name>
```

This example creates a role assignment between the Mail Recipients and Mail Recipient Creation roles and the Accounting Administrators role group, using the Accounting Databases database scope.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Accounting Administrators" -Role "Mail Recipients" -CustomConfigWriteScope "Accounting Databases"
New-ManagementRoleAssignment -SecurityGroup "Accounting Administrators" -Role "Mail Recipient Creation" -CustomConfigWriteScope "Accounting Databases"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

### Modify an existing role assignment

Use this procedure if you have an existing role group that already has role assignments between it and the roles you want to apply the new database scope to.

This procedure uses pipelining. For more information, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

Use the following syntax to modify a role assignment between the management role that you want to apply the database scope to, and an existing role group.

```powershell
Get-ManagementRoleAssignment -RoleAssignee <role group name> -Role <role name> | Set-ManagementRoleAssignment -CustomConfigWriteScope <database scope name>
```

This example adds the Accounting Databases database scope to the Mail Recipients and Mail Recipient Creation roles assigned to the Accounting Administrators role group.

```powershell
Get-ManagementRoleAssignment -RoleAssignee "Accounting Administrators" -Role "Mail Recipients" | Set-ManagementRoleAssignment -CustomConfigWriteScope "Accounting Databases"
Get-ManagementRoleAssignment -RoleAssignee "Accounting Administrators" -Role "Mail Recipient Creation" | Set-ManagementRoleAssignment -CustomConfigWriteScope "Accounting Databases"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment) or [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Step 3: Add members to a role group (if applicable)

If you want to add members to a role group, see [Manage role group members](manage-role-group-members-exchange-2013-help.md).

> [!IMPORTANT]
> If you add members to this role group to restrict what databases they can create users on, or move mailboxes to, make sure they aren't members of other role groups that could grant extra permissions.

## Step 4: Remove members from a role group (if applicable)

If you've added members to a new role group that restricts what databases they can create mailbox on, or move mailboxes to, and they're members of another role group that has additional permissions, remove them from the old role group. For more information, see [Manage role group members](manage-role-group-members-exchange-2013-help.md).
