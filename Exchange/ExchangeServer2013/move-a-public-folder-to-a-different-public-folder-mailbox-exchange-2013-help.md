---
title: 'Move a public folder to a different public folder mailbox: Exchange 2013 Help'
TOCTitle: Move a public folder to a different public folder mailbox
ms:assetid: b8744934-a3cb-443e-acce-a9a6ca5d88f6
ms:mtpsurl: https://technet.microsoft.com/library/JJ906435(v=EXCHG.150)
ms:contentKeyID: 50630968
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Move a public folder to a different public folder mailbox

_**Applies to:** Exchange Server 2013_

If the content of a public folder mailbox begins to exceed your mailbox quotas, you may need to move public folders to a different public folder mailbox. There are a couple ways to do this. To move one or more public folders that don't contain subfolders, you can use the **PublicFolderMoveRequest** cmdlets. If you need to move an entire public folder branch (which includes the parent public folder and all subfolders), you can use the `Move-PublicFolderBranch.ps1` script that's available when you install Exchange 2013.

For additional management tasks related to public folders see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task depends on the size of the public folder.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Public folders" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- You can't use the EAC to perform these procedures. You must use the Shell.

- If the folder you're moving has subfolders, those subfolders won't be moved by default. If you want to move a public folder and all its subfolders, use the **Move-PublicFolderBranch.ps1** script.

- Moving public folders only moves the physical contents of the public folder; it doesn't change the logical hierarchy.

- Depending on the size of the public folder and the amount of content it contains, the move may take several hours to complete. During that time, users will be able to access the public folders. However, users won't be able to access the public folders for a brief period while the folder is in the "Completion in Progress" state.

- You can perform only one public folder move request at a time. You must use the **Remove-PublicFolderMoveRequest** cmdlet to remove the request after it's complete.

- To check the status of an ongoing public folder move request, run the [Get-PublicFolderMoveRequest](https://docs.microsoft.com/powershell/module/exchange/Get-PublicFolderMoveRequest) cmdlet.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Move a single public folder

This example starts the move request for the public folder \\CustomerEnagagements from the public folder mailbox DeveloperReports to DeveloperReports01

```powershell
New-PublicFolderMoveRequest -Folders \DeveloperReports\CustomerEngagements -TargetMailbox DeveloperReports01
```

> [!NOTE]
> The target public folder mailbox will be locked while the move request is active.

For detailed syntax and parameter information, see [New-PublicFolderMoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-PublicFolderMoveRequest).

## Move multiple public folders

This example begins the move request for public folders under the \\Dev public folder branch to the target public folder mailbox DeveloperReports01. This example doesn't move the public folder \\Dev.

```powershell
New-PublicFolderMoveRequest -Folders \Dev\CustomerEngagements,\Dev\RequestsforChange,\Dev\Usability -TargetMailbox DeveloperReports01
```

> [!NOTE]
> The target public folder mailbox will be locked while the move request is active.

For detailed syntax and parameter information, see [New-PublicFolderMoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-PublicFolderMoveRequest).

## Move a branch of public folders

This example uses the `Move-PublicFolderBranch.ps1` script to move a branch of public folders. This starts the move request for the public folder \\Dev and all its subfolders to the public folder mailbox DeveloperReports01. The script is located in the scripts folder and must be run from that location.

```powershell
CD $env:ExchangeInstallPath\scripts
```

```powershell
.\Move-PublicFolderBranch.ps1 -FolderRoot \Dev -TargetPublicFolderMailbox DeveloperReports01
```

## How do you know this worked?

To verify that the public folder move request was successful, run the following command:

```powershell
Get-PublicFolderMoveRequest | Format-List Status
```

A status of `Completed` indicates that the move request was successful.

If the move request was unsuccessful, you may need to restore the public folder or its contents. For more information, see [Restore public folders and public folder mailboxes from failed moves](restore-public-folders-and-public-folder-mailboxes-from-failed-moves-exchange-2013-help.md).
