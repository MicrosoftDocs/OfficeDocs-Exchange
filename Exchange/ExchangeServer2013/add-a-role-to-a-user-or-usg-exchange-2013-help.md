---
title: 'Add a role to a user or USG: Exchange 2013 Help'
TOCTitle: Add a role to a user or USG
ms:assetid: ae5608de-a141-4714-8876-bce7d2a22cb5
ms:mtpsurl: https://technet.microsoft.com/library/Dd351056(v=EXCHG.150)
ms:contentKeyID: 49289370
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add a role to a user or USG

_**Applies to:** Exchange Server 2013_

Management role assignments can assign a management role to a user or universal security group (USG). By assigning a role to a user or USG, you enable those users to perform tasks dependent on cmdlets or scripts and their parameters defined on the management role.

If you want to assign roles to a management role group or a management role assignment policy, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Manage role assignment policies](manage-role-assignment-policies-exchange-2013-help.md)

If you want to add members to a role group or assign a role assignment policy to an end user, see the following topics:

- [Manage role group members](manage-role-group-members-exchange-2013-help.md)

- [Change the assignment policy on a mailbox](change-the-assignment-policy-on-a-mailbox-exchange-2013-help.md)

For more information, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role assignments" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- Although you can assign roles directly to users and USGs, the recommended method of granting permissions to administrators and end users is to use management role groups and management role assignment policies. When you use role groups and assignment policies, you simplify your permissions model.

- Role assignments are additive. This means that all the roles are added together when they're evaluated. If two roles are assigned to a user and one role contains a cmdlet but the other doesn't, the cmdlet will still be available to the user.

    By default, role assignments don't grant the ability to assign roles to other users. To enable a user to assign roles to other users or USGs, see [Delegate role assignments](delegate-role-assignments-exchange-2013-help.md).

- If you create an assignment with a scope, the scope overrides the role's implicit write scope. However, the role's implicit read scope still applies. The new scope can't return objects outside of the role's implicit read scope. For more information, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

- All the procedures in this topic use the *SecurityGroup* parameter to assign roles to a USG. If you want to assign the role to a specific user, use the *User* parameter instead of the *SecurityGroup* parameter. All other syntax for each command is the same.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a role assignment with no scope

You can create a role assignment with no scope. When you do this, the implicit read and implicit write scopes of the role apply.

Use the following syntax to assign a role to a USG without any scope.

```powershell
New-ManagementRoleAssignment -Name <assignment name> -SecurityGroup <USG> -Role <role name>
```

This example assigns the Exchange Servers role to the SeattleAdmins USG.

```powershell
New-ManagementRoleAssignment -Name "Exchange Servers_SeattleAdmins" -SecurityGroup SeattleAdmins -Role "Exchange Servers"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Create a role assignment with a predefined relative scope

If a predefined relative scope meets your business requirements, you can apply that scope to the role assignment rather than create a custom scope. For a list of predefined scopes and their descriptions, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

Use the following syntax to assign a role to a USG with a predefined scope.

```powershell
New-ManagementRoleAssignment -Name <assignment name> -SecurityGroup < USG> -Role <role name> -RecipientRelativeWriteScope < MyDistributionGroups | Organization | Self >
```

This example assigns the Exchange Servers role to the SeattleAdmins USG and applies the Organization predefined scope.

```powershell
New-ManagementRoleAssignment -Name "Exchange Servers_SeattleAdmins" -SecurityGroup SeattleAdmins -Role "Exchange Servers" -RecipientRelativeWriteScope Organization
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Create a role assignment with a recipient filter-based scope

If you created a recipient filter-based scope and want to use it with a role assignment, you need to include the scope in the command used to assign the role to a USG by using the *CustomRecipientWriteScope* parameter. If you use the *CustomRecipientWriteScope* parameter, you can't use the *RecipientOrganizationalUnitScope* parameter.

Before you can add a scope to a role assignment, you need to create one. For more information, see [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md).

Use the following syntax to assign a role to a USG with a recipient filter-based scope.

```powershell
New-ManagementRoleAssignment -Name <assignment name> -SecurityGroup < USG> -Role <role name> -CustomRecipientWriteScope <role scope name>
```

This example assigns the Mail Recipients role to the Seattle Recipient Admins USG and applies the Seattle Recipients scope.

```powershell
New-ManagementRoleAssignment -Name "Mail Recipients_Seattle Recipient Admins" -SecurityGroup "Seattle Recipient Admins" -Role "Mail Recipients" -CustomRecipientWriteScope "Seattle Recipients"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Create a role assignment with a server or database filter or list-based configuration scope

If you created a server or database filter or list-based configuration scope and want to use it with a role assignment, you need to include the scope in the command used to assign the role to a USG by using the *CustomConfigWriteScope* parameter.

Before you can add a scope to a role assignment, you need to create one. For more information, see [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md).

Use the following syntax to assign a role to a USG with a configuration scope.

```powershell
New-ManagementRoleAssignment -Name <assignment name> -SecurityGroup <USG> -Role <role name> -CustomConfigWriteScope <role scope name>
```

This example assigns the Exchange Servers role to the MailboxAdmins USG and applies the Mailbox Servers scope.

```powershell
New-ManagementRoleAssignment -Name "Exchange Servers_MailboxAdmins" -SecurityGroup MailboxAdmins -Role "Exchange Servers" -CustomConfigWriteScope "Mailbox Servers"
```

The preceding example shows how to add a role assignment with a server configuration scope. The syntax to add a database configuration scope is the same. You specify the name of a database scope instead of a server scope.

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Create a role assignment with an OU scope

If you want to scope a role's write scope to an organizational unit (OU), you can specify the OU in the *RecipientOrganizationalUnitScope* parameter directly. If you use the *RecipientOrganizationalUnitScope* parameter, you can't use the *CustomRecipientWriteScope* parameter.

Use the following syntax to assign a role to a USG and restrict the write scope of a role to a specific OU.

```powershell
New-ManagementRoleAssignment -Name <assignment name> -SecurityGroup <USG> -Role <role name> -RecipientOrganizationalUnitScope <OU>
```

This example assigns the Mail Recipients role to the SalesRecipientAdmins USG and scopes the assignment to the sales/users OU in the contoso.com domain.

```powershell
New-ManagementRoleAssignment -Name "Mail Recipients_SalesRecipientAdmins" -SecurityGroup SalesRecipientAdmins -Role "Mail Recipients" -RecipientOrganizationalUnitScope contoso.com/sales/users
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Create a role assignment with an exclusive recipient or configuration scope

To create an exclusive role assignment with an exclusive recipient or configuration scope, the same procedures provided in the Create a role assignment with a recipient filter-based scope and Create a role assignment with a server or database filter or list-based configuration scope sections can be used. The only difference is that when you create a role assignment with an exclusive scope, you must specify the following exclusive parameters depending on whether you're using an exclusive recipient scope or an exclusive configuration scope:

- **Exclusive recipient scopes**: Use the *ExclusiveRecipientWriteScope* parameter instead of the *CustomRecipientWriteScope* parameter.

- **Exclusive configuration scopes**: Use the *ExclusiveConfigWriteScope* parameter instead of the *CustomConfigWriteScope* parameter.

When you perform this procedure, the role assignees assigned the role can perform actions against the objects included in the exclusive scope. For more information about exclusive scopes, see [Understanding exclusive scopes](understanding-exclusive-scopes-exchange-2013-help.md).

You can't create a role assignment with both exclusive and regular scopes.

This example assigns the Mail Recipients role to the Protected User Admins USG and applies the Protected Users exclusive scope.

```powershell
New-ManagementRoleAssignment -Name "Mail Recipients_Protected User Admins" -SecurityGroup "Protected User Admins" -Role "Mail Recipients" -ExclusiveRecipientWriteScope "Protected Users"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).
