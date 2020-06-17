---
title: 'Remove a role entry from a role: Exchange 2013 Help'
TOCTitle: Remove a role entry from a role
ms:assetid: 4736367a-750f-44d3-8a20-5149bd35e9ff
ms:mtpsurl: https://technet.microsoft.com/library/Dd297947(v=EXCHG.150)
ms:contentKeyID: 49289243
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a role entry from a role

_**Applies to:** Exchange Server 2013_

Management role entries on a management role determine what cmdlets and parameters are available on a management role. By removing role entries or parameters on a role entry, you can restrict what users assigned the management role can perform. For more information about management role entries in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Remove a single entire role entry from a role

When you remove a role entry from a role, you remove the ability for users assigned that role to access the associated cmdlet or script.

Use the following syntax to remove an entire management role entry from a role.

```powershell
Remove-ManagementRoleEntry <management role>\<management role entry>
```

This example removes the **Enable-MailUser** cmdlet from the Seattle Server Administrators role.

```powershell
Remove-ManagementRoleEntry "Seattle Server Administrators\Enable-MailUser"
```

For detailed syntax and parameter information, see [Remove-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleEntry).

## Remove multiple entire role entries from a role

When you remove multiple role entries from a role, you remove the ability for users assigned that role to access the associated cmdlets or scripts.

To remove multiple role entries from a role, you need to retrieve the list of role entries to remove using the **Get-ManagementRoleEntry** cmdlet. Then you need to pipe the output to the **Remove-ManagementRoleEntry** cmdlet. You can use wildcard characters with the **Get-ManagementRoleEntry** cmdlet to match multiple role entries. It's a good idea to use the *WhatIf* switch to verify that you're removing the correct role entries. Use the following syntax.

```powershell
Get-ManagementRoleEntry <management role>\<role entry with wildcard character> | Remove-ManagementRoleEntry -WhatIf
```

This example removes all the role entries that contain the word journal from the Seattle Server Administrators role.

```powershell
Get-ManagementRoleEntry "Seattle Server Administrators\*Journal*" | Remove-ManagementRoleEntry -WhatIf
```

When you run the command with the *WhatIf* switch, the cmdlet returns a list of all the role entries that would be removed. If the list looks correct, run the command again without the *WhatIf* switch to remove the role entries.

```powershell
Get-ManagementRoleEntry "Seattle Server Administrators\*Journal*" | Remove-ManagementRoleEntry
```

For detailed syntax and parameter information, see [Get-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleEntry) and [Remove-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleEntry).

## Remove parameters from a role entry on a role

When you remove parameters from a role entry on a role, those parameters are no longer available to users assigned the role.

Use the following syntax to remove parameters from a role entry.

```powershell
Set-ManagementRoleEntry <management role>\<role entry> -Parameters <parameter 1>,<parameter 2...> -RemoveParameter
```

This example removes the *MaxSafeSenders*, *MaxSendSize*, *SecondaryAddress*, and *UseDatabaseQuotaDefaults* parameters from the **Set-Mailbox** role entry on the Seattle Server Administrators role.

```powershell
Set-ManagementRoleEntry "Seattle Server Administrators\Set-Mailbox" -Parameters MaxSafeSenders,MaxSendSize,SecondaryAddress,UseDatabaseQuotaDefaults -RemoveParameter
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).
