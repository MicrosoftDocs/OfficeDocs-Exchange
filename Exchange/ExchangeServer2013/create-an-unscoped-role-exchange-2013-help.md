---
title: 'Create an unscoped role: Exchange 2013 Help'
TOCTitle: Create an unscoped role
ms:assetid: 5a042ccf-4d5f-4609-a91b-21c20d1e6459
ms:mtpsurl: https://technet.microsoft.com/library/Dd876886(v=EXCHG.150)
ms:contentKeyID: 49289258
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an unscoped role

_**Applies to:** Exchange Server 2013_

An unscoped management role can be used to provide administrators and specialist users access to Windows PowerShell scripts and non-Exchange cmdlets. You can either create an unscoped top-level role and add scripts or non-Exchange cmdlets to that role, or create a role that's based on an existing, unscoped top-level role. After an unscoped role has been created and customized, the role can be assigned to management role groups, users, and universal security groups (USGs). Unscoped roles can't be assigned to management role assignment policies. For more information about unscoped roles, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

> [!WARNING]
> Unscoped roles can be powerful because, as their name implies, no management scopes are applied to them. This means that the scripts and non-Exchange cmdlets that they contain can be run against any object in your Exchange organization. Consider this when adding scripts or non-Exchange cmdlets to an unscoped role and when assigning the unscoped role.

> [!NOTE]
> If you want to create a role that contains Exchange cmdlets, you must create a role that's based on an existing management role. For more information about creating roles with Exchange cmdlets, see <A href="create-a-role-exchange-2013-help.md">Create a role</A>.

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- The ability to create unscoped roles isn't included in any management role group by default. You must first assign the Unscoped Role Management role to a user, or to a USG or role group of which the user is a member, before the user is able to create a role group. For more information about adding a role to a user, USG, or role group, see the following topics:

  - [Manage role groups](manage-role-groups-exchange-2013-help.md)

  - [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create an unscoped top-level management role

If you want to make scripts or non-Exchange cmdlets available to administrators or specialists in your organization, you need to create an unscoped top-level role. Scripts and non-Exchange cmdlets can only be added to an unscoped role that's created as a top-level role, because the initial unscoped role doesn't inherit from other roles. The new, unscoped top-level role can then be a parent to other unscoped roles that can also use the added scripts and non-Exchange cmdlets.

Here are the steps to create an unscoped top-level role:

## Step 1: Create the unscoped top-level role

Unscoped top-level roles don't have a parent role. You need to specify the *UnscopedTopLevel* switch to create a role without a parent. Use the following syntax to create the new role.

```powershell
New-ManagementRole <name of new role> -UnscopedTopLevel
```

This example creates the IT Scripts unscoped top-level role.

```powershell
New-ManagementRole "IT Scripts" -UnscopedTopLevel
```

After it's created, the role is empty until you add scripts or non-Exchange cmdlets to it.

For detailed syntax and parameter information, see [New-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRole).

## Step 2a: Add script management role entries

If you want to add a script to the new unscoped role, use this step. If you want to add a non-Exchange cmdlet to the new unscoped role, use Step 2b.

To add a Windows PowerShell script to an unscoped top-level role, you must add a management role entry to the role. The role entry contains the script's name and the parameters on the script that you want to make available to the role.

The script must reside in the `RemoteScripts` directory in the Microsoft Exchange Server 2013 installation path on every server running Exchange 2013 where users might connect to run the script. If a user has access to run a script, but the script isn't located on the Exchange 2013 server the user is connected to, an error occurs. By default, the path to the `RemoteScripts` directory is C:\\Program Files\\Microsoft\\Exchange Server\\V15\\RemoteScripts.

After you copy the script to the appropriate Exchange 2013 servers and you decide what script parameters should be used, create the role entry using the following syntax.

```powershell
Add-ManagementRoleEntry <unscoped top-level role name>\<script filename> -Parameters <parameter 1, parameter 2, parameter...> -Type Script -UnscopedTopLevel
```

This example adds the BulkProvisionUsers.ps1 script to the IT Scripts role with the *Name* and *Location* parameters.

```powershell
Add-ManagementRoleEntry "IT Scripts\BulkProvisionUsers.ps1" -Parameters Name, Location -Type Script -UnscopedTopLevel
```

> [!NOTE]
> The <STRONG>Add-ManagementRoleEntry</STRONG> cmdlet performs basic validation to make sure that you add only the parameters that exist in the script. However, no further validation is done after the role entry is added. If parameters are later added or removed, you must manually update the role entries that contain the script.

## Step 2b: Add non-Exchange cmdlet role entries

If you want to add a non-Exchange cmdlet to the new unscoped role, use this step. If you want to add a script to the new unscoped role, use Step 2a.

To add a non-Exchange cmdlet to an unscoped top-level role, you must add a management role entry to the role. The role entry contains the cmdlet snap-in, cmdlet name, and the parameters on the cmdlet that you want to make available to the role.

If you add non-Exchange cmdlets to the new role, the cmdlets must be installed on every Exchange 2013 server where users might connect to run the cmdlets. To learn how to properly install and register the Windows PowerShell snap-ins that contain the cmdlets you want to use, refer to the documentation for your product.

After you install the Windows PowerShell snap-in that contains the cmdlets on the appropriate Exchange 2013 servers and you decide what cmdlet parameters should be used, create the role entry using the following syntax.

```powershell
Add-ManagementRoleEntry <unscoped top-level role name>\<cmdlet name> -PSSnapinName <snap-in name> -Parameters <parameter 1, parameter 2, parameter...> -Type Cmdlet -UnscopedTopLevel
```

This example adds the **Set-WidgetConfiguration** cmdlet in the Contoso.Admin.Cmdlets snap-in to the Widget Cmdlets role with the *Database* and *Size* parameters.

```powershell
Add-ManagementRoleEntry "Widget Cmdlets\Set-WidgetConfiguration" -PSSnapinName Contoso.Admin.Cmdlets -Parameters Database, Size -Type Cmdlet -UnscopedTopLevel
```

> [!NOTE]
> The <STRONG>Add-ManagementRoleEntry</STRONG> cmdlet performs basic validation to make sure that you add only the parameters that exist in the cmdlet. However, no further validation is done after the role entry is added. If the cmdlet is later changed, and parameters are added or removed, you must manually update the role entries that contain the cmdlet.

## Step 3: Assign the management role

The final step when you create and configure a role is to assign it to a role assignee.

> [!IMPORTANT]
> Management scopes can't be configured on role assignments that assign an unscoped role. Whether you choose to create a role assignment for a role group, user, or USG, you must choose the option to create a role assignment without a management scope.

You can assign the new role to a role group, user, or USG. For more information, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

## Create an unscoped role based on another unscoped role

If you have an existing, unscoped top-level role or other unscoped roles that you want to base new unscoped roles on, you can create child unscoped roles. The child unscoped roles can contain a subset of the scripts and cmdlets that exist on the parent unscoped roles. This is useful, for example, if you want to give only a subset of the scripts or cmdlets available on a parent unscoped role to a less experienced administrator.

Here are the steps to create an unscoped child role:

## Step 1: Create the unscoped child role

New, unscoped child roles can be based on existing unscoped roles. When you create a role, an existing role and its management role entries are copied to the new role. The existing role becomes the parent to the new child role. If you create an unscoped role that's based on another unscoped role, you must choose a role that contains all the cmdlets and parameters you need to use, and then remove the ones you don't want. Child unscoped roles can't have management role entries that don't exist in the parent role.

> [!NOTE]
> If you need to create an unscoped role that contains scripts or non-Exchange cmdlets that don't exist in any other unscoped role, create an unscoped top-level role. For more information, see Create an unscoped top-level management role earlier in this topic.

Use the following syntax to create the new role.

```powershell
New-ManagementRole -Parent <existing unscoped role to copy> -Name <name of new unscoped role>
```

This example copies the IT Global Scripts role and its management role entries to the Diagnostic IT Scripts role.

```powershell
New-ManagementRole -Parent "IT Global Scripts" -Name "Diagnostic IT Scripts"
```

For detailed syntax and parameter information, see [New-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRole).

## Step 2: Change the role's management role entries

After you create your role, you need to change the role's entries. You can remove an entire role entry, which removes access to the associated script or non-Exchange cmdlet completely. Or, you can remove parameters from a role entry to remove access to those specific parameters on the associated script or non-Exchange cmdlet.

You can't add role entries or parameters on role entries unless they exist in the parent role. Because you just created a role from a parent role in Step 1, you can't add any additional role entries or parameters on role entries because they don't exist in the parent role.

When you change a role entry on a role, you can do one of the following:

- Remove a single, entire role entry.

- Remove multiple, entire role entries.

- Remove parameters from a role entry.

To remove role entries from your new role, see [Remove a role entry from a role](remove-a-role-entry-from-a-role-exchange-2013-help.md).

## Step 3: Assign the management role

The final step when you create and configure a role is to assign it to a role assignee.

> [!IMPORTANT]
> Management scopes can't be configured on role assignments that assign an unscoped role. Whether you choose to create a role assignment for a role group, user, or USG, you must choose the option to create a role assignment without a management scope.

You can assign the new role to a role group, user, or USG. For more information, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)
