---
localization_priority: Normal
description: Administrators can search for and recover deleted email messages in a user's mailbox.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9e0e34ce-efc5-454e-8d15-57b4da867f12
ms.reviewer: 
f1.keywords:
- NOCSH
title: Recover deleted messages in a user's mailbox
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Recover deleted messages in a user's mailbox

(This topic is intended for Exchange administrators.)

Administrators can search for and recover deleted email messages in a user's mailbox. This includes items that are permanently deleted (purged) by a person (by using the Recover Deleted Items feature in Outlook or Outlook on the web (formerly known as Outlook Web App), or items deleted by an automated process, such as the retention policy assigned to user mailboxes. In these situations, the purged items can't be recovered by a user. But administrators can recover purged messages if the deleted item retention period for the item hasn't expired.

> [!NOTE]
> In addition to using this procedure to search for and recover deleted items (which are moved to the Recoverable Items\Purges folder if either single item recovery or litigation hold is enabled), you can also use this procedure to search for items residing in other folders in the mailbox and to delete items from the source mailbox (also known as search and destroy).

## What you need to know before you begin?

- Estimated time to complete: 15-30 minutes.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Single item recovery must be enabled for a mailbox before the item you want to recover is deleted. In Exchange Online, single item recovery is enabled by default when a new mailbox is created. In Exchange Server, single item recovery is disabled when a mailbox is created. For more information, see [Enable or disable single item recovery for a mailbox](enable-or-disable-single-item-recovery.md).

- To search for and recover items, you must have the following information:

  - **Source mailbox**: This is the mailbox being searched.

  - **Target mailbox**: This is the discovery mailbox in which messages will be recovered. Exchange Setup creates a default discovery mailbox. In Exchange Online, a discovery mailbox is also created by default. If required, you can create additional discovery mailboxes. For details, see [Create a discovery mailbox](../../security-and-compliance/in-place-ediscovery/create-a-discovery-mailbox.md).

    > [!NOTE]
    > When using the **Search-Mailbox** cmdlet, you can also specify a target mailbox that isn't a discovery mailbox. However, you can't specify the same mailbox as the source and target mailbox.

  - **Search criteria**: Criteria include sender or recipient, or keywords (words or phrases) in the message.

- This topic focuses on using PowerShell to recover deleted items in a user's mailbox. You can also use the GUI-based In-Place eDiscovery tool to find and export deleted items to a PST file. The user will use this PST file to restore the deleted messages to their mailbox. For detailed instructions, see [Recover deleted items in a user's mailbox - Admin Help](https://docs.microsoft.com/office365/enterprise/recover-deleted-items-in-a-mailbox).

## Step 1: Connect to Exchange Online PowerShell

For instructions, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=517283).

## Step 2: Search for and recover missing items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

> [!NOTE]
> You can use In-Place eDiscovery in the Exchange admin center (EAC) to search for missing items. However, when using the EAC, you can't restrict the search to the Recoverable Items folder. Messages matching your search parameters will be returned even if they're not deleted. After they're recovered to the specified discovery mailbox, you may need to review the search results and remove unnecessary messages before recovering the remaining messages to the user's mailbox or exporting them to a .pst file. > For details about how to use the EAC to perform an In-Place eDiscovery search, see [Create an In-Place eDiscovery search](../../security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search.md).

The first step in the recovery process is to search for messages in the source mailbox. Use one of the following methods to search a user mailbox and copy messages to a discovery mailbox.

This example searches for messages in April Stewart's mailbox that meet the following criteria:

- Sender: Ken Kwok

- Keyword: Seattle

```PowerShell
Search-Mailbox "April Stewart" -SearchQuery "from:'Ken Kwok' AND seattle" -TargetMailbox "Discovery Search Mailbox" -TargetFolder "April Stewart Recovery" -LogLevel Full
```

> [!NOTE]
> When using the **Search-Mailbox** cmdlet, you can scope the search by using the _SearchQuery_ parameter to specify a query formatted using Keyword Query Language (KQL). You can also use the _SearchDumpsterOnly_ switch to search only items in the Recoverable Items folder.

For detailed syntax and parameter information, see [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/search-mailbox).

### How do you know this worked?

To verify that you have successfully searched the messages you want to recover, log on to the discovery mailbox you selected as the target mailbox and review the search results.

## Step 3: Restore recovered items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

> [!NOTE]
> You can't use the EAC to restore recovered items.

After messages have been recovered to a discovery mailbox, you can restore them to the user's mailbox by using the **Search-Mailbox** cmdlet.

### Use Exchange Online PowerShell to restore messages

This example restores messages to April Stewart's mailbox and deletes them from the Discovery Search Mailbox.

```PowerShell
Search-Mailbox "Discovery Search Mailbox" -SearchQuery "from:'Ken Kwok' AND seattle" -TargetMailbox "April Stewart" -TargetFolder "Recovered Messages" -LogLevel Full -DeleteContent
```

For detailed syntax and parameter information, see [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/search-mailbox).

### How do you know this worked?

To verify that you have successfully recovered messages to the user's mailbox, have the user review messages in the target folder you specified in the above command.

## More information

- The ability to recover deleted items is enabled by single item recovery, which lets an administrator recover a message that's been purged by a user or by retention policy as long as the deleted item retention period hasn't expired for that item. To learn more about single item recovery, see [Recoverable Items folder in Exchange Online](../../security-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- An Exchange Online mailbox is configured to retain deleted items for 14 days, by default. You can change this setting to a maximum of 30 days. For more information, see [Change how long permanently deleted items are kept for an Exchange Online mailbox](change-deleted-item-retention.md).

- As previously explained, you can also use the In-Place eDiscovery tool to find and export deleted items to a PST file. The user will use this PST file to restore the deleted messages to their mailbox. For detailed instructions, see [Recover deleted items in a user's mailbox - Admin Help](https://docs.microsoft.com/office365/enterprise/recover-deleted-items-in-a-mailbox).

- Users can recover a deleted item if it hasn't been purged and if the deleted item retention period for that item hasn't expired. If users need to recover deleted items from the Recoverable Items folder, point them to the following topics:

  - [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

  - [Recover deleted items or email in Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207)

- This topic shows you how to use the **Search-Mailbox** cmdlet to search for and recover missing items. If you use this cmdlet, you can search only one mailbox at a time. If you want to search multiple mailboxes at the same time, you can use [In-Place eDiscovery](../../security-and-compliance/in-place-ediscovery/in-place-ediscovery.md) in the Exchange admin center (EAC) or the [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch) cmdlet in Windows PowerShell.

- In addition to using this procedure to search for and recover deleted items, you can also use a similar procedure to search for items in user mailboxes and then delete those items from the source mailbox. For more information, see [Search for and delete email messages](https://docs.microsoft.com/microsoft-365/compliance/search-for-and-delete-messages-in-your-organization).
