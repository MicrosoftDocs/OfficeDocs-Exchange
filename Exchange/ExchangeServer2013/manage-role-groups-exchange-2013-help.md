---
title: 'Manage role groups: Exchange 2013 Help'
TOCTitle: Manage role groups
ms:assetid: ab9b7a3b-bf67-4ba1-bde5-8e6ac174b82c
ms:mtpsurl: https://technet.microsoft.com/library/JJ657480(v=EXCHG.150)
ms:contentKeyID: 49289369
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage role groups

_**Applies to:** Exchange Server 2013_

This topic shows you how to add, remove, copy, and view management role groups in Microsoft Exchange Server 2013. It also shows you how to add, remove, and list management roles on role groups and how to change management scopes and delegates on role groups. For more information about role groups in Exchange 2013, see [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md).

For additional management tasks related to role groups, see [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 to 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a role group

If you want to customize the permissions that you can assign to a group of users, create a new custom management role group.

## Use the EAC to create a role group

1. In the Exchange admin center (EAC), navigate to **Permissions** \> **Admin Roles** and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New role group** window, provide a name for the new role group.

3. You can either select the roles that you want to be assigned to the role group and the members you want to be added to the role group now, or you can do this at another time.

4. Select the write scope that you want to apply to the new role group.

5. Click **Save** to create the role group.

## Use the Shell to create a role group

To create a role group, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup#examples) section in [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## How do you know this worked?

To verify that you have successfully created a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Verify that the new role group appears in the role group list and then select it.

3. Verify that members, assigned roles, and scope that you specified on the new role group are listed in the role group details pane.

## Copy a role group

## Use the EAC to copy a role group

If you have a role group that contains the permissions you want to grant to users, but you want to apply a different management scope, or remove or add one or two management roles without having to add all the other roles manually, you can copy the existing role group.

> [!IMPORTANT]
> You can't use the EAC to copy a role group if you've used the Exchange Management Shell to configure multiple management role scopes or exclusive scopes on the role group. If you've configured multiple scopes or exclusive scopes on the role group, you must use the Shell procedures later in this topic to copy the role group. For more information about management role scopes, see <A href="understanding-management-role-scopes-exchange-2013-help.md">Understanding management role scopes</A>.

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to copy and then click **Copy** ![Copy Icon](images/JJ657480.ed7f7abf-39d8-4f43-a918-ccb3bff87ef5(EXCHG.150).gif "Copy Icon").

3. In the **New role group** window, provide a name for the new role group.

4. Review the roles that have been copied to the new role group. Add or remove roles as necessary.

5. Review the write scope, and change it as necessary.

6. Review the members that have been copied to the new role group. Add or remove members as necessary.

7. Click **Save** to create the role group.

## Use the Shell to copy a role group with no scope

1. Store the role group that you want to copy in a variable using the following syntax.

    ```powershell
    $RoleGroup = Get-RoleGroup <name of role group to copy>
    ```

2. Create the new role group, and also add members to the role group and specify who can delegate the new role group to other users, using the following syntax.

    ```powershell
    New-RoleGroup <name of new role group> -Roles $RoleGroup.Roles -Members <member1, member2, member3...> -ManagedBy <user1, user2, user3...>
    ```

For example, the following commands copy the Organization Management role group, and name the new role group "Limited Organization Management". It adds the members Isabelle, Carter, and Lukas and can be delegated by Jenny and Katie.

```powershell
$RoleGroup = Get-RoleGroup "Organization Management"
New-RoleGroup "Limited Organization Management" -Roles $RoleGroup.Roles -Members Isabelle, Carter, Lukas -ManagedBy Jenny, Katie
```

After the new role group is created, you can add or remove roles, change the scope of role assignments on the role, and more.

For detailed syntax and parameter information, see [Get-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup) and [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## Use the Shell to copy a role group with a custom scope

1. Store the role group that you want to copy in a variable using the following syntax.

    ```powershell
    $RoleGroup = Get-RoleGroup <name of role group to copy>
    ```

2. Create the new role group with a custom scope using the following syntax.

    ```powershell
    New-RoleGroup <name of new role group> -Roles $RoleGroup.Roles -CustomRecipientWriteScope <recipient scope name> -CustomConfigWriteScope <configuraiton scope name>
    ```

For example, the following commands copy the Organization Management role group and create a new role group called Vancouver Organization Management with the Vancouver Users recipient scope and Vancouver Servers configuration scope.

```powershell
$RoleGroup = Get-RoleGroup "Organization Management"
New-RoleGroup "Vancouver Organization Management" -Roles $RoleGroup.Roles -CustomRecipientWriteScope "Vancouver Users" -CustomConfigWriteScope "Vancouver Servers"
```

You can also add members to the role group when you create it by using the *Members* parameter as shown in Use the Shell to copy a role group with no scope earlier in this topic. For more information about management scopes, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

After the new role group is created, you can add or remove roles, change the scope of role assignments on the role, and perform other tasks.

For detailed syntax and parameter information, see [Get-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup) and [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## Use the Shell to copy a role group with an OU scope

1. Store the role group that you want to copy in a variable using the following syntax.

    ```powershell
    $RoleGroup = Get-RoleGroup <name of role group to copy>
    ```

2. Create the new role group with a custom scope using the following syntax.

    ```powershell
    New-RoleGroup <name of new role group> -Roles $RoleGroup.Roles -RecipientOrganizationalUnitScope <OU name>
    ```

For example, the following commands copy the Recipient Management role group and create a new role group called Toronto Recipient Management that allows management of only users in the Toronto Users OU.

```powershell
$RoleGroup = Get-RoleGroup "Recipient Management"
New-RoleGroup "Toronto Recipient Management" -Roles $RoleGroup.Roles -RecipientOrganizationalUnitScope "contoso.com/Toronto Users"
```

You can also add members to the role group when you create it by using the *Members* parameter as shown in Use the Shell to copy a role group with no scope earlier in this topic. For more information about management scopes, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

After the new role group is created, you can add or remove roles, change the scope of role assignments on the role, and more.

For detailed syntax and parameter information, see [Get-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup) and [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## How do you know this worked?

To verify that you have successfully copied a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Verify that the copied role group appears in the role group list, and then select it.

3. Verify that members, assigned roles, and scope that you specified on the copied role group are listed in the role group details pane.

## Remove a role group

If you no longer need a role group you created, you can remove it. When you remove a role group, the management role assignments between the role group and the management roles are deleted. The management roles aren't deleted. If a user depended on the role group for access to a feature, the user will no longer have access to the feature. You can't remove built-in role groups.

## Use the EAC to remove a role group

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to remove and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. Verify that you want to remove the selected role group, and if so, respond **Yes** to the warning.

## Use the Shell to remove a role group

To remove a role group, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroup#examples) section in [Remove-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroup).

## View role groups

You can view either a list of role groups or the detailed information about a specific role group that exists in your organization.

## Use the EAC to view a list of role groups and role group details

1. In the EAC, navigate to **Permissions** \> **Admin Roles**. All of the role groups in your organization are listed here.

2. Select a role group to view the members, assigned roles, and scope that are configured on the role group.

## Use the Shell to view a list of role groups and role group details

To view a list of role groups, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup#examples) section in [Get-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup).

## Add a role to a role group

Adding a management role to a role group is the best and simplest way to grant permissions to a group of administrators or specialist users. If you want to give users that are members of a role group the ability to manage a feature, you add the management role that manages the feature to the role group. After the role is added, the members of the role group are granted the permissions provided by the role.

## Use the EAC to add a management role to a role group

> [!IMPORTANT]
> You can't use the EAC to add roles to a role group if you've used the Shell to configure multiple management role scopes or exclusive scopes on the role group. If you've configured multiple scopes or exclusive scopes on the role group, you must use the Shell procedures later in this topic to add roles to the role group. For more information about management role scopes, see <A href="understanding-management-role-scopes-exchange-2013-help.md">Understanding management role scopes</A>.

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to add a role to, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In the **Roles** section, select the roles you want to add to the role group.

4. When you've finished adding roles to the role group, click **Save**.

## Use the Shell to create a role assignment with no scope

You can create a role assignment with no scope between a role and a role group. When you do this, the implicit read and implicit write scopes of the role apply.

Use the following syntax to assign a role without any scope to a role group. A role assignment name is created automatically if you don't specify one.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name>
```

This example assigns the Transport Rules management role to the Seattle Compliance role group.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Seattle Compliance" -Role "Transport Rules"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Use the Shell to create a role assignment with a predefined scope

If a predefined scope meets your business requirements, you can apply that scope to the role assignment rather than create a new one. For a list of predefined scopes and their descriptions, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

For more information about role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

Use the following syntax to assign a role to a role group with a predefined scope. A role assignment name is created automatically if you don't specify one.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name> -RecipientRelativeWriteScope < MyGAL | MyDistributionGroups | Organization | Self >
```

This example assigns the Message Tracking role to the Enterprise Support role group and applies the Organization predefined scope.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Enterprise Support" -Role "Message Tracking" -RecipientRelativeWriteScope Organization
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Use the Shell to create a role assignment with a recipient filter-based scope

If you created a recipient filter-based scope, you need to include the scope in the command used to assign the role to a role group by using the *CustomRecipientWriteScope* parameter.

You can also include a configuration write scope when you create a role assignment that has a recipient write scope.

For more information about role assignments and scopes, see the following topics:

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

Use the following syntax to assign a role to a role group with a recipient filter-based scope. A role assignment name is created automatically if you don't specify one.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name> -CustomRecipientWriteScope <role scope name>
```

This example assigns the Message Tracking role to the Seattle Recipient Admins role group and applies the Seattle Recipients scope.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Seattle Recipient Admins" -Role "Message Tracking" -CustomRecipientWriteScope "Seattle Recipients"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Use the Shell to create a role assignment with a configuration scope

If you created a server or database configuration filter or list-based scope, you need to include the scope in the command used to assign the role to a role group by using the *CustomConfigWriteScope* parameter.

You can also include a recipient write scope when you create a role assignment that has a configuration write scope.

For more information about role assignments and management scopes, see the following topics:

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

Use the following syntax to assign a role to a role group with a configuration scope. A role assignment name is created automatically if you don't specify one.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name> -CustomConfigWriteScope <role scope name>
```

This example assigns the Databases role to the Seattle Server Admins role group and applies the Seattle Servers scope.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Seattle Server Admins" -Role "Databases" -CustomConfigWriteScope "Seattle Servers"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Use the Shell to create a role assignment with an OU scope

If you want to scope a role's write scope to an OU, you can specify the OU in the *RecipientOrganizationalUnitScope* parameter directly.

For more information about role assignments and management scopes, see the following topics:

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

Use the following command to assign a role to a role group and restrict the write scope of a role to a specific OU. A role assignment name is created automatically if you don't specify one.

```powershell
New-ManagementRoleAssignment -SecurityGroup <role group name> -Role <role name> -RecipientOrganizationalUnitScope <OU>
```

This example assigns the Mail Recipients role to the Seattle Recipient Admins role group and scopes the assignment to the Sales\\Users OU in the Contoso.com domain.

```powershell
New-ManagementRoleAssignment -SecurityGroup "Seattle Recipient Admins" -Role "Mail Recipients" -RecipientOrganizationalUnitScope contoso.com/sales/users
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## How do you know this worked?

To verify that you have successfully added roles to a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you added roles to. In the role group details pane, verify that the roles that you added are listed.

## Remove a role from a role group

Removing a role from a management role group is the best and simplest way to revoke permissions granted to a group of administrators or specialist users. If you don't want administrators or specialist users to have permissions to manage a feature, you remove the management role from the management role group that manages the permissions. After the role is removed, the members of the role group will no longer have permissions to manage the feature.

> [!NOTE]
> Some role groups, such as the Organization Management role group, restrict what roles can be removed from a role group. For more information, see <A href="understanding-management-role-groups-exchange-2013-help.md">Understanding management role groups</A>.<BR>If an administrator is a member of another role group that contains management roles that grants permissions to manage the feature, you need to either remove the administrator from the other role groups, or remove the role that grants permissions to manage the feature from the other role groups.

## Use the EAC to remove a management role from a role group

> [!IMPORTANT]
> You can't use the EAC to remove roles from a role group if you've used the Shell to configure multiple scopes or exclusive scopes on the role group. If you've configured multiple scopes or exclusive scopes on the role group, you must use the Shell procedures later in this topic to remove roles from the role group. For more information about management role scopes, see <A href="understanding-management-role-scopes-exchange-2013-help.md">Understanding management role scopes</A>.

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to remove a role from, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In the **Roles** section, select the roles you want to remove from the role group.

4. When you've finished removing roles from the role group, click **Save**.

## Use the Shell to remove a role from a role group

You can remove roles from role groups by retrieving the associated management role assignment using the **Get-ManagementRoleAssignment** cmdlet and then piping the role assignment returned to the **Remove-ManagementRoleAssignment** cmdlet. Unless you want to remove both delegating and regular role assignments at the same time, specify the *Delegating* parameter to specify whether you want to remove regular or delegating role assignments.

For more information about regular and delegating role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

This procedure uses pipelining. For more information about pipelining, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

To remove a role from a role group, use the following syntax.

```powershell
Get-ManagementRoleAssignment -RoleAssignee <role group name> -Role <role name> -Delegating <$true | $false> | Remove-ManagementRoleAssignment
```

This example removes the Distribution Groups role, which enables administrators to manage distribution groups, from the Seattle Recipient Administrators role group. Because we want to remove the role assignment that provides permissions to manage distribution groups, the *Delegating* parameter is set to `$False`, which returns only regular role assignments.

```powershell
Get-ManagementRoleAssignment -RoleAssignee "Seattle Recipient Administrators" -Role "Distribution Groups" -Delegating $false | Remove-ManagementRoleAssignment
```

For detailed syntax and parameter information, see [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleAssignment).

## How do you know this worked?

To verify that you have successfully removed roles from a role group, do the following:

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you removed roles from. In the role group details pane, verify that the roles that you removed are no longer listed.

## Change a role group's scope

The management role assignments between a role group and a role contain management scopes, which determine what objects are made available to members of that role group. By changing the write scope on a role group, you can change what objects are made available to role group members to create, change, or remove. You can't change the read scope on a role group.

Exchange 2013 includes scopes that are applied by default to role assignments when no custom scopes are created. If you want to use a custom scope with a role assignment on a role group, you must create one first. For more information about creating custom scopes, which is an advanced task, see [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md).

For more information about management role scopes and assignments in Exchange 2013, see the following topics:

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

## Use the EAC to change the scope on a role group

When you use the EAC to change the scope on a role group, you're actually changing the scope on all the role assignments between the role group and each of the management roles assigned to the role group. If you want to change the scope on specific role assignments, you must use the Shell procedures later in this topic.

> [!IMPORTANT]
> You can't use the EAC to manage scopes on role assignments between roles and a role group if you've used the Shell to configure multiple scopes or exclusive scopes on those role assignments. If you've configured multiple scopes or exclusive scopes on those role assignments, you must use the Shell procedures later in this topic to manage scopes. For more information about management role scopes, see <A href="understanding-management-role-scopes-exchange-2013-help.md">Understanding management role scopes</A>.

1. In the EAC, navigate to **Permissions** \> **Admin Roles**.

2. Select the role group you want to change the scope on, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. Select one of the two following **Write scope** options:

   - A write scope from the drop-down box, where you can select either the default write scope or a custom write scope.

   - **Organizational unit**: Select this option and provide an organizational unit (OU) if you want to scope this role group to an OU.

4. Click **Save** to save the changes to the role group.

## Use the Shell to change the scope of all role assignments on a role group at the same time

Role assignments between the role group and the roles assigned to it can use the implicit scope obtained from the roles themselves, the same custom scope, or different custom scopes. For more information about role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

The scopes on the role assignments are managed using the **Set-ManagementRoleAssignment** cmdlet. You can't manage scopes using the **Set-RoleGroup** cmdlet.

To change the scope of all the role assignments between a role group and a set of management roles at the same time, you need to first retrieve the role assignments on the role group, and then set the new scope on each of the assignments. You can do this by using the **Get-ManagementRoleAssignment** cmdlet to retrieve the role assignments, and then pipe them to the **Set-ManagementRoleAssignment** cmdlet.

This procedure uses the concepts of pipelining and the *WhatIf* switch. For more information, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [WhatIf, Confirm, and ValidateOnly switches](whatif-confirm-and-validateonly-switches-exchange-2013-help.md)

To set the scope on all of the role assignments on a role group at the same time, use the following syntax.

```powershell
Get-ManagementRoleAssignment -RoleAssignee <name of role group> | Set-ManagementRoleAssignment -CustomRecipientWriteScope <recipient scope name> -CustomConfigWriteScope <configuration scope name> -RecipientRelativeScopeWriteScope < MyDistributionGroups | Organization | Self> -ExclusiveRecipientWriteScope <exclusive recipient scope name> -ExclusiveConfigWriteScope <exclusive configuration scope name> -RecipientOrganizationalUnitScope <organizational unit>
```

You use only the parameters you need to configure the scope you want to use. For example, if you want to change the recipient scope for all role assignments on the Sales Recipient Management role group to Direct Sales Employees, use the following command.

```powershell
Get-ManagementRoleAssignment -RoleAssignee "Sales Recipient Management" | Set-ManagementRoleAssignment -CustomRecipientWriteScope "Direct Sales Employees"
```

> [!NOTE]
> You can use the <EM>WhatIf</EM> switch to verify that only the role assignments you want to change are changed. Run the preceding command with the <EM>WhatIf</EM> switch to verify the results, and then remove the <EM>WhatIf</EM> switch to apply the changes.

For more information about changing management role assignments, see [Change a role assignment](change-a-role-assignment-exchange-2013-help.md).

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment).

## Use the Shell to change the scope of individual role assignments on a role group

Role assignments between the role group and the roles assigned to it can use the implicit scope obtained from the roles themselves, the same custom scope, or different custom scopes. For more information about role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

The scopes on the role assignments are managed using the **Set-ManagementRoleAssignment** cmdlet. You can't manage scopes using the **Set-RoleGroup** cmdlet.

This procedure uses the concepts of pipelining and the **Format-List** cmdlet. For more information, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

To change the scope on a role assignment between a role group and a management role, you first find the name of the role assignment, and then set the scope on the role assignment.

1. To find the names of all the role assignments on a role group, use the following command. By piping the management role assignments to the **Format-List** cmdlet, you can view the full name of the assignment.

    ```powershell
    Get-ManagementRoleAssignment -RoleAssignee <role group name> | Format-List Name
    ```

2. Find the name of the role assignment you want to change. Use the name of the role assignment in the next step.

3. To set the scope on an individual assignment, use the following syntax.

    ```powershell
    Set-ManagementRoleAssignment <role assignment name> -CustomRecipientWriteScope <recipient scope name> -CustomConfigWriteScope <configuration scope name> -RecipientRelativeScopeWriteScope < MyDistributionGroups | Organization | Self> -ExclusiveRecipientWriteScope <exclusive recipient scope name> -ExclusiveConfigWriteScope <exclusive configuration scope name> -RecipientOrganizationalUnitScope <organizational unit>
    ```

You use only the parameters you need to configure the scope you want to use. For example, if you want to change the recipient scope for the Mail Recipients\_Sales Recipient Management role assignment to All Sales Employees, use the following command.

```powershell
Set-ManagementRoleAssignment "Mail Recipients_Sales Recipient Management" -CustomRecipientWriteScope "All Sales Employees"
```

For more information about changing management role assignments, see [Change a role assignment](change-a-role-assignment-exchange-2013-help.md).

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment).

## How do you know this worked?

To verify that you have successfully changed the scope of a role assignment on a role group, do the following:

- If you used the EAC to configure the scope on the role group, do the following:

  1. In the EAC, navigate to **Permissions**\> **Admin Roles**. All the role groups in your organization are listed here.

  2. Select a role group to view the scope that's configured on the role group.

- If you used the Shell to configure the scope on the role group, do the following:

  1. Run the following command in the Shell.

     ```powershell
     Get-ManagementRoleAssignment -RoleAssignee <role group name> | Format-Table *WriteScope
     ```

  2. Verify that the write scope on the role assignments has been changed to the scope you specified.

## Add or remove a role group delegate

Role group delegates are users or universal security groups (USGs) that can add or remove members from a role group or change the properties of a role group. By adding or removing role group delegates, you can control who is allowed to manage a role group.

> [!IMPORTANT]
> After you add a delegate to a role group, the role group can only be managed by the delegates on the role group, or by users who are assigned, either directly or indirectly, the Role Management management role.<BR>If a user is assigned, either directly or indirectly, the Role Management role and isn't added as a delegate of the role group, the user must use the <EM>BypassSecurityGroupManagerCheck</EM> switch on the <STRONG>Add-RoleGroupMember</STRONG>, <STRONG>Remove-RoleGroupMember</STRONG>, <STRONG>Update-RoleGroupMember</STRONG>, and <STRONG>Set-RoleGroup</STRONG> cmdlets to manage a role group.

> [!NOTE]
> You can't use the EAC to add a delegate to a role group.

## Use the Shell to add a delegate to a role group

To change the list of delegates on a role group, you use the *ManagedBy* parameter on the **Set-RoleGroup** cmdlet. The *ManagedBy* parameter overwrites the entire delegate list on the role group. If you want to add delegates to the role group rather than replace the entire list of delegates, use the following steps:

1. Store the role group in a variable using the following command.

    ```powershell
    $RoleGroup = Get-RoleGroup <role group name>
    ```

2. Add the delegate to the role group stored in the variable using the following command.

    ```powershell
    $RoleGroup.ManagedBy += (Get-User <user to add>).Identity
    ```

    > [!NOTE]
    > Use the <STRONG>Get-Group</STRONG> cmdlet if you want to add a USG.

3. Repeat Step 2 for each delegate you want to add.

4. Apply the new list of delegates to the actual role group using the following command.

    ```powershell
    Set-RoleGroup <role group name> -ManagedBy $RoleGroup.ManagedBy
    ```

This example adds the user David Strome as a delegate on the Organization Management role group.

```powershell
$RoleGroup = Get-RoleGroup "Organization Management"
$RoleGroup.ManagedBy += (Get-User "David Strome").Identity
Set-RoleGroup "Organization Management" -ManagedBy $RoleGroup.ManagedBy
```

For detailed syntax and parameter information, see [Set-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Set-RoleGroup).

## Use the Shell to remove a delegate from a role group

To change the list of delegates on a role group, you use the *ManagedBy* parameter on the **Set-RoleGroup** cmdlet. The *ManagedBy* parameter overwrites the entire delegate list on the role group. If you want to remove delegates from the role group rather than replace the entire list of delegates, use the following steps:

1. Store the role group in a variable using the following command.

    ```powershell
    $RoleGroup = Get-RoleGroup <role group name>
    ```

2. Remove the delegate from the role group stored in the variable using the following command.

    ```powershell
    $RoleGroup.ManagedBy -= (Get-User <user to remove>).Identity
    ```

    > [!NOTE]
    > Use the <STRONG>Get-Group</STRONG> cmdlet if you want to remove a USG.

3. Repeat Step 2 for each delegate you want to remove.

4. Apply the new list of delegates to the actual role group using the following command.

    ```powershell
    Set-RoleGroup <role group name> -ManagedBy $RoleGroup.ManagedBy
    ```

This example removes the user David Strome as a delegate on the Organization Management role group.

```powershell
$RoleGroup = Get-RoleGroup "Organization Management"
$RoleGroup.ManagedBy -= (Get-User "David Strome").Identity
Set-RoleGroup "Organization Management" -ManagedBy $RoleGroup.ManagedBy
```

For detailed syntax and parameter information, see [Set-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Set-RoleGroup).

## How do you know this worked?

To verify that you have successfully changed the delegate list on a role group, do the following:

1. In the Shell, run the following command.

    ```powershell
    Get-RoleGroup <role group name> | Format-List ManagedBy
    ```

2. Verify that the delegates listed on the *ManagedBy* property include only the delegates that should be able to manage the role group.
