---
localization_priority: Normal
description: Admins can learn about role assignment policies, and how to view, create, modify, remove, and assign them in Exchange Online.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 
ms.reviewer: 
f1.keywords:
- NOCSH
title: Role assignment policies in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Role assignment policies in Exchange Online

A role assignment policy is a collection of one or more end-user roles that enable users to manage their mailbox settings and distribution groups in Exchange Online. End-users roles are part of the role based access control (RBAC) permissions model in Exchange Online. You can assign different role assignment policies to different users to allow or prevent specific self-management features in Exchange Online. For more information, see [Role assignment policies](permissions-exo.md#role-assignment-policies).

In Exchange Online, a default role assignment policy named Default Role Assignment Policy is specified by the mailbox plan that's assigned to users when their account is licensed. For more information about mailbox plans, see [Mailbox plans in Exchange Online](../recipients-in-exchange-online/manage-user-mailboxes/mailbox-plans.md).

> [!NOTE]
> Currently **User roles** and **Outlook Web App** policies are not available in new Exchange admin center.

Role assignment policies are how end-user roles (as opposed to management roles) are assigned to users in Exchange Online. There are several ways you can use role assignment policies to assign permissions to users:

- **New users**:

   - Change the end-user roles that are assigned to the default role assignment policy.

   - Create a custom role assignment policy and set it as the default. Note that this method only affects mailboxes that you create without specifying a role assignment policy or assigning a license (the license specifies the mailbox plan, which specifies the role assignment policy).

   - Specify a custom role assignment policy in the mailbox plan. For more information, see [Use Exchange Online PowerShell to modify mailbox plans](../recipients-in-exchange-online/manage-user-mailboxes/mailbox-plans.md#use-exchange-online-powershell-to-modify-mailbox-plans).

- **Existing users**:

   - Assign a different license to the user. This will apply the settings of the different mailbox plan, which specifies the role assignment policy to apply.

   - Manually assign a custom role assignment policy to mailboxes.

The available end-user roles that you can assign to mailbox plans are described in the following table:

|**Role**|**Assigned to Default Role Assignment Policy by default?**|**Description**|
|:-----|:-----|:-----|
|My Custom Apps|Yes|Install custom apps.|
|My Marketplace Apps|Yes|Install marketplace apps.|
|My ReadWriteMailbox Apps|Yes|Install apps with ReadWriteMailbox permissions.|
|MyBaseOptions|Yes|Required for users to access options in Outlook on the web from their own mailbox.|
|MyContactInformation|Yes|Edit their address and telephone number in the global address list (GAL). <br/><br/> This role contains the following child roles: <br/>• **MyAddressInformation**: Change all elements of their mailing address, work telephone number, and fax number. <br/>• **MyMobileInformation**: Change their mobile phone and pager numbers. <br/>• **MyPersonalInformation**: Change their home telephone number and web page. <br/><br/> If you think this role gives users too much power, you can remove the role from the role assignment policy, and assign one or more of the child roles. For instructions, see the [Add or remove roles from a role assignment policy](#add-or-remove-roles-from-a-role-assignment-policy) section in this topic.|
|MyDistributionGroupMembership|Yes|Join or leave existing distribution groups (if the group is configured to let members join or leave the group).|
|MyDistributionGroups|Yes|Create new distribution groups, delete groups they own, modify groups they own, and manage group membership for groups they own.|
|MyMailboxDelegation|No|Allows users to grant send on behalf of permissions to other users on their mailbox. Messages clearly show the sender in the From field (\<Sender\> on behalf of \<Mailbox\>), but replies are delivered to the mailbox, not the sender.|
|MyMailSubscriptions|Yes|Connected accounts were removed from Outlook on the web in November 2018. For more information, see [Connected accounts are no longer supported in Outlook on the web](https://support.microsoft.com/office/5cc526bf-e928-4a99-8b9f-5e089df7d887).|
|MyProfileInformation|Yes|Edit their first name, middle initial, last name, and display name in the GAL. <br/><br/> This role contains the following child roles: <br/>• **MyDisplayName**: Change their display name. <br/>• **MyName**: Change their first name, middle initial, last name and Notes property. <br/><br/> If you think this role gives users too much power, you can remove the role from the role assignment policy, and assign one of the child roles. For instructions, see the [Add or remove roles from a role assignment policy](#add-or-remove-roles-from-a-role-assignment-policy) section in this topic.|
|MyRetentionPolicies|Yes|Allows users to add personal tags that aren't part of their assigned retention policy.<sup>*</sup>|
|MyTeamMailboxes|Yes|Site mailboxes were discontinued in favor of Microsoft 365 groups in September 2017. For more information, see [Use Microsoft 365 Groups instead of Site Mailboxes](https://support.microsoft.com/office/737d6b1f-67cc-41fe-8db8-f2d09dd1673b).|
|MyTextMessaging|Yes|Enable text message notifications for meetings and new email messages.<sup>*</sup>|
|MyVoiceMail|Yes|Update their voice mail settings.<sup>*</sup>|

<sup>*</sup>This feature isn't available in all regions or organizations.

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes.

- The procedures in this topic require the Role Management RBAC role in Exchange Online. Typically, you get this permission via membership in the Organization Management role group (the Microsoft 365 or Office 365 Global administrator role). For more information, see [Manage role groups in Exchange Online](role-groups.md).

- To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](../exchange-admin-center.md). To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- Changes to permissions take effect after the user logs out and logs in again.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).


## View roles assigned to a role assignment policy

### Use the EAC to view roles assigned to a role assignment policy

1. In the EAC, go to **Permissions** \> **User roles**, and select the role assignment policy.

2. The roles that are assigned to the policy are displayed in the details pane. You can also click **Edit** ![Edit button](../media/ITPro_EAC_EditIcon.png) to see the roles, including the available roles that aren't assigned to the policy.

### Use Exchange Online PowerShell to view roles assigned to a role assignment policy

To view the roles assigned to a role assignment policy, use the following syntax:

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "<RoleAssignmentPolicyName>" | Format-Table Name,Role -Auto
```

This example returns the roles that are assigned to the policy named Default Role Assignment Policy.

```PowerShell
Get-ManagementRoleAssignment -RoleAssignee "Default Role Assignment Policy" | Format-Table Name,Role -Auto
```

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/get-managementroleassignment).

**Note**: To return a list of all available end-user roles, run the following command:

```PowerShell
Get-ManagementRole | Where {$_.IsEndUserRole -eq $true} | Format-Table Name,Parent
```

## Add or remove roles from a role assignment policy

### Use the EAC to add or remove roles from a role assignment policy

1. In the EAC, go to **Permissions** \> **User roles**, select the role assignment policy, and then click **Edit** ![Edit button](../media/ITPro_EAC_EditIcon.png).

2. In the policy properties window that opens, do one of the following steps:

   - To add a role, select the check box next to the role.

   - To remove a role that's already assigned, clear the check box.

   If you select a check box for a role that has child roles, the check boxes for the child roles are also selected. If you clear the check box of the parent role, the check boxes for the child roles are also cleared. You can select a child role by clearing the check box of the parent role and then selecting the individual child role.

3. When you're finished, click **Save**.

### Use Exchange Online PowerShell to add roles to a role assignment policy

Adding a role to a role assignment policy creates a new role assignment with a unique name that's a combination of the names of the role and the role assignment policy.

To add roles to a role assignment policy, use the following syntax:

```PowerShell
New-ManagementRoleAssignment -Role <RoleName> -Policy "<RoleAssignmentPolicyName>"
```

This example adds the role MyMailboxDelegation to the role assignment policy named Default Role Assignment Policy.

```PowerShell
New-ManagementRoleAssignment -Role MyMailboxDelegation -Policy "Default Role Assignment Policy"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/new-managementroleassignment).

### Use Exchange Online PowerShell to remove roles from a role assignment policy

1. Use the procedure from the [Use Exchange Online PowerShell to view roles assigned to a role assignment policy](#use-exchange-online-powershell-to-view-roles-assigned-to-a-role-assignment-policy) section earlier in this topic to find the name of the **role assignment** for the role that you want to remove (it's a combination of the names of the role and the role assignment policy).


2. To remove the role from the role assignment policy, use this syntax:

   ```PowerShell
   Remove-ManagementRoleAssignment -Identity "<RoleAssignmentName>"
   ```

   This example removes the MyDistributionGroups role from the role assignment policy named Default Role Assignment Policy.

   ```PowerShell
   Remove-ManagementRoleAssignment -Identity "MyDistributionGroups-Default Role Assignment Policy"
   ```

For detailed syntax and parameter information, see [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/remove-managementroleassignment).

## Create role assignment policies

### Use the EAC to create role assignment policies

1. In the EAC, go to **Permissions** \> **User roles** and click **New** ![New button](../media/ITPro_EAC_AddIcon.png).

2. In the new role assignment policy window that opens, configure the following settings:

   - **Name**: Enter a descriptive name.

   - **Description**: Enter an optional description.

   - Select the roles that you want to assign to the policy.

3. When you're finished, click **Save**

### Use Exchange Online PowerShell to create role assignment policies

To create a role assignment policy, use the following syntax:

```PowerShell
New-RoleAssignmentPolicy -Name <UniqueName> [-Description "<Descriptive Text>"] [-Roles "<EndUserRole1>","<EndUserRole2>"...] [-IsDefault]
```

This example creates a new role assignment policy named Contoso Contractors that include the specified end-user roles.

```PowerShell
New-RoleAssignmentPolicy -Name "Contoso Contractors" -Description "Limited self-management capabilities for contingent staff."] -Roles "MyBaseOptions","MyContactInformation","MyProfileInformation"
```

For detailed syntax and parameter information, see [New-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/new-roleassignmentpolicy).

## Modify role assignment policies

You can use the EAC or Exchange PowerShell to [Add or remove roles from a role assignment policy](#add-or-remove-roles-from-a-role-assignment-policy).

You can only use Exchange Online PowerShell to [specify the default role assignment policy](#use-exchange-online-powershell-to-specify-the-default-role-assignment-policy) that's applied to new mailboxes that aren't assigned a license or a role assignment policy when they're created.

Otherwise, all you can do in the EAC or Exchange Online PowerShell is modify the name and description of the role assignment policy.

### Use Exchange Online PowerShell to specify the default role assignment policy

To specify the default role assignment policy, use the following syntax:

```PowerShell
Set-RoleAssignmentPolicy -Identity "<RoleAssignmentPolicyName>" -IsDefault
```

This example configures Contoso Users as the default role assignment policy.

```PowerShell
Set-RoleAssignmentPolicy -Identity "Contoso Users" -IsDefault
```

**Note**: The _IsDefault_ switch is also available on the **New-RoleAssignmentPolicy** cmdlets.

For detailed syntax and parameter information, see [Set-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/set-roleassignmentpolicy).

## Remove role assignment policies

You can't remove the role assignment policy that's currently specified as the default. You first need to [specify another role assignment policy as the default](#use-exchange-online-powershell-to-specify-the-default-role-assignment-policy) before you can delete the policy.

You can't remove a role assignment policy that's assigned to mailboxes. Use the procedures described in the [Use Exchange Online PowerShell to modify role assignment policy assignments on mailboxes](#use-exchange-online-powershell-to-modify-role-assignment-policy-assignments-on-mailboxes) section to replace the role assignment policy that's assigned to mailboxes.

### Use the EAC to remove role assignment policies

1. In the EAC, go to **Permissions** \> **User roles**, select the policy that you want to delete, and then click **Delete** ![Delete button](../media/ITPro_EAC_DeleteIcon.png).

2. In the warning dialog box that appears, click **Yes**.

### Use Exchange Online PowerShell to remove role assignment policies

To remove a role assignment policy, use the following syntax:

```PowerShell
Remove-RoleAssignmentPolicy -Identity "<RoleAssignmentPolicyName>"
```

This example removes the role assignment policy named Contoso Managers.

```PowerShell
Remove-RoleAssignmentPolicy -Identity "Contoso Managers"
```

For detailed syntax and parameter information, see [Remove-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-roleassignmentpolicy).

## View role assignment policy assignments on mailboxes

### Use the EAC to view role assignment policy assignments on mailboxes

1. In the EAC, go to **Recipients** \> **Mailboxes**, select the mailbox, and click **Edit** ![Edit button](../media/ITPro_EAC_EditIcon.png).

2. In the mailbox properties window that opens, click **Mailbox features**. The role assignment policy is shown in the **Role assignment policy** field.

3. When you're finished, click **Save**.

### Use Exchange Online PowerShell to view role assignment policy assignments on mailboxes

To see the role assignment policy assignment on a specific mailbox, use the following syntax:

```PowerShell
Get-Mailbox -Identity <MailboxIdentity> | Format-List RoleAssignmentPolicy
```

This example returns the role assignment policy for the mailbox named Pedro Pizarro.

```PowerShell
Get-Mailbox -Identity "Pedro Pizarro" | Format-List RoleAssignmentPolicy
```

To return all mailboxes that have a specific role assignment policy assigned, use the following syntax:

```PowerShell
$<VariableName> = Get-Mailbox -ResultSize unlimited
```

```PowerShell
$<VariableName> | where {$_.RoleAssignmentPolicy -eq '<RoleAssignmentPolicyName>'}
```

This example returns all mailboxes that have the role assignment policy named Contoso Managers assigned.

```PowerShell
$Mgrs = Get-Mailbox -ResultSize unlimited
```

```PowerShell
$Mgrs | where {$_.RoleAssignmentPolicy -eq 'Contoso Managers'}
```

## Modify role assignment policy assignments on mailboxes

A mailbox can have only one role assignment policy assigned. The role assignment policy that you assign to the mailbox will replace the existing role assignment policy that's assigned.

### Use the EAC to modify role assignment policy assignments on mailboxes

In the EAC, go to **Recipients** \> **Mailboxes**, and do one of the following steps:

- **Individual mailboxes**: Select the mailbox \> click **Edit** ![Edit button](../media/ITPro_EAC_EditIcon.png) \> click **Mailbox features** in the window that opens \> click the dropdown next to **Role assignment policy** \> select a new role assignment policy \> click **Save**.

- **Multiple mailboxes**: Select multiple mailboxes of the same type (for example, **User**) by selecting a mailbox, holding down the Shift key, and select another mailbox farther down in the list or by holding down the CTRL key as you select each mailbox. In the details pane (that's now titled **Bulk Edit**): click **More options** \> click **Update** under **Role Assignment Policy** \> select the role assignment policy in the window that appears \> click **Save**.

### Use Exchange Online PowerShell to modify role assignment policy assignments on mailboxes

To change the role assignment policy assignment on a specific mailbox, use this syntax:

```PowerShell
Set-Mailbox -Identity <MailboxIdentity> -RoleAssignmentPolicy "<RoleAssignmentPolicyName>"
```

This example applies the role assignment policy named Contoso Managers to the mailbox named Pedro Pizarro.

```PowerShell
Get-Mailbox -Identity "Pedro Pizarro" -RoleAssignmentPolicy "<RoleAssignmentPolicyName>"
```

To change the assignment for all mailboxes that have a specific role assignment policy assigned, use the following syntax:

```PowerShell
$<VariableName> = Get-Mailbox -ResultSize unlimited
```

```PowerShell
$<VariableName> | where {$_.RoleAssignmentPolicy -eq '<CurrentRoleAssignmentPolicyName>'} | Set-Mailbox -RoleAssignmentPolicy '<NewRoleAssignmentPolicyName>'
```

This example changes the role assignment policy from Default Role Assignment Policy to Contoso Staff for all mailboxes that currently have Default Role Assignment Policy assigned.

```PowerShell
$Users = Get-Mailbox -ResultSize unlimited
```

```PowerShell
$Users | where {$_.RoleAssignmentPolicy -eq 'Default Role Assignment Policy'} | Set-Mailbox -RoleAssignmentPolicy 'Contoso Staff'
```

