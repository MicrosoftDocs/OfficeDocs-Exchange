---
title: 'Add a role entry to a role: Exchange 2013 Help'
TOCTitle: Add a role entry to a role
ms:assetid: 30cd37bc-b3e8-4f39-a8ba-a4c20b1b27b7
ms:mtpsurl: https://technet.microsoft.com/library/Dd335180(v=EXCHG.150)
ms:contentKeyID: 49289216
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add a role entry to a role

_**Applies to:** Exchange Server 2013_

If you want to grant access to a cmdlet, you need to add the associated management role entry to a management role. After you add the role entry to a role, the users assigned the role will be able to access that cmdlet. For more information about management role entries in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

You can't add role entries to built-in roles. If you want to customize roles, you must create a new role. For more information about how to create a new role, see [Create a role](create-a-role-exchange-2013-help.md).

> [!NOTE]
> This topic doesn't discuss how to add unscoped management role entries to an unscoped management role. For more information about how to add unscoped role entries, see [Add a role entry to an unscoped top-level role](add-a-role-entry-to-an-unscoped-top-level-role-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- A role entry that you want to add to a management role must exist in that role's immediate parent management role.

- This topic makes use of pipelining. For more information about pipelining, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Add a single role entry from a parent role

You can add a role entry to a role exactly as it appears on the parent role by using the following syntax.

```powershell
Add-ManagementRoleEntry <child role name>\<cmdlet>
```

This example adds the **Set-Mailbox** cmdlet to the Recipient Administrators role.

```powershell
Add-ManagementRoleEntry "Recipient Administrators\Set-Mailbox"
```

This command checks the parent role, and if the role entry exists, adds it to the child role. If the role entry already exists on the child role, you can include the *Overwrite* parameter to overwrite the existing role entry.

For detailed syntax and parameter information, see [Add-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Add-ManagementRoleEntry).

## Add a single role entry from a parent role and include only specific parameters

If you want to add a role entry from a parent role, but you want to include only specific parameters in the role entry on the child role, use the following syntax.

```powershell
Add-ManagementRoleEntry <child role name>\<cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...>
```

This example adds the **Set-Mailbox** cmdlet to the Help Desk role, but includes only the *DisplayName* and *EmailAddresses* parameters in the entry on the child role.

```powershell
Add-ManagementRoleEntry "Help Desk\Set-Mailbox" -Parameters DisplayName, EmailAddresses
```

This command checks the parent role, and if the role entry exists, adds it to the child role. If the role entry already exists on the child role, you can include the *Overwrite* parameter to overwrite the existing role entry.

For detailed syntax and parameter information, see [Add-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Add-ManagementRoleEntry).

## Add multiple role entries from a parent role

If you want to add more than one role entry to a role, you need to retrieve a list of role entries that exist on the parent role that you want to add to the child role, and then add them to the child role. To do this, you retrieve the list of role entries on a parent role by using the **Get-ManagementRoleEntry** cmdlet. Then you pipe the output of the **Get-ManagementRoleEntry** cmdlet to the **Add-ManagementRoleEntry** cmdlet. To retrieve multiple role entries, you need to use the wildcard character (\*).

To add multiple entries from a parent role to a child role, use the following syntax.

```powershell
Get-ManagementRoleEntry <parent role name>\*<partial cmdlet name>* | Add-ManagementRoleEntry -Role <child role name>
```

This example adds all the role entries that contain the string `Mailbox` in the cmdlet name on the Mail Recipients parent role to the Seattle Mail Recipients child role.

```powershell
Get-ManagementRoleEntry "Mail Recipients\*Mailbox*" | Add-ManagementRoleEntry -Role "Seattle Mail Recipients"
```

If the role entries already exist on the child role, you can include the *Overwrite* parameter to overwrite the existing role entries.

For more information about retrieving a list of management role entries, see [View role entries](view-role-entries-exchange-2013-help.md).

For detailed syntax and parameter information, see [Get-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleEntry) and [Add-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Add-ManagementRoleEntry).
