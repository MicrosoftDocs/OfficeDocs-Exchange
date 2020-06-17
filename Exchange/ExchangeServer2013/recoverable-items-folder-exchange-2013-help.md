---
title: 'Recoverable Items folder: Exchange 2013 Help'
TOCTitle: Recoverable Items folder
ms:assetid: efc48fb4-2ed8-4d05-93af-f3505fbc389d
ms:mtpsurl: https://technet.microsoft.com/library/Ee364755(v=EXCHG.150)
ms:contentKeyID: 49289452
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Recoverable Items folder

_**Applies to:** Exchange Server 2013_

To protect from accidental or malicious deletion and to facilitate discovery efforts commonly undertaken before or during litigation or investigations, Microsoft Exchange Server 2013 and Exchange Online use the Recoverable Items folder. The Recoverable Items folder replaces the feature that was known as *the dumpster* in earlier versions of Exchange. The Recoverable Items folder is used by the following Exchange features:

- Deleted item retention

- Single item recovery

- In-Place Hold

- Litigation Hold

- Mailbox audit logging

- Calendar logging

## Terminology

Knowledge of the following terms will help you understand the content in this topic.

- **Delete**:

  Describes when an item is deleted from any folder and placed in the Deleted Items default folder.

- **Permanently delete**:

  Describes when an item is deleted from the Deleted Items default folder and placed in the Recoverable Items folder. Additionally describes when a Microsoft Outlook user deletes an item by pressing Shift+Delete, which bypasses the Deleted Items folder and places the item directly in the Recoverable Items folder.

- **Purged**:

  Describes when an item is marked to be purged from the mailbox database. This is also known as a *store hard delete*.

## Recoverable Items folder

The Recoverable Items folder resides in the non-IPM subtree of each mailbox. The non-IPM subtree is a storage area within the mailbox that contains operational data about the mailbox. This subtree isn't visible to users using Outlook, Microsoft Office Outlook Web App, or other email clients.

This architectural change provides the following key benefits:

- When a mailbox is moved to another mailbox database, the Recoverable Items folder moves with it.

- The Recoverable Items folder is indexed by Exchange Search and can be discovered using In-Place eDiscovery.

- The Recoverable Items folder has its own storage quota.

- Exchange can prevent data from being purged from the Recoverable Items folder.

- Exchange can track edits of certain content.

The Recoverable Items folder contains the following subfolders:

- **Deletions**: This subfolder contains all items deleted from the Deleted Items folder. (In Outlook, a user can permanently delete an item by pressing Shift+Delete.) This subfolder is exposed to users through the Recover Deleted Items feature in Outlook and Outlook Web App.

- **Versions**: If In-Place Hold or Litigation Hold is enabled, this subfolder contains the original and modified copies of the deleted items. This folder isn't visible to end users.

- **Purges**: If either Litigation Hold or single item recovery is enabled, this subfolder contains all items that are purged. This folder isn't visible to end users.

- **Audits**: If mailbox audit logging is enabled for a mailbox, this subfolder contains the audit log entries. To learn more about mailbox audit logging, see [Mailbox audit logging](mailbox-audit-logging-exchange-2013-help.md).

- **DiscoveryHolds**: If In-Place Hold is enabled, this subfolder contains all items that meet the hold query parameters and are purged.

- **Calendar Logging**: This subfolder contains calendar changes that occur within a mailbox. This folder isn't available to users.

The following illustration shows the subfolders in the Recoverable Items folders. It also shows the deleted item retention, single item recovery, and hold workflow processes that are described in the following sections.

![Recoverable Items folder](images/Ee364755.a1a08afc-3617-4adb-83ab-a6904516954e(EXCHG.150).gif "Recoverable Items folder")

## Deleted item retention

An item is considered to be permanently deleted in the following cases:

- A user deletes an item or empties all items from the Deleted Items folder.

- A user presses Shift+Delete to delete an item from any other mailbox folder.

Permanently deleted items are moved to the Deletions subfolder of the Recoverable Items folder. This provides an additional layer of protection so users can recover permanently deleted items without requiring Help desk intervention. Users can use the Recover Deleted Items feature in Outlook or Outlook Web App to recover a deleted item. Users can also use this feature to permanently delete an item. For more information, see:

- [Restore deleted items in Outlook](https://go.microsoft.com/fwlink/p/?linkid=524923)

- [Recover deleted items or email in Outlook Web App](https://go.microsoft.com/fwlink/p/?linkid=524924)

Items remain in the Deletions subfolder until the deleted item retention period is reached. The default deleted item retention period for a mailbox database is 14 days. You can modify this period for a mailbox database or for a specific mailbox. In addition to a deleted item retention period, the Recoverable Items folder is also subject to quotas. To learn more, see Recoverable Items Mailbox Quotas later in this topic.

After the deleted item retention period elapses, the item is moved to the Purges folder and is no longer visible to the user. When the Managed Folder Assistant processes the mailbox, items in the Purges subfolder are purged from the mailbox database and can't be recovered.

## Single item recovery

If an item is removed from the Deletions subfolder, either by a user purging the item by using the Recover Deleted Items feature or by an automated process such as the Managed Folder Assistant, the item can't be recovered by the user. In previous versions of Exchange, recovering these items required the administrator to restore the mailbox database or a mailbox from backup copies. This process generally delayed recovery by minutes or hours, depending on the backup mechanism used.

In Exchange 2013, you can use *single item recovery* to recover items without having to restore the mailbox databases using backup media. This results in considerably shorter recovery periods. When the Managed Folder Assistant processes the Recoverable Items folder for a mailbox that has single item recovery enabled, any item in the Purges subfolder isn't purged if the deleted item retention period hasn't expired for that item.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if single item recovery is enabled.

### Recoverable Items folder and single item recovery

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>State of single item recovery</th>
<th>Recoverable Items folder contains permanently deleted items</th>
<th>Recoverable Items folder contains purged items</th>
<th>Users can purge items from the Recoverable Items folder</th>
<th>Managed Folder Assistant automatically purges items from the Recoverable Items folder</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Enabled</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes. Users can purge items using the Recover Deleted Items feature in Outlook or Outlook Web App. But purged items won't be permanently removed from the mailbox database until the deleted item retention period expires.</p></td>
<td><p>Yes. By default, all items are purged after 14 days, with the exception of calendar items, which are purged after 31 days.</p></td>
</tr>
<tr class="even">
<td><p>Disabled</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Yes. In this case, purged items are marked to be permanently removed from the mailbox database and can't be recovered.</p></td>
<td><p>Yes. By default, all items are purged after 14 days, with the exception of calendar items, which are purged after 31 days. If the Recoverable Items warning quota is reached before the deleted item retention period elapses, messages are deleted in first in, first out (FIFO) order.</p></td>
</tr>
</tbody>
</table>

In Exchange 2013, single item recovery isn't enabled by default for new mailboxes or mailboxes moved from a previous version of Exchange. You must use the Exchange Management Shell to enable single item recovery for a mailbox, and then configure or modify the deleted item retention period. For details about how to perform a single item recovery, see [Recover deleted messages in a user's mailbox](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-user-mailboxes/recover-deleted-messages).

## In-Place Hold and Litigation Hold

In Exchange 2013 and Exchange Online, discovery managers can use In-Place eDiscovery with delegated [Discovery Management](discovery-management-exchange-2013-help.md) permissions to perform eDiscovery searches of mailbox content. Exchange 2013 and Exchange Online also introduced In-Place Hold, which you can use to preserve mailbox items that match query parameters and protect the items from deletion by users or automated processes. You can also use Litigation Hold to preserve all items in user mailboxes and protect the items from deletion by users or automated processes.

Placing a mailbox on In-Place Hold or Litigation Hold stops the Managed Folder Assistant from automatically purging messages from the DiscoveryHolds and Purges subfolders. Additionally, copy-on-write page protection is also enabled for the mailbox. Copy-on-write page protection creates a copy of the original item before any modifications are written to the Exchange store. After mailbox is removed from hold, the Managed Folder Assistant resumes automated purging.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if Litigation Hold is enabled.

### Recoverable Items folder and holds

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>State of hold</th>
<th>Recoverable Items folder contains permanently deleted items</th>
<th>Recoverable Items folder contains modified and purged items</th>
<th>Users can purge items from the Recoverable Items folder</th>
<th>Managed Folder Assistant automatically purges items from the Recoverable Items folder</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Enabled</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
<td><p>Yes. Users can purge items using the Recover Deleted Items feature in Outlook or Outlook Web App. But the Managed Folder Assistant doesn't permanently remove them from the mailbox database if the mailbox is on hold.</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Disabled</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

To learn more about In-Place eDiscovery, In-Place Hold, and Litigation Hold, see the following topics:

- [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery)

- [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds)

## Copy-on-write page protection and modified items

If a user who is placed on In-Place Hold or Litigation Hold modifies specific properties of a mailbox item, a copy of the original mailbox item is created before the changed item is written. The original copy is saved in the Versions subfolder. This process is known as *copy-on-write page protection*. Copy-on-write page protection applies to items residing in any mailbox folder. The Versions subfolder isn't visible to users.

The following table lists the message properties that trigger copy-on-write page protection.

### Properties that trigger copy-on-write page protection

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Item type</th>
<th>Properties that trigger copy-on-write page protection</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Messages (IPM.Note*)</p>
<p>Posts (IPM.Post*)</p></td>
<td><ul>
<li><p>Subject</p></li>
<li><p>Body</p></li>
<li><p>Attachments</p></li>
<li><p>Senders and recipients</p></li>
<li><p>Sent and received dates</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Items other than messages and posts</p></td>
<td><p>Any change to a visible property, except the following:</p>
<ul>
<li><p>Item location (when an item is moved between folders)</p></li>
<li><p>Item status change (read or unread)</p></li>
<li><p>Changes to a retention tag applied to an item</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Items in the Drafts default folder</p></td>
<td><p>None. Items in the Drafts folder are exempt from copy-on-write page protection.</p></td>
</tr>
</tbody>
</table>

> [!IMPORTANT]
> Copy-on-write page protection doesn't save a version of the meeting when a meeting organizer receives responses from attendees and the meeting's tracking information is updated. Also, changes to RSS feeds aren't captured by copy-on-write page protection.

When a mailbox is no longer on In-Place Hold or litigation hold, copies of modified items stored in the Versions folder are removed.

## Recoverable Items mailbox quotas

When an item is moved to the Recoverable Items folder, its size is deducted from the mailbox quota and added to the size of the Recoverable Items folder. In Exchange 2013, mailbox databases have a configurable Recoverable Items warning quota (*soft limit*) of 20 GB and a Recoverable Items quota (*hard limit*) of 30 GB. By default, these limits are inherited by all mailboxes in the database. However, you can configure individual mailboxes with different quotas. To learn more, see [Configure Deleted Item retention and Recoverable Items quotas](configure-deleted-item-retention-and-recoverable-items-quotas-exchange-2013-help.md).

In Exchange Online, the default limits for the Recoverable Items quota are the same as Exchange 2013; a soft limit of 20 GB and a hard limit of 30 GB. However, the quotas for the Recoverable Items folder are automatically increased to 90 GB and 100 GB, respectively, when you place a mailbox on Litigation Hold or In-Place Hold.

When the Recoverable Items folder for a mailbox reaches the Recoverable Items quota, no more items can be stored in the folder. This impacts mailbox functionality in the following ways:

- Mailbox users can't delete items.

- The Managed Folder Assistant can't delete items based on retention tag or managed folder settings.

- For mailboxes that have single item recovery, In-Place Hold or Litigation Hold enabled, the copy-on-write page protection process can't maintain versions of items edited by the user.

- For mailboxes that have mailbox audit logging enabled, no mailbox audit log entries can be saved in the Audits subfolder.

For mailboxes that aren't placed on In-Place Hold or Litigation Hold, the Managed Folder Assistant automatically purges items from the Recoverable Items folder when the deleted item retention period expires. If the folder reaches the Recoverable Items warning quota, the assistant automatically purges items in FIFO order.

When the Recoverable Items folder reaches the soft and hard limit defaults, you are notified by means of an event log and a Microsoft System Center Operations Manager alert. This alert fires when the Recoverable Items folder first reaches the soft and hard limit defaults, and then once daily afterward.

The following table lists the events logged when the Recoverable Items folder reaches the soft and hard limit defaults.

### Recoverable Items quota warnings and errors

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Event ID</th>
<th>Type</th>
<th>Source</th>
<th>Message</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>10024</p></td>
<td><p>Warning</p></td>
<td><p>MSExchangeIS Mailbox Store</p></td>
<td><p>The mailbox for &lt;<em>mailbox user</em>&gt; (<em>GUID</em>) has exceeded the Recoverable Items Warning Quota. Please remove items from Recoverable Items or increase the Recoverable Items Warning Quota and Recoverable Items Quota. If the Recoverable Items Quota is exceeded, the user will be unable to delete items from the mailbox.</p></td>
</tr>
<tr class="even">
<td><p>10023</p></td>
<td><p>Error</p></td>
<td><p>MSExchangeIS Mailbox Store</p></td>
<td><p>The mailbox for &lt;<em>mailbox user</em>&gt; (<em>GUID</em>) has exceeded the maximum Recoverable Items Quota. Items cannot be deleted from this mailbox. The mailbox owner should be notified about the condition of the mailbox as soon as possible. Please remove items from Recoverable Items or increase the Recoverable Items Quota to restore functionality.</p></td>
</tr>
<tr class="odd">
<td><p>10023</p></td>
<td><p>Warning</p></td>
<td><p>MSExchangeMailboxAssistants</p></td>
<td><p>The mailbox:&lt;<em>mailbox user</em>&gt; Recoverable Items size has exceeded the warning quota limit. Items were deleted from Recoverable Items folders to prevent mailbox outage. Recoverable Items Warning Quota: 20 GB (21,474,836,480 bytes) Original Recoverable Items size: 21475005311 Current Recoverable Items size: 21474823820 Folder stats: - Folders processed: RecoverableItemsRoot, RecoverableItemsVersions, RecoverableItemsPurges, RecoverableItemsDeletions - Original folder sizes: 21391661934, 55190914, 1987247, 26157788 (item counts: 276828, 400, 84, 646) - Current folder sizes: 21391480443, 55190914, 1987247, 26157788 (item counts: 276817, 400, 84, 646)</p></td>
</tr>
</tbody>
</table>

If the mailbox is placed on In-Place Hold or Litigation Hold, copy-on-write page protection can't maintain versions of modified items. To maintain versions of modified items, you must reduce the size of the Recoverable Items folder. You can use the [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Search-Mailbox) cmdlet to copy messages from the Recoverable Items folder of a mailbox to a discovery mailbox, and then delete the items from the mailbox. Alternatively, you can also raise the Recoverable Items quota for the mailbox. For details, see [Clean up the Recoverable Items folder](clean-up-the-recoverable-items-folder-exchange-2013-help.md).

## More information

- Copy-on-write is only enabled when a mailbox is on In-Place Hold or Litigation Hold.

- If users need to recover deleted items from the Recoverable Items folder, point them to the following topics:

  - [Restore deleted items in Outlook](https://go.microsoft.com/fwlink/p/?linkid=524923)

  - [Recover deleted items or email in Outlook Web App](https://go.microsoft.com/fwlink/p/?linkid=524924)
