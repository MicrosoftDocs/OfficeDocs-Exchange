---
title: 'Change a role entry on an unscoped top-level role: Exchange 2013 Help'
TOCTitle: Change a role entry on an unscoped top-level role
ms:assetid: 65c0bfb3-aafd-4c64-8429-7616c57adf1c
ms:mtpsurl: https://technet.microsoft.com/library/Dd876896(v=EXCHG.150)
ms:contentKeyID: 49289281
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change a role entry on an unscoped top-level role

_**Applies to:** Exchange Server 2013_

Management role entries on unscoped top-level management roles refer to the scripts and non-Exchange cmdlets, and their parameters, that you want to make available to those assigned the role. By changing the parameters available on a role entry, you control what those assigned the role can do with the script or non-Exchange cmdlet. For more information about unscoped role entries, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

> [!NOTE]
> If you want to change a role entry on a management role that contains Exchange cmdlets, see <A href="change-a-role-entry-exchange-2013-help.md">Change a role entry</A>.

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- The ability to change a role entry on an unscoped top-level role isn't included in any management role group by default. You must first assign the Unscoped Role Management role to a user, or to a universal security group (USG) or role group of which the user is a member, before the user is able to add or change an unscoped top-level role entry. For more information about adding a role to a user, USG, or role group, see the following topics:

  - [Manage role groups](manage-role-groups-exchange-2013-help.md)

  - [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to add one or more parameters to a role entry

To add parameters to an unscoped top-level role entry, you need to do the following:

- Specify the parameters you want to add using the *Parameters* parameter.

- Specify the *AddParameter* parameter to indicate that you want to perform an add operation.

- Specify the *UnscopedTopLevel* parameter to indicate that you're changing a role entry on an unscoped top-level role. If you don't specify this parameter when you change a role entry on an unscoped role, an error occurs.

To add parameters to a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<script or non-Exchange cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...> -AddParameter -UnscopedTopLevel
```

This example adds the *EmailAddress* and *City* parameters to the **CreateUsers.ps1** script on the Recipient Administrators unscoped role.

```powershell
Set-ManagementRoleEntry "Recipient Administrators\CreateUsers.ps1" -Parameters EmailAddress, City -AddParameter -UnscopedTopLevel
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to remove one or more parameters from a role entry

To remove parameters from a role entry, you need to do the following:

- Specify the parameters you want to remove using the *Parameters* parameter.

- Specify the *RemoveParameter* parameter to indicate that you want to perform a remove operation.

- Specify the *UnscopedTopLevel* parameter to indicate that you're changing a role entry on an unscoped top-level role. If you don't specify this parameter when you change a role entry on an unscoped role, an error occurs.

> [!WARNING]
> You can't undo remove operations. If you mistakenly remove a parameter from a role entry, you must add it again manually.

To remove parameters from a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<script or non-Exchange cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...> -RemoveParameter -UnscopedTopLevel
```

This example removes the *Delay*, *Force*, and *Credential* parameters from the **Start-Widget** non-Exchange cmdlet on the Tier 1 Server Administrators role.

```powershell
Set-ManagementRoleEntry "Tier 1 Server Administrators\Start-Widget" -Parameters Delay, Force, Credential -RemoveParameter -UnscopedTopLevel
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to remove all parameters from a role entry

To remove all of the parameters from a role entry, you need to do the following:

- Specify the value `$Null` on the *Parameters* parameter. You don't need to include the *RemoveParameter* parameter.

- Specify the *UnscopedTopLevel* parameter to indicate that you're changing a role entry on an unscoped top-level role. If you don't specify this parameter when you change a role entry on an unscoped role, an error occurs.

Removing all the parameters from a role entry is most useful when you want to make only a few parameters available on a script or non-Exchange cmdlet and exclude all of the other parameters.

If you don't want the role to have access to a script or non-Exchange cmdlet, remove the associated role entry from the role completely instead of just removing the parameters. For more information about how to remove a role entry from a role, see [Remove a role entry from a role](remove-a-role-entry-from-a-role-exchange-2013-help.md).

> [!WARNING]
> You can't undo remove operations. If you mistakenly remove all the parameters from a role entry, you must add them again manually.

To remove all the parameters from a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<script or non-Exchange cmdlet> -Parameters $Null -UnscopedTopLevel
```

This example removes all the parameters from the FindMailboxesOverQuota.ps1 script on the Recipient Administrators role.

```powershell
Set-ManagementRoleEntry "Recipient Administrators\FindMailboxesOverQuota.ps1" -Parameters $Null -UnscopedTopLevel
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to apply a specific set of parameters

If you want only a specific set of parameters to be included on a role entry, you need to do the following:

- Specify the *Parameters* parameter only. Don't include the *AddParameter* or *RemoveParameter* parameters.

- Specify the *UnscopedTopLevel* parameter to indicate that you're changing a role entry on an unscoped role. If you don't specify this parameter when you change a role entry on an unscoped top-level role, an error occurs.

> [!WARNING]
> When you specify only the <EM>Parameters</EM> parameter, only the parameters you specify in the command are included on the role entry. All other parameters are removed.

To specify a specific set of parameters, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<script or non-Exchange cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...> -UnscopedTopLevel
```

This example includes only the *Alias*, *DisplayName*, *WidgetConfig*, and *Enabled* parameters on the **Set-Widget** cmdlet on the Seattle Mail Recipient Admins role.

```powershell
Set-ManagementRoleEntry "Seattle Mail Recipient Admins\Set-UMMailbox" -Parameters Alias, DisplayName, WidgetConfig, Enabled -UnscopedTopLevel
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Other tasks

After you change a role entry on an unscoped top-level role, you may also want to:

[Add a role entry to a role](add-a-role-entry-to-a-role-exchange-2013-help.md)

[Manage role groups](manage-role-groups-exchange-2013-help.md)

[Manage role group members](manage-role-group-members-exchange-2013-help.md)

[Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

[Remove a role from a user or USG](remove-a-role-from-a-user-or-usg-exchange-2013-help.md)
