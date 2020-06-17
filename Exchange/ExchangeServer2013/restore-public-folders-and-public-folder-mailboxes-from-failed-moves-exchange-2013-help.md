---
title: 'Restore public folders and public folder mailboxes from failed moves'
TOCTitle: Restore public folders and public folder mailboxes from failed moves
ms:assetid: 2ade83c9-5f9b-4945-bf32-48fa8185b515
ms:mtpsurl: https://technet.microsoft.com/library/JJ983802(v=EXCHG.150)
ms:contentKeyID: 51407261
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Restore public folders and public folder mailboxes from failed moves

_**Applies to:** Exchange Server 2013_

If a move request for a public folder or public folder mailbox fails, you can restore the folder or mailbox as long as the following conditions apply:

- **Failed public folder move**: A soft-deleted copy of the public folder still exists in the source public folder mailbox and is still within the retention period.

- **Failed public folder mailbox move**: A soft-deleted copy of the public folder mailbox still exists in the source mailbox database and is still within the mailbox retention period.

If the mailbox retention period has elapsed, you can recover an individual public folder mailbox from backup. You then extract data from the restored mailbox and copy it to a target folder or merge it with another mailbox. For more information, see [Restore data using a recovery database](restore-data-using-a-recovery-database-exchange-2013-help.md).

For additional management tasks related to public folders, see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox restore request" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't use the EAC to perform this procedure. You must use the Shell.

- To create a restore request, you must provide the values for the *DisplayName*, *LegacyDN*, or *MailboxGUID* parameters for the soft-deleted public folder mailbox.

- By default, dumpster folders will be restored along with regular folders. This can be prevented by using the *ExcludeDumpster* parameter.

- Restoring public folder mailboxes differs from restoring regular mailboxes in that folders won't be created if they don't exist in the target mailbox. Missing folders will be displayed in a warning message at the end of restore request.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Restore a soft-deleted public folder

This example restores the public folder \\Dev\\CustomerEnagagements to the target public folder mailbox Development01.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox Development -SourceDatabase MBX_DB01 -TargetMailbox Development01 -AllowLegacyDNMismatch -IncludeFolders \Dev\CustomerEngagements
```

For detailed syntax and parameter information, see [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest).

## Restore a soft-deleted public folder mailbox

This example restores the public folder mailbox PF\_Singapore to the new public folder mailbox PF\_Singapore\_Restore.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox PF_Singapore -SourceDatabase MBX_DB01 -TargetMailbox PF_Singapore_Restore -AllowLegacyDNMismatch
```

For detailed syntax and parameter information, see [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest).
