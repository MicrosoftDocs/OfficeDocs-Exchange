---
title: 'Turn off or suspend messaging records management: Exchange 2013 Help'
TOCTitle: Turn off or suspend messaging records management
ms:assetid: 631191aa-3bba-4ebf-a727-c48ed2ebe176
ms:mtpsurl: https://technet.microsoft.com/library/Aa998580(v=EXCHG.150)
ms:contentKeyID: 51439479
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Turn off or suspend messaging records management

_**Applies to:** Exchange Server 2013_

To meet individual, IT, or business requirements, you may need to turn off or temporarily suspend messaging records management (MRM) for an individual user or for a Mailbox server. Reasons you may need to turn off or suspend MRM include:

- If a mailbox user is away from the office or is otherwise unable to access e-mail, you can temporarily disable MRM for the mailbox by placing it on retention hold. When a mailbox is on retention hold, it's no longer processed by the Managed Folder Assistant. When the mailbox user returns or is able to access the mailbox again, you can remove the retention hold from the mailbox.

- If you need to test or troubleshoot performance issues, you can temporarily turn off MRM on that server by clearing the schedule for the Managed Folder Assistant.

- If you need to remove a retention tag from mailboxes (which have a retention policy with that tag applied), you can remove the tag from the policy.

- If you want a retention policy or a managed folder mailbox policy to no longer apply to a mailbox, you can remove the policy from the mailbox.

- If your organization decides not to use MRM features, you can turn off MRM permanently for the entire organization. If you later decide to deploy MRM, you have the ability to do so.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Place mailboxes on retention hold

You can place mailboxes on retention hold to turn off MRM temporarily (for example when users are on vacation). This suspends the processing of retention policies for the mailbox until retention hold is disabled. This is different from placing mailboxes on In-Place Hold or litigation hold.

For details about how to place a mailbox on retention hold, see [Place a mailbox on retention hold](https://docs.microsoft.com/exchange/security-and-compliance/messaging-records-management/mailbox-retention-hold).

To learn more about In-Place Hold and litigation hold, see [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds).

## Remove retention tags from mailboxes

To remove a retention tag from a mailbox, you unlink the tag from the retention policy. When you unlink a retention policy tag (RPT) for a default folder, the default mailbox tag applies to all items in that folder. When you unlink a personal tag, it's no longer available to the user. Tags applied to existing messages will continue to be processed unless you remove the tag from the Exchange organization.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

This Shell example unlinks the retention tag Delete - 3 Days from the retention policy Corp-Users.

```powershell
$tags = (Get-RetentionPolicy "Corp-Users").RetentionPolicyTagLinks
$tags -= "Deleted Items - 3 Days"
Set-RetentionPolicy "Corp-Users" -RetentionPolicyTagLinks $tags
```

For detailed syntax and parameter information, see [Get-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/Get-RetentionPolicy) and [Set-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-RetentionPolicy).

## Remove retention policies from mailboxes

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Apply retention policies" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

You can stop a retention policy from applying to a mailbox by removing the policy from the mailbox user's properties.

This Shell example removes the retention policy from the mailbox jpeoples.

```powershell
Set-Mailbox jpeoples -RetentionPolicy $null.
```

This Shell example removes the retention policy from all mailboxes in the Exchange organization.

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RetentionPolicy -ne `$null" | Set-Mailbox -RetentionPolicy $null
```

This Shell example removes the retention policy Corp-Finance from all mailbox users who have the policy applied.

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RetentionPolicy -eq 'Corp-Finance'" | Set-Mailbox -RetentionPolicy $null
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) and [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox).

## Turn off MRM permanently for an entire organization

To turn off MRM for an organization, delete all retention tags and retention policies except for the ArbitrationMailbox policy, which is created by Exchange Setup. After this is complete, retention policies aren't enforced.

> [!WARNING]
> Retention policies also include Move to Archive tags, which move messages to the user's archive mailbox. If you remove a retention policy that has a Move to Archive tag, users who had the policy applied will no longer have messages moved to the archive by the Managed Folder Assistant.<BR>To avoid this, remove only the Delete and Allow Recovery and Permanently Delete tags from your organization and keep the policies that have the Move to Archive tags applied. Alternatively, users who have and archive enabled could manually move items to their archive mailbox using Outlook or Outlook Web App.<BR>Before removing retention tags or retention policies, we recommend that you check the settings of the tags being removed. Don't delete tags with the Move to Archive retention action.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

> [!NOTE]
> Include the <EM>WhatIf</EM> switch in the following commands to simulate the action taken by a command.

This example removes all delete tags from an Exchange organization except the Never Delete tag, which is used in the ArbitrationMailbox policy created by Exchange Setup.

```powershell
Get-RetentionPolicyTag | ? {$_.RetentionAction -ne "MoveToArchive" -and $_.Name -ne "Never Delete"} | Remove-RetentionPolicyTag
```

This example removes all retention tags except the Never Delete tag.

```powershell
Get-RetentionPolicyTag | ? {$_.Name -ne "Never Delete"} | Remove-RetentionPolicyTag
```

This command removes the Corp-Users retention policy from an Exchange organization.

```powershell
Remove-RetentionPolicy Corp-Users
```

For detailed syntax and parameter information, see the following topics:

- [Get-RetentionPolicyTag](https://docs.microsoft.com/powershell/module/exchange/Get-RetentionPolicyTag)

- [Remove-RetentionPolicyTag](https://docs.microsoft.com/powershell/module/exchange/Remove-RetentionPolicyTag)

- [Remove-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/Remove-RetentionPolicy)
