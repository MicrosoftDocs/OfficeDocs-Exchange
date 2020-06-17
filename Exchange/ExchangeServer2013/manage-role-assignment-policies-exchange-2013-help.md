---
title: 'Manage role assignment policies: Exchange 2013 Help'
TOCTitle: Manage role assignment policies
ms:assetid: f93d502e-5df4-4ba0-b68d-01a17ccffb4d
ms:mtpsurl: https://technet.microsoft.com/library/JJ657511(v=EXCHG.150)
ms:contentKeyID: 49289465
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage role assignment policies

_**Applies to:** Exchange Server 2013_

If you want to customize the permissions that you assign to a group of end users, create a new custom management role assignment policy. The assignment policy you create can be customized to suit your end user's specific requirements. For more information about assignment policies in Microsoft Exchange Server 2013, see [Understanding management role assignment policies](understanding-management-role-assignment-policies-exchange-2013-help.md).

Looking for other management tasks related to managing permissions? Check out [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Assignment policies" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Add an assignment policy

After you've created the new assignment policy, you assign users to it. For more information, see [Change the assignment policy on a mailbox](change-the-assignment-policy-on-a-mailbox-exchange-2013-help.md).

## Use the EAC to create a new assignment policy

> [!NOTE]
> You can only create explicit assignment policies using the Exchange admin center (EAC). If you want to create a new default assignment policy, you must use the Exchange Management Shell. For more information, see the "Use the Shell to create a default assignment policy" section later in this topic.

1. In the EAC, navigate to **Permissions** \> **User Roles** and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the role assignment policy window, provide a name for the new assignment policy.

3. Select the check box next to the role or roles you want to add to the assignment policy. You can select multiple roles, including end-user roles you've added. If you select a role that has child roles, the child roles are automatically selected.

4. Click **Save** to save the changes to the assignment policy.

## Use the Shell to create an explicit assignment policy

To create an explicit assignment policy that can be manually assigned to mailboxes, use the following syntax.

```powershell
New-RoleAssignmentPolicy <assignment policy name> -Roles <roles to assign>
```

This example creates the explicit assignment policy Limited Mailbox Configuration and assigns the `MyBaseOptions`, `MyAddressInformation`, and `MyDisplayName` roles to it.

```powershell
New-RoleAssignmentPolicy "Limited Mailbox Configuration" -Roles MyBaseOptions, MyAddressInformation, MyDisplayName
```

For detailed syntax and parameter information, see [New-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/New-RoleAssignmentPolicy).

## Use the Shell to create a default assignment policy

To create a default assignment policy assigned to new mailboxes, use the following syntax.

```powershell
New-RoleAssignmentPolicy <assignment policy name> -Roles <roles to assign> -IsDefault
```

This example creates the default assignment policy Limited Mailbox Configuration and assigns the `MyBaseOptions`, `MyAddressInformation`, and `MyDisplayName` roles to it.

```powershell
New-RoleAssignmentPolicy "Limited Mailbox Configuration" -Roles MyBaseOptions, MyAddressInformation, MyDisplayName -IsDefault
```

For detailed syntax and parameter information, see [New-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/New-RoleAssignmentPolicy).

## Remove an assignment policy

If you no longer need a management role assignment policy, you can remove it.

## What do you need to know before you begin?

- All users assigned the assignment policy must be changed to another assignment policy. For more information about how to change an assignment policy on a mailbox, see [Change the assignment policy on a mailbox](change-the-assignment-policy-on-a-mailbox-exchange-2013-help.md).

- All the management role assignments between the assignment policy and the assigned management roles must be removed. For more information about how to remove a role assignment from an assignment policy, see the Use the Shell to remove a role from an assignment policy section later in this topic.

- If you want to remove a default assignment policy, it must be the last assignment policy in the Exchange 2013 organization.

## Use the EAC to remove an assignment policy

1. In the EAC, navigate to **Permissions** \> **User Roles**.

2. Select the assignment policy you want to remove, and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

## Use the Shell to remove an assignment policy

To remove an assignment policy, use the following syntax.

```powershell
Remove-RoleAssignmentPolicy <role assignment policy>
```

This example removes the New York Temporary Users assignment policy.

```powershell
Remove-RoleAssignmentPolicy "New York Temporary Users"
```

For detailed syntax and parameter information, see [Remove-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-RoleAssignmentPolicy).

## View a list of assignment policies or assignment policy details

You can view management role assignment policies in a variety of ways, depending on the information you want and whether you're using the EAC or the Shell.

In the EAC, you can view the list of assignment policies and the roles assigned to them. In the Shell, you can view all the assignment policies in your organization, list the mailboxes assigned a specific policy, and more.

## Use the EAC to view a list of assignment policies

1. In the EAC, navigate to **Permissions** \> **User Roles**. All of the assignment policies in the organization are listed here.

2. To view the details of a specific assignment policy, select the assignment policy you want to view. The description and the roles assigned to the assignment policy are displayed in the details pane.

## Use the Shell to view a list of assignment policies

You can view a list of all the assignment policies in your organization by not specifying any assignment policies when you run the **Get-RoleAssignmentPolicy** cmdlet.

This procedure makes use of pipelining and the **Format-Table** cmdlet. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

To return a list of all assignment policies in your organization, use the following command.

```powershell
Get-RoleAssignmentPolicy
```

To return a list of specific properties for all the assignment policies in your organization, you can pipe the results to the **Format-Table** cmdlet and specify the properties you want in the list of results. Use the following syntax.

```powershell
Get-RoleAssignmentPolicy | Format-Table <property 1>, <property 2...>
```

This example returns a list of all the assignment policies in your organization and includes the **Name** and **IsDefault** properties.

```powershell
Get-RoleAssignmentPolicy | Format-Table Name, IsDefault
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) or [Get-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RoleAssignmentPolicy).

## Use the Shell to view the details of a single assignment policy

You can view the details of a specific assignment policy by using the **Get-RoleAssignmentPolicy** cmdlet and piping the output to the **Format-List** cmdlet.

This procedure makes use of pipelining and the **Format-List** cmdlet. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

To view the details of a specific assignment policy, use the following syntax.

```powershell
Get-RoleAssignmentPolicy <assignment policy name> | Format-List
```

This example views the details about the Redmond Users - no Text Messaging assignment policy.

```powershell
Get-RoleAssignmentPolicy "Redmond Users - no Text Messaging" | Format-List
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) or [Get-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RoleAssignmentPolicy).

## Use the Shell to find the default assignment policy

You can find the default assignment policy by piping the output of the **Get-RoleAssignmentPolicy** cmdlet to the **Where** cmdlet. With the **Where** cmdlet, filter the data returned to display only the assignment policy that has its *IsDefault* property set to `$True`.

This procedure makes use of pipelining and the **Where** cmdlet. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

This example returns the default assignment policy.

```powershell
Get-RoleAssignmentPolicy | Where {Get-RoleAssignmentPolicy | Where {$_.IsDefault -eq $True}.IsDefault -eq $True }
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) or [Get-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RoleAssignmentPolicy).

## Use the Shell to view mailboxes that are assigned a specific policy

You can find all the mailboxes assigned a specific assignment policy by piping the output of the **Get-Mailbox** cmdlet to the **Where** cmdlet. With the **Where** cmdlet, filter the data returned to display only the mailboxes that have their *RoleAssignmentPolicy* property set to the assignment policy name you specify.

This procedure makes use of pipelining and the **Where** cmdlet. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

Use the following syntax.

```powershell
Get-Mailbox | Where {Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "<role assignment policy>"}.RoleAssignmentPolicy -Eq "<role assignment policy>" }
```

This example finds all the mailboxes assigned the policy Vancouver End Users.

```powershell
Get-Mailbox | Where {Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "Vancouver End Users"}.RoleAssignmentPolicy -Eq "Vancouver End Users" }
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) or [Get-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RoleAssignmentPolicy).

## Change the default assignment policy

You can change the management role assignment policy assigned to new mailboxes that are created. Changing the default role assignment policy doesn't change the assignment policy assigned to existing mailboxes. To change the assignment policy assigned to existing mailboxes, see [Change the assignment policy on a mailbox](change-the-assignment-policy-on-a-mailbox-exchange-2013-help.md).

> [!NOTE]
> You can't use the EAC to change the default assignment policy. You need to use the Shell.

## Use the Shell to change the default assignment policy

To change the default assignment policy, use the following syntax.

```powershell
Set-RoleAssignmentPolicy <assignment policy name> -IsDefault
```

This example sets the Vancouver End Users assignment policy as the default assignment policy.

```powershell
Set-RoleAssignmentPolicy "Vancouver End Users" -IsDefault
```

> [!IMPORTANT]
> New mailboxes are assigned the default assignment policy even if the policy hasn't been assigned management roles. Mailboxes assigned assignment policies with no assigned management roles can't access any mailbox configuration features in Microsoft Outlook Web App.

For detailed syntax and parameter information, see [Set-RoleAssignmentPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-RoleAssignmentPolicy).

## Add a role to an assignment policy

## Use the EAC to add a role to an assignment policy

1. In the EAC, navigate to **Permissions** \> **User Roles**.

2. Select the assignment policy you want to add one or more roles to, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. Select the check box next to the role or roles you want to add to the assignment policy. You can select multiple roles, including end-user roles you've added. If you select a role that has child roles, the child roles are automatically selected.

4. Click **Save** to save the changes to the assignment policy.

## Use the Shell to add a role to an assignment policy

To create a management role assignment between a role and an assignment policy, use the following syntax.

```powershell
New-ManagementRoleAssignment -Name <role assignment name> -Role <role name> -Policy <assignment policy name>
```

This example creates the role assignment Seattle Users - Voicemail between the MyVoicemail role and the Seattle Users assignment policy.

```powershell
New-ManagementRoleAssignment -Name "Seattle Users - Voicemail" -Role MyVoicemail -Policy "Seattle Users"
```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Remove a role from an assignment policy

If you don't want end users to have permissions to manage certain features of their mailbox or distribution group, you can remove the management role that grants the permissions from the management role assignment policy to which the user is assigned. If other users are assigned the same assignment policy, they also lose the ability to manage that feature.

## Use the EAC to remove a role from an assignment policy

1. In the EAC, navigate to **Permissions** \> **User Roles**.

2. Select the assignment policy you want to remove one or more roles from, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon") .

3. Clear the check box next to the role or roles you want to remove from the assignment policy. If you clear the check box for a role that has child roles, the check boxes for the child roles are also cleared.

4. Click **Save** to save the changes to the assignment policy.

## Use the Shell to remove a role from an assignment policy

You can remove roles from assignment policies by retrieving the associated management role assignment using the **Get-ManagementRoleAssignment** cmdlet and then piping the role assignment returned to the **Remove-ManagementRoleAssignment** cmdlet.

For more information about regular and delegating role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

This procedure uses pipelining. For more information about pipelining, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

To remove a role from an assignment policy, use the following syntax.

```powershell
Get-ManagementRoleAssignment -RoleAssignee <assignment policy name> -Role <role name> | Remove-ManagementRoleAssignment
```

This example removes the MyVoicemail management role, which enables users to manage their voice mail options, from the Seattle Users assignment policy.

```powershell
Get-ManagementRoleAssignment -RoleAssignee "Seattle Users" -Role MyVoicemail | Remove-ManagementRoleAssignment
```

For detailed syntax and parameter information, see [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleAssignment).
