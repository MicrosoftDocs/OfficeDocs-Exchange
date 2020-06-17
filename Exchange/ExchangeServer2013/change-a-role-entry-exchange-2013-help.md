---
title: 'Change a role entry: Exchange 2013 Help'
TOCTitle: Change a role entry
ms:assetid: 5aa4f39c-16a4-4815-ac4f-2cdcfa2b3ee1
ms:mtpsurl: https://technet.microsoft.com/library/Dd298005(v=EXCHG.150)
ms:contentKeyID: 49289260
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change a role entry

_**Applies to:** Exchange Server 2013_

Each management role entry on a management role represents a single cmdlet. By adding parameters to or removing parameters from a role entry, which is then added to a management role, you control whether those parameters are available on that cmdlet. For more information about management role entries in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

You can't modify the role entries on built-in management roles.

> [!NOTE]
> This topic doesn't discuss how to modify unscoped management role entries on an unscoped management role. For more information about how to modify unscoped role entries, see <A href="create-a-role-exchange-2013-help.md">Create a role</A>.

> [!WARNING]
> To add or remove parameters from a role entry, you must use the <EM>AddParameter</EM> or <EM>RemoveParameter</EM> parameters. If you omit the <EM>AddParameter</EM> or <EM>RemoveParameter</EM> parameter when you run the <STRONG>Set-ManagementRoleEntry</STRONG> cmdlet, only the parameters you specify using the <EM>Parameters</EM> parameter will be included in the role entry. All other parameters on the role entry will be removed.

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- If you want to add parameters to a role entry, the parameters you add must exist in the role entry in the parent role. The parameters must also exist on the cmdlet you specify.

- If you want to remove parameters from a role entry, the parameters you remove can't exist in the role entries of any child roles. You must remove the parameters from the role entries of the child roles. Use the "Use the Shell to remove one or more parameters from a role entry" procedure later in this topic to remove the parameters from the role entries of all child roles.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to add one or more parameters to a role entry

To add parameters to a role entry, you need to specify the parameters you want to add using the *Parameters* parameter. You then need to specify the *AddParameter* parameter to indicate that you want to perform an add operation.

To add parameters to a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...> -AddParameter
```

This example adds the *EmailAddresses* and *Type* parameters to the **Set-Mailbox** cmdlet on the Recipient Administrators role.

```powershell
Set-ManagementRoleEntry "Recipient Administrators\Set-Mailbox" -Parameters EmailAddresses, Type -AddParameter
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to remove one or more parameters from a role entry

To remove parameters from a role entry, you need to specify the parameters you want to remove using the *Parameters* parameter. You then need to specify the *RemoveParameter* parameter to indicate that you want to perform a remove operation.

To remove parameters from a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...> -RemoveParameter
```

This example removes the *Port*, *ProtocolLoggingLevel*, and *SmartHostAuthMechanism* parameters from the **Set-SendConnector** cmdlet on the Tier 1 Server Administrators role.

```powershell
Set-ManagementRoleEntry "Tier 1 Server Administrators\Set-SendConnector" -Parameters Port, ProtocolLoggingLevel, SmartHostAuthMechanism -RemoveParameter
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to remove all parameters from a role entry

To remove all the parameters from a role entry, you need to specify the value `$Null` on the *Parameters* parameter. You don't need to include the *RemoveParameters* parameter.

Removing all the parameters from a role entry is most useful when you want to make only a few parameters available on a cmdlet and exclude all of the other parameters. If you don't want the role to have access to a cmdlet, remove the associated role entry from the role completely instead of just removing the parameters. For more information about how to remove a role entry from a role, see [Remove a role entry from a role](remove-a-role-entry-from-a-role-exchange-2013-help.md).

> [!WARNING]
> You can't undo remove operations. If you mistakenly remove all the parameters from a role entry, you must add them again manually.

To remove all the parameters from a role entry, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<cmdlet> -Parameters $Null
```

This example removes all the parameters from the **Set-CASMailbox** cmdlet on the Recipient Administrators role.

```powershell
Set-ManagementRoleEntry "Recipient Administrators\Set-CASMailbox" -Parameters $Null
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).

## Use the Shell to apply a specific set of parameters

If you want only a specific set of parameters to be included on a role entry, specify the *Parameters* parameter only. Don't include the *AddParameter* or *RemoveParameter* parameters. When you specify only the *Parameters* parameter, only the parameters you specify in the command are included on the role entry. All other parameters are removed.

To specify a specific set of parameters, use the following syntax.

```powershell
Set-ManagementRoleEntry <role name>\<cmdlet> -Parameters <parameter 1>, <parameter 2>, <parameter...>
```

This example includes only the *Identity*, *DisplayName*, *MissedCallNotificationEnabled*, and *PersonalAuthAttendantEnabled* parameters on the **Set-UMMailbox** cmdlet on the Seattle Mail Recipients role.

```powershell
Set-ManagementRoleEntry "Seattle Mail Recipients\Set-UMMailbox" -Parameters Identity, DisplayName, MissedCallNotificationEnabled, PersonalAutoAttendantEnabled
```

For detailed syntax and parameter information, see [Set-ManagementRoleEntry](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleEntry).
