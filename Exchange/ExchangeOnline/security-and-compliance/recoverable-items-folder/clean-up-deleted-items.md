---
localization_priority: Normal
description: 'Summary: Admins can learn how to remove items from the Recoverable Items folder in Exchange Online.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 82c310f8-de2f-46f2-8e1a-edb6055d6e69
ms.reviewer:
f1.keywords:
- NOCSH
title: Clean up or delete items from the Recoverable Items folder in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Clean up or delete items from the Recoverable Items folder in Exchange Online

The Recoverable Items folder (known in earlier versions of Exchange as *the dumpster*) exists to protect from accidental or malicious deletions and to facilitate discovery efforts commonly undertaken before or during litigation or investigations.

How you clean up or delete items from a user's Recoverable Items folder depends on whether the mailbox is placed on In-Place Hold or Litigation Hold, or had single item recovery enabled:

- If a mailbox isn't placed on In-Place Hold or Litigation Hold or other types of holds in Office 365, or if a mailbox doesn't have single item recovery enabled, you can simply delete items from the Recoverable Items folder. After items are deleted, you can't use single item recovery to recover them.

- If the mailbox is placed on In-Place Hold or Litigation Hold or other types of holds in Office 365, or if single item recovery is enabled, you'll want to preserve the mailbox data until the hold is removed or single item recovery is disabled. In this case, you need to perform more detailed steps to clean up the Recoverable Items folder.

To learn more about In-Place Hold and Litigation Hold, see [In-Place Hold and Litigation Hold in Exchange Online](../in-place-and-litigation-holds.md). To learn more about single item recovery, see [Single item recovery](recoverable-items-folder.md#single-item-recovery).

## What do you need to know before you begin?

- By default, the Mailbox Import Export role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Mailbox Import Export role, you need to add the role to a role group. For more information, see [Manage role groups in Exchange Online](../../permissions-exo/role-groups.md)|

- Because incorrectly cleaning up the Recoverable Items folder can result in data loss, it's important that you're familiar with the Recoverable Items folder and the impact of removing its contents. Before performing this procedure, we recommend that you review the information in [Recoverable Items folder in Exchange Online](recoverable-items-folder.md).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to delete items from the Recoverable Items folder for mailboxes that aren't placed on hold or don't have single item recovery enabled

This example permanently deletes items from the user Gurinder Singh's Recoverable Items folder and also copies the items to the GurinderSingh-RecoverableItems folder in the Discovery Search Mailbox (a built-in mailbox in Exchange Online).

```powershell
Search-Mailbox -Identity "Gurinder Singh" -SearchDumpsterOnly -TargetMailbox "Discovery Search Mailbox" -TargetFolder "GurinderSingh-RecoverableItems" -DeleteContent
```

> [!NOTE]
> To delete items from the mailbox without copying them to another mailbox, use the preceding command without the _TargetMailbox_ and _TargetFolder_ parameters.

For detailed syntax and parameter information, see [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/search-mailbox).

## Use Exchange Online PowerShell to clean up the Recoverable Items folder for mailboxes that are placed on hold or have single item recovery enabled

This scenario is fully covered in the topic [Delete items in the Recoverable Items folder of cloud-based mailboxes on hold](https://docs.microsoft.com/office365/securitycompliance/delete-items-in-the-recoverable-items-folder-of-mailboxes-on-hold).

## How do you know this worked?

To verify that you've successfully cleaned up or deleted items from the Recoverable Items folder of a mailbox, use [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/get-mailboxfolderstatistics) cmdlet the check the size of the Recoverable Items folder.

This example retrieves the size of the Recoverable Items folder and its subfolders and an item count in the folder and each subfolder.

```powershell
Get-MailboxFolderStatistics -Identity "Gurinder Singh" -FolderScope RecoverableItems | Format-Table Name,FolderAndSubfolderSize,ItemsInFolderAndSubfolders -Auto
```
