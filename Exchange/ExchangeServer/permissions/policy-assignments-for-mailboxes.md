---
localization_priority: Normal
description: 'Summary: Learn how to change the management role assignment policy assigned to a mailbox.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 011690a5-233a-4c03-8842-92276f899a89
ms.reviewer:
title: Change the assignment policy on a mailbox
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Change the assignment policy on a mailbox

When you change a mailbox's assignment policy, the change takes effect as soon as the user refreshes the connection, such as the next time they log into their mailbox or open the mailbox options page. For more information about assignment policies in Exchange Server, see [Understanding Management Role Assignment Policies](https://docs.microsoft.com/exchange/understanding-management-role-assignment-policies-exchange-2013-help).

Looking for other management tasks related to permissions? Check out [Permissions](permissions.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](feature-permissions/rbac-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to change the assignment policy on a mailbox

1. In the Exchange admin center (EAC), navigate to **Recipients** \> **Mailboxes**.

2. Select the user or resource mailbox you want to change the assignment policy on and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.png).

3. Select **Mailbox Features**.

4. In the **Role assignment policy** list, select the assignment policy you want to assign to the mailbox and then click **Save**.

## Use the Exchange Management Shell to change the assignment policy on a mailbox

To change the assignment policy that's assigned to a mailbox, use the following syntax.

```powershell
Set-Mailbox <mailbox alias or name> -RoleAssignmentPolicy <assignment policy>
```

This example sets the assignment policy to Engineering Users on the mailbox Brian.

```powershell
Set-Mailbox Brian -RoleAssignmentPolicy "Engineering Users"
```

## Use the Exchange Management Shell to change the assignment policy on a group of mailboxes assigned a specific assignment policy

> [!NOTE]
> You can't use the EAC to change the assignment policy on a group of mailboxes all at once.

This procedure makes use of pipelining, the **Where** cmdlet, and the _WhatIf_ parameter. For more information about these concepts, see the following topics:

- [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines)

- [Working with Command Output](https://docs.microsoft.com/exchange/working-with-command-output-exchange-2013-help)

- [WhatIf, Confirm, and ValidateOnly Switches](https://docs.microsoft.com/exchange/whatif-confirm-and-validateonly-switches-exchange-2013-help)

If you want to change the assignment policy for a group of mailboxes that are assigned a specific policy, use the following syntax.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "<assignment policy to find>"} | Set-Mailbox -RoleAssignmentPolicy <assignment policy to set>
```

This example finds all the mailboxes assigned to the Redmond Users - No Voicemail assignment policy and changes the assignment policy to Redmond Users - Voicemail Enabled.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "Redmond Users - No Voicemail"} | Set-Mailbox -RoleAssignmentPolicy "Redmond Users - Voicemail Enabled"
```

This example includes the _WhatIf_ parameter so that you can see all the mailboxes that would be changed without committing any changes.

```powershell
Get-Mailbox | Where {$_.RoleAssignmentPolicy -Eq "Redmond Users - No Voicemail"} | Set-Mailbox -RoleAssignmentPolicy "Redmond Users - Voicemail Enabled" -WhatIf
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) or [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).
