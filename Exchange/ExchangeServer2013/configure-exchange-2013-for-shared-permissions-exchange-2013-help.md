---
title: 'Configure Exchange 2013 for shared permissions: Exchange 2013 Help'
TOCTitle: Configure Exchange 2013 for shared permissions
ms:assetid: 7d119977-b420-4b66-acf0-0a978b188cdd
ms:mtpsurl: https://technet.microsoft.com/library/Dd638146(v=EXCHG.150)
ms:contentKeyID: 49289319
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Exchange 2013 for shared permissions

_**Applies to:** Exchange Server 2013_

Shared permissions enable you, as an administrator of Microsoft Exchange Server 2013, to create Active Directory security principals, such as users, and then configure them as Exchange recipients. Unlike split permissions, which separate management tasks between groups of Exchange administrators and Active Directory administrators, there's no separation of tasks with shared permissions.

For more information about shared and split permissions, see [Understanding split permissions](understanding-split-permissions-exchange-2013-help.md).

You can configure your Exchange 2013 organization for shared permissions if you've previously set your organization for split permissions. The procedure to switch to shared permissions is different depending on whether you're currently using Role Based Access Control (RBAC) split permissions or Active Directory split permissions. Choose the procedure that follows that's applicable to your current configuration. If the following are true, your organization is using Active Directory split permissions:

- The Microsoft Exchange Protected Groups organizational unit (OU) exists.

- The Exchange Windows Permissions security group is located in the Microsoft Exchange Protected Groups OU.

- The Exchange Trusted Subsystem security group is a member of the Exchange Windows Permissions security group.

- There are no regular management role assignments to the Mail Recipient Creation role or Security Group Creation and Membership role.

If you've never configured your organization for split permissions, you don't need to perform this procedure. Exchange 2013 is configured for shared permissions by default.

For more information about management role groups, management roles, and regular and delegating management role assignments, see the following topics:

- [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md)

- [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md)

- [Understanding management roles](understanding-management-roles-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for other management tasks related to permissions? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- You must use Windows PowerShell, the Windows Command Shell, or both, to perform these procedures. For more information, see each procedure.

- The Exchange 2013 organization must currently be configured for RBAC or Active Directory split permissions.

- If you have Microsoft Exchange Server 2010 servers in your organization, the permissions model you select will also be applied to those servers.

- You must have permissions to delegate the Mail Recipient Creation management role and the Security Group Creation and Membership management role to the Organization Management management role group or another role group that's assigned the Mail Recipients role.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Switch from RBAC split permissions to shared permissions

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

To switch from RBAC split permissions to Exchange 2013 shared permissions, you must assign the Mail Recipient Creation role and the Security Group Creation and Membership role to a role group that's also assigned the Mail Recipients role and has Exchange 2013 administrators as members. In the default shared permissions configuration, the Organization Management role group contains each of these roles. Because of this, the Organization Management role group is in this procedure.

## Configure shared permissions

To configure shared permissions on the Organization Management role group, do the following using an account that has permissions to delegate role assignments for the Mail Recipient Creation role and the Security Group Creation and Membership role:

1. Add delegating role assignments for the Mail Recipient Creation role and Security Group Creation and Membership role to the Organization Management role group using the following commands.

    ```powershell
    New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Organization Management" -Delegating
    New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management" -Delegating
    ```

    > [!NOTE]
    > The role group (in this procedure, the Active Directory Administrators role group) that has delegating role assignments for the Mail Recipient Creation role and Security Group Creation and Membership role must be assigned the Role Management role to run the <STRONG>New-ManagementRoleAssignment</STRONG> cmdlet. The role assignee that can delegate the Role Management role must assign that role to the Active Directory Administrators role group.

2. Add regular role assignments for the Mail Recipient Creation role to the Organization Management and Recipient Management role groups using the following commands.

    ```powershell
    New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Organization Management"
    New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Recipient Management"
    ```

3. Add a regular role assignment for the Security Group Creation and Membership role to the Organization Management role group using the following command.

    ```powershell
    New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management"
    ```

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).

## Remove permissions from Active Directory administrators (Optional)

You can optionally remove the permissions granted to Active Directory administrators if you no longer want them to be able to create or manage Active Directory objects using the Exchange management tools. If you want to remove permissions from Active Directory administrators, perform this procedure.

> [!NOTE]
> Although you can remove permissions for Active Directory administrators to manage Active Directory objects using the Exchange management tools, Active Directory administrators can continue to manage Active Directory objects using Active Directory management tools, if their Active Directory permissions allow it. They won't, however, be able to manage Exchange-specific attributes on Active Directory objects. For more information, see <A href="understanding-split-permissions-exchange-2013-help.md">Understanding split permissions</A>.

To remove Exchange-related split permissions from Active Directory administrators, do the following:

1. Remove the regular and delegating role assignments that assign the Mail Recipient Creation role to the role group or universal security group (USG) that contains the Active Directory administrators as members using the following command. This command uses the Active Directory Administrators role group as an example. The *WhatIf* switch lets you see what role assignments will be removed. Remove the *WhatIf* switch, and run the command again to remove the role assignments.

   ```powershell
   Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Where {$_.RoleAssigneeName -EQ "Active Directory Administrators"} | Remove-ManagementRoleAssignment -WhatIf
   ```

2. Remove the regular and delegating role assignments that assign the Security Group Creation and Membership role to the role group or USG that contains the Active Directory administrators as members using the following command. This command uses the Active Directory Administrators role group as an example. The *WhatIf* switch lets you see what role assignments will be removed. Remove the *WhatIf* switch, and run the command again to remove the role assignments.

   ```powershell
   Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Where {$_.RoleAssigneeName -EQ "Active Directory Administrators"} | Remove-ManagementRoleAssignment -WhatIf
   ```

3. Optional. If you want to remove all Exchange permissions from the Active Directory administrators, you can remove the role group or USG in which they're members. For more information about how to remove a role group, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment) or [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleAssignment).

## Switch from Active Directory split permissions to shared permissions

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Active Directory split permissions" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

To switch from Active Directory split permissions to Exchange 2013 shared permissions, you must rerun Exchange Setup to disable Active Directory split permissions in the Exchange organization, and then create role assignments between a role group and the Mail Recipient Creation role and Security Group Creation and Membership role. In the default shared permissions configuration, the Organization Management role group contains each of these roles. Because of this, the Organization Management role group is in this procedure.

> [!IMPORTANT]
> The setup.com command in this procedure makes changes to Active Directory. You must use an account that has the permissions required to make these changes. This account might not be the same account that has permissions to create role assignments using the <STRONG>New-ManagementRoleAssignment</STRONG> cmdlet. Use the account, or accounts, with the permissions necessary to successfully complete each step in this procedure.

To switch from Active Directory split permissions to shared permissions, do the following:

1. From a Windows command shell, run the following command from the Exchange 2013 installation media to disable Active Directory split permissions.

   ```powershell
   setup.exe /PrepareAD /ActiveDirectorySplitPermissions:false
   ```

2. From the Exchange Management Shell, run the following commands to add regular role assignments between the Mail Recipient Creation role and Security Group Creation and Management role and the Organization Management and Recipient Management role groups.

   ```powershell
   New-ManagementRoleAssignment "Mail Recipient Creation_Organization Management" -Role "Mail Recipient Creation" -SecurityGroup "Organization Management"
   New-ManagementRoleAssignment "Security Group Creation and Membership_Org Management" -Role "Security Group Creation and Membership" -SecurityGroup "Organization Management"
   New-ManagementRoleAssignment "Mail Recipient Creation_Recipient Management" -Role "Mail Recipient Creation" -SecurityGroup "Recipient Management"
   ```

3. Restart the Exchange 2013 servers in your organization.

   > [!NOTE]
   > If you have Exchange 2010 servers in your organization, you also need to restart those servers.

For detailed syntax and parameter information, see [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment).
