---
localization_priority: Normal
description: Placing a mailbox on retention hold suspends the processing of a retention policy or managed folder mailbox policy for that mailbox. Retention hold is designed for situations such as a user being on vacation or away temporarily.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 2baac4a7-3402-4142-bfb3-1649a950e677
ms.reviewer: 
title: Place a mailbox on retention hold
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Place a mailbox on retention hold

Placing a mailbox on retention hold suspends the processing of an MRM retention policy by the Managed Folder Assistant for that mailbox. Retention hold is designed for situations such as a user being on vacation or away temporarily.

During retention hold, users can log on to their mailbox and change or delete items. When you perform a mailbox search, deleted items that are past the deleted item retention period aren't returned in search results. To make sure items changed or deleted by users are preserved in legal hold scenarios, you must place a mailbox on legal hold. For more information, see [Create or remove an In-Place Hold](../../security-and-compliance/create-or-remove-in-place-holds.md).

You can also include retention comments for mailboxes you place on retention hold. The comments are displayed in supported versions of Microsoft Outlook.

For additional management tasks related to messaging records management (MRM), see [Messaging Records Management Procedures](https://technet.microsoft.com/library/bc2ff408-4a2b-4202-9515-e3e922a6320d.aspx).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging Policy and Compliance Permissions](https://technet.microsoft.com/library/ec4d3b9f-b85a-4cb9-95f5-6fc149c3899b.aspx) topic.

- You can't use the Exchange admin center (EAC) to place a mailbox on retention hold. You must use Exchange Online PowerShell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to place a mailbox on retention hold

This example places Michael Allen's mailbox on retention hold.

```PowerShell
Set-Mailbox "Michael Allen" -RetentionHoldEnabled $true
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailbox).

## Use Exchange Online PowerShell to remove retention hold for a mailbox

This example removes the retention hold from Michael Allen's mailbox.

```PowerShell
Set-Mailbox "Michael Allen" -RetentionHoldEnabled $false
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/set-mailbox).

## How do you know this worked?

To verify that you have successfully placed a mailbox on retention hold, use the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/get-mailbox) cmdlet to retrieve the *RetentionHoldEnabled* property of the mailbox.

This command retrieves the *RetentionHoldEnabled* property for Michael Allen's mailbox.

```PowerShell
Get-Mailbox "Michael Allen" | Select RetentionHoldEnabled
```

This command retrieves all mailboxes in the Exchange organization, filters the mailboxes that are placed on retention hold, and lists them along with the retention policy applied to each.

> [!IMPORTANT]
> Because *RetentionHoldEnabled* isn't a filterable property in Exchange Server, you can't use the _Filter_ parameter with the **Get-Mailbox** cmdlet to filter mailboxes that are placed on retention hold on the server-side. This command retrieves a list of all mailboxes and filters on the client running Exchange Online PowerShell session. In large environments with thousands of mailboxes, this command may take a long time to complete.

```PowerShell
Get-Mailbox -ResultSize unlimited | Where-Object {$_.RetentionHoldEnabled -eq $true} | Format-Table Name,RetentionPolicy,RetentionHoldEnabled -Auto
```

## Difference between *ElcProcessingDisabled* and *RetentionHoldEnabled*

*ElcProcessingDisabled* is another mailbox property that's related to the processing of a mailbox by the Managed Folder Assistant (the default value for this property is **False**). When the *ElcProcessingDisabled* property is set to **True** (by using the `Set-Mailbox -ElcProcessingDisabled $true` command), it prevents the Managed Folder Assistant from processing the mailbox at all. So in addition to not processing the MRM retention policy, other functions performed by the Managed Folder assistant, such as expiring items in the Recoverable Items folder by marking them for permanent removal, won't be performed. For more information, see [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/organization/set-organizationconfig).

In contrast, when *RetentionHoldEnabled* is set to **True**, the Managed Folder Assistant will continue to process the MRM retention policy on the mailbox (including applying retention tags to items), but it will not expire items in folders that are visible to the user (that is, in folders in the IPM subtree of the mailbox). However, the Managed Folder Assistant will continue to process items in the Recoverable Items folder, including purging expired items. So setting *ElcProcessingDisabled* to **True** is more restrictive and has more consequences than setting the *RetentionHoldEnabled* property to **True**.

Another significant difference between these two mailbox properties is that the *ElcProcessingDisabled* property can be set at the organizational level with the `Set-OrganizationConfig -ElcProcessingDisabled $true` command (the default setting is **False**). This means that you could prevent the Managed Folder Assistant from processing all mailboxes in your organization. In contrast, you can only set the *RetentionHoldEnabled* property on a per mailbox basis.

Keep the following things in mind when managing the *ElcProcessingDisabled* property for a mailbox:

- If the *ElcProcessingDisabled* property is set to **False** on a mailbox, but the organizational setting is set to **True**, the organizational setting overrides the mailbox setting and the Managed Folder Assistant won't process the mailbox.

- If the *ElcProcessingDisabled* property is set to **True** on a mailbox, but the organizational setting is set to **False**,  the Managed Folder Assistant won't process the mailbox.

- If an Office 365 or Microsoft 365 retention policy with a Preservation Lock is applied to a mailbox, then the setting of the *ElcProcessingDisabled* property (at both the mailbox and organizational level) will be ignored. In other words, the Managed Folder Assistant can't be disabled for any mailbox that's been assigned a retention policy that's been locked. For more information, see [Locking a retention policy](https://docs.microsoft.com/microsoft-365/compliance/retention-policies#locking-a-retention-policy).
