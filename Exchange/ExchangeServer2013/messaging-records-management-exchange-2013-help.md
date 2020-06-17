---
title: 'Messaging records management: Exchange 2013 Help'
TOCTitle: Messaging records management
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 0dd92e9c-881e-43c0-9bbf-f41fdc9dfd87
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Messaging records management in Exchange 2013

_**Applies to:** Exchange Server 2013_

Users send and receive email every day. If left unmanaged, the volume of email generated and received each day can inundate users, impact user productivity, and expose your organization to risks. As a result, email lifecycle management is a critical component for most organizations.

Messaging records management (MRM) is the records management technology in Exchange Server that helps organizations manage email lifecycle and reduce the legal risks associated with email. Deploying MRM can help your organization in several ways:

- **Meet business requirements**: Depending on your organization's messaging policies, you may need to retain important email messages for a certain period. For example, a user's mailbox may contain critical messages related to business strategy, transactions, product development, or customer interactions.

- **Meet legal and regulatory requirements**: Many organizations have a legal or regulatory requirement to store messages for a designated period and remove messages older than that period. Storing messages longer than necessary may increase your organization's legal or financial risks.

- **Increase user productivity**: If left unmanaged, the ever-increasing volume of email in your users' mailboxes can also impact their productivity. For example, although newsletter subscriptions and automated notifications may have informational value when they're received, users may not remove them after reading (often they're never read). Many of these types of messages don't have a retention value beyond a few days. Using MRM to remove such messages can help reduce information clutter in users' mailboxes, thereby increasing productivity.

- **Improve storage management**: Due to expectations driven by free consumer email services, many users keep old messages for a long period or never remove them. Maintaining large mailboxes is increasingly becoming a standard practice, and users shouldn't be forced to change their work habits based on restrictive mailbox quotas. However, retaining messages beyond the period that's necessary for business, legal, or regulatory reasons also increases storage costs.

MRM provides the flexibility to implement the records management policy that best meets your organization's requirements. With a good understanding of MRM, In-Place Archiving, and In-Place Hold, you can help meet your goals of managing mailbox storage and meeting regulatory retention requirements.

Looking for management tasks related to MRM? See [Messaging Records Management Procedures](messaging-records-management-procedures-exchange-2013-help.md).

## MRM in Exchange Server 2013

In Exchange Server 2013, MRM is accomplished through the use of retention tags and retention policies. Retention tags are used to apply retention settings to an entire mailbox and default mailbox folders such as Inbox and Deleted Items. You can also create and deploy retention tags that Outlook 2010 and later and Outlook Web App users can use to apply to folders or individual messages. After they're created, you add retention tags to a retention policy and then apply the policy to users. The Managed Folder Assistant processes mailboxes and applies retention settings in the user's retention policy. To learn more about retention policies, see [Retention tags and retention policies](retention-tags-and-policies-exchange-2013-help.md).

When a message reaches its retention age specified in the applicable retention tag, the Managed Folder Assistant takes the retention action specified by the tag. Messages can then be deleted permanently or deleted with the ability to recover them. If an archive has been provisioned for the user, you can also use retention tags to move items to the user's In-Place Archive.

## MRM strategies

You can use retention policies to enforce basic message retention for an entire mailbox or for specific default folders. Although there are several strategies for deploying MRM, here are some of the most common:

 **Remove all messages after a specified period**: In this strategy, you implement a single MRM policy that removes all messages after a certain period. In this strategy, there's no classification of messages. You can implement this policy by creating a single default policy tag (DPT) for the mailbox. However, this doesn't ensure that messages are retained for the specified period. Users can still delete messages before retention period is reached.

 **Move messages to archive mailboxes**: In this strategy, you implement MRM policies that move items to the user's archive mailbox. An archive mailbox provides additional storage for users to maintain old and infrequently accessed content. Retention tags that move items are also known as archive policies. Within the same retention policy, you can combine a DPT and personal tags to move items, and a DPT, RPTs, and personal tags to delete items. To learn more about archiving policies, see:

- **Exchange Server:**: [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md)

> [!NOTE]
> In an Exchange hybrid deployment, you can enable a cloud-based archive mailbox for an on-premises primary mailbox. If you assign an archive policy to an on-premises mailbox, items are moved to the cloud-based archive. If an item is moved to the archive mailbox, a copy of it isn't retained in the on-premises mailbox. If the on-premises mailbox is placed on hold, an archive policy will still move items to the cloud-based archive mailbox where they are preserved for the duration specified by the hold.

 **Remove messages based on folder location**: In this strategy, you implement MRM policies based on email location. For example, you can specify that messages in the Inbox are retained for one year and messages in the Junk Email folder are retained for 60 days. You can implement this policy by using a combination of retention policy tags (RPTs) for each default folder you want to configure and a DPT for the entire mailbox. The DPT applies to all custom folders and all default folders that don't have an RPT applied.

> [!NOTE]
> In Exchange 2013, you can create RPTs for the Calendar and Tasks folders. If you don't want items in these folders or other default folders to expire, you can create a disabled retention tag for that default folder.

 **Allow users to classify messages**: In this strategy, you implement MRM policies that include a baseline retention setting for all messages but allow users to classify messages based on business or regulatory requirements. In this case, users become an important part of your records management strategy - often they have the best understanding of a message's retention value.

Users can apply different retention settings to messages that need to be retained for a longer or shorter period. You can implement this policy using a combination of the following:

- A DPT for the mailbox

- Personal tags that users can apply to custom folders or individual messages

- (Optional) Additional RPTs to expire items in specific default folders

For example, you can use a retention policy with personal tags that have a shorter retention period (such as two days, one week, or one month), as well as personal tags that have a longer retention period (such as one, two, or five years). Users can apply personal tags with the shorter retention periods for items such as newsletter subscriptions that may lose their value within days of receiving them, and apply the tags with longer periods to preserve items that have a high business value. They can also automate the process by using Inbox rules in Outlook to apply a personal tag to messages that match rule conditions.

 **Retain messages for eDiscovery purposes**: In this strategy, you implement MRM policies that remove messages from mailboxes after a specified period but also retain them in the Recoverable Items folder for [In-Place eDiscovery](in-place-ediscovery-exchange-2013-help.md) purposes, even if the messages were deleted by the user or another process.

You can meet this requirement by using a combination of retention policies and [In-Place Hold and Litigation Hold](in-place-and-litigation-holds-exchange-2013-help.md) or Litigation Hold. Retention policies remove messages from the mailbox after the specified period. A time-based In-Place Hold or Litigation Hold preserves messages that were deleted or modified before that period. For example, to retain messages for seven years, you can create a retention policy with a DPT that deletes messages in seven years and Litigation Hold to hold messages for seven years. Messages that aren't removed by users will be deleted after seven years; messages deleted by users before the seven year period will be retained in the Recoverable Items folder for seven years. To learn more about this folder, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

Optionally, you can use RPTs and personal tags to allow users to clean up their mailboxes. However, In-Place Hold and Litigation Hold continues to retain the deleted messages until the hold period expires.

> [!NOTE]
> A time-based In-Place Hold or Litigation Hold is similar to what was informally referred to as a rolling legal hold in Exchange 2010. Rolling legal hold was implemented by configuring the deleted item retention period for a mailbox database or individual mailbox. However, deleted item retention retains deleted and modified items based on the date deleted. In-Place Hold and Litigation Hold preserves items based on the date they're received or created. This ensures that messages are preserved for at least the specified period.

## For more information

[Messaging records management terminology in Exchange 2013](messaging-records-management-terminology-in-exchange-2013-exchange-2013-help.md)

[Retention tags and retention policies](retention-tags-and-policies-exchange-2013-help.md)
