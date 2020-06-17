---
title: 'Get Recoverable Items folder statistics: Exchange 2013 Help'
TOCTitle: Get Recoverable Items folder statistics
ms:assetid: dee77958-ee87-4908-85e4-ad053bacd8b0
ms:mtpsurl: https://technet.microsoft.com/library/Ff714343(v=EXCHG.150)
ms:contentKeyID: 51439484
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Get Recoverable Items folder statistics

_**Applies to:** Exchange Server 2013_

The Recoverable Items folder contains items deleted by Microsoft Outlook and Microsoft Office Outlook Web App users or by the Mailbox Assistant. The duration that deleted items remain in this folder is based on the deleted item retention settings configured for the mailbox database or the mailbox. By default, a mailbox database is configured to retain deleted items for 14 days, and the Recoverable Items warning quota and Recoverable Items quota are set to 20 gigabytes (GB) and 30 GB respectively. However, if In-Place Hold or Litigation Hold is enabled for the mailbox, the Recoverable Items folder can accumulate deleted items beyond the specified retention period and can also maintain different versions of modified mailbox items.

When the Recoverable Items folder reaches the Recoverable Items warning quota, a warning event is logged in the Application event log. If the mailbox isn't on Litigation Hold, items are then removed on a first in, first out (FIFO) basis. However, if the mailbox is on Litigation Hold, the mailbox is never emptied and upon reaching the Recoverable Items quota, mailbox functionality is impacted.

Therefore, it's important to monitor the event log for alerts generated when mailboxes reach the Recoverable Items quotas. You can also use this procedure to report statistics for the Recoverable Items folder, particularly for mailboxes placed on Litigation Hold.

To learn more, see the following topics:

- [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md)

- [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds)

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox folders" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to get Recoverable Items folder statistics for a mailbox.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Get Recoverable Items folder statistics for a mailbox

This example gets folder statistics for Soumya Singhi's Recoverable Items folder and displays the output in a list format.

```powershell
Get-MailboxFolderStatistics -Identity "Soumya Singhi" -FolderScope RecoverableItems | Format-List
```

This example gets folder statistics for Soumya Singhi's Recoverable Items folder and displays the folder name, folder path, number of items in the folder, and folder size in a table format.

```powershell
Get-MailboxFolderStatistics -Identity "Soumya Singhi" -FolderScope RecoverableItems | Format-Table Name,FolderPath,ItemsInFolder,FolderAndSubfolderSize
```

For detailed syntax and parameter information, see [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxFolderStatistics).

## Get Recoverable Items folder statistics for all mailboxes on Litigation Hold

This example retrieves a list of all mailboxes placed on Litigation Hold and retrieves mailbox folder statistics for the Recoverable Items folder and its subfolders for each mailbox. The **Identity** (mailbox folder identity) and the **FolderAndSubfolderSize** properties are displayed in a table format.

```powershell
Get-Mailbox -ResultSize Unlimited -Filter "LitigationHoldEnabled -eq `$true" | Get-MailboxFolderStatistics -FolderScope RecoverableItems | Format-Table Identity,FolderAndSubfolderSize
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) and [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxFolderStatistics).

## Get Recoverable Items quota for a mailbox

This example displays the quota and warning quota for the Recoverable Items folder for a user mailbox. The example also retrieves information about whether a Litigation Hold or an In-Place Hold is placed on the mailbox.

```powershell
Get-Mailbox -Identity <identity of mailbox> | Format-List RecoverableItems*,LitigationHoldEnabled,InPlaceHolds
```

This example displays the quota and warning quota for the Recoverable Items folder for all user mailboxes in your organization. The example also retrieves hold information.

```powershell
Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,RecoverableItems*,LitigationHoldEnabled,InPlaceHolds
```
