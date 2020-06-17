---
title: 'View a role: Exchange 2013 Help'
TOCTitle: View a role
ms:assetid: 1875b15f-22db-4ede-b310-ea894d6211c8
ms:mtpsurl: https://technet.microsoft.com/library/Dd335117(v=EXCHG.150)
ms:contentKeyID: 49289181
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View a role

_**Applies to:** Exchange Server 2013_

Management roles can be listed in a variety of ways, depending on the information you want. For example, you can choose to return only roles of a specific role type, roles that contain only specific cmdlets and parameters, or view the details of a specific management role. For more information about management roles in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

If you want to view a list of all management role entries on a role, see [View role entries](view-role-entries-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- This topic makes use of pipelining and the **Format-List** and **Format-Table** cmdlets. For more information about these concepts, see the following topics:

  - [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

  - [Working with command output](working-with-command-output-exchange-2013-help.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## View a specific management role

You can view the details of a specific role by retrieving a specific role using the **Get-ManagementRole** cmdlet and piping the output to the **Format-List** cmdlet.

To view the details of a specific role, use the following syntax.

```powershell
Get-ManagementRole <role name> | Format-List
```

This example retrieves the details about the Mail Recipients management role.

```powershell
Get-ManagementRole "Mail Recipients" | Format-List
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List all management roles

You can view a list of all the management roles in your organization by not specifying any roles when you run the **Get-ManagementRole** cmdlet. By default, the role name and role type of each role are included in the results.

This example returns a list of all roles in your organization.

```powershell
Get-ManagementRole
```

To return a list of specific properties for all the roles in your organization, you can pipe the results of the **Format-Table** cmdlet and specify the properties you want in the list of results. Use the following syntax.

```powershell
Get-ManagementRole | Format-Table <property 1>, <property 2...>
```

This example returns a list of all the roles in your organization and includes the **Name** property and any property with the word **Implicit** at the beginning of the property name.

```powershell
Get-ManagementRole | Format-Table Name, Implicit*
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List management roles that contain a specific cmdlet

You can return a list of roles that contain a cmdlet that you specify by using the *Cmdlet* parameter on the **Get-ManagementRole** cmdlet.

To return a list of roles that contain the cmdlet you specify, use the following syntax.

```powershell
Get-ManagementRole -Cmdlet <cmdlet>
```

This example returns a list of roles that contain the **New-Mailbox** cmdlet.

```powershell
Get-ManagementRole -Cmdlet New-Mailbox
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List management roles that contain a specific parameter

You can return a list of roles that contain one or more specified parameters by using the *CmdletParameters* parameter on the **Get-ManagementRole** cmdlet. Only roles that contain all the parameters you specify are returned.

When you use the *CmdletParameters* parameter, you can choose to include the *Cmdlet* parameter. If you include the *Cmdlet* parameter, only roles that contain the parameters you specify on the cmdlet you specify are returned. If you don't include the *Cmdlet* parameter, roles that contain the parameters you specify, regardless of the cmdlet they're on, are returned.

To return a list of roles that contain the parameters you specify, use the following syntax.

```powershell
Get-ManagementRole [-Cmdlet <cmdlet>] -CmdletParameters <parameter 1>, <parameter 2...>
```

This example returns a list of roles that contain the *Database* and *Server* parameters, regardless of the cmdlets they exist on.

```powershell
Get-ManagementRole -CmdletParameters Database, Server
```

This example returns a list of roles where the *EmailAddresses* parameter exists only on the **Set-Mailbox** cmdlet.

```powershell
Get-ManagementRole -Cmdlet Set-Mailbox -CmdletParameters EmailAddresses
```

You can also use the wildcard character (\*) with either the *Cmdlet* or *CmdletParameters* parameters to match partial cmdlet or parameter names.

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List management roles of a specific role type

You can return a list of roles based on a specified role type by using the *RoleType* parameter on the **Get-ManagementRole** cmdlet.

To return a list of roles that match the role type you specify, use the following syntax.

```powershell
Get-ManagementRole -RoleType <roletype>
```

This example returns a list of roles based on the `UmMailboxes` role type.

```powershell
Get-ManagementRole -RoleType UmMailboxes
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List the immediate child roles of a parent role

You can return a list of roles that are the immediate children of the specified parent role by using the *GetChildren* parameter on the **Get-ManagementRole** cmdlet. Only roles that contain the role you specify as the parent role are returned.

To return a list of the immediate children roles of a parent role, use the following syntax.

```powershell
Get-ManagementRole <parent role name> -GetChildren
```

This example returns a list of immediate children of the Disaster Recovery role.

```powershell
Get-ManagementRole "Disaster Recovery" -GetChildren
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).

## List all child roles below a parent role

You can return a list of the entire chain of roles from a specified parent role to the last child role by using the *Recurse* parameter on the **Get-ManagementRole** cmdlet. The *Recurse* parameter tells the **Get-ManagementRole** cmdlet to recurse down through every parent and child relationship it finds until it reaches the last child role. The parent role is included in the list that's returned.

This example returns a list of all the child roles of a parent role.

```powershell
Get-ManagementRole <parent role name> -Recurse
```

This example returns all the child roles of the Mail Recipients role.

```powershell
Get-ManagementRole "Mail Recipients" -Recurse
```

For detailed syntax and parameter information, see [Get-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRole).
