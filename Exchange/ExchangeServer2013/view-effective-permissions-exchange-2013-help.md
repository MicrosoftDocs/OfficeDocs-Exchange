---
title: 'View effective permissions: Exchange 2013 Help'
TOCTitle: View effective permissions
ms:assetid: ae6cb7cf-f998-44a6-a69a-02ad736c8260
ms:mtpsurl: https://technet.microsoft.com/library/Dd638167(v=EXCHG.150)
ms:contentKeyID: 49289376
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View effective permissions

_**Applies to:** Exchange Server 2013_

Permissions in Microsoft Exchange Server 2013 are granted using management roles that are assigned to management role groups, management role assignment policies, universal security groups (USGs), or directly to users. Users are granted the permissions if they're members of the role groups or USGs, or are assigned role assignment policies.

Most permissions are granted based on role group membership or the assignment of assignment policies to end users. Although using role groups and assignment policies makes it easy to grant permissions to large numbers of users, you may not be aware of who is a member of a role group, or who has been assigned an assignment policy. This is where the *GetEffectiveUsers* switch on the **Get-ManagementRoleAssignment** cmdlet is useful. It shows you what users are granted the permissions given by a management role through the role groups, assignment policies, and USGs that are assigned to them.

The *GetEffectiveUsers* switch is used with the **Get-ManagementRoleAssignment** cmdlet when the *Role* parameter is used. By specifying this switch with a particular role, the **Get-ManagementRoleAssignment** cmdlet examines all the role assignees assigned to the role, such as role groups, assignment policies, and USGs, and lists the members of each.

> [!NOTE]
> The <EM>GetEffectiveUser</EM> switch doesn't list users that are members of a linked foreign role group. Instead of a list of users, if a linked role group is found, <STRONG>All Linked Group Members</STRONG> is displayed. For more information about permissions in multiple forests, see <A href="understanding-multiple-forest-permissions-exchange-2013-help.md">Understanding multiple-forest permissions</A>.

For more information about management roles, role groups, and assignment policies, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md). For more information about management role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

Looking for other management tasks related to managing permissions? Check out [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" or "Role assignment policy" entries in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- The procedures in this topic can only be performed in the Shell. You can't use the Exchange admin center (EAC) to view effective permissions.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to list all effective users

To list all the users that are granted the permissions provided by a management role, use the following syntax.

```powershell
Get-ManagementRoleAssignment -Role <role name> -GetEffectiveUsers
```

This example lists all the users that are granted permissions provided by the Mail Recipients role.

```powershell
Get-ManagementRoleAssignment -Role "Mail Recipients" -GetEffectiveUsers
```

If you want to change what properties are returned in the list or export the list to a comma-separated value (.csv) file, see Use the Shell to customize output and display it later in this topic.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## Use the Shell to find a specific user on a role

To find a specific user that's been granted permissions by a management role, you must use the **Get-ManagementRoleAssignment** cmdlet to retrieve a list of all effective users, and then pipe the output of the cmdlet to the **Where** cmdlet. The **Where** cmdlet filters the output and returns only the user you specified. Use the following syntax.

```powershell
Get-ManagementRoleAssignment -Role <role name> -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "<name of user>"}
```

This example finds the user David Strome on the Journaling role.

```powershell
Get-ManagementRoleAssignment -Role Journaling -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "David Strome"}
```

If you want to change what properties are returned in the list or export the list to a .csv file, see Use the Shell to customize output and display it later in this topic.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## Use the Shell to find a specific user on all roles

To know every role that a user receives permissions from, you must use the **Get-ManagementRoleAssignment** cmdlet to retrieve all effective users on all management roles and then pipe the output of the cmdlet to the **Where** cmdlet. The **Where** cmdlet filters the output and returns only the role assignments that grant the user permissions.

```powershell
Get-ManagementRoleAssignment -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "<name of user>"}
```

This example finds all the role assignments that grant permissions to the user Kim Akers.

```powershell
Get-ManagementRoleAssignment -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "Kim Akers"}
```

If you want to change what properties are returned in the list or export the list to a CSV file, see Use the Shell to customize output and display it later in this topic.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## Use the Shell to customize output and display it

The default output of the **Get-ManagementRoleAssignment** cmdlet might not have the information you want. The output of the cmdlet contains many more properties that you can access. The following are some of the properties that could be useful:

- **EffectiveUserName**: Name of the user.

- **Role**: Role that's granting the permissions.

- **RoleAssigneeName**: Role group, assignment policy, or USG that's assigned to the role and contains the user in the `EffectiveUserName` property.

- **RoleAssigneeType**: Indicates whether the role assignment is to a role group, assignment policy, USG, or user.

- **AssignmentMethod**: Indicates whether the assignment between the role and the role assignee is direct or indirect.

- **CustomRecipientWriteScope**: Indicates the custom recipient write scope, if any, that was applied to the role assignment when it was created. The scope specified in this property overrides the implicit recipient write scope specified in the `RecipientWriteScope` property.

- **CustomConfigWriteScope**: Indicates the custom configuration write scope, if any, that was applied to the role assignment when it was created. The scope specified in this property overrides the implicit configuration write scope specified in the `ConfigWriteScope` property.

- **RecipientReadScope**: Indicates the implicit recipient read scope that's applied to the role.

- **RecipientWriteScope**: Indicates the implicit recipient write scope that's applied to the role.

- **ConfigReadScope**: Indicates the implicit configuration read scope that's applied to the role.

- **ConfigWriteScope**: Indicates the implicit configuration write scope that's applied to the role.

To select the properties you want to display in your list, you use commands similar to those used in the Use the Shell to list all effective users, Use the Shell to find a specific user on a role, and Use the Shell to find a specific user on all roles sections. The difference is that you pipe the results of those commands to the **Format-Table** or **Select-Object** cmdlets. The **Format-Table** cmdlet is useful to output the list of results to your screen. The **Select-Object** cmdlet is useful to output the list of your results to a .csv file.

Both cmdlets let you specify the properties you want to see and in the order you want to see them. The **Format-Table** cmdlet gives you more options when you list results to a screen while **Select-Object** doesn't modify the output in any way, which is useful when piping the list to a .csv file.

For more information about the **Format-Table** and **Select-Object** cmdlets, see [Working with command output](working-with-command-output-exchange-2013-help.md).

## Output a customized list to your screen

1. Choose the information you want to see and find the associated command from one of the following procedures:

   - Use the Shell to list all effective users

   - Use the Shell to find a specific user a role

   - Use the Shell to find a specific user on all roles

2. Choose the properties you want to see in your list.

3. Use the following syntax to view the list.

   ```powershell
   <command to retrieve list > | Format-Table <property 1>, <property 2>, <property ...>
   ```

This example finds the user David Strome on all roles, and displays the `EffectiveUserName`, `Role`, `CustomRecipientWriteScope`, and `CustomConfigWriteScope` properties.

```powershell
Get-ManagementRoleAssignment -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "David Strome"} | Format-Table EffectiveUserName, Role, CustomRecipientWriteScope, CustomConfigWriteScope
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## Output a customized list to a .csv file

To export a list to a .csv file, you need to pipe the results of the **Get-ManagementRoleAssignment** command from the appropriate procedure listed previously to the **Select-Object** cmdlet. The output of the **Select-Object** cmdlet is then piped to the **Export-CSV** cmdlet, which saves the .csv output to a file name you specify.

1. Choose the information you want to see and find the associated command from one of the following procedures:

   - Use the Shell to list all effective users

   - Use the Shell to find a specific user a role

      - Use the Shell to find a specific user on all roles

2. Choose the properties you want to see in your list.

3. Use the following syntax to export the list to a .csv file.

   ```powershell
   <command to retrieve list > | Select-Object <property 1>, <property 2>, <property ...> | Export-CSV <filename>
   ```

This example finds the user David Strome on all roles, and displays the `EffectiveUserName`, `Role`, `CustomRecipientWriteScope`, and `CustomConfigWriteScope` properties.

```powershell
Get-ManagementRoleAssignment -GetEffectiveUsers | Where {$_.EffectiveUserName -Eq "David Strome"} | Select-Object EffectiveUserName, Role, CustomRecipientWriteScope, CustomConfigWriteScope | Export-CSV c:\output.csv
```

You can now view the .csv file in a viewer of your choice.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).
