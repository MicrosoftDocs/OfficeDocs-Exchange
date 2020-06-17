---
title: 'Limits for public folders: Exchange 2013 Help'
TOCTitle: Limits for public folders
ms:assetid: 709b075e-9584-484b-bcaa-e781c26497b4
ms:mtpsurl: https://technet.microsoft.com/library/Dn594582(v=EXCHG.150)
ms:contentKeyID: 61218734
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Limits for public folders

_**Applies to:** Exchange Server 2013_

In Exchange Server 2013, we moved public folders from a traditional database architecture to a mailbox architecture. This shift allows public folders to benefit from things such as the resiliency of a Database Availability Group (DAG) and other mailbox enhancements made over the years. However, there are new limits and performance concerns that should be taken into account. In this document we provide some high level guidance for configuration options you have that could affect public folder performance and connectivity.

## Limits

The following table lists the limits for public folders in on-premises Exchange Server 2013. Unless the limits are specifically stated as recommended, the values listed in this table are the supported limits for public folders.

> [!IMPORTANT]
> Looking for Exchange Online limits for Office 365? See <A href="https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits">Exchange Online Limits</A>.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Item</th>
<th>Limits</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Total number of public folder mailboxes</p></td>
<td><p>100</p></td>
<td><p>Although you can create more than 100 public folder mailboxes, it isn't supported. <a href="https://docs.microsoft.com/exchange/collaboration-exo/public-folders/create-public-folder-mailbox">Create a public folder mailbox</a></p></td>
</tr>
<tr class="even">
<td><p>Total public folders in hierarchy</p></td>
<td><p>1,000,000</p></td>
<td><p>Although you can create more than 1,000,000 public folders, it isn't supported. For any deployment of 100,000 or more public folders, we recommend reading <a href="considerations-when-deploying-public-folders-exchange-2013-help.md">Considerations when deploying public folders</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Sub-folders under the parent folder</p></td>
<td><p>10,000</p></td>
<td><p>While you can create more than 1,000 sub-folders under a parent folder, we don't recommend that you do so.</p>
<p><em>FolderHierarchyChildrenCountReceiveQuota</em> parameter on the <a href="https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox">Set-Mailbox</a> cmdlet.</p></td>
</tr>
<tr class="even">
<td><p>Folder depth</p></td>
<td><p>300</p></td>
<td><p>The folder depth is the number levels of nested folders that can exist in one branch of a public folder tree. <em>FolderHierarchyDepthRecieveQuota</em> parameter on the <a href="https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox">Set-Mailbox</a> cmdlet.</p></td>
</tr>
<tr class="odd">
<td><p>Maximum messages per public folder</p></td>
<td><p>1 million</p></td>
<td><p><em>MailboxMessagesPerFolderCountRecieveQuota</em> parameter on the <a href="https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox)">Set-Mailbox</a> cmdlet.</p></td>
</tr>
<tr class="even">
<td><p>Maximum individual public folder size</p></td>
<td><p>10 GB</p></td>
<td><p>This limit doesn't include subfolders beneath a single folder.</p>
<p><a href="configure-storage-quotas-for-a-mailbox-exchange-2013-help.md">Configure storage quotas for a mailbox</a></p></td>
</tr>
<tr class="odd">
<td><p>Public folder mailbox size</p></td>
<td><p>100 GB</p></td>
<td><p><a href="configure-storage-quotas-for-a-mailbox-exchange-2013-help.md">Configure storage quotas for a mailbox</a></p></td>
</tr>
<tr class="even">
<td><p>Number of user logons per public folder mailbox</p></td>
<td><p>2,000 concurrent user logons</p></td>
<td><p>We recommend that you configure your hierarchy so that you have no more than 2,000 users per public folder mailbox. For example, if you have 20,000 users, you should have 10 public folder mailboxes.</p></td>
</tr>
<tr class="odd">
<td><p>Moved item retention</p></td>
<td><p>14 days recommended</p></td>
<td><p>Use the <em>DefaultPublicFolderMovedItemRetention</em> parameter on the <strong>Set-OrganizationConfig</strong> cmdlet.</p></td>
</tr>
<tr class="even">
<td><p>Age limit</p></td>
<td><p>We recommend that you set this as the same default that you use for regular mailboxes.</p></td>
<td><p>These settings can be set at the following levels:</p>
<ul>
<li><p><strong>Organizational level:</strong> <em>DefaultPublicFolderAgeLimit</em> parameter on the <strong>Set-OrganizationConfig</strong> cmdlet.</p></li>
<li><p><strong>Mailbox level:</strong> <em>AgeLimit</em> parameter on the <strong>Set-Mailbox</strong> cmdlet.</p></li>
<li><p><strong>Folder level:</strong> <em>AgeLimit</em> parameter on the <strong>Set-PublicFolder</strong> cmdlet.</p></li>
</ul>
<p></p></td>
</tr>
<tr class="odd">
<td><p>Deleted item retention</p></td>
<td><p>We recommend that you set this as the same default that you use for regular mailboxes.</p></td>
<td><p>These settings can be set at the following levels:</p>
<ul>
<li><p><strong>Organizational level:</strong> <em>DefaultPublicFolderMovedItemRetention</em> parameter on the <strong>Set-OrganizationConfig</strong> cmdlet.</p></li>
<li><p><strong>Mailbox level:</strong> <em>RetainDeletedItemsFor</em> on the <strong>Set-Mailbox</strong> cmdlet.</p></li>
<li><p><strong>Folder level:</strong> <em>RetainDeleteItemsFor</em> parameter on the <strong>Set-PublicFolder</strong> cmdlet.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Maximum number of public folders that can be migrated to Exchange 2013</p></td>
<td><p>500,000</p></td>
<td><p>This is the maximum number of public folders you can move to Exchange 2013 from a legacy version of Exchange in a single migration. For details on migrating public folders, see <a href="use-batch-migration-to-migrate-public-folders-to-exchange-2013-from-previous-versions-exchange-2013-help.md">Use batch migration to migrate public folders to Exchange 2013 from previous versions</a></p></td>
</tr>
</tbody>
</table>
