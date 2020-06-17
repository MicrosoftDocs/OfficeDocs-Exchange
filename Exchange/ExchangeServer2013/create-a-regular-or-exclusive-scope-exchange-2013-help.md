---
title: 'Create a regular or exclusive scope: Exchange 2013 Help'
TOCTitle: Create a regular or exclusive scope
ms:assetid: b97a5be3-15cc-4954-ba30-a824a95e21be
ms:mtpsurl: https://technet.microsoft.com/library/Dd351083(v=EXCHG.150)
ms:contentKeyID: 49289387
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a regular or exclusive scope

_**Applies to:** Exchange Server 2013_

Management role scopes determine what objects are made available to a user so that the objects can be changed using the cmdlets and parameters assigned to them. By adding a management scope, you can configure management role assignments so users can administer specific servers, databases, recipients, and other objects in your organization while being restricted from changing other objects.

> [!IMPORTANT]
> When you create a regular or exclusive scope, you override the write scope that's defined on the management role you're assigning. You can't override the read scope that's configured on the management role.

You can create a custom management scope and add or change a management role assignment. If you want to create a management role assignment with a prebuilt or organizational unit (OU) management scope, see [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md).

For more information about management role scopes and assignments in Microsoft Exchange Server 2013, see the following topics:

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for other management tasks related to scopes? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management scopes" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create a custom scope

To create a custom scope, choose one of the following types of scopes.

### Recipient filter scope

Recipient filter-based scopes are created by using the *RecipientRestrictionFilter* parameter on the **New-ManagementScope** cmdlet. When you create a recipient filter, in addition to the recipient properties to filter, you can specify the OU in which the filter query runs. When you specify a base OU, you further restrict the write scope of the role.

For more information about management scope filters, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

Use the following syntax to create a domain restriction filter scope with a base OU.

```powershell
New-ManagementScope -Name <scope name> -RecipientRestrictionFilter <filter query> [-RecipientRoot <OU>]
```

This example creates a scope that includes all mailboxes within the contoso.com/Sales OU.

```powershell
New-ManagementScope -Name "Mailboxes in Sales OU" -RecipientRestrictionFilter "RecipientType -eq 'UserMailbox'" -RecipientRoot "contoso.com/Sales OU"
```

> [!NOTE]
> You can omit the <EM>RecipientRoot</EM> parameter if you want the filter to apply to the entire implicit read scope of the management role and not just within a specific OU.

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Server filter configuration scope

Server filter-based configuration scopes are created by using the *ServerRestrictionFilter* parameter on the **New-ManagementScope** cmdlet. A server filter enables you to create a scope that applies only to the servers that match the filter you specify.

For more information about management scope filters and for a list of filterable server properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

Use the following syntax to create a server filter scope.

```powershell
New-ManagementScope -Name <scope name> -ServerRestrictionFilter <filter query>
```

This example creates a scope that includes all the servers within the 'CN=Redmond,CN=Sites,CN=Configuration,DC=contoso,DC=com' AD (Active Directory) site.

```powershell
New-ManagementScope -Name "Servers in Seattle AD site" -ServerRestrictionFilter "ServerSite -eq 'CN=Redmond,CN=Sites,CN=Configuration,DC=contoso,DC=com'"
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Server list configuration scope

Server list-based configuration scopes are created by using the *ServerList* parameter on the **New-ManagementScope** cmdlet. A server list scope enables you to create a scope that applies only to the servers you specify in a list.

Use the following syntax to create a server list scope.

```powershell
New-ManagementScope -Name <scope name> -ServerList <server 1>, <server 2...>
```

This example creates a scope that applies only to MBX1, MBX3, and MBX5.

```powershell
New-ManagementScope -Name "Mailbox servers" -ServerList MBX1,MBX3,MBX5
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Database filter configuration scope

Database filter-based configuration scopes are created by using the *DatabaseRestrictionFilter* parameter on the **New-ManagementScope** cmdlet. A database filter enables you to create a scope that applies only to the databases that match the filter you specify.

> [!IMPORTANT]
> Role assignments associated with database scopes are applied only to users who connect to servers running Microsoft Exchange Server 2010 Service Pack&nbsp;1 (SP1) or later or Exchange 2013. If a user assigned a role assignment associated with a database scope connects to a pre-Exchange 2010 SP1 server, the role assignment isn't applied to the user, and the user won't be granted any permissions provided by the role assignment.

For more information about management scope filters and for a list of filterable database properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

Use the following syntax to create a database restriction filter.

```powershell
New-ManagementScope -Name <scope name> -DatabaseRestrictionFilter <filter query>
```

This example creates a scope that includes all the databases that contain the string "Executive" in the **Name** property of the database.

```powershell
New-ManagementScope -Name "Executive Databases" -DatabaseRestrictionFilter "Name -Like '*Executive*'"
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Database list configuration scope

Database list-based configuration scopes are created by using the *DatabaseList* parameter on the **New-ManagementScope** cmdlet. A database list scope enables you to create a scope that applies only to the databases you specify in a list.

> [!IMPORTANT]
> Role assignments associated with database scopes are applied only to users who connect to servers running Microsoft Exchange Server 2010 Service Pack&nbsp;1 (SP1) or later or Exchange 2013. If a user assigned a role assignment associated with a database scope connects to a pre-Exchange 2010 SP1 server, the role assignment isn't applied to the user, and the user won't be granted any permissions provided by the role assignment.

Use the following syntax to create a database list scope.

```powershell
New-ManagementScope -Name <scope name> -DatabaseList <database 1>, <database 2...>
```

This example creates a scope that applies only to the databases Database 1, Database 2, and Database 3.

```powershell
New-ManagementScope -Name "Primary databases" -DatabaseList "Database 1", "Database 2", "Database 3"
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

### Exclusive scope

Any scope that you create with the **New-ManagementScope** cmdlet can be designated as an exclusive scope. To create an exclusive scope, you use the same commands in one of the preceding sections to create a recipient filter-based scope, server filter-based scope, server list-based scope, database filter-based scope, or database list-based scope, and then add the *Exclusive* switch to the command.

> [!WARNING]
> When you create exclusive management scopes, only the role assignees assigned exclusive scopes that contain objects to be modified can access those objects. Only those administrators assigned a role with the exclusive scope can access these exclusive, or protected, objects.

This example creates an exclusive recipient filter-based scope that matches any user in the Executives department.

```powershell
New-ManagementScope "Executive Users Exclusive Scope" -RecipientRestrictionFilter "Department -Eq 'Executives'" -Exclusive
```

By default, when an exclusive scope is created, you're required to acknowledge that you created an exclusive scope and that you're aware of the impact that an exclusive scope has on existing role assignments that aren't exclusive. If you want to suppress the warning, you can use the *Force* switch. This example creates the same scope as the previous example, but without a warning.

```powershell
New-ManagementScope "Executive Users Exclusive Scope" -RecipientRestrictionFilter "Department -Eq 'Executives'" -Exclusive -Force
```

For detailed syntax and parameter information, see [New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope).

## Step 2: Add or change a management role assignment

After you create the scope, you must add it to a new or existing management role assignment.

If you create a management scope and want to add it to a new management role assignment that you're going to create, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

If you create a management role scope and want to add it to an existing management role assignment, see the following topics:

- [Manage role assignment policies](manage-role-assignment-policies-exchange-2013-help.md)

- [Change a role assignment](change-a-role-assignment-exchange-2013-help.md)
