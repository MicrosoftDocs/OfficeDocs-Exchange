---
localization_priority: Normal
description: 'Summary: Learn how to view, create, copy, modify, and remove management role groups in Exchange Online.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: ab9b7a3b-bf67-4ba1-bde5-8e6ac174b82c
ms.reviewer:
f1.keywords:
- NOCSH
title: Manage role groups in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Manage role groups in Exchange Online

A role group is a special kind of universal security group (USG) that's used in the Role Based Access Control (RBAC) permissions model in Exchange Online. Management role groups simplify the assignment and maintenance of permissions to users in Exchange Online. The members of the role group are assigned the same set of roles, and you add and remove permissions from users by adding them to or removing them from the role group. For more information about role groups in Exchange Online, see [Permissions in Exchange Online](permissions-exo.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 to 10 minutes

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../exchange-admin-center.md). To open Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- The procedures in this topic require the Role Management RBAC role in Exchange Online. Typically, you get this permission via membership in the Organization Management role group (the Microsoft 365 or Office 365 Global administrator role).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html), or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## View role groups

### Use the new EAC to view role groups

1. In the new EAC, go to **Roles** \> **Admin roles**. All of the role groups in your organization are listed here.

2. Select a role group. The details pane shows the **Name**, **Description**, **Managed by**, **Write scope**, **Assigned**, and **Permissions** of the role group. 

### Use the Classic EAC to view role groups

1. In the Classic EAC, go to **Permissions** \> **Admin Roles**. All of the role groups in your organization are listed here.

2. Select a role group. The details pane shows the **Name**, **Description**, **Assigned roles**, **Members**, **Managed by**, and **Write scope** of the role group. You can also see this information by clicking **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

### Use Exchange Online PowerShell to view role groups

To view a role group, use the following syntax:

```PowerShell
Get-RoleGroup [-Identity "<Role Group Name>"] [-Filter <Filter>]
```

This example returns a summary list of all role groups.

```PowerShell
Get-RoleGroup
```

This example returns detailed information for the role group named Recipient Administrators.

```PowerShell
Get-RoleGroup -Identity "Recipient Administrators" | Format-List
```

This example returns all role groups where the user Julia is a member. You need to use the DistinguishedName (DN) value for Julia, which you can find by running the command: `Get-User -Identity Julia | Format-List DistinguishedName`.

```PowerShell
Get-RoleGroup -Filter "Members -eq 'CN=Julia,OU=contoso.onmicrosoft.com,OU=Microsoft Exchange Hosted Organizations,DC=NAMPR001,DC=PROD,DC=OUTLOOK,DC=COM'"
```

For detailed syntax and parameter information, see [Get-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Get-RoleGroup).

## Create role groups

When you create a new role group, you need to configure all of the settings yourself (during the creation of the group or after). To start with the configuration of an existing role group and modify it, see [Copy existing role groups](#copy-existing-role-groups).

### Use the new EAC to create role groups

1. In the new EAC, go to **Roles** \> **Admin roles** and then click **Add role group**.

2. In the **Add role group** window, under **Set up the basics** section, configure the following settings and click **Next**:

    - **Name**: Enter a unique name for the role group.

    - **Description**: Enter an optional description for the role group.

    - **Write scope**: The default value is **Default**, but you can also select a custom recipient write scope from the drop-down list.
    
3. In **Add permissions** section, select the roles and click **Next**. Roles define the scope of the tasks that the members assigned to this role group have permission to manage.
   
4. In **Assign admins** section, select the users to assign to this role group and click **Next**. They'll have permissions to manage the roles that you assigned. 

5. In **Review role group and finish** section, verify all the details, and then click **Add role group**.

6. Click **Done**.

### Use the Classic EAC to create role groups

1. In the Classic EAC, go to **Permissions** \> **Admin Roles** and then click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png).

2. In the **New role group** window that appears, configure the following settings:

    - **Name**: Enter a unique name for the role group.

    - **Description**: Enter an optional description for the role group.

    - **Write scope**: The default value is **Default**, but you can also select a custom recipient write scope that you've already created.

    - **Roles**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to select the roles that you want to be assigned to the role group in the new window that appears.

    - **Members**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) to select the members that you want to add to the role group in the new window that appears. You can select users, mail-enabled universal security groups (USGs), or other role groups (security principals).

   When you're finished, click **Save** to create the role group.

### Use Exchange Online PowerShell to create a role group

To create a new role group, use the following syntax:

```PowerShell
New-RoleGroup -Name "Unique Name" -Description "Descriptive text" -Roles <"Role1","Role2"...> -ManagedBy <Managers> -Members <Members> -CustomRecipientWriteScope "<Existing Write Scope Name>"
```

  - The _Roles_ parameter specifies the management roles to assign to the role group by using the following syntax `"Role1","Role1",..."RoleN"`. You can see the available roles by using the **Get-ManagementRole** cmdlet.

  - The _Members_ parameter specifies the members of the role group by using the following syntax: `"Member1","Member2",..."MemberN"`. You can specify users, mail-enabled universal security groups (USGs), or other role groups (security principals).

  - The _ManagedBy_ parameter specifies the delegates who can modify and remove the role group by using the following syntax: `"Delegate1","Delegate2",..."DelegateN"`. Note that this setting isn't available in the EAC.

  - The _CustomRecipientWriteScope_ parameter specifies the existing custom recipient write scope to apply to the role group. You can see the available custom recipient write scopes by using the **Get-ManagementScope** cmdlet.

This example creates a new role group named "Limited Recipient Management" with the following settings:

- The Mail Recipients and Mail Enabled Public Folders roles are assigned to the role group.

- The users Kim and Martin are added as members. Because no custom recipient write scope was specified, Kim and Martin can manage any recipient in the organization.

```PowerShell
New-RoleGroup -Name "Limited Recipient Management" -Roles "Mail Recipients","Mail Enabled Public Folders" -Members "Kim","Martin"
```

This is the same example with a custom recipient write scope, which means Kim and Martin can only manage recipients that are included in the Seattle Recipients scope (recipients who have their **City** property set to the value Seattle).

```PowerShell
New-RoleGroup -Name "Limited Recipient Management" -Roles "Mail Recipients","Mail Enabled Public Folders" -Members "Kim","Martin" -CustomRecipientWriteScope "Seattle Recipients"
```

For detailed syntax and parameter information, [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## Copy existing role groups

If an existing role group is close in terms of the permissions and settings that you want to assign to users, you can copy the existing role group and modify the copy to suit your needs.

### Use the new EAC to copy a role group

**Note**: You can't use the new EAC to copy a role group if you've used Exchange Online PowerShell to configure multiple scopes or exclusive scopes on the role group. To copy role groups that have these settings, you need to use Exchange Online PowerShell.

1. In the new EAC, go to **Roles** \> **Admin roles**.

2. Select the role group that you want to copy and then click **Copy role group**.

3. In the **Copy role group** window, under **Set up the basics** section, configure the following settings and click **Next**:

   - **Name**: The default value is "Copy of _\<Role Group Name\>_, but you can enter a unique name for the role group.

   - **Description**: The existing description is present, but you can change it.

   - **Write scope**: The existing write scope is selected, but you can select **Default** or a custom recipient write scope from the drop-down list.
   
4. In **Edit permissions** section, modify the roles and click **Next**. Roles define the scope of the tasks that the members assigned to this role group have permission to manage.
   
5. In **Assign admins** section, modify the role group membership and click **Next**. They'll have permissions to manage the roles that you assigned. 

6. In **Review role group and finish** section, verify all the details, and then click **Copy role group**.

7. Click **Done**.   

### Use the Classic EAC to copy a role group

**Note**: You can't use the Classic EAC to copy a role group if you've used Exchange Online PowerShell to configure multiple scopes or exclusive scopes on the role group. To copy role groups that have these settings, you need to use Exchange Online PowerShell.

1. In the Classic EAC, go to **Permissions** \> **Admin Roles**.

2. Select the role group that you want to copy and then click **Copy** ![Copy icon](../media/ITPro_EAC_CopyIcon.png).

3. In the **New role group** window that appears, configure the following settings:

   - **Name**: The default value is "Copy of _\<Role Group Name\>_, but you can enter a unique name for the role group.

   - **Description**: The existing description is present, but you can change it.

   - **Write scope**: The existing write scope is selected, but you can select **Default** or another custom recipient write scope that you've already created.

   - **Roles**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) or **Remove** ![ITPro_EAC_RemoveIcon.png](../media/ITPro_EAC_RemoveIcon.png) to modify the roles that are assigned to the role group.

   - **Members**: Click **Add** ![Add icon](../media/ITPro_EAC_AddIcon.png) or **Remove** ![ITPro_EAC_RemoveIcon.png](../media/ITPro_EAC_RemoveIcon.png) to modify the role group membership.

   When you're finished, click **Save** to create the role group.

### Use Exchange Online PowerShell to copy a role group

1. Store the role group that you want to copy in a variable using the following syntax:

   ```PowerShell
   $RoleGroup = Get-RoleGroup "<Existing Role Group Name>"
   ```

2. Create the new role group using the following syntax:

   ```PowerShell
   New-RoleGroup -Name "<Unique Name>" -Roles $RoleGroup.Roles [-Members <Members>] [-ManagedBy <Managers>] [-CustomRecipientWriteScope "<Existing Custom Recipient Write Scope Name>"]
   ```

   - The _Members_ parameter specifies the members of the role group by using the following syntax: `"Member1","Member2",..."MemberN"`. You can specify users, mail-enabled universal security groups (USGs), or other role groups (security principals).

   - The _ManagedBy_ parameter specifies the delegates who can modify and remove the role group by using the following syntax: `"Delegate1","Delegate2",..."DelegateN"`. Note that this setting isn't available in the EAC.

   - The _CustomRecipientWriteScope_ parameter specifies the existing custom recipient write scope to apply to the role group. You can see the available custom recipient write scopes by using the **Get-ManagementScope** cmdlet.

This example copies the Organization Management role group to the new role group named "Limited Organization Management". The role group members are Isabelle, Carter, and Lukas and the role group delegates are Jenny and Katie.

```PowerShell
$RoleGroup = Get-RoleGroup "Organization Management"
New-RoleGroup "Limited Organization Management" -Roles $RoleGroup.Roles -Members "Isabelle","Carter","Lukas" -ManagedBy "Jenny","Katie"
```

This example copies the Organization Management role group to the new role group called Vancouver Organization Management with the Vancouver Users recipient custom recipient write scope.

```PowerShell
$RoleGroup = Get-RoleGroup "Organization Management"
New-RoleGroup "Vancouver Organization Management" -Roles $RoleGroup.Roles -CustomRecipientWriteScope "Vancouver Users"
```

For detailed syntax and parameter information, [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup).

## Modify role groups

### Use the new EAC to modify role groups

1. In the new EAC, go to **Roles** \> **Admin roles**, select the role group you want to modify, and then edit the following in the details pane:

   - In **General** section, click **Edit basics** to change the name and description.

   - In **Assigned** section, add/delete users from this role group. 

   - In **Permissions** section, add/remove roles assigned to the role group. 

2. When you're finished, click **Save**.

### Use the Classic EAC to modify role groups

1. In the Classic EAC, go to **Permissions** \> **Admin Roles**, select the role group you want to modify, and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

The same options are available when you modify role groups as when you [Use the Classic EAC to create role groups](#use-the-classic-eac-to-create-role-groups). You can:

- Change the name and description.

- Change the write scope (if you've created custom recipient write scopes).

- Add and remove management roles (create or remove role assignments).

- Add and remove members.

**Notes**:

- You can't use the Classic EAC to modify the write scope, roles, and members of a role group if you've used Exchange Online PowerShell to configure multiple scopes or exclusive scopes on the role group. To modify the settings of these role groups, you need to use Exchange Online PowerShell.

- Some role groups (for example, the Organization Management role group) restrict the roles that you can remove from group.

- You can add or remove delegates to a role group in the Classic EAC. You can only use Exchange Online PowerShell.

### Use Exchange Online PowerShell to add roles to role groups (create role assignments)

To add roles to role groups in Exchange Online PowerShell, you create _management role assignments_ by using the following syntax:

```PowerShell
New-ManagementRoleAssignment [-Name "<Unique Name>"] -SecurityGroup "<Role Group Name>" -Role "<Role Name>" [-RecipientRelativeWriteScope <MyGAL | MyDistributionGroups | Organization | Self>] [-CustomRecipientWriteScope "<Role Scope Name>]
```

- The role assignment name is created automatically if you don't specify one.

- If you don't use the _RecipientRelativeWriteScope_ parameter, the implicit read scope and implicit write scope of the role is applied to the role assignment.

- If a predefined scope meets your business requirements, you can use the _RecipientRelativeWriteScope_ parameter to apply the scope to the role assignment.

- To apply a custom recipient write scope, use the _CustomRecipientWriteScope_ parameter.

This example assigns the Transport Rules management role to the Seattle Compliance role group.

```PowerShell
New-ManagementRoleAssignment -SecurityGroup "Seattle Compliance" -Role "Transport Rules"
```

This example assigns the Message Tracking role to the Enterprise Support role group and applies the Organization predefined scope.

```PowerShell
New-ManagementRoleAssignment -SecurityGroup "Enterprise Support" -Role "Message Tracking" -RecipientRelativeWriteScope Organization
```

This example assigns the Message Tracking role to the Seattle Recipient Admins role group and applies the Seattle Recipients scope.

```PowerShell
New-ManagementRoleAssignment -SecurityGroup "Seattle Recipient Admins" -Role "Message Tracking" -CustomRecipientWriteScope "Seattle Recipients"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/new-managementroleassignment).

### Use Exchange Online PowerShell to remove roles from role groups (remove role assignments)

To remove roles from role groups in Exchange Online PowerShell, you remove _management role assignments_ by using the following syntax:

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "<Role Group Name>" -Role "<Role Name>" -Delegating <$true | $false> | Remove-ManagementRoleAssignment
```

- To remove _regular_ role assignments that grant permissions to users, use the value `$false` for the _Delegating_ parameter.

- To remove _delegating_ role assignments that allow the role to be assigned to others, use the value `$true` for the _Delegating_ parameter.

This example removes the Distribution Groups role from the Seattle Recipient Administrators role group.

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "Seattle Recipient Administrators" -Role "Distribution Groups" -Delegating $false | Remove-ManagementRoleAssignment
```

For detailed syntax and parameter information, see [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/remove-managementroleassignment).

### Use Exchange Online PowerShell to modify the scope of role assignments in role groups

The write scope of a role assignment in a role group defines the objects that the members of the role group can operate on (for example, all users, or only the users whose **City** property has the value Vancouver). You can modify the write scope of the roles assigned to a role group to:

- The implicit scope from the roles themselves. This means you didn't specify any custom scopes when you created the role group, or you set the value of all role assignments in an existing role group to the value `$null`.

- The same custom scope for all role assignments.

- Different custom scopes for each individual role assignment.

To set the scope on all of the role assignments on a role group at the same time, use the following syntax:

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "<Role Group Name>" | Set-ManagementRoleAssignment [-CustomRecipientWriteScope "<Recipient Write Scope Name>"] [-RecipientRelativeScopeWriteScope <MyDistributionGroups | Organization | Self>] [-ExclusiveRecipientWriteScope "<Exclusive Recipient Write Scope name>"]
```

This example changes the recipient scope for all role assignments on the Sales Recipient Management role group to Direct Sales Employees.

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "Sales Recipient Management" | Set-ManagementRoleAssignment -CustomRecipientWriteScope "Direct Sales Employees"
```

To change the scope on an individual role assignment between a role group and a management role, do the following steps:

1. Replace \<Role Group Name\> with the name of the role group and run the following command to find the names of all the role assignments on the role group:

   ```PowerShell
   Get-ManagementRoleAssignment -RoleAssignee "<Role Group Name>" | Format-List Name
   ```

2. Find the name of the role assignment you want to change. Use the name of the role assignment in the next step.

3. To set the scope on the individual role assignment, use the following syntax:

   ```PowerShell
   Set-ManagementRoleAssignment -Identity "<Role Assignment Name"> [-CustomRecipientWriteScope "<Recipient Write Scope Name>"] [-RecipientRelativeScopeWriteScope <MyDistributionGroups | Organization | Self>] [-ExclusiveRecipientWriteScope "<Exclusive Recipient Write Scope name>"]
   ```

   This example changes the recipient scope for the role assignment named Mail Recipients_Sales Recipient Management to All Sales Employees.

   ```PowerShell
   Set-ManagementRoleAssignment "Mail Recipients_Sales Recipient Management" -CustomRecipientWriteScope "All Sales Employees"
   ```

For detailed syntax and parameter information, see [Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/set-managementroleassignment).

### Use Exchange Online PowerShell modify the list of delegates in role groups

Role group delegates define who is allowed to modify and delete the role group. You can't manage role group delegates in the EAC.

To modify the list of delegates in a role group, use the following syntax:

```PowerShell
Set-RoleGroup -Identity "<Role Group Name>" -ManagedBy <Delegates>
```

- To _replace_ the existing list of delegates with the values you specify, use the following syntax: `"Delegate1","Delegate2",..."DelegateN"`.

- To _selectively modify_ the existing list of delegates, use the following syntax: `@{Add="Delegate1","Delegate2"...; Remove="Delegate3","Delegate4"...}`.

This example replaces all current delegates of the Help Desk role group with the specified users.

```PowerShell
Set-RoleGroup -Identity "Help Desk" -ManagedBy "Gabriela Laureano","Hyun-Ae Rim","Jacob Berger"
```

This example adds Daigoro Akai and removes Valeria Barrio from the list of delegates on the Help Desk role group.

```PowerShell
Set-RoleGroup -Identity "Help Desk" -ManagedBy @{Add="Daigoro Akai"; Remove="Valeria Barrios"}
```

For detailed syntax and parameter information, see [Set-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/set-rolegroup).

## Use Exchange Online PowerShell modify the list of members in role groups

- The **Add-RoleGroupMember** and **Remove-RoleGroupMember** cmdlets add or remove individual members one at a time. The **Update-RoleGroupMember** cmdlet can replace or modify the existing list of members.

- The members of a role group can be users, mail-enabled universal security groups (USGs), or other role groups (security principals).

To modify the members of a role group, use the following syntax:

```PowerShell
Update-RoleGroupMember -Identity "<Role Group Name>" -Members <Members> [-BypassSecurityGroupManagerCheck]
```

- To _replace_ the existing list of members with the values you specify, use the following syntax: `"Member1","Member2",..."MemberN"`.

- To _selectively modify_ the existing list of members, use the following syntax: `@{Add="Member1","Member2"...; Remove="Member3","Member4"...}`.

This example replaces all current members of the Help Desk role group with the specified users.

```PowerShell
Update-RoleGroupMember -Identity "Help Desk" -Members "Gabriela Laureano","Hyun-Ae Rim","Jacob Berger"
```

This example adds Daigoro Akai and removes Valeria Barrio from the list of members on the Help Desk role group.

```PowerShell
Update-RoleGroupMember -Identity "Help Desk" -Members @{Add="Daigoro Akai"; Remove="Valeria Barrios"}
```

For detailed syntax and parameter information, see [Update-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Update-RoleGroupMember).

## Remove role groups

You can't remove built-in role groups, but you can remove custom role groups that you've created.

**Notes**:

- When you remove a role group, the management role assignments between the role group and the management roles are deleted. Any management roles that are assigned to the role group aren't deleted.

- If a user depends on the role group for access to a feature, the user will no longer have access to the feature after you delete the role group.

### Use the new EAC to remove a role group

1. In the new EAC, go to **Roles** > **Admin roles**.

2. Select the role group and click **Delete**.

3. Click **Confirm** in the confirmation window.

### Use the EAC to remove a role group

1. In the EAC, go to **Permissions** \> **Admin Roles**.

2. Select the role group you want to remove and then click **Delete** ![Delete icon](../media/ITPro_EAC_DeleteIcon.png).

3. Click **Yes** in the confirmation window that appears.

### Use Exchange Online PowerShell to remove a role group

To remove a custom role group, use the following syntax:

```PowerShell
Remove-RoleGroup -Identity "<Role Group Name>" [-BypassSecurityGroupManagerCheck]
```

This example removes the Training Administrators role group.

```PowerShell
Remove-RoleGroup -Identity "Training Administrators"
```

This example removes the Vancouver Recipient Administrators role group. Because the user running the command isn't defined in the **ManagedBy** property of the role group, the _BypassSecurityGroupManagerCheck_ switch is required in the command. The user that's running the command is assigned the Role Management role, which enables the user to bypass the security group manager check.

```PowerShell
Remove-RoleGroup - Identity "Vancouver Recipient Administrators" -BypassSecurityGroupManagerCheck
```

For detailed syntax and parameter information, see [Remove-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleGroup).
