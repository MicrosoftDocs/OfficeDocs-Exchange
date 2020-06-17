---
title: 'Change a role assignment: Exchange 2013 Help'
TOCTitle: Change a role assignment
ms:assetid: 0fa77efc-e393-461f-b3c0-232cc56cee85
ms:mtpsurl: https://technet.microsoft.com/library/Dd335096(v=EXCHG.150)
ms:contentKeyID: 49289170
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change a role assignment

_**Applies to:** Exchange Server 2013_

Management role assignments assign a management role to a role assignee. By changing the role assignment, you can control what objects role assignees assigned a role can change. Management role scopes applied to role assignments override the role's implicit write scope. However, the role's implicit read scope still applies. Scopes that you apply can't return objects outside of the role's implicit read scope.

For more information about management role scopes and assignments in Microsoft Exchange Server 2013, see the following topics:

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

Looking for other management tasks related to role assignments? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role assignments" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable a role assignment

Role assignments are enabled by default, meaning that the associated role is applied to the role assignee to which the role is assigned. If a role assignment is disabled, the associated role isn't applied to the role assignee.

To enable a role assignment, use the following syntax.

```powershell
Set-ManagementRoleAssignment <role assignment> -Enabled $true
```

To disable a role assignment, use the following syntax.

```powershell
Set-ManagementRoleAssignment <role assignment> -Enabled $false
```

This example disables the Help Desk Assignment role assignment.

```powershell
Set-ManagementRoleAssignment "Help Desk Assignment" -Enabled $false
```

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change a management role or role assignee on a role assignment

You can't change the management role or role assignee specified on a role assignment. If you want a role assignment to be associated with another role or role assignee, you must create a new role assignment, and then delete the old role assignment. For more information about how to add and remove role assignments, see the following topics:

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

- [Remove a role from a user or USG](remove-a-role-from-a-user-or-usg-exchange-2013-help.md)

If you've created assignments directly to a user or universal security group (USG), we recommend that you consider using management role groups and management role assignment policies. Role groups and assignment policies enable you to simplify your permissions model and reduce the number of role assignments you need to manage. For more information, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

## Use the Shell to change a predefined relative scope on a role assignment

You can change or add a predefined relative scope on a role assignment. If you add or change a predefined scope, any previously specified recipient scopes are removed from the role assignment. For a list of predefined scopes and their descriptions, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

To change or add a predefined scope on a role assignment, use the following syntax.

```powershell
Set-ManagementRoleAssignment <assignment name> -RecipientRelativeWriteScope < MyDistributionGroups | Organization | Self >
```

This example changes the predefined scope on the John's Assignment role assignment to MyDistributionGroups.

```powershell
Set-ManagementRoleAssignment "John's Assignment" - RecipientRelativeWriteScope MyDistributionGroups
```

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change a recipient filter scope on a role assignment

You can either specify a new recipient filter-based scope or change the recipient filter-based scope that's already applied to the role assignment. If you add a recipient filter scope, any previously defined recipient scopes are removed from the role assignment.

To specify a new recipient filter-based scope or replace an existing one, use the following syntax.

```powershell
Set-ManagementRoleAssignment <assignment name> -CustomRecipientWriteScope <role scope name>
```

This example adds or changes the recipient filter-based scope to Redmond Recipients.

```powershell
Set-ManagementRoleAssignment "Redmond Recipient Administrators Assignment" -CustomRecipientWriteScope "Redmond Recipients"
```

If you want to keep the same recipient filter-based scope that's applied to the role assignment but change the recipient filter used to match recipient objects, you need to change the recipient filter on the scope itself. For more information about how to change scopes, see [Change a role scope](change-a-role-scope-exchange-2013-help.md).

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change the server filter or list-based configuration scope on a role assignment

You can either specify a new server filter or list-based configuration scope, or change the scope that's already applied to the role assignment. If you add or change the configuration scope, any previously specified configuration scopes are removed from the role assignment.

To specify a new configuration scope or replace an existing one, use the following syntax.

```powershell
Set-ManagementRoleAssignment <assignment name> -CustomConfigWriteScope <role scope name>
```

This example adds or changes the configuration scope to Redmond Servers.

```powershell
Set-ManagementRoleAssignment "Redmond Administrators Assignment" -CustomConfigWriteScope "Redmond Servers"
```

If you want to keep the same configuration scope that's applied to the role assignment but change the server filter or server list on the scope, you need to change the configuration scope itself. For more information about how to change scopes, see [Change a role scope](change-a-role-scope-exchange-2013-help.md).

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change the database filter or list-based configuration scope on a role assignment

You can either specify a new database filter or list-based configuration scope, or change the scope that's already applied to the role assignment. If you add or change the configuration scope, any previously specified configuration scopes are removed from the role assignment.

To specify a new configuration scope or replace an existing one, use the following syntax.

```powershell
Set-ManagementRoleAssignment <assignment name> -CustomConfigWriteScope <role scope name>
```

This example adds or changes the configuration scope to Redmond Databases.

```powershell
Set-ManagementRoleAssignment "Redmond Database Admins" -CustomConfigWriteScope "Redmond Databases"
```

If you want to keep the same configuration scope that's applied to the role assignment but change the database filter or database list on the scope, you need to change the configuration scope itself. For more information about how to change scopes, see [Change a role scope](change-a-role-scope-exchange-2013-help.md).

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change the organizational unit on a role assignment

You can either add a new OU or change an OU that's already applied to the role assignment. If you specify a new OU, any previously specified recipient scopes are removed from the role assignment.

To change or add a new OU on a role assignment, use the following syntax.

```powershell
Set-ManagementRoleAssignment <assignment name> -RecipientOrganizationalUnitScope <OU>
```

This example adds the Engineering\\Users OU in the contoso.com domain to the Engineering Help Desk role assignment.

```powershell
Set-ManagementRoleAssignment "Engineering Help Desk" -RecipientOrganizationalUnitScope contoso.com/Engineering/Users
```

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## Use the Shell to change an exclusive recipient or configuration scope

To change exclusive recipient or exclusive configuration scopes, you can use the procedures provided in the "Use the Shell to change a recipient filter scope on a role assignment," "Use the Shell to change the server filter or list-based configuration scope on a role assignment," and "Use the Shell to change the database filter or list-based configuration scope on a role assignment" sections earlier in this topic. The only difference is that when you change an exclusive scope, you must specify the following exclusive parameters depending on whether you're changing an exclusive recipient scope or an exclusive configuration scope:

- **Exclusive recipient scopes**: Use the *ExclusiveRecipientWriteScope* parameter instead of the *CustomRecipientWriteScope* parameter.

- **Exclusive server and database configuration scopes**: Use the *ExclusiveConfigWriteScope* parameter instead of the *CustomConfigWriteScope* parameter.

As with regular recipient and configuration scopes, if you add or change an exclusive scope, any previously defined recipient or configuration scopes are replaced.

This example changes an exclusive recipient write scope.

```powershell
Set-ManagementRoleAssignment "Exclusive Executive Users" -ExclusiveRecipientWriteScope "Exclusive Executives"
```

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).
