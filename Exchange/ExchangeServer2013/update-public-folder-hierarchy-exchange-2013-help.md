---
title: 'Update the public folder hierarchy: Exchange 2013 Help'
TOCTitle: Update the public folder hierarchy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: a7b2fb51-0207-4d7d-938d-466ae110bb90
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Update the public folder hierarchy in Exchange 2013

_**Applies to:** Exchange Server 2013_

You only need to update the public folder hierarchy if you want to manually invoke the hierarchy synchronizer and the mailbox assistant. Both these are invoked at least once every 24 hours for each public folder mailbox in the organization. The hierarchy synchronizer is invoked every 15 minutes if any users are logged on to a secondary mailbox through Microsoft Outlook or a Microsoft Exchange Web Services client.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- You can't perform this procedure in the EAC. You must use the Shell.

- We recommend that when you run this command with the _InvokeSynchronizer_ parameter, you use the _SuppressStatus_ parameter. If you don't use this parameter in the command, the output will display status messages every 3 seconds for up to one minute. Until the minute passes, you can't use that instance of the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Update the public folder hierarchy

This example updates the public folder hierarchy on the public folder mailbox PF_marketing and suppresses the command's output.

```powershell
Update-PublicFolderMailbox -Identity PF_marketing -InvokeSynchronizer -SuppressStatus
```

This example updates all public folder mailboxes and suppresses the command's output.

```powershell
Get-Mailbox -PublicFolder | Update-PublicFolderMailbox InvokeSynchronizer -SuppressStatus
```
