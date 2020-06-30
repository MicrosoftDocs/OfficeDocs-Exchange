---
localization_priority: Normal
description: Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization. Public folders help organize content in a deep hierarchy that's easy to browse. Users will see the full hierarchy in Outlook, which makes it easy for them to browse for the content they're interested in.
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bf65842b-a4db-49a8-bb3a-d0bafb7d3e45
ms.reviewer: 
f1.keywords:
- NOCSH
title: Public folders in Microsoft 365, Office 365, and Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Public folders in Microsoft 365, Office 365, and Exchange Online

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization. Public folders help organize content in a deep hierarchy that's easy to browse. Users will see the full hierarchy in Outlook, which makes it easy for them to browse for the content they're interested in.

> [!NOTE]
> Public folders are available in the following Outlook clients: Outlook on the web (formerly known as Outlook Web App), Outlook 2007 or later, and Outlook for Mac.

Public folders can also be used as an archiving method for distribution groups. When you mail-enable a public folder and add it as a member of the distribution group, email sent to the group is automatically added to the public folder for later reference.

Public folders aren't designed for the following purposes:

- **Data archiving**. Users who have mailbox limits sometimes use public folders instead of mailboxes to archive data. This practice isn't recommended because it affects storage in public folders and undermines the goal of mailbox limits. Instead, we recommend that you use [In-Place Archiving](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes) as your archiving solution.

- **Document sharing and collaboration**. Public folders don't provide versioning or other document management features, such as controlled check-in and check-out functionality and automatic notifications of content changes. Instead, we recommend that you use [SharePoint Online](https://products.office.com/SharePoint/sharepoint-online-collaboration-software) as your documentation sharing solution.

For more information about public folders and other collaboration methods in Microsoft 365, Office 365, and Exchange Online, see [Collaboration in Exchange Online](../../collaboration-exo/collaboration-exo.md).

For more information about public folder quotas in Microsoft 365, Office 365, and Exchange Online, see the service description topics [Sharing and collaboration](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/sharing-and-collaboration) and [Exchange Online limits](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits).

For a list of public folder management tasks, see [Public folder procedures in Microsoft 365, Office 365, and Exchange Online](public-folder-procedures.md).

For more information about the public folder limits in Microsoft 365, Office 365, and Exchange Online, see [Exchange Online limits](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits).

Looking for the Exchange Server version of this topic? See [Public folders in Microsoft 365, Office 365, and Exchange Online](public-folders.md).

## Public folder architecture

Public folder architecture uses specially designed mailboxes to store both the public folder hierarchy and the content. The main architectural components of public folders are the public folder mailboxes.

### Public folder mailboxes

There are two types of public folder mailboxes: the primary hierarchy mailbox and secondary hierarchy mailboxes. Both types of mailboxes can contain content:

- **Primary hierarchy mailbox**: The primary hierarchy mailbox is the one writable copy of the public folder hierarchy. The public folder hierarchy is copied to all other public folder mailboxes, but these will be read-only copies.

- **Secondary hierarchy mailboxes**: Secondary hierarchy mailboxes contain public folder content as well and a read-only copy of the public folder hierarchy.

There are two ways you can manage public folder mailboxes:

- In the Exchange admin center (EAC), navigate to **Public folders** \> **Public folder mailboxes**.

- In Exchange Online PowerShell, use the **\*-Mailbox** set of cmdlets.

### Public folder hierarchy

The public folder hierarchy contains the folders' properties and organizational information, including tree structure. Each public folder mailbox contains a copy of the public folder hierarchy. There's only one writeable copy of the hierarchy, which is in the primary public folder mailbox. For a specific folder, the hierarchy information is used to identify the following:

- Permissions on the folder

- The folder's position in the public folder tree, including its parent and child folders

> [!NOTE]
> The hierarchy doesn't store information about email addresses for mail-enabled public folders. Email addresses are stored in the directory.

#### Hierarchy synchronization

The public folder hierarchy synchronization process uses Incremental Change Synchronization (ICS), which provides a mechanism to monitor and synchronize changes to an Exchange store hierarchy or content. The changes include creating, modifying, and deleting folders and messages. When users are connected to and using content mailboxes, synchronization occurs every 15 minutes. If no users are connected to content mailbox, synchronization will be triggered less often (every 24 hours).If a write operation such as a creating a folder is performed on the primary hierarchy, synchronization is triggered immediately (synchronously) to the content mailbox.

> [!IMPORTANT]
> Because there's only one writeable copy of the hierarchy, folder creation is proxied to the hierarchy mailbox by the content mailbox users are connected to.

For more information, see [Update the public folder hierarchy](update-public-folder-hierarchy.md).

#### Public folder content

Public folder content can include email messages, posts, documents, and eForms. The content is stored in the public folder mailbox but isn't replicated across multiple public folders mailboxes. All users access the same public folder mailbox for the same set of content. Although a full text search of public folder content is available, public folder content isn't searchable across public folders (except when using the [Content Search eDiscovery tool](https://docs.microsoft.com/microsoft-365/compliance/content-search) in the Microsoft 365 compliance center) and the content isn't indexed by Exchange Search.

## Considerations

Although there are many advantages to using public folders in Microsoft 365, Office 365, and Exchange Online, there are some things to consider before implementing them in your organization:

- Outlook on the web is supported, but with limitations. You can add and remove favorite public folders and perform item-level operations such as creating, editing, deleting posts, and replying to posts. However, you can't create or delete public folders from Outlook on the web.

- Although a full text search of public folder content is available, public folder content isn't searchable across public folders and the content isn't indexed by Exchange Search.

- You must use Exchange Online supported Outlook client or later to access public folders in Microsoft 365, Office 365, and Exchange Online.

## Migrating public folders to Microsoft 365 or Office 365 and Exchange Online

When you migrate your public folders, you'll use a process called batch public folder migration. Batch public folder migration (or simply batch migration) creates a mailbox migration request for each public folder mailbox that will exist in Exchange Online. Using multiple requests means the migration will move along much faster because it's able to make more efficient use of available network bandwidth. It's also more reliable because it reduces the possibility of a single failure or bottleneck affecting the entire migration.

While batch migrations need to be started using the **New-MigrationBatch** cmdlet in Exchange Online PowerShell, the progress and completion of the migration can be viewed and managed in the EAC. Because the **New-MigrationBatch** cmdlet initiates a mailbox migration request for each public folder mailbox, you can view the status of these requests using the mailbox migration page. You can get to the mailbox migration page, and create migration reports that can be emailed to you, by opening the EAC in Exchange Online and navigating to **Mailbox** \> **Migration**.

To use batch migration to migrate your public folders to Exchange Online, your legacy Exchange server needs to meet the requirements in the following list. If it does, and you're ready to start, check out [Use batch migration to migrate legacy public folders to Microsoft 365 or Office 365 and Exchange Online](batch-migration-of-legacy-public-folders.md).

Exchange supports moving your public folders to Microsoft 365 or Office 365 and Exchange Online from the following legacy versions of Exchange Server:

- Exchange Server 2010 SP3 RU8 or later

See [Use batch migration to migrate Exchange Server public folders to Exchange Online](https://docs.microsoft.com/Exchange/collaboration/public-folders/migrate-to-exchange-online) to migrate your Exchange Server public folders.

We recommend that you use batch migration instead of Outlook's PST export feature to migrate public folders to Microsoft 365 or Office 365 and Exchange Online. Microsoft 365 and Office 365 public folder mailbox growth is managed using an auto-split feature that splits the public folder mailbox when it exceeds size quotas. Auto-split can't handle the sudden growth of public folder mailboxes when you use PST export to migrate your public folders and you might have to wait for up to two weeks for auto-split to move the data from the primary mailbox. We provide batch migration instructions in [Use batch migration to migrate legacy public folders to Microsoft 365 or Office 365 and Exchange Online](batch-migration-of-legacy-public-folders.md) and [Use batch migration to migrate Exchange Server public folders to Exchange Online](https://docs.microsoft.com/Exchange/collaboration/public-folders/migrate-to-exchange-online). However, if you've elected to do a PST migration and have run into an issue where the primary mailbox is full, you have two options for recovering the PST migration:

1. Wait for the auto-split to move the data from the primary mailbox. This may take up to two weeks. However, all the public folders in a completely filled public folder mailbox won't be able to receive new content until the auto-split completes.

2. [Create a public folder mailbox](create-public-folder-mailbox.md) and then use the **New-PublicFolder** cmdlet with the _Mailbox_ parameter to create the remaining public folders in the secondary public folder mailbox. This example creates a new public folder named PF201 in the secondary public folder mailbox.

   ```PowerShell
   New-PublicFolder -Name PF201 -Mailbox SecondaryPFMbx
   ```
