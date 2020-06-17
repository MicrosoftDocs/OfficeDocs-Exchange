---
title: 'Change a role scope: Exchange 2013 Help'
TOCTitle: Change a role scope
ms:assetid: 9180e1e0-c352-4ccd-8da6-885a2e309867
ms:mtpsurl: https://technet.microsoft.com/library/Dd298145(v=EXCHG.150)
ms:contentKeyID: 49289346
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change a role scope

_**Applies to:** Exchange Server 2013_

Management role scopes determine what objects are made available to a user so that the objects can be changed using the cmdlets and parameters assigned to them. By changing a scope, you can change what objects are made available to users to create, change, or remove.

You can change a custom management scope. You can change either exclusive or regular scopes. If you change an exclusive scope, the new scope takes effect immediately. If you want to change a management role assignment with a predefined or organizational unit (OU) management scope, see [Change a role assignment](change-a-role-assignment-exchange-2013-help.md).

For more information about management role scopes and assignments in Microsoft Exchange Server 2013, see the following topics:

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for other management tasks related to role scopes? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management scopes" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Change the name of a scope

To change the name of a scope, use the following syntax.

```powershell
Set-ManagementScope <current scope name> -Name <new scope name>
```

This example changes the Seattle Servers scope to Seattle Exchange Servers.

```powershell
Set-ManagementScope "Seattle Servers" -Name "Seattle Exchange Servers"
```

For detailed syntax and parameter information, see [Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope).

## Change a recipient filter on a scope

To change the recipient filter on a scope, use the following syntax.

```powershell
Set-ManagementScope <scope name> -RecipientRestrictionFilter "<new recipient filter>"
```

This example changes the recipient filter to match all the recipient objects where the **Company** property is set to contoso.

```powershell
Set-ManagementScope "Company Scope" -RecipientRestrictionFilter "Company -eq 'contoso'"
```

For detailed syntax and parameter information, see [Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope).

For more information about recipient filters and to see a list of filterable recipient properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

## Change the organizational unit root on a scope

To change the OU root on a scope, use the following syntax.

```powershell
Set-ManagementScope <scope name> -RecipientRoot <OU>
```

This example changes the OU root to the North America/Sales Sales Users OU under the contoso.com domain.

```powershell
Set-ManagementScope "Sales Users" -RecipientRoot "contoso.com/North America/Sales"
```

For detailed syntax and parameter information, see [Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope).

## Change a server filter on a scope

To change the server filter on a scope, use the following syntax.

```powershell
Set-ManagementScope <scope name> -ServerRestrictionFilter "<new server filter>"
```

This example changes the server filter to match all the server objects where the **ServerSite** property is set to 'CN=Redmond,CN=Sites,CN=Configuration,DC=contoso,DC=com'.

```powershell
Set-ManagementScope "Company Scope" -ServerRestrictionFilter "ServerSite -eq 'CN=Redmond,CN=Sites,CN=Configuration,DC=contoso,DC=com'"
```

For detailed syntax and parameter information, see [Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope).

For more information about server filters and to see a list of filterable server properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

## Change the server list on a scope

You can't change the list of servers on a scope. If you need to change the server list, you need to do the following:

1. If needed, retrieve the current server list in the scope to be replaced by using the "View a specific scope" procedure in the [View role scopes](view-role-scopes-exchange-2013-help.md) topic.

2. Create a scope with the new server list by using the "Step 1: Create a custom scope" procedure in the [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md) topic.

3. Change all the management role assignments that use the old scope to use the new scope by using the "Use the Shell to change the server filter or list-based scope on a role assignment" procedure in the [Change a role assignment](change-a-role-assignment-exchange-2013-help.md) topic.

4. Remove the old scope by using the procedure in the [Remove a role scope](remove-a-role-scope-exchange-2013-help.md) topic.

## Change a database filter on a scope

To change the database filter on a scope, use the following syntax.

```powershell
Set-ManagementScope <scope name> -DatabaseRestrictionFilter "<new database filter>"
```

This example changes the database filter to match all the database objects where the **Name** property contains the string "Executive".

```powershell
Set-ManagementScope "Database Executive Scope" -DatabaseRestrictionFilter "Name -Like '*Executive*'"
```

For detailed syntax and parameter information, see [Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope).

For more information about database filters and to see a list of filterable database properties, see [Understanding management role scope filters](understanding-management-role-scope-filters-exchange-2013-help.md).

## Change the database list on a scope

You can't change the list of databases on a scope. If you need to change the database list, you need to do the following:

1. If needed, retrieve the current database list in the scope to be replaced by using the "View a specific scope" procedure in the [View role scopes](view-role-scopes-exchange-2013-help.md) topic.

2. Create a scope with the new database list by using the "Step 1: Create a custom scope" procedure in the [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md) topic.

3. Change all the management role assignments that use the old scope to use the new scope by using the "Use the Shell to change the database filter or list-based scope on a role assignment" procedure in the [Change a role assignment](change-a-role-assignment-exchange-2013-help.md) topic.

4. Remove the old scope by using the procedure in the [Remove a role scope](remove-a-role-scope-exchange-2013-help.md) topic.
