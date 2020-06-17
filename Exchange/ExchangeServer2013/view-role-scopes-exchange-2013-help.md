---
title: 'View role scopes: Exchange 2013 Help'
TOCTitle: View role scopes
ms:assetid: 0bb3a434-6651-473a-94eb-4eb9a34e6f70
ms:mtpsurl: https://technet.microsoft.com/library/Dd335084(v=EXCHG.150)
ms:contentKeyID: 49289158
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View role scopes

_**Applies to:** Exchange Server 2013_

Management role scopes determine what objects are made available to a user so that the objects can be changed using the cmdlets and parameters assigned to them. You can view scopes to determine what scopes have been added to your organization, the configuration of a specific scope, or what scopes are orphans.

For more information about management role scopes in Microsoft Exchange Server 2013, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

Looking for other management tasks related to role scopes? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management scopes" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- This topic makes use of pipelining and the **Format-List** cmdlet. For more information about these concepts, see the following topics:

  - [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

  - [Working with command output](working-with-command-output-exchange-2013-help.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## View a specific scope

You can view the details of a scope by piping the output of the **Get-ManagementScope** cmdlet to the **Format-List** cmdlet.

To view the details of a specific scope, use the following syntax.

```powershell
Get-ManagementScope <scope name> | Format-List
```

This example retrieves the details of the Seattle Servers scope.

```powershell
Get-ManagementScope "Seattle Servers" | Format-List
```

For detailed syntax and parameter information, see [Get-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementScope).

## List all scopes

This example retrieves a list of scopes in your organization.

```powershell
Get-ManagementScope
```

This cmdlet retrieves both exclusive and regular scopes. If you only want to return exclusive scopes or regular scopes, see "List all exclusive or regular scopes only" later in this topic.

For detailed syntax and parameter information, see [Get-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementScope).

## List all orphaned scopes

*Orphaned scopes* are scopes that haven't been associated with any management role assignments.

This examples retrieves a list of orphaned scopes.

```powershell
Get-ManagementScope -Orphan
```

For detailed syntax and parameter information, see [Get-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementScope).

## List all exclusive or regular scopes only

By default, the **Get-ManagementScope** cmdlet returns a list of scopes that contains both exclusive and regular scopes. If you want to return only exclusive scopes or only regular scopes use the following syntax.

```powershell
Get-ManagementScope -Exclusive < $true | $false >
```

This example returns only exclusive scopes.

```powershell
Get-ManagementScope -Exclusive $true
```

This example returns a list of regular scopes only.

```powershell
Get-ManagementScope -Exclusive $false
```

For detailed syntax and parameter information, see [Get-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementScope).
