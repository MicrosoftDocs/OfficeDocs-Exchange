---
localization_priority: Normal
description: This topic explains how to retrieve statistics about a public folder, such as the display name, creation time, last user modified time, last user access, and item size. You can use this information to make decisions about deleting or retaining public folders.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4e412710-9a74-4649-ab01-502e969a7eda
ms.reviewer: 
f1.keywords:
- NOCSH
title: View statistics for public folders and public folder items
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# View statistics for public folders and public folder items

This topic explains how to retrieve statistics about a public folder, such as the display name, creation time, last user modified time, last user access, and item size. You can use this information to make decisions about deleting or retaining public folders.

> [!NOTE]
> In the Exchange admin center (EAC), you can view some of the quota and usage information for public folders by navigating to **Public Folders** \> **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif) \> **Mailbox usage**. However, this information is incomplete, and we recommend that you use Exchange Online PowerShell to view public folder statistics.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- You can't use the EAC to retrieve public folder statistics.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to retrieve public folder statistics

This example returns the statistics for the public folder Marketing with a piped command to format the list.

```PowerShell
Get-PublicFolderStatistics -Identity \Marketing | Format-List
```

> [!NOTE]
> The value for the _Identity_ parameter must include the path to the public folder. For example, if the public folder Marketing existed under the parent folder Business, you would provide the following value: `\Business\Marketing`

For detailed syntax and parameter information, see [Get-PublicFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderstatistics).

## Use Exchange Online PowerShell to view statistics for public folder items

You can view the following information about items within a public folder:

- Type of item

- Subject

- Last user modification time

- Last user access time

- Creation time

- Attachments

- Message size

You can use this information to make decisions about what actions to take for your public folders, such as which public folders to delete. For example, you may want to delete a public folder if the items haven't been accessed for over two years, or you may want to convert a public folder that's being used as a document repository to another client access application.

This example returns default statistics for all items in the public folder Pamphlets under the path \Marketing\2013. Default information includes item identity, creation time, and subject.

```PowerShell
Get-PublicFolderItemStatistics -Identity "\Marketing\2013\Pamphlets"
```

This example returns additional information about the items within the public folder Pamphlets, such as subject, last modification time, creation time, attachments, message size, and the type of item. It also includes a piped command to format the list.

```PowerShell
Get-PublicFolderItemStatistics -Identity "\Marketing\2010\Pamphlets" | Format-List
```

For detailed syntax and parameter information, see [Get-PublicFolderItemStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderitemstatistics).

## Use Exchange Online PowerShell to export the output of the Get-PublicFolderItemStatistics cmdlet to a .csv file

This example exports the output of the cmdlet to the PFItemStats.csv file that includes the following information for all items within the public folder \Marketing\Reports:

- Subject of the message ( `Subject`)

- Date and time that the item was last modified ( `LastModificationTime`)

- Whether the item has attachments ( `HasAttachments`)

- Type of item ( `ItemType)`

- Size of the item ( `MessageSize`)

```PowerShell
Get-PublicFolderItemStatistics -Identity "\Marketing\Reports" | Select Subject,LastModificationTime,HasAttachments,ItemType,MessageSize | Export-CSV C:\PFItemStats.csv
```

For detailed syntax and parameter information, see [Get-PublicFolderItemStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderitemstatistics).
