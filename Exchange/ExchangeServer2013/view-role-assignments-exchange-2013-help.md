---
title: 'View role assignments: Exchange 2013 Help'
TOCTitle: View role assignments
ms:assetid: 0be4def9-af6d-476a-9c97-7155ae11b587
ms:mtpsurl: https://technet.microsoft.com/library/Dd335086(v=EXCHG.150)
ms:contentKeyID: 49289159
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View role assignments

_**Applies to:** Exchange Server 2013_

Management role assignments assign a management role to a role assignee. For more information about management role assignments in Microsoft Exchange Server 2013, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role assignments" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- This topic makes use of pipelining and the **Format-List** cmdlet. For more information about these concepts, see the following topics:

  - [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

  - [Working with command output](working-with-command-output-exchange-2013-help.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## View a list of all role assignments

You can view a list of all role assignments configured in your organization by running the **Get-ManagementRoleAssignment** cmdlet. If you want to retrieve a list of role assignments that match a partial string that you specify, use wildcard characters (\*). This example retrieves a list of all the role assignments that start with the string "Tier 1".

```powershell
Get-ManagementRoleAssignment "Tier 1*"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View the details of a specific role assignment

You can view the details of a role assignment by piping the results of the **Get-ManagementRoleAssignment** cmdlet to the **Format-List** cmdlet. Use the following syntax.

```powershell
Get-ManagementRoleAssignment <assignment name> | Format-List
```

This example retrieves the details of the Help Desk Assignment role assignment.

```powershell
Get-ManagementRoleAssignment "Help Desk Assignment" | Format-List
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View the list of role assignments assigned to a specific role assignee

To view a list of role assignments associated with a management role group, role, or role assignment policy, or associated with a user or universal security group (USG), use the following syntax.

```powershell
Get-ManagementRoleAssignment -RoleAssignee <role assignee name>
```

This example retrieves all of the role assignments associated with the Server Management role group.

```powershell
Get-ManagementRoleAssignment -RoleAssignee "Server Management"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View the role assignments associated with a specific role

Each role can have multiple role assignments. You can use the **Get-ManagementRoleAssigment** cmdlet to view a list of role assignments associated with a specified role.

To view a list of role assignments associated with a specified role, use the following syntax.

```powershell
Get-ManagementRoleAssignment -Role <role name>
```

This example retrieves all of the role assignments associated with the Mail Recipients role.

```powershell
Get-ManagementRoleAssignment -Role "Mail Recipients"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View a list of role assignments that use a specific predefined scope

To view a list of role assignments that use a specific predefined scope, use the following syntax.

```powershell
Get-ManagementRoleAssignment -RecipientWriteScope < MyGAL | MyDistributionGroups | Organization | Self | CustomRecipientScope | ExecutiveRecipientScope >
```

This example retrieves all of the role assignments that use the Organization predefined scope.

```powershell
Get-ManagementRoleAssignment -RecipientWriteScope Organization
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View a list of role assignments that have been scoped to a specific OU

To view a list of role assignments that have been scoped to a specific organizational unit (OU), use the following syntax.

```powershell
Get-ManagementRoleAssignment -RecipientOrganizationalUnitScope <OU>
```

This example retrieves all of the role assignments that have been scoped to the North America\\Engineering\\Users OU in the contoso.com domain.

```powershell
Get-ManagementRoleAssignment -RecipientOrganizationalUnitScope "contoso.com/North America/Engineering/Users"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View a list of assignments that use a specific custom scope

To view a list of role assignments that use a specific custom scope, you need to first determine whether the scope is a recipient scope, configuration scope, exclusive recipient scope, or exclusive configuration scope. Each type of scope uses a different parameter on the **Get-ManagementRoleAssignment** cmdlet. The following lists each scope and its associated parameter:

- **Recipient scopes**: *CustomRecipientWriteScope*

- **Configuration scopes**: *CustomConfigWriteScope*

- **Exclusive recipient scopes**: *ExclusiveRecipientWriteScope*

- **Exclusive configuration scopes**: *ExclusiveConfigWriteScope*

The syntax for each parameter is the same. Specify the name of the scope with the parameter that matches the type of scope it is.

This example retrieves all of the role assignments that use the Vancouver Recipients recipient scope.

```powershell
Get-ManagementRoleAssignment -CustomRecipientWriteScope "Vancouver Recipients"
```

This example retrieves all of the role assignments that use the Seattle AD Site exclusive configuration scope.

```powershell
Get-ManagementRoleAssignment -ExclusiveConfigWriteScope "Seattle AD Site"
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View a list of exclusive or regular scopes

To view a list of exclusive or regular role assignments, use the following syntax.

```powershell
Get-ManagementRoleAssignment -Exclusive < $True | $False >
```

For example, to view a list of exclusive scopes, run the following command:

```powershell
Get-ManagementRoleAssignment -Exclusive $True
```

This example retrieves a list of regular scopes without any exclusive scopes.

```powershell
Get-ManagementRoleAssignment -Exclusive $False
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View who can modify a specific recipient or server

To view a list of role assignments that can modify a specific recipient or server, use the *WritableRecipient* and *WritableServer* parameters. Specify the name of the recipient with the *WritableRecipient* parameter, and the name of the server with the *WritableServer* parameter.

This example retrieves a list of role assignments that can modify the recipient Brian.

```powershell
Get-ManagementRoleAssignment -WritableRecipient "Brian"
```

You can combine the *WritableRecipient* and *WritableServer* parameters with other parameters, such as the *RoleAssignee* parameter and the *GetEffectiveUsers* switch to refine your query and expand any role groups or USGs. This example retrieves all of the users who can modify the server EX02 and who are assigned the Server Management role group.

```powershell
Get-ManagementRoleAssignment -WritableServer EX02 -RoleAssignee "Server Management" -GetEffectiveUsers
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View the users who receive permissions from an assignment via a role group or USG

To view a list of users that receive permissions from a role assignment, use the following syntax.

```powershell
Get-ManagementRoleAssignment <assignment name> -GetEffectiveUsers
```

This example retrieves a list of users in the Help Desk Assignment role assignment.

```powershell
Get-ManagementRoleAssignment "Help Desk Assignment" -GetEffectiveUsers
```

You can also combine the *GetEffectiveUsers* switch with several other parameters on the **Get-ManagementRoleAssignment** cmdlet to expand the role groups and USGs that the role assignments are assigned to. For an example of how the *GetEffectiveUsers* switch is used with other parameters, see "View who can modify a specific recipient or server" earlier in this topic.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## View a list of role assignments that are enabled or disabled

To view a list of role assignments that are enabled or disabled, use the following syntax.

```powershell
Get-ManagementRoleAssignment -Enabled < $True | $False >
```

This example retrieves a list of role assignments that are disabled.

```powershell
Get-ManagementRoleAssignment -Enabled $False
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).
