---
title: 'In-Place Hold and Litigation Hold: Exchange 2013 Help'
TOCTitle: In-Place Hold and Litigation Hold
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 71031c06-852d-44d8-b558-dff444eaef8c
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# In-Place Hold and Litigation Hold in Exchange 2013

_**Applies to:** Exchange Server 2013_

When a reasonable expectation of litigation exists, organizations are required to preserve electronically stored information (ESI), including email that's relevant to the case. This expectation often exists before the specifics of the case are known, and preservation is often broad. Organizations may need to preserve all email related to a specific topic or all email for certain individuals. Depending on the organization's electronic discovery (eDiscovery) practices, the following measures can be adopted to preserve email:

- End users may be asked to preserve email by not deleting any messages. However, users can still delete email knowingly or inadvertently.

- Automated deletion mechanisms such as messaging records management (MRM) may be suspended. This could result in large volumes of email cluttering the user mailbox, and thus impacting user productivity. Suspending automated deletion also doesn't prevent users from manually deleting email.

- Some organizations copy or move email to an archive to make sure it isn't deleted, altered, or tampered with. This increases costs due to the manual efforts required to copy or move messages to an archive, or third-party products used to collect and store email outside Exchange.

Failure to preserve email can expose an organization to legal and financial risks such as scrutiny of the organization's records retention and discovery processes, adverse legal judgments, sanctions, or fines.

You can use In-Place Hold or Litigation Hold to accomplish the following goals:

- Place user mailboxes on hold and preserve mailbox items immutably.

- Preserve mailbox items deleted by users or automatic deletion processes such as MRM.

- Use query-based In-Place Hold to search for and retain items matching specified criteria.

- Preserve items indefinitely or for a specific duration.

- Place a user on multiple holds for different cases or investigations.

- Keep holds transparent from the user by not having to suspend MRM.

- Enable In-Place eDiscovery searches of items placed on hold.

## In-Place Hold scenarios

In previous versions of Exchange, the notion of legal hold is to hold all mailbox data for a user indefinitely or until when hold is removed. In Exchange 2013, In-Place Hold includes a new model that allows you to specify the following parameters:

- **What to hold**: You can specify which items to hold by using query parameters such as keywords, senders and recipients, start and end dates, and also specify the message types such as email messages or calendar items that you want to place on hold.

- **How long to hold**: You can specify a duration for items on hold.

Using this new model, In-Place Hold allows you to create granular hold policies to preserve mailbox items in the following scenarios:

- **Indefinite hold**: The indefinite hold scenario is similar to Litigation Hold. It's intended to preserve mailbox items so you can meet eDiscovery requirements. During the period of litigation or investigation, items are never deleted. The duration isn't known in advance, so no end date is configured. To hold all mail items indefinitely, you don't specify any query parameters or time duration when creating an In-Place Hold.

- **Query-based hold** If your organization preserves items based on specified query parameters, you can use a query-based In-Place Hold. You can specify query parameters such as keywords, start and end dates, sender and recipient addresses, and message types. After you create a query-based In-Place Hold, all existing and future mailbox items (including messages received at a later date) that match the query parameters are preserved.

   > [!IMPORTANT]
   > Items that are marked as unsearchable, generally because of failure to index an attachment, are also preserved because it can't be determined whether they match query parameters. For more details about unsearchable item, see [Unsearchable items in Exchange eDiscovery](unsearchable-items-in-exchange-ediscovery-exchange-2013-help.md).

- **Time-based hold**: Both In-Place Hold and Litigation Hold allow you to specify a duration of time for which to hold items. The duration is calculated from the date a mailbox item is received or created.

    If your organization requires that all mailbox items be preserved for a specific period, you can create a time-based hold. For example, consider a mailbox that's placed on a time-based In-Place Hold and has a retention period set to 365 days. If an item in that mailbox is deleted after 300 days from the date it was received, it's held for an additional 65 days before being permanently deleted. You can use a time-based In-Place Hold in conjunction with a retention policy to make sure items are preserved for the specified duration and permanently removed after that period.

You can use In-Place Hold to place a user on multiple holds. When a user is placed on multiple holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

## In-Place Hold and Litigation Hold

Litigation Hold, the hold feature introduced in Exchange 2010 to preserve data for eDiscovery, is still available in Exchange 2013. Litigation Hold uses the **LitigationHoldEnabled** property of a mailbox. Whereas In-Place Hold provides granular hold capability based on query parameters and the ability to place multiple holds, Litigation Hold only allows you to place all items on hold. You can also specify a duration period to hold items when a mailbox is placed on Litigation Hold. The duration is calculated from the date a mailbox item is received or created. If a duration isn't set, items are held indefinitely or until the hold is removed.

When a mailbox is placed on one or more In-Place Holds and on Litigation Hold (without a duration period) at the same time, all items are held indefinitely or until the holds are removed. If you remove Litigation Hold and the user is still placed on one or more In-Place Holds, items matching the In-Place Hold criteria are held for the period specified in the hold settings. When you move a mailbox that's on Litigation Hold in Exchange 2010 to an Exchange 2013 Mailbox server, the Litigation Hold setting continues to apply, ensuring that compliance requirements are met during and after the move.

> [!NOTE]
> When you place a mailbox on In-Place Hold or Litigation Hold, the hold is placed on both the primary and the archive mailbox. If you place an on-premises primary mailbox on hold in an Exchange hybrid deployment, the cloud-based archive mailbox (if enabled) is also placed on hold.

For more information, see:

- [Place a mailbox on Litigation Hold](place-a-mailbox-on-litigation-hold-exchange-2013-help.md)

- [Place all mailboxes on hold](place-all-mailboxes-on-hold-exchange-2013-help.md)

## Placing a mailbox on In-Place Hold

Authorized users that have been added to the [Discovery Management](discovery-management-exchange-2013-help.md) role-based access control (RBAC) role group or assigned the Legal Hold and Mailbox Search management roles can place mailbox users on In-Place Hold. You can delegate the task to records managers, compliance officers, or attorneys in your organization's legal department, while assigning the least privileges. To learn more about assigning the Discovery Management role group, see [Assign eDiscovery permissions in Exchange](assign-ediscovery-permissions-exchange-2013-help.md).

> [!IMPORTANT]
> In Exchange 2010, the Legal Hold role provided users with sufficient permissions to place mailboxes on Litigation Hold. In Exchange 2013, you can use the same permission to place mailboxes on an indefinite or time-based In-Place Hold. However, to create a query-based In-Place Hold, the user must be assigned the Mailbox Search role. The Discovery Management role group has both these roles assigned.

In Exchange 2013, In-Place Hold functionality is integrated with In-Place eDiscovery searches. You can use the **In-Place eDiscovery & Hold** wizard in the Exchange admin center (EAC) or the **New-MailboxSearch** and related cmdlets in Exchange Management Shell to place a mailbox on In-Place Hold. To learn more about placing a mailbox on In-Place Hold, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md).

> [!NOTE]
> If you use Exchange Online Archiving to provision a cloud-based archive for your on-premises mailboxes, you must manage In-Place Hold from your on-premises Exchange 2013 organization. Hold settings are automatically propagated to the cloud-based archive using DirSync. As previously stated, when you put an on-premises mailbox on hold, the corresponding cloud-based archive is also placed on hold.

Many organizations require that users be informed when they're placed on hold. Additionally, when a mailbox is on hold, any retention policies applicable to the mailbox user don't need to be suspended. Because messages continue to be deleted as expected, users may not notice they're on hold. If your organization requires that users on hold be informed, you can add a notification message to the mailbox user's **Retention Comment** property and use the **RetentionUrl** property to link to a web page for more information. Outlook 2010 and later displays the notification and URL in the backstage area. You must use the Shell to add and manage these properties for a mailbox.

## Holds and the Recoverable Items folder

In-Place Hold and Litigation Hold uses the Recoverable Items folder to preserve items. The Recoverable Items folder replaces the feature informally known as the dumpster in previous versions of Exchange. The Recoverable Items folder is hidden from the default view of Outlook, Outlook Web App, and other email clients. To learn more about the Recoverable Items folder, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

By default, when a user deletes a message from a folder other than the Deleted Items folder, the message is moved to the Deleted Items folder. This is known as a move. When a user soft deletes an item (accomplished by pressing the SHIFT and DELETE keys) or deletes an item from the Deleted Items folder, the message is moved to the Recoverable Items folder, thereby disappearing from the user's view.

Items in the Recoverable Items folder are retained for the deleted item retention period configured on the user's mailbox database. By default, the deleted item retention period is set to 14 days for mailbox databases. You can also configure a storage quota for the Recoverable Items folder. This protects the organization from a potential denial of service (DoS) attack due to rapid growth of the Recoverable Items folder and therefore the mailbox database. If a mailbox isn't placed on In-Place Hold or Litigation Hold, items are purged permanently from the Recoverable Items folder on a first in, first out basis when the Recoverable Items warning quota is exceeded, or the item has resided in the folder for a longer duration than the deleted item retention period.

The Recoverable Items folder contains the following subfolders used to store deleted items in various sites and facilitate In-Place Hold and Litigation Hold:

- **Deletions**: Items removed from the Deleted Items folder or soft-deleted from other folders are moved to the Deletions subfolder and are visible to the user when using the Recover Deleted Items feature in Outlook and Outlook Web App. By default, items reside in this folder until the deleted item retention period configured for the mailbox database or the mailbox expires.

- **Purges**: When a user deletes an item from the Recoverable Items folder (by using the Recover Deleted Items tool in Outlook and Outlook Web App, the item is moved to the Purges folder. Items that exceed the deleted item retention period configured on the mailbox database or the mailbox are also moved to the Purges folder. Items in this folder aren't visible to users if they use the Recover Deleted Items tool. When the mailbox assistant processes the mailbox, items in the Purges folder are purged from the mailbox database. When you place the mailbox user on Litigation Hold, the mailbox assistant doesn't purge items in this folder.

- **DiscoveryHold**: If a user is placed on an In-Place Hold, deleted items are moved to this folder. When the mailbox assistant processes the mailbox, it evaluates messages in this folder. Items matching the In-Place Hold query are retained until the hold period specified in the query. If no hold period is specified, items are held indefinitely or until the user is removed from the hold.

- **Versions**: When a user placed on In-Place Hold or Litigation Hold, mailbox items must be protected from tampering or modification by the user or a process. This is accomplished using a copy-on-write process. When a user or a process changes specific properties of a mailbox item, a copy of the original item is saved in the Versions folder before the change is committed. The process is repeated for subsequent changes. Items captured in the Versions folder are also indexed and returned in In-Place eDiscovery searches. After the hold is removed, copies in the Versions folder are removed by the Managed Folder Assistant.

|**Item type**|**Properties that trigger copy-on-write**|
|:-----|:-----|
|Messages (IPM.Note\*)  <br/> Posts (IPM.Post\*)| Subject  <br/>  Body  <br/>  Attachments  <br/>  Senders/Recipients  <br/>  Sent/Received Dates|
|Items other than messages and posts| Any change to a visible property, except the following:  <br/>  Item location (when an item is moved between folders)  <br/>  Item status change (read or unread)  <br/>  Changes to retention tag applied to an item|
|Items in the default folder Drafts|None (items in the Drafts folder are exempt from copy on write)|

> [!IMPORTANT]
> Copy-on-write is disabled for calendar items in the organizer's mailbox when meeting responses are received from attendees and the tracking information for the meeting is updated. For calendar items and items that have a reminder set, copy-on-write is disabled for the ReminderTime and ReminderSignalTime properties. Changes to these properties are not captured by copy-on-write. Changes to RSS feeds aren't captured by copy-on-write.

Although the DiscoveryHold, Purges, and Versions folders aren't visible to the user, all items in the Recoverable Items folder are indexed by Exchange Search and are discoverable using In-Place eDiscovery. After a mailbox user is removed from In-Place Hold or Litigation Hold, items in the DiscoveryHold, Purges, and Versions folders are purged by the Managed Folder Assistant.

## Holds and mailbox quotas

Items in the Recoverable Items folder aren't calculated toward the user's mailbox quota. In Exchange, the Recoverable Items folder has its own quota. For Exchange, the default values for the _RecoverableItemsWarningQuota_ and _RecoverableItemsQuota_ mailbox properties are set to 20 GB and 30 GB respectively. To modify these values for a mailbox database for Exchange Server 2013, use the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet. To modify them for individual mailboxes, use the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet.

When a user's Recoverable Items folder exceeds the warning quota for recoverable items (as specified by the _RecoverableItemsWarningQuota_ parameter), an event is logged in the Application event log of the Mailbox server. When the folder exceeds the quota for recoverable items (as specified by the _RecoverableItemsQuota_ parameter), users won't be able to empty the Deleted Items folder or permanently delete mailbox items. Also copy-on-write won't be able to create copies of modified items. Therefore, it's critical that you monitor Recoverable Items quotas for mailbox users placed on In-Place Hold.

In Exchange Online, the quota for the Recoverable Items folder (in the user's primary mailbox) is automatically increased to 100 GB when you place a mailbox on Litigation Hold or In-Place Hold. When the storage quota for the Recoverable Items folder in the primary mailbox of a mailbox on hold is close to reaching its limit, you can do the following things:

- **Enable the archive mailbox and turn on auto-expanding archiving**: You can enable an unlimited storage capacity for the Recoverable Items folder simply by enabling the archive mailbox and then turning on the auto-expanding archiving feature in Exchange Online. This results in 100 GB for the Recoverable Items folder in the primary mailbox and an unlimited amount of storage capacity for the Recoverable Items folder in the user's archive. See how: [Enable archive mailboxes in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes) and [Enable unlimited archiving - Admin Help](https://docs.microsoft.com/microsoft-365/compliance/enable-unlimited-archiving).

  **Notes**:

  - After you enable the archive for a mailbox that's close to exceeding the storage quota for the Recoverable Items folder, you might want to run the Managed Folder Assistant to manually trigger the assistant to process the mailbox so that expired items are moved the Recoverable Items folder in the archive mailbox. For instructions, see Step 4 in [Increase the Recoverable Items quota for mailboxes on hold](https://docs.microsoft.com/microsoft-365/compliance/increase-the-recoverable-quota-for-mailboxes-on-hold).

  - Note that other items in the user's mailbox might be moved to the new archive mailbox. Consider telling the user that this might happen after you enable the archive mailbox.

- **Create a custom retention policy for mailboxes on hold** In addition to enabling the archive mailbox and auto-expanding archiving for mailboxes on Litigation Hold or In-Place Hold, you might also want to create a custom retention policy for mailboxes on hold. This let's you apply a retention policy to mailboxes on hold that's different from the Default MRM Policy that's applied to mailboxes that aren't on hold. This lets you to apply retention tags that are specifically designed for mailboxes on hold. This includes creating a new retention tag for the Recoverable Items folder.

For more information, see [Increase the Recoverable Items quota for mailboxes on hold](https://docs.microsoft.com/microsoft-365/compliance/increase-the-recoverable-quota-for-mailboxes-on-hold).

## Holds and email forwarding

Users can use Outlook and Outlook Web App to set up email forwarding for their mailbox. Email forwarding lets users configure their mailbox to forward email messages sent to their mailbox to another mailbox located in or outside of their organization. Email forwarding can be configured so that any message sent to the original mailbox isn't copied to that mailbox and is only sent to the forwarding address.

If email forwarding is set up for a mailbox and messages aren't copied to the original mailbox, what happens if the mailbox is on hold?

If messages are forwarded to another mailbox and not copied to the original mailbox, they aren't captured and copied to the Recoverable Items folder. That means forwarded messages won't be returned in an In-Place eDiscovery search. To address this issue, Exchange 2013 organizations can consider removing the ability for users to configure email forwarding.

## Preserving archived Lync content

Exchange 2013, Microsoft Lync 2013 and Microsoft SharePoint 2013 provide an integrated preservation and eDiscovery experience that allows you to preserve and search for items across the different data stores. Exchange 2013 allows you to archive Lync Server 2013 content in Exchange, removing the requirement of having a separate SQL Server database to store archived Lync content. The integrated hold and eDiscovery capability in SharePoint 2013 allows you to preserve and search data across all stores from a single console.

When you place an Exchange 2013 mailbox on In-Place Hold or Litigation Hold, Microsoft Lync 2013 content (such as instant messaging conversations and files shared in an online meeting) are archived in the mailbox. If you search the mailbox using the eDiscovery Center in Microsoft SharePoint 2013 or In-Place eDiscovery in Exchange 2013, any archived Lync content matching the search query is also returned in search results. You can also restrict the search to Lync content archived in the mailbox.

To enable archiving of Lync content in Exchange 2013 mailbox, you must configure Lync 2013 integration with Exchange 2013. For details, see the following topics:

- [Planning for Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-planning-for-archiving)

- [Deploying Archiving](https://docs.microsoft.com/lyncserver/lync-server-2013-deploying-archiving)

## Deleting a mailbox on hold

If an administrator deletes a user account that has a mailbox, the Exchange Information store will eventually detect that the mailbox is no longer connected to a user account and mark that mailbox for deletion, even if the mailbox is on hold. If you want to retain the mailbox, you must do the following:

1. Instead of deleting the user account, disable the user account.

2. Change the properties of the mailbox to restrict its use and access to the mailbox. For example, set send and receive quotas equal to 1, block who can send messages to the mailbox, and restrict who can access the mailbox.

3. Retain the mailbox until all data has been expunged, or until preserving the data is no longer required.

## Migrating mailboxes on hold from Exchange 2013 to Office 365

If you have an Exchange hybrid deployment, the following conditions are true when you move (onboard) an on-premises Exchange 2013 mailbox to Exchange Online in Office 365:

- If the on-premises mailbox is on Litigation Hold or In-Place Hold, the hold settings are preserved after the mailbox is moved to Exchange Online.

- If the on-premises mailbox is on Litigation Hold or In-Place Hold, any content in the Recoverable Items folder is moved to the Exchange Online mailbox.

> [!NOTE]
> Hold settings and content in the Recoverable Items folder are also preserved when you move (offboard) an Exchange Online mailbox to your on-premises Exchange 2013 organization.

There are other ways to migrate on-premises email data to Office 365, such as using a Staged Exchange migration or a Cutover Exchange migration.

- A staged migration can be used to migrate mailboxes from Exchange 2003 or Exchange 2007 to Office 365. In these versions of Exchange, the Recoverable Items folder (and its functionality) doesn't exist. So when you migrate Exchange 2003 or Exchange 2007 mailboxes to Office 365, there isn't any Recoverable Items folder content to move.

- A cutover migration can be used to migrate mailboxes from Exchange 2003, Exchange 2007, and Exchange 2010 to Office 365. As previously stated, Exchange 2003 and Exchange 2007 mailboxes don't have a Recoverable Items folder that can be migrated. Because the Recover Items folder was introduced in Exchange 2010, content in the Recoverable Items folder is migrated to Office 365 when you use a cutover migration to migrate Exchange 2010 mailboxes.

> [!TIP]
> For Exchange 2013 and Exchange 2010, an Exchange hybrid deployment is the recommended way to migrate on-premises mailboxes to Office 365.
