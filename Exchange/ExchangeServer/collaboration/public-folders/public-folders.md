---
localization_priority: Normal
description: Learn about public folders and how they work in Exchange 2016 and Exchange 2019.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9ca7a32d-2436-462f-b71a-94129d05b6fa
ms.reviewer:
title: Public folders
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Public folders

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization. Public folders help make content in a deep hierarchy easier to browse. Users will see the full hierarchy in Outlook, which makes it easy for them to find the content they're interested in.

Public folders are available in the following Outlook clients:

- Outlook on the web (formerly known as Outlook Web App) for Exchange 2016 or later

- Supported versions of Outlook for [Exchange Server](../../plan-and-deploy/system-requirements.md).

- Outlook for Mac 2016 and Outlook for Mac for Office 365.

Public folders can also be used as an archiving method for distribution groups. When you mail-enable a public folder and add it as a member of the distribution group, email sent to the group is automatically added to the public folder for later reference.

Public folders aren't designed to do the following:

- **Data archiving**: Users who have mailbox limits sometimes use public folders instead of mailboxes to archive data. This practice isn't recommended because it affects storage in public folders and undermines the goal of mailbox limits. Instead, we recommend that you use [In-Place Archiving in Exchange 2016](../../policy-and-compliance/in-place-archiving/in-place-archiving.md) as your archiving solution.

- **Document sharing and collaboration**: Public folders don't provide versioning or other document management features, such as controlled check-in and check-out functionality and automatic notifications of content changes. Instead, we recommend that you use [SharePoint](https://docs.microsoft.com/sharepoint/) as your documentation sharing solution.

To learn more about public folders and other collaboration methods in Exchange, see [Collaboration](../../collaboration/collaboration.md).

To browse some frequently asked questions about public folders in Exchange, see [FAQ: Public folders](faq.md).

For more information about the limits and quotas for public folders, see [Limits for public folders](limits.md).

## Public folder architecture
<a name="PFArchitecture"> </a>

Public folders use a mailbox infrastructure to take advantage of the existing high availability and storage technologies of the mailbox database. Public folder architecture uses specially designed mailboxes to store both the public folder hierarchy and the content. This also means that there's no longer a public folder database as there was in earlier version of Exchange. High availability for the public folder mailboxes is provided by a database availability group (DAG). To learn more about DAGs, see [Database availability groups](../../high-availability/database-availability-groups/database-availability-groups.md).

The main architectural components of public folders are the public folder mailboxes, which can reside in one or more mailbox databases.

### Public folder mailboxes
<a name="PFMailboxes"> </a>

There are two types of public folder mailboxes: the *primary hierarchy mailbox* and *secondary hierarchy mailboxes*. Both types of mailboxes can contain content:

- **Primary hierarchy mailbox**: The primary hierarchy mailbox is the one writable copy of the public folder hierarchy. The public folder hierarchy is copied to all other public folder mailboxes, but these will be read-only copies.

- **Secondary hierarchy mailboxes**: Secondary hierarchy mailboxes contain public folder content as well and a read-only copy of the public folder hierarchy.

> [!NOTE]
> Retention policies aren't supported for public folder mailboxes.

There are two ways you can manage public folder mailboxes:

- In the Exchange admin center (EAC), navigate to **Public folders** \> **Public folder mailboxes**.

- In the Exchange Management Shell, use the **\*-Mailbox** set of cmdlets. The following parameters have been added to the [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/new-mailbox) cmdlet to support public folder mailboxes:

  - _PublicFolder_: This parameter is used with the **New-Mailbox** cmdlet to create a public folder mailbox. When you create a public folder mailbox, a new mailbox is created with the mailbox type of `PublicFolder`. For more information, see [Create a public folder mailbox](create-public-folder-mailboxes.md).

  - _HoldForMigration_: This parameter is used only if you're migrating public folders from Exchange 2010 to Exchange 2016. For more information, see [Migrate public folders](public-folders.md#MigratePFs) later in this topic.

  - _IsHierarchyReady_: This parameter indicates whether the public folder mailbox is ready to serve the public folder hierarchy to users. It's set to `$True` only after the entire hierarchy has been synced to the public folder mailbox. If the parameter is set to $False, users won't use it to access the hierarchy. However, if you set the _DefaultPublicFolderMailbox_ property on a user mailbox to a specific public folder mailbox, the user will still access the specified public folder mailbox even if the _IsHierarchyReady_ parameter is set to `$False`.

  - _IsExcludedFromServingHierarchy_: This parameter prevents users from accessing the public folder hierarchy on the specified public folder mailbox. For load-balancing purposes, users are equally distributed across public folder mailboxes by default. When this parameter is set on a public folder mailbox, that mailbox isn't included in this automatic load balancing and won't be accessed by users to retrieve the public folder hierarchy. However, if you set the _DefaultPublicFolderMailbox_ property on a user mailbox to a specific public folder mailbox, the user will still access the specified public folder mailbox even if the _IsExcludedFromServingHierarchy_ parameter is set for that public folder mailbox.

A secondary hierarchy mailbox will serve only public folder hierarchy information to users if it's specified explicitly on the users' mailboxes using the _DefaultPublicFolderMailbox_ property, or if the following conditions are met:

- The _IsHierarchyReady_ property on the public folder mailbox is set to `$True`.

- The _IsExcludedFromServingHierarchy_ property on the public folder mailbox is set to `$False`.

### Public folder hierarchy
<a name="PFHierarchy"> </a>

The public folder hierarchy contains the folders' properties and organizational information, including tree structure. Each public folder mailbox contains a copy of the public folder hierarchy. There's only one writeable copy of the hierarchy, which is in the primary public folder mailbox. For a specific folder, the hierarchy information is used to identify the following:

- Permissions on the folder

- The folder's position in the public folder tree, including its parent and child folders

> [!NOTE]
> The hierarchy doesn't store information about email addresses for mail-enabled public folders. The email addresses are stored on the directory object in Active Directory.

#### Hierarchy synchronization

The public folder hierarchy synchronization process uses Incremental Change Synchronization (ICS), which provides a mechanism to monitor and synchronize changes to an Exchange store hierarchy or content. The changes include creating, modifying, and deleting folders and messages. When users are connected to and using content mailboxes, synchronization occurs every 15 minutes. If no users are connected to content mailbox, synchronization will be triggered less often (every 24 hours).If a write operation such as a creating a folder is performed on the primary hierarchy, synchronization is triggered immediately (synchronously) to the content mailbox.

> [!IMPORTANT]
> Because there's only one writeable copy of the hierarchy, folder creation is proxied to the hierarchy mailbox by the content mailbox users are connected to.

In a large organization, when you create a new public folder mailbox, the hierarchy must synchronize to that public folder before users can connect to it. Otherwise, users may see an incomplete public folder structure when connecting with Outlook. To allow time for this synchronization to occur without users attempting to connect to the new public folder mailbox, set the _IsExcludedFromServingHierarchy_ parameter on the **New-Mailbox** cmdlet when creating the public folder mailbox. This parameter prevents users from connecting to the newly created public folder mailbox. When synchronization is complete, run the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet with the _IsExcludedFromServingHierarchy_ parameter set to `false`, indicating that the public folder mailbox is ready to be connected to. You can use also the [Get-PublicFolderMailboxDiagnostics](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxdiagnostics) cmdlet to view the sync status by the _SyncInfo_ and the _AssistantInfo_ properties.

For more information, see [Create a public folder](create-public-folders.md).

### Public folder content
<a name="PFHierarchy"> </a>

Public folder content can include email messages, posts, documents, and eForms. The content is stored in the public folder mailbox but isn't replicated across multiple public folders mailboxes. All users access the same public folder mailbox for the same set of content. Although a full text search of public folder content is available, public folder content isn't searchable across public folders and the content isn't indexed by Exchange Search.

> [!NOTE]
> Outlook on the web is supported, but with limitations. You can add and remove public folders to your Favorites through Outlook, and then perform item-level operations such as creating, editing, deleting posts, and replying to posts through Outlook on the web. However, you can't create or delete public folders from Outlook on the web. Also, only Mail, Post, Calendar, and Contact public folders can be added to the Favorites list in Outlook on the web.

## Migrate public folders
<a name="MigratePFs"> </a>

- You can migrate public folders in the following scenarios:

- From Exchange 2010 to Exchange 2016 or to Exchange Online.

- From Exchange 2016 or later to Exchange Online.

If you already have Exchange 2010 SP3 public folders in your organization prior to installing Exchange 2016, you must migrate those public folders to Exchange 2016. To do this, use the **PublicFolderMigrationRequst** cmdlets. For more information, see [Use batch migration to migrate Exchange 2010 public folders to Exchange 2016](batch-migration-from-previous-versions.md). If your organization is moving to Exchange Online, you can migrate your public folders to the cloud and upgrade them at the same time. For details, see [Use batch migration to migrate legacy public folders to Office 365 and Exchange Online](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/batch-migration-of-legacy-public-folders) and [Use batch migration to migrate Exchange Server public folders to Exchange Online](migrate-to-exchange-online.md).

Due to the changes in how public folders are stored, Exchange 2010 mailboxes are unable to access the public folder hierarchy on Exchange 2016 or on Exchange Online. However, user mailboxes on Exchange 2016 can connect to Exchange 2010 public folders. Exchange 2016 public folders and legacy public folders can't exist in your Exchange organization simultaneously. This effectively means that there's no coexistence between versions. Migrating public folders to Exchange Server 2016 or Exchange Online is currently a one-time cutover process.

For this reason, we recommend that prior to migrating your Exchange 2010 public folders, you should first migrate your Exchange 2010 mailboxes to Exchange 2016 or Exchange Online. For more information about migrating mailboxes, see [Mailbox moves in Exchange Server](../../recipients/mailbox-moves.md), [Perform a cutover migration of email to Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/cutover-migration-to-office-365), and [Perform a staged migration of email to Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/perform-a-staged-migration/perform-a-staged-migration).

## Public folder moves
<a name="Moves"> </a>

You can move public folders to a different public folder mailbox, and you can move public folder mailboxes to different mailbox databases. To move public folders to different public folder mailboxes, use the **PublicFolderMoveRequest** set of cmdlets. Subfolders under the public folder that's being moved won't be moved by default. If you want to move a branch of public folders, you can use the `Move-PublicFolderBranch.ps1` script that's installed by default with Exchange. For more information, see [Move a Public Folder to a different Public Folder Mailbox](https://docs.microsoft.com/exchange/move-a-public-folder-to-a-different-public-folder-mailbox-exchange-2013-help).

In addition to moving public folders, you can move public folder mailboxes to different mailbox databases by using the **MoveRequest** set of cmdlets. This is the same set of cmdlets that are used for moving regular mailboxes. For more information, see [Move a public folder mailbox to a different mailbox database](https://docs.microsoft.com/exchange/move-a-public-folder-mailbox-to-a-different-mailbox-database-exchange-2013-help).

 **PublicFolderMoveRequest** cmdlets and the **MoveRequest** cmdlets use the Mailbox Replication Service to move public folders asynchronously. That means that the cmdlet doesn't do the actual work and, during most of the move, the public folder and public folder mailboxes will still be available to users. Because the Mailbox Replication Service performs mailbox moves, import and export requests, and public folder move requests, it's important to consider throttling and workload management.

## Public folder quotas
<a name="Quotas"> </a>

By default, new public folder mailboxes automatically inherit the size limits of the mailbox database. As a result, to accurately evaluate the current storage quota status for the public folder mailbox using the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) cmdlet, you first need to review the value of the _UseDatabaseQuotaDefaults_ property:

- If the value is `True`, the per-mailbox settings are ignored and the mailbox database limits are used.

- If the value is `False`, the per-mailbox settings are used.

If the _UseDatabaseQuotaDefaults_ property is `True` and the _ProhibitSendQuota_, _ProhibitSendReceiveQuota_, and _IssueWarningQuota_ properties are `unlimited`, the mailbox size isn't really unlimited. Instead, you need to use the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase) cmdlet and review the corresponding mailbox database storage limits to find out what the limits for the mailbox are. The default mailbox database quota limits are:

- _IssueWarningQuota_: 1.9 GB

- _ProhibitSendQuota_: 2 GB

- _ProhibitSendReceiveQuota_: 2.3 GB

To find the mailbox database quotas, run the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase) cmdlet.

To set the quotas on a public folder mailbox, use the [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/set-organizationconfig) cmdlet with the _DefaultPublicFolderIssueWarningQuota_ and _DefaultPublicFolderProhibitPostQuota_ parameters.

## Disaster recovery
<a name="DR"> </a>

Public folders are built on mailbox infrastructure and use the same mechanisms for availability and redundancy. Every public folder mailbox can have multiple redundant copies with automatic failover, just like regular mailboxes. To learn more, see [Plan for high availability and site resilience](../../high-availability/plan-ha.md).

In addition to the overall disaster recovery scenario, you can also restore public folders in the following situations:

- **Soft-deleted public folder restore**: The public folder was deleted but is still within the retention period.

- **Soft-deleted public folder mailbox restore**: The public folder mailbox was deleted and is still within the mailbox retention period.

- **Public folder mailbox restore from a recovery database**: You can recover an individual public folder mailbox from backup when the deleted mailbox retention period has elapsed. You then extract data from the restored mailbox and copy it to a target folder or merge it with another mailbox.

In all of these situations, the public folder or public folder mailbox is recoverable by using the **MailboxRestoreRequest** cmdlets.

For more information, see [Restore public folders and public folder mailboxes from failed moves](https://docs.microsoft.com/exchange/restore-public-folders-and-public-folder-mailboxes-from-failed-moves-exchange-2013-help).
