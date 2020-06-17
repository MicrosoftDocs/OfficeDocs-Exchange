---
localization_priority: Normal
description: You can add retention tags to a retention policy when the policy is created or any time thereafter. For details about how to create a retention policy, including how to simultaneously add retention tags, see Create a Retention Policy.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3a5196ce-2764-453d-9bc1-5ec22d06b40d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Add retention tags to or remove retention tags from a retention policy
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Add retention tags to or remove retention tags from a retention policy

You can add retention tags to a retention policy when the policy is created or any time thereafter. For details about how to create a retention policy, including how to simultaneously add retention tags, see [Create a Retention Policy](create-a-retention-policy.md).

A retention policy can contain the following retention tags:

- One or more retention policy tags (RPTs) for supported default folders

- One default policy tag (DPT) with the **Move to Archive** action

- One DPT with the **Delete and Allow Recovery** or the **Permanently Delete** action

- One DPT for voice mail

- Any number of personal tags

For more information about retention tags, see [Retention tags and retention policies](retention-tags-and-policies.md).

## What do you need to know before you begin?

- Estimated time to completion: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Retention tags aren't applied to a mailbox until they're linked to a retention policy and the Managed Folder Assistant processes the mailbox. Use the [Start-ManagedFolderAssistant](https://docs.microsoft.com/powershell/module/exchange/start-managedfolderassistant) cmdlet to manually trigger the assistant to process a specified mailbox.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to add or remove retention tags

1. Go to **Compliance management** \> **Retention policies**.

2. In the list view, select the retention policy to which you want to add retention tags and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. In **Retention Policy**, use the following settings:

   - **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) Click this button to add a retention tag to the policy.

   - **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.gif) Select a tag from the list, and then click this button to remove the tag from the policy.

## Use Exchange Online PowerShell to add or remove retention tags

This example adds the retention tags VPs-Default, VPs-Inbox, and VPs-DeletedItems to the retention policy RetPolicy-VPs, which doesn't already have retention tags linked to it.

> [!CAUTION]
> If the policy has retention tags linked to it, this command replaces the existing tags.

```PowerShell
Set-RetentionPolicy -Identity "RetPolicy-VPs" -RetentionPolicyTagLinks "VPs-Default","VPs-Inbox","VPs-DeletedItems"
```

This example adds the retention tag VPs-DeletedItems to the retention policy RetPolicy-VPs, which already has other retention tags linked to it.

```PowerShell
$TagList = (Get-RetentionPolicy "RetPolicy-VPs").RetentionPolicyTagLinks
$TagList.Add((Get-RetentionPolicyTag 'VPs-DeletedItems').DistinguishedName)
Set-RetentionPolicy "RetPolicy-VPs" -RetentionPolicyTagLinks $TagList
```

This example removes the retention tag VPs-Inbox from the retention policy RetPolicy-VPs.

```PowerShell
$TagList = (Get-RetentionPolicy "RetPolicy-VPs").RetentionPolicyTagLinks
$TagList.Remove((Get-RetentionPolicyTag 'VPs-Inbox').DistinguishedName)
Set-RetentionPolicy "RetPolicy-VPs" -RetentionPolicyTagLinks $TagList
```

For detailed syntax and parameter information, see [set-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/set-retentionpolicy) and [get-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/get-retentionpolicy).

## How do you know this worked?

To verify that you have successfully added or removed a retention tag from a retention policy, use the [get-RetentionPolicy](https://docs.microsoft.com/powershell/module/exchange/get-retentionpolicy) cmdlet to verify the _RetentionPolicyTagLinks_ property.

This example use the **Get-RetentionPolicy** cmdlet to retrieve retention tags added to the Default MRM Policy and pipes them to the **Format-Table** cmdlet to output only the name property of each tag.

```PowerShell
(Get-RetentionPolicy "Default MRM Policy").RetentionPolicyTagLinks | Format-Table name
```
