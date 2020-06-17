---
localization_priority: Normal
description: 'Summary: Learn about In-Place Hold and Litigation Hold in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 71031c06-852d-44d8-b558-dff444eaef8c
ms.reviewer:
title: In-Place Hold and Litigation Hold in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# In-Place Hold and Litigation Hold in Exchange Server

When a reasonable expectation of litigation exists, organizations are required to preserve electronically stored information (ESI), including email that's relevant to the case. This expectation often exists before the specifics of the case are known, and preservation is often broad. Organizations may need to preserve all email related to a specific topic or all email for certain individuals. Depending on the organization's electronic discovery (eDiscovery) practices, the following measures can be adopted to preserve email:

- End users may be asked to preserve email by not deleting any messages. However, users can still delete email knowingly or inadvertently.

- Automated deletion mechanisms such as messaging records management (MRM) may be suspended. This could result in large volumes of email cluttering the user mailbox, and thus impacting user productivity. Suspending automated deletion also doesn't prevent users from manually deleting email.

- Some organizations copy or move email to an archive to make sure it isn't deleted, altered, or tampered with. This increases costs due to the manual efforts required to copy or move messages to an archive, or third-party products used to collect and store email outside Exchange.

Failure to preserve email can expose an organization to legal and financial risks such as scrutiny of the organization's records retention and discovery processes, adverse legal judgments, sanctions, or fines.

## Litigation Hold and In-Place Hold
<a name="lithold"> </a>

There are two types of holds available in Exchange Server: Litigation Hold and In-Place Hold. Litigation Hold uses the **LitigationHoldEnabled** property of a mailbox. When Litigation Hold is enabled, all mailbox all items are placed on hold. In contrast, you can use an In-Place Hold to preserve only those items that meet that the criteria of a search query that you define by using the In-Place eDiscovery tool. You can place multiple In-Place Holds on a mailbox, but Litigation Hold is either enabled or disabled for a mailbox. For both types of holds, you can also specify the duration period to hold items. The duration is calculated from the date a mailbox item is received or created. If a duration isn't set, items are held indefinitely or until the hold is removed. If you remove a Litigation Hold from a mailbox, but one or more In-Place Holds are still placed on the mailbox, items matching the In-Place Hold criteria are held for the period specified in the hold settings.

You can use In-Place Hold to place a user on multiple holds. When a user is placed on multiple holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

When you move a mailbox that's on Litigation Hold in Exchange 2010 or Exchange 2013 to a Mailbox server in Exchange 2016, the Litigation Hold setting continues to apply, ensuring that compliance requirements are met during and after the move.

> [!IMPORTANT]
> When you put a mailbox on Litigation Hold or In-Place Hold, the hold is placed on both the primary and the archive mailbox.

For more information when to use each type of hold, see [Place all mailboxes on hold](place-all-mailboxes-on-hold.md).

## Hold goals and features
<a name="scenarios"> </a>

You can use Litigation Hold and In-Place Hold to accomplish the following goals:

- Place user mailboxes on hold and preserve mailbox items immutably.

- Preserve items indefinitely or for a specific duration.

- Preserve mailbox items deleted by users or automatic processes such as MRM.

- Preserve messages that are forwarded to another mailbox.

- Use query-based In-Place Hold to search for and retain items matching specified criteria (you can also place all items hold by including all mailbox content when you create the hold)

- Place a user on multiple holds for different cases or investigations.

- Keep holds transparent from the user by not having to suspend MRM.

- Use In-Place eDiscovery to search for items that are preserved by being placed on hold

If you're upgrading from Exchange Server 2010, the notion of legal hold is to hold all mailbox data for a user indefinitely or until when hold is removed. In Exchange 2016, In-Place Hold introduced a different model that allows you to specify the following parameters:

- **Query-based hold**: With Litigation Hold, all items in a mailbox are preserved. However, an In-Place Hold allows you to specify which items to hold by using search query parameters such as keywords, senders and recipients, start and end dates, and also specify the message types such as email messages, calendar items, and Skype for Business conversations that you want to place on hold. After you create a query-based In-Place Hold, all existing and future mailbox items (including messages received at a later date) that match the query parameters are preserved. Litigation Hold doesn't support query-based holds.

- **Hold duration**: In both Litigation Hold and In-Place Hold, you can specify how long to hold items. You can either specify an infinite hold duration or a time-based hold duration. The duration is calculated from the date a mailbox item is received or created. For example, if your organization requires that all mailbox items be preserved for 7 years, you can create a time-based hold. So if a mailbox is placed on hold and the hold duration is set to 7 years, and an item in the mailbox is permanently deleted after 2 years from the date it was received, it's held for an 5 years before being purged from the mailbox database.

    > [!TIP]
    > You can use a time-based hold together with a retention policy to make sure items are preserved for a specified duration and then permanently removed from Exchange after the retention age and the hold duration expire.

## Placing a mailbox on hold
<a name="MBX"> </a>

The Legal Hold management role is required to place a mailbox on Litigation Hold or In-Place Hold. But to create a query-based In-Place Hold, you must also be assigned the Mailbox Search role. Users that have been added to the [Discovery Management](https://docs.microsoft.com/exchange/discovery-management-exchange-2013-help) role-based access control (RBAC) role group (or assigned the Legal Hold and Mailbox Search roles) can place users hold and create a query-based In-Place Hold. To learn how to add members to the Discovery Management role group, see [Assign eDiscovery permissions in Exchange Server](../../policy-and-compliance/ediscovery/assign-permissions.md).

You can place a mailbox Litigation Hold on the **Recipients** page in the Exchange admin center or by using the `Set-Mailbox -LitigationHoldEnabled $true` command in the Exchange Management Shell.

The In-Place Hold functionality is integrated with In-Place eDiscovery searches. You can place a mailbox on In-Place Hold by using the **In-Place eDiscovery & Hold** wizard in the EAC or the **New-MailboxSearch** cmdlet in the Exchange Management Shell. To learn how, see:

- [Place a mailbox on Litigation Hold](litigation-holds.md)

- [Create or remove an In-Place Hold](in-place-holds.md)

> [!NOTE]
> If you use Exchange Online Archiving to provision a cloud-based archive for your on-premises mailboxes, you must manage In-Place Holds from your on-premises Exchange Server organization. Hold settings are automatically propagated to the cloud-based archive using DirSync.

Many organizations require that users be informed when they're placed on hold. Additionally, when a mailbox is on hold, any retention policies applicable to the mailbox user don't need to be suspended. Because messages continue to be deleted as expected, users may not notice they're on hold. If your organization requires that users on hold be informed, you can add a notification message to the mailbox user's by populating the **Retention Comment** property and using the **RetentionUrl** property to link to a web page for more information. Outlook 2010 and later versions display the retention comment and URL in the backstage area, which is located on the **Files** ribbon. You can use the **Set-Mailbox** cmdlet to add these properties.

## Holds and the Recoverable Items folder
<a name="RIF"> </a>

Litigation Hold and In-Place Hold use the Recoverable Items folder to preserve items. The Recoverable Items folder is hidden from the default view of Outlook, Outlook on the web, and other email clients. To learn more about the Recoverable Items folder, see [Recoverable Items folder in Exchange Server](../../policy-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

By default, when a user deletes a message from a folder other than the Deleted Items folder, the message is moved to the Deleted Items folder. When a user *soft deletes* an item (by pressing SHIFT+DELETE) or deletes an item from the Deleted Items folder, the message is moved to the Recoverable Items folder, thereby disappearing from the user's view.

Items in the Recoverable Items folder are retained for the deleted item retention period configured on the user's mailbox database. By default, the deleted item retention period is set to 14 days for mailbox databases.

The Recoverable Items folder contains the following subfolders used to store deleted items in various sites and facilitate Litigation Hold and In-Place Hold:

- **Deletions**: Items removed from the Deleted Items folder or soft-deleted from other folders are moved to the Deletions subfolder and are visible to the user when using the Recover Deleted Items feature in Outlook and Outlook on the web. By default, items reside in this folder until the deleted item retention period configured for the mailbox database or the mailbox expires.

- **Purges**: When a user deletes an item from the Recoverable Items folder (by using the Recover Deleted Items tool in Outlook and Outlook on the web, the item is moved to the Purges folder. Items that exceed the deleted item retention period configured on the mailbox database or the mailbox are also moved to the Purges folder. Items in this folder aren't visible to users if they use the Recover Deleted Items tool. When the mailbox assistant processes the mailbox, items in the Purges folder are purged from the mailbox database. When you place the mailbox user on Litigation Hold, the mailbox assistant doesn't purge items in this folder.

- **DiscoveryHolds**: If a user is put on an In-Place Hold, deleted items are moved to this folder. When the mailbox assistant processes the mailbox, it evaluates messages in this folder. Items that match the In-Place Hold query are retained until the hold period specified in the query. If no hold period is specified, items are held indefinitely or until the user is removed from the hold. However, if you put a user who was already on an In-Place Hold on Litigation Hold, the Litigation Hold takes preference. Therefore, deleted items are moved to the Purges folder instead.

- **Versions**: When a user is put on In-Place Hold or Litigation Hold, mailbox items must be protected from tampering or modification by the user or a process. This is done by using a *copy-on-write* process. When a user or a process changes specific properties of a mailbox item, a copy of the original item is saved in the Versions folder before the change is committed. This process is repeated for subsequent changes. Items captured in the Versions folder are also indexed and returned in In-Place eDiscovery searches. After the hold is removed, copies in the Versions folder are removed by the Managed Folder Assistant.

**Properties that trigger copy-on-write**

|**Item type**|**Properties that trigger copy-on-write**|
|:-----|:-----|
|Messages (IPM.Note\*)  <br/> Posts (IPM.Post\*)|Subject  <br/> Body  <br/> Attachments  <br/> Senders/Recipients  <br/> Sent/Received Dates|
|Items other than messages and posts|Any change to a visible property, except the following:  <br/> • Item location (when an item is moved between folders)  <br/> • Item status change (read or unread)  <br/> • Changes to retention tag applied to an item|
|Items in the default folder Drafts|None (items in the Drafts folder are exempt from copy on write)|

> [!IMPORTANT]
> Copy-on-write is disabled for calendar items in the organizer's mailbox when meeting responses are received from attendees and the tracking information for the meeting is updated. For calendar items and items that have a reminder set, copy-on-write is disabled for the ReminderTime and ReminderSignalTime properties. Changes to these properties are not captured by copy-on-write. Changes to RSS feeds aren't captured by copy-on-write.

Although the DiscoveryHolds, Purges, and Versions folders aren't visible to the user, all items in the Recoverable Items folder are discoverable by using In-Place eDiscovery. After a mailbox user is removed from In-Place Hold or Litigation Hold, items in the DiscoveryHolds, Purges, and Versions folders are purged by the Managed Folder Assistant.

If a mailbox isn't placed on Litigation Hold or In-Place Hold, items in the Purges folder are permanently deleted from the Recoverable Items folder on a first in, first out basis when the item has resided in the folder for longer than the deleted item retention period.

## Holds and mailbox quotas
<a name="quotas"> </a>

Items in the Recoverable Items folder aren't calculated toward the user's mailbox quota. In Exchange, the Recoverable Items folder has its own quota. For Exchange, the default values for the _RecoverableItemsWarningQuota_ and _RecoverableItemsQuota_ mailbox properties are set to 20 GB and 30 GB respectively. To modify these values for a mailbox database for Exchange Server, use the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet. To modify them for individual mailboxes, use the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet.

When a user's Recoverable Items folder exceeds the warning quota for recoverable items (as specified by the _RecoverableItemsWarningQuota_ parameter), an event is logged in the Application event log of the Mailbox server. When the folder exceeds the quota for recoverable items (as specified by the _RecoverableItemsQuota_ parameter), users won't be able to empty the Deleted Items folder or permanently delete mailbox items. Also copy-on-write won't be able to create copies of modified items. Therefore, it's critical that you monitor Recoverable Items quotas for mailbox users placed on In-Place Hold.

## Holds and email forwarding
<a name="emailforwarding"> </a>

Users with mailboxes on Exchange Server can use Outlook and Outlook on the web to set up email forwarding for their mailbox. Email forwarding lets users configure their mailbox to forward email messages sent to their mailbox to another mailbox located inside or outside of their organization. Administrators can also set up mail flow rules (also known as transport rules) to forward messages to another mailbox. In both cases, email forwarding can be configured so that any message sent to the original mailbox isn't copied to that mailbox and is only sent to the forwarding address.

If a mailbox is on hold, additional steps are taken. During the delivery process, the hold settings for the mailbox are checked. If the message meets the hold criteria for the mailbox, a copy of the message is saved to the Inbox folder. That means you can use In-Place eDiscovery to search the original mailbox to find messages that were forwarded to another mailbox.

## Preserving archived Skype for Business content
<a name="lync"> </a>

Exchange 2016 and Exchange 2019, Skype for Business, and SharePoint 2016 provide an integrated preservation and eDiscovery experience that allows you to preserve and search for items across the different data stores. Exchange 2016 and 2019 allow you to archive Skype for Business content in Exchange, removing the requirement of having a separate SQL Server database to store archived Lync content. The integrated hold and eDiscovery capability in SharePoint 2016 allows you to preserve and search data across all stores from a single console.

When you place an Exchange Server mailbox on In-Place Hold or Litigation Hold, Skype for Business content (such as instant messaging conversations and files shared in an online meeting) are archived in the mailbox. If you search the mailbox using In-Place eDiscovery, any archived Skype for Business content matching the search query is also returned in search results. You can also restrict the search to Skype for Business content archived in the mailbox.

To enable archiving of Skype for Business content in Exchange Server mailboxes, you must configure Skype for Business Server 2015 integration with Exchange Server. For details, see the following topics:

- [Planning for Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-planning-for-archiving)

- [Deploying Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-deploying-archiving)

## Deleting a mailbox on hold
<a name="deletehold"> </a>

If you delete a user account that has a mailbox, the Exchange Information store will eventually detect that the mailbox is no longer connected to a user account and mark that mailbox for deletion, even if the mailbox is on hold. If you want to preserve the mailbox, you have to do the following:

1. Instead of deleting the user account, disable the user account.

2. Change the properties of the mailbox to restrict the use and access to the mailbox. For example, set send and receive quotas equal to 1, block who can send messages to the mailbox, and restrict who can access the mailbox.

3. Retain the mailbox until all data has been removed or until preserving the data is no longer required.

## Migrating mailboxes on hold from Exchange Server to Office 365
<a name="migration"> </a>

If you have an Exchange hybrid deployment, the following conditions are true when you move (onboard) an on-premises Exchange Server mailbox to Exchange Online in Office 365:

- If the on-premises mailbox is on Litigation Hold or In-Place Hold, the hold settings are preserved after the mailbox is moved to Exchange Online.

- If the on-premises mailbox is on Litigation Hold or In-Place Hold, any content in the Recoverable Items folder is moved to the Exchange Online mailbox.

Hold settings and content in the Recoverable Items folder are also preserved when you move (offboard) an Exchange Online mailbox to your on-premises Exchange Server organization.

> [!TIP]
> For Exchange Server, an Exchange hybrid deployment is the recommended way to migrate on-premises mailboxes to Office 365.
