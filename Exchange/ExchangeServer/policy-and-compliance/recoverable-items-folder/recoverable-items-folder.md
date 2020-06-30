---
localization_priority: Normal
description: 'Summary: Learn about protecting user account data in Exchange Server 2016 and Exchange Server 2019 by using the Recoverable Items folder.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: efc48fb4-2ed8-4d05-93af-f3505fbc389d
ms.reviewer:
title: Recoverable Items folder in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recoverable Items folder in Exchange Server

To protect from accidental or malicious deletion and to facilitate discovery efforts commonly undertaken before or during litigation or investigations, Exchange Server and Exchange Online use the Recoverable Items folder. The Recoverable Items folder replaces the feature that was known as *the dumpster* in earlier versions of Exchange. The following Exchange features use the Recoverable Items folder:

- Deleted item retention

- Single item recovery

- In-Place Hold

- Litigation Hold

- Mailbox audit logging

- Calendar logging

## Terminology
<a name="Term"> </a>

Knowledge of the following terms will help you understand the content in this topic.

 **Delete**

> Describes when an item is deleted from any folder and placed in the Deleted Items default folder.

 **Soft delete**

> Describes when an item is deleted from the Deleted Items default folder and placed in the Recoverable Items folder. Also describes when an Outlook user deletes an item by pressing Shift+Delete, which bypasses the Deleted Items folder and places the item directly in the Recoverable Items folder.

 **Hard delete**

> Describes when an item is marked to be purged from the mailbox database. This is also known as a *store hard delete*.

## Recoverable Items folder
<a name="RIF"> </a>

Each user mailbox is divided into two subtrees: the IPM (interpersonal messaging) subtree, which contains the normal, visible folders such as Inbox, Calendar, and Sent Items and the non-IPM subtree, which contains internal data, preferences, and other operational data about the mailbox. The Recoverable Items folder resides in the non-IPM subtree of each mailbox. This subtree isn't visible to users using Outlook, Outlook on the web, or other email clients.

This architectural change provides the following key benefits:

- When a mailbox is moved to another mailbox database, the Recoverable Items folder moves with it.

- The Recoverable Items folder is indexed by Exchange Search and can be discovered by using In-Place eDiscovery.

- The Recoverable Items folder has its own storage quota.

- Exchange can prevent data from being purged from the Recoverable Items folder.

- Exchange can track edits of certain content.

The Recoverable Items folder contains the following subfolders:

- **Deletions**: This subfolder contains all items deleted from the Deleted Items folder. (In Outlook, a user can soft delete an item by pressing Shift+Delete.) This subfolder is exposed to users through the Recover Deleted Items feature in Outlook and Outlook on the web.

- **Versions**: If In-Place Hold or Litigation Hold is enabled, this subfolder contains the original and modified copies of the deleted items. This folder isn't visible to end users.

- **Purges**: If either Litigation Hold or single item recovery is enabled, this subfolder contains all items that are hard deleted. This folder isn't visible to end users.

- **Audits**: If mailbox audit logging is enabled for a mailbox, this subfolder contains the audit log entries. To learn more about mailbox audit logging, see [Mailbox audit logging in Exchange Server](../../policy-and-compliance/mailbox-audit-logging/mailbox-audit-logging.md).

- **DiscoveryHolds**: If In-Place Hold is enabled, this subfolder contains all items that meet the hold query parameters and are hard deleted.

- **Calendar Logging**: This subfolder contains calendar changes that occur within a mailbox. This folder isn't available to users.

The following illustration shows the subfolders in the Recoverable Items folders. It also shows the deleted item retention, single item recovery, and hold workflow processes that are described in the following sections.

![Recoverable Items folder](../../media/ITPro_RecoverableItems.gif)

### Deleted item retention
<a name="retention"> </a>

An item is considered to be soft deleted in the following cases:

- A user deletes an item or empties all items from the Deleted Items folder.

- A user presses Shift+Delete to delete an item from any other mailbox folder.

Soft-deleted items are moved to the Deletions subfolder of the Recoverable Items folder. This provides an additional layer of protection so users can recover deleted items without requiring Help desk intervention. Users can use the Recover Deleted Items feature in Outlook or Outlook on the web to recover a deleted item. Users can also use this feature to permanently delete an item. For more information, see:

- [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

- [Recover deleted items or email messages in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)

Items remain in the Deletions subfolder until the deleted item retention period is reached. The default deleted item retention period for a mailbox database is 14 days. You can modify this period for a mailbox database or for a specific mailbox. In addition to a deleted item retention period, the Recoverable Items folder is also subject to quotas. To learn more, see [Recoverable Items mailbox quotas](#recoverable-items-mailbox-quotas) later in this topic.

After the deleted item retention period expires, the item is moved to the Purges folder and is no longer visible to the user. When the Managed Folder Assistant processes the mailbox, items in the Purges subfolder are purged from the mailbox database.

### Single item recovery
<a name="SIR"> </a>

If an item is removed from the Deletions subfolder, either by a user purging the item by using the Recover Deleted Items feature or by an automated process such as the Managed Folder Assistant, the item can't be recovered by the user. In previous versions of Exchange, recovering these items required the administrator to restore the mailbox database or a mailbox from backup copies. This process generally delayed recovery by minutes or hours, depending on the backup mechanism used.

In Exchange Server, you can use *single item recovery* to recover items without using backup media to restore the mailbox databases. This results in considerably shorter recovery periods. When the Managed Folder Assistant processes the Recoverable Items folder for a mailbox that has single item recovery enabled, any item in the Purges subfolder isn't purged if the deleted item retention period hasn't expired for that item.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if single item recovery is enabled.

**Recoverable Items folder and single item recovery**

|**State of single item recovery**|**Recoverable Items folder contains soft-deleted items**|**Recoverable Items folder contains hard-deleted items**|**Users can purge items from the Recoverable Items folder**|**Managed Folder Assistant automatically purges items from the Recoverable Items folder**|
|:-----|:-----|:-----|:-----|:-----|
|Enabled|Yes|Yes|No|Yes. By default, all items are purged after 14 days, with the exception of calendar items, which are purged after 120 days.|
|Disabled|Yes|No|Yes|Yes. By default, all items are purged after 14 days, with the exception of calendar items, which are purged after 120 days. If the Recoverable Items warning quota is reached before the deleted item retention period elapses, messages are deleted in first in, first out (FIFO) order.|

In Exchange Server, single item recovery isn't enabled by default for new mailboxes or mailboxes moved from a previous version of Exchange. You need to use the Exchange Management Shell to enable single item recovery for a mailbox, and then configure or modify the deleted item retention period. For details about how to perform a single item recovery, see [Recover deleted messages in a user's mailbox](../../recipients/user-mailboxes/recover-deleted-messages.md).

### In-Place Hold and Litigation Hold
<a name="hold"> </a>

In Exchange Server and Exchange Online, discovery managers can use In-Place eDiscovery with delegated [Discovery Management](https://docs.microsoft.com/exchange/discovery-management-exchange-2013-help) role group permissions to perform eDiscovery searches of mailbox content. In Exchange Server and Exchange Online, you can use In-Place Hold to preserve mailbox items that match query parameters and protect the items from deletion by users or automated processes. You can also use Litigation Hold to preserve all items in user mailboxes and protect the items from deletion by users or automated processes.

Putting a mailbox on In-Place Hold or Litigation Hold stops the Managed Folder Assistant from automatically purging messages from the DiscoveryHolds and Purges subfolders. Additionally, copy-on-write page protection is also enabled for the mailbox. Copy-on-write page protection creates a copy of the original item before any modifications are written to the Exchange store. After the mailbox is removed from hold, the Managed Folder Assistant resumes automated purging.

> [!NOTE]
> If you put a mailbox on both In-Place Hold and Litigation Hold, Litigation Hold takes preference because this puts the entire mailbox on hold.

The following table lists the contents of and actions that can be performed in the Recoverable Items folder if Litigation Hold is enabled.

**Recoverable Items folder and holds**

|**State of hold**|**Recoverable Items folder contains soft-deleted items**|**Recoverable Items folder contains modified and hard-deleted items**|**Users can purge items from the Recoverable Items folder**|**Managed Folder Assistant automatically purges items from the Recoverable Items folder**|
|:-----|:-----|:-----|:-----|:-----|
|Enabled|Yes|Yes|No|No|
|Disabled|Yes|No|Yes|Yes|

To learn more about In-Place eDiscovery, In-Place Hold, and Litigation Hold, see the following topics:

- [In-Place eDiscovery in Exchange Server](../../policy-and-compliance/ediscovery/ediscovery.md)

- [In-Place Hold and Litigation Hold in Exchange Server](../../policy-and-compliance/holds/holds.md)

### Copy-on-write page protection and modified items
<a name="COW"> </a>

If a user who is placed on In-Place Hold or Litigation Hold modifies specific properties of a mailbox item, a copy of the original mailbox item is created before the changed item is written. The original copy is saved in the Versions subfolder. This process is known as *copy-on-write page protection*. Copy-on-write page protection applies to items residing in any mailbox folder. The Versions subfolder isn't visible to users.

The following table lists the message properties that trigger copy-on-write page protection.

**Properties that trigger copy-on-write page protection**

|**Item type**|**Properties that trigger copy-on-write page protection**|
|:-----|:-----|
|Messages (IPM.Note\*)  <br/> Posts (IPM.Post\*)|Subject  <br/> Body  <br/> Attachments  <br/> Senders and recipients  <br/> Sent and received dates|
|Items other than messages and posts|Any change to a visible property, except the following:  <br/> • Item location (when an item is moved between folders)  <br/> • Item status change (read or unread)  <br/> • Changes to a retention tag applied to an item|
|Items in the Drafts default folder|None. Items in the Drafts folder are exempt from copy-on-write page protection.|

> [!IMPORTANT]
> Copy-on-write page protection doesn't save a version of the meeting when a meeting organizer receives responses from attendees and the meeting's tracking information is updated. Also, changes to RSS feeds aren't captured by copy-on-write page protection.

When a mailbox is no longer on In-Place Hold or litigation hold, copies of modified items stored in the Versions folder are removed.

## Recoverable Items mailbox quotas
<a name="RIQuotas"> </a>

When an item is moved to the Recoverable Items folder, its size is deducted from the mailbox quota and added to the size of the Recoverable Items folder. In Exchange Server, mailbox databases have a configurable Recoverable Items warning quota (*soft limit*) of 20 GB and a Recoverable Items quota ( *hard limit*) of 30 GB. By default, these limits are inherited by all mailboxes in the database. However, you can configure individual mailboxes with different quotas. To learn more, see [Configure Deleted Item retention and Recoverable Items quotas](../../recipients/user-mailboxes/deleted-item-retention-and-recoverable-items-quotas.md).

In Exchange Online, the default limits for the Recoverable Items quota are the same as Exchange Server: a soft limit of 20 GB and a hard limit of 30 GB. However, the quotas for the Recoverable Items folder are automatically increased to 90 GB and 100 GB, respectively, when you place a mailbox on Litigation Hold or In-Place Hold.

When the Recoverable Items folder for a mailbox reaches the Recoverable Items quota, no more items can be stored in the folder. This impacts mailbox functionality in the following ways:

- Mailbox users can't delete items.

- The Managed Folder Assistant can't delete items based on retention tag or managed folder settings.

- For mailboxes that have single item recovery, In-Place Hold or Litigation Hold enabled, the copy-on-write page protection process can't maintain versions of items edited by the user.

- For mailboxes that have mailbox audit logging enabled, no mailbox audit log entries can be saved in the Audits subfolder.

For mailboxes that aren't placed on In-Place Hold or Litigation Hold, the Managed Folder Assistant automatically purges items from the Recoverable Items folder when the deleted item retention period expires. If the folder reaches the Recoverable Items warning quota, the assistant automatically purges items in first-in-first-out order.

When the Recoverable Items folder reaches the soft and hard limit defaults, you are notified by means of the event log and a Microsoft System Center Operations Manager alert. This alert fires when the Recoverable Items folder first reaches the soft and hard limit defaults, and then once daily afterward.

The following table lists the events logged when the Recoverable Items folder reaches the soft and hard limit defaults.

**Recoverable Items quota warnings and errors**

|**Event ID**|**Type**|**Source**|**Message**|
|:-----|:-----|:-----|:-----|
|10024|Warning|MSExchangeIS Mailbox Store|The mailbox for _\<mailbox user\>_ (_\<GUID\>_) has exceeded the Recoverable Items Warning Quota. Please remove items from Recoverable Items or increase the Recoverable Items Warning Quota and Recoverable Items Quota. If the Recoverable Items Quota is exceeded, the user will be unable to delete items from the mailbox.|
|10023|Error|MSExchangeIS Mailbox Store|The mailbox for _\<mailbox user\>_ (_\<GUID\>_) has exceeded the maximum Recoverable Items Quota. Items cannot be deleted from this mailbox. The mailbox owner should be notified about the condition of the mailbox as soon as possible. Please remove items from Recoverable Items or increase the Recoverable Items Quota to restore functionality.|
|10023|Warning|MSExchangeMailboxAssistants|The mailbox: _\<mailbox user\>_ Recoverable Items size has exceeded the warning quota limit. Items were deleted from Recoverable Items folders to prevent mailbox outage. Recoverable Items Warning Quota: 20 GB (21,474,836,480 bytes) Original Recoverable Items size: 21475005311 Current Recoverable Items size: 21474823820 Folder stats: - Folders processed: RecoverableItemsRoot, RecoverableItemsVersions, RecoverableItemsPurges, RecoverableItemsDeletions - Original folder sizes: 21391661934, 55190914, 1987247, 26157788 (item counts: 276828, 400, 84, 646) - Current folder sizes: 21391480443, 55190914, 1987247, 26157788 (item counts: 276817, 400, 84, 646)|

If the mailbox is placed on In-Place Hold or Litigation Hold, copy-on-write page protection can't maintain versions of modified items. To maintain versions of modified items, you need to reduce the size of the Recoverable Items folder. You can use the [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/search-mailbox) cmdlet to copy messages from the Recoverable Items folder of a mailbox to a discovery mailbox, and then delete the items from the mailbox. Alternatively, you can also raise the Recoverable Items quota for the mailbox. For details, see [Clean up or delete items from the Recoverable Items folder](clean-up-deleted-items.md).

## More information
<a name="RIQuotas"> </a>

- Copy-on-write is only enabled when a mailbox is on In-Place Hold or Litigation Hold.

- If users need to recover deleted items from the Recoverable Items folder, point them to the following topics:

  - [Restore deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

  - [Recover deleted items or email in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)
