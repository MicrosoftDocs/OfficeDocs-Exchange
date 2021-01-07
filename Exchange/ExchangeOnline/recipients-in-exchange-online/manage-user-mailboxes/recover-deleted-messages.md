---
localization_priority: Normal
description: Administrators can search for and recover deleted email messages in a user's mailbox.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 9e0e34ce-efc5-454e-8d15-57b4da867f12
ms.reviewer: 
f1.keywords:
- NOCSH
title: Recover deleted messages in a user's mailbox in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars
---

# Recover deleted messages in a user's mailbox in Exchange Online

(This topic is intended for Exchange administrators.)

Administrators can search for and recover deleted email messages in a user's mailbox. This includes items that are permanently deleted (purged) by a person (by using the Recover Deleted Items feature in Outlook or Outlook on the web (formerly known as Outlook Web App)), or items deleted by an automated process, such as the retention policy assigned to user mailboxes. In these situations, the purged items can't be recovered by a user. But administrators can recover purged messages if the deleted item retention period for the item hasn't expired.

> [!NOTE]
> In addition to using this procedure to search for and recover deleted items (which are moved to the Recoverable Items\Purges folder if either single item recovery or litigation hold is enabled), you can also use this procedure to search for items residing in other folders in the mailbox and to delete items from the source mailbox (also known as search and destroy).

## What you need to know before you begin?

- Estimated time to complete: 15-30 minutes.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- Single item recovery must be enabled for a mailbox before the item you want to recover is deleted. In Exchange Online, single item recovery is enabled by default when a new mailbox is created. In Exchange Server, single item recovery is disabled when a mailbox is created. For more information, see [Enable or disable single item recovery for a mailbox](enable-or-disable-single-item-recovery.md).

- To search for and recover items, you must have the following information:

  - **Source mailbox**: This is the mailbox being searched.

  - **Target mailbox**: This is the discovery mailbox in which messages will be recovered. Exchange Setup creates a default discovery mailbox. In Exchange Online, a discovery mailbox is also created by default. If required, you can create additional discovery mailboxes. For details, see [Create a discovery mailbox](../../security-and-compliance/in-place-ediscovery/create-a-discovery-mailbox.md).

  - **Search criteria**: Criteria include sender or recipient, or keywords (words or phrases) in the message.

- This topic focuses on using PowerShell to recover deleted items in a user's mailbox. You can also use the GUI-based In-Place eDiscovery tool to find and export deleted items to a PST file. The user will use this PST file to restore the deleted messages to their mailbox. For detailed instructions, see [Recover deleted items in a user's mailbox - Admin Help](https://docs.microsoft.com/office365/enterprise/recover-deleted-items-in-a-mailbox).

## Use new EAC for recovering deleted messages

1. In the new EAC, navigate to **Recipients > Mailboxes**.
  
2. Select the mailbox for which you want to recover deleted messages, and click on the display name.

3. Under **More actions**, click **Recover deleted items**.

4. Enter values for each or either of the filter criteria from the drop-down lists.

5. Click **Apply filter**.

## Using Powershell to manage deleted items

### Step 1: Connect to Exchange Online PowerShell

For instructions, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

### Step 2: Search for and recover missing items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

> [!NOTE]
> You can use In-Place eDiscovery in the Exchange admin center (EAC) to search for missing items. However, when using the EAC, you can't restrict the search to the Recoverable Items folder. Messages matching your search parameters will be returned even if they're not deleted. After they're recovered to the specified discovery mailbox, you may need to review the search results and remove unnecessary messages before recovering the remaining messages to the user's mailbox or exporting them to a .pst file. For details about how to use the EAC to perform an In-Place eDiscovery search, see [Create an In-Place eDiscovery search](../../security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search.md).

### Use the Exchange Online PowerShell to search for messages

```PowerShell
Get-RecoverableItems -Identity laura@contoso.com -SubjectContains "FY17 Accounting" -FilterItemType IPM.Note -FilterStartTime "2/1/2018 12:00:00 AM" -FilterEndTime "2/5/2018 11:59:59 PM"
```
This example returns all of the available recoverable deleted messages with the specified subject in the mailbox laura@contoso.com for the specified date/time range.

> [!TIP]
> Use the **Get-RecoverableItems** cmdlet to create a search query to find an Outlook item. Once you have a list of results you can use properties like last modified date, item type, etc. to narrow the amount of items restored or to restore a specific item.

For detailed syntax and parameter information, see [Get-RecoverableItems](https://docs.microsoft.com/powershell/module/exchange/get-recoverableitems?view=exchange-ps).

#### How do you know this worked?

To verify that you have successfully searched the messages you want to recover, log on to the discovery mailbox you selected as the target mailbox and review the search results.

### Step 3: Restore recovered items

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

> [!NOTE]
> You can't use the EAC to restore recovered items.

After messages have been recovered to a discovery mailbox, you can restore them to the user's mailbox by using the **Restore-RecoverableItems** cmdlet.

### Use Exchange Online PowerShell to restore messages

```PowerShell
Restore-RecoverableItems -Identity "malik@contoso.com","lillian@contoso.com" -FilterItemType IPM.Note -SubjectContains "COGS FY17 Review" -FilterStartTime "3/15/2019 12:00:00 AM" -FilterEndTime "3/25/2019 11:59:59 PM" -MaxParallelSize 2
```

After using the Get-RecoverableItems cmdlet to verify the existence of the item, this example restores the specified deleted items in the specified mailboxes:

Mailboxes: malik@contoso.com, lillian@contoso.com

Item type: Email message

Message subject: COGS FY17 Review

Location: Recoverable Items\Deletions

Date range: 3/15/2019 to 3/25/2019

Number of mailboxes processed simultaneously: 2

For detailed syntax and parameter information, see [Restore-RecoverableItems](https://docs.microsoft.com/powershell/module/exchange/restore-recoverableitems?view=exchange-ps).

### How do you know this worked?

To verify that you have successfully recovered messages to the user's mailbox, have the user review messages in the target folder you specified in the above command.

## More information

- The ability to recover deleted items is enabled by single item recovery, which lets an administrator recover a message that's been purged by a user or by retention policy as long as the deleted item retention period hasn't expired for that item. To learn more about single item recovery, see [Recoverable Items folder in Exchange Online](../../security-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- An Exchange Online mailbox is configured to retain deleted items for 14 days, by default. You can change this setting to a maximum of 30 days. For more information, see [Change how long permanently deleted items are kept for an Exchange Online mailbox](change-deleted-item-retention.md).

- As previously explained, you can also use the In-Place eDiscovery tool to find and export deleted items to a PST file. The user will use this PST file to restore the deleted messages to their mailbox. For detailed instructions, see [Recover deleted items in a user's mailbox - Admin Help](https://docs.microsoft.com/office365/enterprise/recover-deleted-items-in-a-mailbox).

- Users can recover a deleted item if it hasn't been purged and if the deleted item retention period for that item hasn't expired. If users need to recover deleted items from the Recoverable Items folder, point them to the following topics:

  - [Recover deleted items in Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce)

  - [Recover deleted email messages in Outlook on the web](https://support.microsoft.com/office/a8ca78ac-4721-4066-95dd-571842e9fb11)

- In addition to using this procedure to search for and recover deleted items, you can also use a similar procedure to search for items in user mailboxes and then delete those items from the source mailbox. For more information, see [Search for and delete email messages](https://docs.microsoft.com/microsoft-365/compliance/search-for-and-delete-messages-in-your-organization).

## Related article

Are you using Exchange Server? See [Recover deleted messages in a user's mailbox in Exchange Server](https://docs.microsoft.com/Exchange/recipients/user-mailboxes/recover-deleted-messages?view=exchserver-2019).

