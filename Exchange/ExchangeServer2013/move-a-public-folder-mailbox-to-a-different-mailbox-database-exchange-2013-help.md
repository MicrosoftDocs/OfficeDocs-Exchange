---
title: 'Move a public folder mailbox to a different mailbox database'
TOCTitle: Move a public folder mailbox to a different mailbox database
ms:assetid: 67601d45-4824-4ae6-9a7e-b645ec3af4d3
ms:mtpsurl: https://technet.microsoft.com/library/JJ906434(v=EXCHG.150)
ms:contentKeyID: 50630967
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Move a public folder mailbox to a different mailbox database

_**Applies to:** Exchange Server 2013_

You may need to move a public folder mailbox to a different mailbox database for load balancing purposes or for moving resources closer to their geographical location. Similar to moving a regular mailbox, you use the **MoveRequest** cmdlets to move a public folder mailbox. For more information about moving mailboxes, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

For additional management tasks related to public folders, see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete depends on the size of the public folder mailbox.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox move and migration permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't perform this procedure in the EAC. You can only perform this procedure in the Shell.

- Depending on the size of the public folder mailbox, the move may take several hours to complete. During that time, users won't be able to access the public folders. Users also won't have access to public folders for a brief period while the folder is in the "Completion in Progress" state.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a move request

The **New-MoveRequest** cmdlet queues the public folder mailbox into the Microsoft Exchange Mailbox Replication service queue. When the Microsoft Exchange Mailbox Replication service is available, it will pick up the move request and begin moving the public folder mailbox. It will complete the entire request from beginning to end.

This example begins the move request for the public folder mailbox PF\_SanFrancisco to the mailbox database MBX\_DB01.

```powershell
New-MoveRequest -Identity "PF_SanFrancisco" -TargetDatabase MBX_DB01
```

For detailed syntax and parameter information, see [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## Create a move request to complete at a later time

During the final stage of the move request, when it's in the `CompletionInProgress` phase, users may be temporarily locked out the public folder mailbox. If needed, you can suspend the move request before it reaches that phase and resume it at a time when users won't be impacted.

This example begins the move request for the public folder mailbox PF\_SanFrancisco to the mailbox database MBX\_DB01, and suspends it when the move request is ready to complete.

```powershell
New-MoveRequest -Identity "PF_SanFrancisco" -TargetDatabase MBX_DB01 -SuspendWhenReadyToComplete
```

For detailed syntax and parameter information, see [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

This example retrieves the status of the ongoing mailbox move for the public folder mailbox PF\_SanFrancisco.

```powershell
Get-MoveRequest -Identity "PF_SanFrancisco"
```

For detailed syntax and parameter information, see [Get-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/Get-MoveRequest).

When the move request reaches the status of Suspended, you can resume the request. This example resumes the move request for the public folder mailbox PF\_SanFrancisco.

```powershell
Resume-MoveRequest -Identity "PF_SanFrancisco"
```

For detailed syntax and parameter information, see [Resume-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/Resume-MoveRequest).

## How do you know this worked?

To verify that the move request was successfully created, run the following command:

```powershell
Get-MoveRequestStatistics -Identity PF_SanFrancisco | Format-List Status
```

A status of `Completed` indicates that the move request was successful.

If the move was unsuccessful, you may need to restore the public folder. For more information, see [Restore public folders and public folder mailboxes from failed moves](restore-public-folders-and-public-folder-mailboxes-from-failed-moves-exchange-2013-help.md).
