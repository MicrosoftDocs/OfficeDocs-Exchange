---
localization_priority: Normal
description: "When a reasonable expectation of litigation exists, organizations are required to preserve electronically stored information (ESI), including email that's relevant to the case. This expectation often exists before the specifics of the case are known, and preservation is often broad. Organizations may need to preserve all email related to a specific topic or all email for certain individuals. Depending on the organization's electronic discovery (eDiscovery) practices, the following measures can be adopted to preserve email:"
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 71031c06-852d-44d8-b558-dff444eaef8c
ms.reviewer: 
f1.keywords:
- NOCSH
title: In-Place Hold and Litigation Hold
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid: MET150
audience: Admin
ms.service: exchange-online
manager: serdars

---

# In-Place Hold and Litigation Hold

> [!IMPORTANT]
>  As we continue to invest in different ways to preserve mailbox content, we're announcing the retirement of In-Place Holds in the Exchange admin center (EAC) in Exchange Online. Starting July 1, 2020, you won't be able to create new In-Place Holds. But you'll still be able to manage In-Place Holds in the EAC or by using the **Set-MailboxSearch** cmdlet in Exchange Online PowerShell. However, starting October 1, 2020, you won't be able to manage In-Place Holds. You'll only be able to remove them in the EAC or by using the **Remove-MailboxSearch** cmdlet. Using In-Place Holds in Exchange Server and Exchange hybrid deployments will still be supported. You will also still be able to place mailboxes on Litigation Hold. For more information about the retirement of In-Place Holds in Exchange Online, see [Retirement of legacy eDiscovery tools](https://docs.microsoft.com/microsoft-365/compliance/legacy-ediscovery-retirement).

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

In previous versions of Exchange, the notion of legal hold is to hold all mailbox data for a user indefinitely or until when hold is removed. In Exchange Online, In-Place Hold includes a new model that allows you to specify the following parameters:

- **What to hold**: You can specify which items to hold by using query parameters such as keywords, senders and recipients, start and end dates, and also specify the message types such as email messages or calendar items that you want to place on hold.

- **How long to hold**: You can specify a duration for items on hold.

Using this new model, In-Place Hold allows you to create granular hold policies to preserve mailbox items in the following scenarios:

- **Indefinite hold**: The indefinite hold scenario is similar to Litigation Hold. It's intended to preserve mailbox items so you can meet eDiscovery requirements. During the period of litigation or investigation, items are never deleted. The duration isn't known in advance, so no end date is configured. To hold all mail items indefinitely, you don't specify any query parameters or time duration when creating an In-Place Hold.

    > [!IMPORTANT]
    > Placing a mailbox on an indefinite hold means that mail items meeting the hold requirements will never be removed from the mailbox. This could result in the mailbox exceeding the [Recoverable Items Quota](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds#holds-and-the-recoverable-items-folder), which could make the mailbox unusable. Microsoft recommends enabling an [Archive](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-archiving-service-description/archive-features) for the mailbox, as well as enabling the [auto-expanding archive feature](https://docs.microsoft.com/microsoft-365/compliance/unlimited-archiving). See [Holds and Mailbox Quotas](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds#holds-and-mailbox-quotas) for more information.

- **Query-based hold**: If your organization preserves items based on specified query parameters, you can use a query-based In-Place Hold. You can specify query parameters such as keywords, start and end dates, sender and recipient addresses, and message types. After you create a query-based In-Place Hold, all existing and future mailbox items (including messages received at a later date) that match the query parameters are preserved.

    > [!IMPORTANT]
    > Items that are marked as unsearchable, generally because of failure to index an attachment, are also preserved because it can't be determined whether they match query parameters. For more details about partially indexed items, see [Partially indexed items in Content Search in Office 365](https://docs.microsoft.com/office365/securitycompliance/partially-indexed-items-in-content-search).

- **Time-based hold**: Both In-Place Hold and Litigation Hold allow you to specify a duration of time for which to hold items. The duration is calculated from the date a mailbox item is received or created.

    If your organization requires that all mailbox items be preserved for a specific period, for example 7 years, you can create a time-based hold so that items on hold are retained for a specific period of time. For example, consider a mailbox that's placed on a time-based In-Place Hold and has a retention period set to 365 days. If an item in that mailbox is deleted after 300 days from the date it was received, it's held for an additional 65 days before being permanently deleted. You can use a time-based In-Place Hold in conjunction with a retention policy to make sure items are preserved for the specified duration and permanently removed after that period.

You can use In-Place Hold to place a user on multiple holds. When a user is placed on multiple holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

## In-Place Hold and Litigation Hold

Litigation Hold uses the **LitigationHoldEnabled** property of a mailbox to place mailbox content on hold. Whereas In-Place Hold provides granular hold capability based on query parameters and the ability to place multiple holds, Litigation Hold only allows you to place all items on hold. You can also specify a duration period to hold items when a mailbox is placed on Litigation Hold. The duration is calculated from the date a mailbox item is received or created. If a duration isn't set, items are held indefinitely or until the hold is removed.

When a mailbox is placed on one or more In-Place Holds and on Litigation Hold (without a duration period) at the same time, all items are held indefinitely or until the holds are removed. If you remove Litigation Hold and the user is still placed on one or more In-Place Holds, items matching the In-Place Hold criteria are held for the period specified in the hold settings.

> [!NOTE]
> When you place a mailbox on In-Place Hold or Litigation Hold, the hold is placed on both the primary and the archive mailbox. If you place an on-premises primary mailbox on hold in an Exchange hybrid deployment, the cloud-based archive mailbox (if enabled) is also placed on hold.

## Placing a mailbox on In-Place Hold

Authorized users that have been added to the Discovery Management role-based access control (RBAC) role group or assigned the Legal Hold and Mailbox Search management roles can place mailbox users on In-Place Hold. You can delegate the task to records managers, compliance officers, or attorneys in your organization's legal department, while assigning the least privileges. To learn more about assigning the Discovery Management role group, see [Assign eDiscovery permissions in Exchange](in-place-ediscovery/assign-ediscovery-permissions.md).

You can use the **In-Place eDiscovery & Hold** wizard in the Exchange admin center (EAC) or the **New-MailboxSearch** and related cmdlets in Exchange Online PowerShell to place a mailbox on In-Place Hold. To learn more about placing a mailbox on In-Place Hold, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds.md).

Many organizations require that users be informed when they're placed on hold. Additionally, when a mailbox is on hold, any retention policies applicable to the mailbox user don't need to be suspended. Because messages continue to be deleted as expected, users may not notice they're on hold. If your organization requires that users on hold be informed, you can add a notification message to the mailbox user's **Retention Comment** property and use the **RetentionUrl** property to link to a web page for more information. Outlook 2010 and later displays the notification and URL in the backstage area. You must use Exchange Online PowerShell to add and manage these properties for a mailbox.

## Placing public folders on hold

In Exchange Online, you can place public folders on hold by using a In-Place Hold. Using Litigation Hold for public folders isn't supported. When you create an In-Place Hold, the only option is to place a hold on all public folders in your organization. The result is that an In-Place Hold is placed on all public folder mailboxes.

Additionally, when you place public folders on In-Place Hold, email messages related to the public folder hierarchy synchronization process are also preserved. This might result in thousands of hierarchy synchronization related email items being preserved. These messages can fill up the storage quota for the Recoverable Items folder on public folder mailboxes. To prevent this, you can create a query-based In-Place Hold and add the following `property:value` pair to the search query:

```
NOT(subject:HierarchySync*)
```

The result is that any message (related to the synchronization of the public folder hierarchy) that contains the phrase "HierarchySync" in the subject line is not placed on hold.

## Holds and the Recoverable Items folder

In-Place Hold and Litigation Hold uses the Recoverable Items folder to preserve items. The Recoverable Items folder replaces the feature informally known as the dumpster in previous versions of Exchange. The Recoverable Items folder is hidden from the default view of Outlook, Outlook on the web (formerly known as Outlook Web App), and other email clients. To learn more about the Recoverable Items folder, see [Recoverable Items folder in Exchange Online](recoverable-items-folder/recoverable-items-folder.md).

By default, when a user deletes a message from a folder other than the Deleted Items folder, the message is moved to the Deleted Items folder. This is known as a move. When a user soft deletes an item (accomplished by pressing the SHIFT and DELETE keys) or deletes an item from the Deleted Items folder, the message is moved to the Recoverable Items folder, thereby disappearing from the user's view.

Items in the Recoverable Items folder are retained for the deleted item retention period configured for the user's mailbox. By default, the deleted item retention period is 14 days for Exchange Online mailboxes. You can also configure a storage quota for the Recoverable Items folder. This protects the organization from a potential denial of service (DoS) attack due to rapid growth of the Recoverable Items folder. If a mailbox isn't placed on In-Place Hold or Litigation Hold, items are purged permanently from the Recoverable Items folder on a first in, first out basis when the Recoverable Items warning quota is exceeded, or the item has resided in the folder for a longer duration than the deleted item retention period.

The Recoverable Items folder contains the following subfolders used to store deleted items in various sites and facilitate In-Place Hold and Litigation Hold:

- **Deletions** - Items removed from the Deleted Items folder or soft-deleted from other folders are moved to the Deletions subfolder and are visible to the user when using the Recover Deleted Items feature in Outlook and Outlook on the web. By default, items reside in this folder until the deleted item retention period configured for the mailbox expires.

- **Purges** - When a user deletes an item from the Recoverable Items folder (by using the Recover Deleted Items tool in Outlook and Outlook on the web, the item is moved to the Purges folder. Items that exceed the deleted item retention period configured for the mailbox are also moved to the Purges folder. Items in this folder aren't visible to users if they use the Recover Deleted Items tool. When the Managed Folder Assistant processes the mailbox, items in the Purges folder are purged from the mailbox. When you place the mailbox user on Litigation Hold, the Managed Folder Assistant doesn't purge items in this folder.

- **DiscoveryHold** - If a user is placed on an In-Place Hold, deleted items are moved to this folder. When the Managed Folder Assistant processes the mailbox, it evaluates messages in this folder. Items matching the In-Place Hold query are retained until the hold period specified in the query. If no hold period is specified, items are held indefinitely or until the user is removed from the hold.

- **Versions** - When a user placed on In-Place Hold or Litigation Hold, mailbox items must be protected from tampering or modification by the user or a process. This is accomplished using a copy-on-write process. When a user or a process changes specific properties of a mailbox item, a copy of the original item is saved in the Versions folder before the change is committed. The process is repeated for subsequent changes. Items captured in the Versions folder are also indexed and returned in eDiscovery searches. After the hold is removed, copies in the Versions folder are removed by the Managed Folder Assistant.

**Properties that trigger copy-on-write**

|**Item type**|**Properties that trigger copy-on-write**|
|:-----|:-----|
|Messages (IPM.Note\*)  <br/> Posts (IPM.Post\*)| Subject  <br/>  Body  <br/>  Attachments  <br/>  Senders/Recipients  <br/>  Sent/Received Dates|
|Items other than messages and posts| Any change to a visible property, except the following:  <br/>  Item location (when an item is moved between folders)  <br/>  Item status change (read or unread)  <br/>  Changes to retention tag applied to an item|
|Items in the default folder Drafts|None (items in the Drafts folder are exempt from copy on write)|

> [!IMPORTANT]
> Copy-on-write is disabled for calendar items in the organizer's mailbox when meeting responses are received from attendees and the tracking information for the meeting is updated. For calendar items and items that have a reminder set, copy-on-write is disabled for the ReminderTime and ReminderSignalTime properties. Changes to these properties are not captured by copy-on-write. Changes to RSS feeds aren't captured by copy-on-write.

Although the DiscoveryHold, Purges, and Versions folders aren't visible to the user, all items in the Recoverable Items folder are indexed by Exchange Search and are discoverable using In-Place eDiscovery. After a mailbox user is removed from In-Place Hold or Litigation Hold, items in the DiscoveryHold, Purges, and Versions folders are purged by the Managed Folder Assistant.

## Holds and mailbox quotas

Items in the Recoverable Items folder aren't calculated toward the user's mailbox quota. In Exchange Online, the Recoverable Items folder has its own quota. For Exchange, the default values for the _RecoverableItemsWarningQuota_ and _RecoverableItemsQuota_ mailbox properties are set to 20 GB and 30 GB respectively. In Exchange Online, the quota for the Recoverable Items folder (in the user's primary mailbox) is automatically increased to 100 GB when you place a mailbox on Litigation Hold or In-Place Hold. When the storage quota for the Recoverable Items folder in the primary mailbox of a mailbox on hold is close to reaching its limit, you can do the following things:

- **Enable the archive mailbox and turn on auto-expanding archiving** - You can enable an unlimited storage capacity for the Recoverable Items folder simply by enabling the archive mailbox and then turning on the auto-expanding archiving feature in Exchange Online. This results in 110 GB for the Recoverable Items folder in the primary mailbox and an unlimited amount of storage capacity for the Recoverable Items folder in the user's archive. See how: [Enable archive mailboxes in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes) and [Enable unlimited archiving - Admin help](https://docs.microsoft.com/microsoft-365/compliance/enable-unlimited-archiving).

    **Notes**:

  - After you enable the archive for a mailbox that's close to exceeding the storage quota for the Recoverable Items folder, you might want to run the Managed Folder Assistant to manually trigger the assistant to process the mailbox so that expired items are moved the Recoverable Items folder in the archive mailbox. For instructions, see Step 4 in [Increase the Recoverable Items quota for mailboxes on hold](https://docs.microsoft.com/office365/securitycompliance/increase-the-recoverable-quota-for-mailboxes-on-hold#optional-step-4-run-the-managed-folder-assistant-to-apply-the-new-retention-settings).

  - Note that other items in the user's mailbox might be moved to the new archive mailbox. Consider telling the user that this might happen after you enable the archive mailbox.

- **Create a custom retention policy for mailboxes on hold** - In addition to enabling the archive mailbox and auto-expanding archiving for mailboxes on Litigation Hold or In-Place Hold, you might also want to create a custom MRM retention policy in Exchange Online for mailboxes on hold. This let's you apply a retention policy to mailboxes on hold that's different from the Default MRM Policy that's applied to mailboxes that aren't on hold. This lets you to apply retention tags that are specifically designed for mailboxes on hold. This includes creating a new retention tag for the Recoverable Items folder.

For more information, see [Increase the Recoverable Items quota for mailboxes on hold](https://docs.microsoft.com/office365/securitycompliance/increase-the-recoverable-quota-for-mailboxes-on-hold).

## Holds and email forwarding

Users can use Outlook and Outlook on the web to set up email forwarding for their mailbox. Email forwarding lets users configure their mailbox to forward email messages sent to their mailbox to another mailbox located in or outside of their organization. Email forwarding can be configured so that any message sent to the original mailbox isn't copied to that mailbox and is only sent to the forwarding address.

If email forwarding is set up for a mailbox and messages aren't copied to the original mailbox, what happens if the mailbox is on hold? The hold settings for the mailbox are checked during the delivery process. If the message meets the hold criteria for the mailbox, a copy of the message is saved to the Recoverable Items folder. That means you can use eDiscovery tools to search the original mailbox to find messages that were forwarded to another mailbox.

## Deleting a mailbox on hold

When you delete the corresponding Office 365 account for a mailbox that's been placed on Litigation Hold or In-Place Hold, the mailbox is converted to an inactive mailbox, which is a type of soft-deleted mailbox. Inactive mailboxes are used to preserve the contents of a user's mailbox after they leave your organization. Items in an inactive mailbox are preserved for the duration of the hold that was placed on the mailbox before it was made inactive. This allows administrators, compliance officers, or records managers to use the Content Search tool in the Microsoft 365 Compliance Center to access and search the contents of an inactive mailbox. Inactive mailboxes can't receive email and aren't displayed in your organization's shared address book or other lists. For more information, see [Overview of inactive mailboxes in Office 365](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365).
