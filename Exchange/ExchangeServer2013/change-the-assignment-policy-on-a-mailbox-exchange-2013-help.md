---
title: 'Change the assignment policy on a mailbox: Exchange 2013 Help'
TOCTitle: Change the assignment policy on a mailbox
ms:assetid: 011690a5-233a-4c03-8842-92276f899a89
ms:mtpsurl: https://technet.microsoft.com/library/Dd638076(v=EXCHG.150)
ms:contentKeyID: 49289145
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change the assignment policy on a mailbox

_**Applies to:** Exchange Server 2013_

You can change the management role assignment policy assigned to a mailbox. When you change a mailbox's assignment policy, the change takes effect as soon as the user refreshes the connection, such as the next time they log into their mailbox or open the mailbox options page. For more information about assignment policies in Microsoft Exchange Server 2013, see [Understanding management role assignment policies](understanding-management-role-assignment-policies-exchange-2013-help.md).

Looking for other management tasks related to permissions? Check out [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to change the assignment policy on a mailbox

1. In the Exchange admin center (EAC), navigate to **Recipients** \> **Mailboxes**.

2. Select the user or resource mailbox you want to change the assignment policy on and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. Select **Mailbox Features**.

4. In the **Role assignment policy** list, select the assignment policy you want to assign to the mailbox and then click **Save**.

## Use the Shell to change the assignment policy on a mailbox

To change the assignment policy that's assigned to a mailbox, use the following syntax.

```powershell
Set-Mailbox <mailbox alias or name> -RoleAssignmentPolicy <assignment policy>
```

This example sets the assignment policy to Unified Messaging Users on the mailbox Brian.

```powershell
Set-Mailbox Brian -RoleAssignmentPolicy "Unified Messaging Users"
```

## Use the Shell to change the assignment policy on a group of mailboxes assigned a specific assignment policy

> [!NOTE]
> You can't use the EAC to change the assignment policy on a group of mailboxes all at once.

This procedure makes use of pipelining, the **Where** cmdlet, and the *WhatIf* parameter. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with command output](working-with-command-output-exchange-2013-help.md)

- [WhatIf, Confirm, and ValidateOnly switches](whatif-confirm-and-validateonly-switches-exchange-2013-help.md)

If you want to change the assignment policy for a group of mailboxes that are assigned a specific policy, use the following syntax.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "<assignment policy to find>"} | Set-Mailbox -RoleAssignmentPolicy <assignment policy to set>
```

This example finds all the mailboxes assigned to the Redmond Users - No Voicemail assignment policy and changes the assignment policy to Redmond Users - Voicemail Enabled.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "Redmond Users - No Voicemail"} | Set-Mailbox -RoleAssignmentPolicy "Redmond Users - Voicemail Enabled"
```

This example includes the *WhatIf* parameter so that you can see all the mailboxes that would be changed without committing any changes.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "Redmond Users - No Voicemail"} | Set-Mailbox -RoleAssignmentPolicy "Redmond Users - Voicemail Enabled" -WhatIf
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) or [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).
