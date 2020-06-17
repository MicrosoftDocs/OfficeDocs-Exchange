---
title: 'Remove a role: Exchange 2013 Help'
TOCTitle: Remove a role
ms:assetid: 2fb6f453-f37a-4636-8353-3f9927f81298
ms:mtpsurl: https://technet.microsoft.com/library/Dd335178(v=EXCHG.150)
ms:contentKeyID: 49289213
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a role

_**Applies to:** Exchange Server 2013_

Management roles that are no longer required can be removed from your organization. You can only remove management roles that you created. Built-in management roles can't be removed. For more information about management roles in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- Before you can remove a management role, you must remove all its management role assignments. For more information about how to remove a role assignment, see [Remove a role from a user or USG](remove-a-role-from-a-user-or-usg-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Remove a management role with no child roles

To remove a role with no child roles, use the following syntax.

```powershell
Remove-ManagementRole <role name>
```

This example removes the Seattle Server Administrators role.

```powershell
Remove-ManagementRole "Seattle Server Administrators"
```

For detailed syntax and parameter information, see [Remove-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRole).

## Remove a management role with child roles

If a role that you want to remove has child roles, you must remove all the child roles also. You receive an error message if you try to remove a role that has child roles unless you use the *Recurse* switch. If you use the *Recurse* switch when you remove a role, the role you specify and all its child roles are removed.

> [!WARNING]
> If you use the <EM>Recurse</EM> switch, all child roles of the specified role you want to remove are also removed. Make sure that you're aware of what roles will be removed before you run this command.

To make sure that you remove only the roles that you want to remove, use the *WhatIf* switch with your command to verify that it's correct. Use the following syntax.

```powershell
Remove-ManagementRole <role name> -Recurse -WhatIf
```

The *WhatIf* switch performs the command without committing any changes and reports which roles it would have removed. For more information about the *WhatIf* switch, see [WhatIf, Confirm, and ValidateOnly switches](whatif-confirm-and-validateonly-switches-exchange-2013-help.md).

After you confirm that only the roles you want to remove will be removed, run the same command without the *WhatIf* switch. This example removes the London Administrators role and all its child roles.

```powershell
Remove-ManagementRole "London Administrators" -Recurse
```

For detailed syntax and parameter information, see [Remove-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRole).

## Remove an unscoped management role

To remove an unscoped role, the same procedures provided in Remove a management role with no child roles and Remove a management role with child roles earlier in this topic can be used. The only difference is that when you remove an unscoped role, you must specify the *UnScopedTopLevel* switch when you run the command. This example removes an unscoped role and all its child roles.

```powershell
Remove-ManagementRole "Custom IT Scripts" -Recurse -UnScopedTopLevel
```

As with removing other roles, you should use the *WhatIf* switch to verify that you're removing the correct roles.

For detailed syntax and parameter information, see [Remove-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRole).
