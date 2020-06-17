---
title: 'Remove a public folder: Exchange 2013 Help'
TOCTitle: Remove a public folder
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 334b831d-e372-4d85-a407-5c8a5d0e78de
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a public folder in Exchange 2013

_**Applies to:** Exchange Server 2013_

You may need to remove public folders that are no longer being used in your organization. To help determine which public folders should be removed, see [View statistics for public folders and public folder items](view-public-folder-statistics-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- You can't delete a mail-enabled public folder. Before you can delete it, you must first disable email for the public folder. For more information, see [Mail-enable or mail-disable a public folder](enable-or-disable-mail-for-public-folder-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to remove a public folder

1. Navigate to **Public folders** \> **Public folders**.

2. In the list view, select the public folder you want to delete. Note that clicking on the folder name will display sub-folders within that folder, if there are any. At that point you can click to select a specific sub-folder to remove.

     To delete a folder or sub-folder, click anywhere on the folder's row except the underlined name of the folder, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif). If you click the underlined name of the folder, the **Delete** option will not be available to select.

    ![Selecting a public folder to remove](images/8666290d-3f19-4c70-afe3-45569762718b.png)

3. A warning box displays asking if you're sure you want to delete the public folder. Click **Yes** to continue.

## Use the Shell to delete a public folder

This example deletes the public folder Help Desk\Resolved. This command assumes that the Resolved public folder doesn't have any subfolders.

```powershell
Remove-PublicFolder -Identity "\Help Desk\Resolved"
```

This example tests the previous command without making any modifications.

```powershell
Remove-PublicFolder -Identity "\HelpDesk\Resolved" -WhatIf
```

This example removes the public folder Marketing and all its subfolders because the command runs recursively.

```powershell
Remove-PublicFolder -Identity "\Marketing" -Recurse:$True
```

For detailed syntax and parameter information, see [Remove-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/remove-publicfolder).
