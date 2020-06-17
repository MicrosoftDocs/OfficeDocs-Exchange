---
localization_priority: Normal
description: 'Summary: Administrators can learn about In-Place Archiving and archive mailboxes in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b5e4c0e9-0558-4b90-bc12-f67adbfb59ac
ms.reviewer:
title: In-Place Archiving in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# In-Place Archiving in Exchange Server

 *In-Place Archiving* in Exchange Server helps you regain control of your organization's messaging data by eliminating the need for personal store (.pst) files and allowing users to store messages in an *archive mailbox*. The archive mailbox is an additional mailbox that's enabled for a user's primary mailbox. The archive mailbox is accessible in Outlook and Outlook on the web (formerly known as Outlook Web App). Users can view an archive mailbox and move or copy messages between their primary mailbox and their archive mailbox.

You can provision a user's archive mailbox on the same mailbox database as the user's primary mailbox, a different mailbox database on the same Mailbox server, or on a mailbox database on a different Mailbox server in the same Active Directory site. In Exchange hybrid deployments, you can also provision a cloud-based archive mailbox for primary mailboxes located in your on-premises organization.

## Client access to archive mailboxes

The following table lists the client applications that can be used to access archive mailboxes.

|**Client**|**Access to archive mailbox?**|
|:-----|:-----|
|Outlook for Mac for Office 365 <br/><br/> Outlook 2016 for Mac or later <br/><br/> Microsoft 365 Apps for enterprise <br/><br/> Outlook 2013 or later <br/><br/> Outlook on the web|Yes. Users can copy or move items from their primary mailbox to their archive mailbox, and can also use retention policies to move items to the archive. <br/><br/> Outlook doesn't create a local copy of the archive mailbox on a user's computer, even if it's configured to use Cached Exchange Mode. Users can access an archive mailbox in online mode only.|
|Exchange ActiveSync|No|

> [!NOTE]
> • In-Place Archiving is a premium feature and requires an Exchange Enterprise client access license (CAL). For details about how to license Exchange, see [Exchange licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview). <br/>• For details about the versions of Outlook that are required to access an archive mailbox, see [Outlook license requirements for Exchange features](https://support.microsoft.com/office/46b6b7c5-c3ca-43e5-8424-1e2807917c99).

## Moving messages to the archive mailbox

There are several ways to move messages from a user's primary mailbox to their archive mailbox:

- **Move or copy messages manually**: Users can manually move or copy messages from their primary mailbox or a .pst file to their archive mailbox. The archive mailbox appears as another mailbox Outlook and Outlook on the web or like a mounted .pst file in Outlook.

- **Move or copy messages using Inbox rules**: Users can create Inbox rules in Outlook or Outlook on the web to automatically move messages to a folder in their archive mailbox.

- **Move messages using retention policies**: You can use retention policies to automatically move messages to the archive mailbox. Users can also apply personal tags to move messages to their archive mailbox. For details about archive and retention policies, see the next section in this topic.

- **Import messages from .pst files**: In Exchange Server, you can use a mailbox import request to import messages from a .pst file to a user's archive or primary mailbox. For details, see [Mailbox imports and exports in Exchange Server](../../recipients/mailbox-import-and-export/mailbox-import-and-export.md).

## Archiving and retention policies

In Exchange Server, you can apply archive policies to a mailbox to automatically move messages from a user's primary mailbox to the archive mailbox after a specified period. Archive policies are implemented by creating retention tags that use the **Move to Archive** retention action.

Messages are moved to a folder in the archive mailbox that has the same name as the source folder in the primary mailbox. If a folder with the same name doesn't exist in the archive mailbox, it's created when the Managed Folder Assistant moves a message. Re-creating the same folder hierarchy in the archive mailbox allows users to find messages easily.

To learn more about retention policies, retention tags, and the **Move to Archive** retention action, see [Retention tags and retention policies in Exchange Server](../../policy-and-compliance/mrm/retention-tags-and-retention-policies.md).

## Default MRM policy

Exchange Server Setup creates a default archive and retention policy named **Default MRM Policy**. This policy contains retention tags that have the **Move to Archive** action, as shown in the following table.

|**Retention tag name**|**Tag type**|**Description**|
|:-----|:-----|:-----|
|**Default 2 year move to archive**|Default (DPT)|Messages are automatically moved to the archive mailbox after two years. Applies to items in the entire mailbox that don't have a retention tag applied explicitly or inherited from the folder.|
|**Personal 1 year move to archive**|Personal|Messages are automatically moved to the archive mailbox after one year.|
|**Personal 5 year move to archive**|Personal|Messages are automatically moved to the archive mailbox after five years.|
|**Personal never move to archive**|Personal|Messages are never moved to the archive mailbox.|
|**Recoverable Items 14 days move to archive**|Recoverable Items Folder|Messages are moved from the Recoverable Items folder in the user's primary mailbox to the Recoverable Items folder in the archive mailbox. Users attempting to recover deleted items in their archive mailbox must use the Recover Deleted Items tool in the archive mailbox.|

If you enable an In-Place Archive for a mailbox user and the mailbox doesn't already have a retention policy assigned, the default archive and retention policy is automatically assigned. After the Managed Folder Assistant processes the mailbox, these tags become available to the user, who can then tag folders or messages to be moved to the archive mailbox. By default, email messages from the entire mailbox are moved to the archive after two years.

Before provisioning archive mailboxes for your users, we recommend that you inform them about the archive policies that will be applied to their mailbox and provide subsequent training or documentation to meet their needs. This should include details about the following:

- Functionality available within the archive, and the default archive retention policies.

- Information about when messages are automatically moved to the archive.

- Information about the folder hierarchy created in the archive mailbox.

- How to apply personal tags (displayed in the Archive policy menu in Outlook and Outlook on the web).

> [!NOTE]
> If you apply a retention policy to users who have an archive mailbox, the retention policy replaces the default MRM policy. You can create one or more retention tags with the **Move to Archive** action, and then link the tags to the retention policy. You can also add the default **Move to Archive** tags (which are created by Setup and linked to the Default MRM Policy) to any retention policies you create.

## Archive quotas

Archive mailboxes are designed so that users can store historical messaging data outside their primary mailbox. Often, users use .pst files due to low mailbox storage quotas and the restrictions imposed when these quotas are exceeded. For example, users can be prevented from sending messages when their mailbox size exceeds the *Prohibit send quota*. Similarly, users can be prevented from sending and receiving messages when their mailbox size exceeds the *Prohibit send and receive quota*.

To eliminate the need for .pst files, you can provide an archive mailbox with storage limits that meet the user's requirements. However, you may still want to retain some control of the storage quotas and growth of archive mailboxes to help monitor costs and expansion.

To help with this control, you can configure archive mailboxes with an *archive warning quota* and an *archive quota*. When an archive mailbox exceeds the specified archive warning quota, a warning event is logged in the Application event log. When an archive mailbox exceeds the specified archive quota, messages are no longer moved to the archive, a warning event is logged in the Application event log, and a quota message is sent to the mailbox user. By default, in Exchange Server, the archive warning quota is set to 90 GB and the archive quota is set to 100 GB.

The following table lists the events logged and warning messages sent when the archive warning quota and archive quota are met.

|**Quota**|**Event ID**|**Type**|**Source**|**Category**|**Message**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|Archive warning quota|10022|Warning|MSExchangeMailboxAssistants|Managed Folder Assistant|`The archive mailbox '<Display Name>:<GUID>:<Mailbox Database>:<Server FQDN>' exceeded the archive warning quota '<Archive warning quota>'. Archive mailbox size is '<Size>' bytes.`|
|Archive quota|8537|Warning|MSExchangeIS|General|`The archive mailbox for <Legacy DN> has exceeded the maximum archive mailbox size. You can't copy or move items into the archive mailbox. All message retention actions that move items to the archive mailbox will fail, and the primary mailbox may contain items with expired retention tags until the archive mailbox is within the maximum size limit. The mailbox owner should be notified about the condition of the archive mailbox.`|

## In-Place Archiving and other Exchange features

This section explains the functionality between In-Place Archiving and various Exchange features:

- **Exchange Search**: The ability to quickly search messages becomes even more critical with archive mailboxes. For Exchange Search, there's no difference between the primary and archive mailbox. Content in both mailboxes is indexed. Because the archive mailbox isn't cached on a user's computer (even when using Outlook in Cached Exchange Mode), search results for the archive are always provided by Exchange Search. When searching the entire mailbox in Outlook and later and Outlook on the web, search results include the users' primary and archive mailbox.

- **In-Place eDiscovery**: When a discovery manager performs an In-Place eDiscovery search, users' archive mailboxes are also searched. There's no option to exclude archive mailboxes when creating a discovery search from the Exchange admin center (EAC). When using the Exchange Management Shell to create a discovery search, you can exclude the archive by using the _DoNotIncludeArchive_ switch. For details, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch). To learn more, see [In-Place eDiscovery in Exchange Server](../../policy-and-compliance/ediscovery/ediscovery.md).

- **In-Place Hold and Litigation Hold**: When you put a mailbox on In-Place Hold or Litigation Hold, the hold is placed on both the primary and the archive mailbox. To learn more, see [In-Place Hold and Litigation Hold in Exchange Server](../../policy-and-compliance/holds/holds.md).

- **Recoverable Items folder**: The archive mailbox contains its own Recoverable Items folder and is subject to the same Recoverable Items folder quotas as the primary mailbox. To learn more about recoverable items, see [Recoverable Items folder in Exchange Server](../../policy-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

- **Archiving Skype for Business content in Exchange**: You can archive instant messaging conversations and shared online meeting documents in the user's primary mailbox. The mailbox must reside on an Exchange Mailbox server and you must have Skype for Business Server 2015 deployed in your organization.

## Managing archive mailboxes

In Exchange Server, creating and managing archive mailboxes is integrated with common mailbox management tasks. For step by step procedures, see [Manage In-Place Archives in Exchange Server](manage-archives.md).

- **Creating an archive mailbox**: You can enable an archive mailbox for an existing mailbox or you can create an archive mailbox when creating a new mailbox. .

- **Moving an archive mailbox**: You can move a user's archive mailbox to another mailbox database on the same Mailbox server or to another server, independent of the primary mailbox. To move a user's archive mailbox, you must create a mailbox move request. For details, see [Manage on-premises mailbox moves in Exchange Server](../../architecture/mailbox-servers/manage-mailbox-moves.md).


- **Disabling an archive mailbox**: You may want to disable a user's archive mailbox for troubleshooting purposes or if you're moving the primary mailbox to a version of Exchange that doesn't support In-Place Archiving. Disabling an archive is similar to disabling a primary mailbox. In on-premises deployments, a disabled archive mailbox is retained in the mailbox database until the deleted mailbox retention period for that database is reached. During this period, you can reconnect the same disabled archive mailbox to a user's primary mailbox. When the deleted mailbox retention period is reached, the disconnected archive mailbox is purged from the mailbox database.

- **Retrieving mailbox statistics and folder statistics**: You can retrieve mailbox statistics and mailbox folder statistics for a user's archive mailbox by using the _Archive_ switch with the [Get-MailboxStatistics](https://docs.microsoft.com/powershell/module/exchange/get-mailboxstatistics) and [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/get-mailboxfolderstatistics) cmdlets.

- **Test archive connectivity**: In Exchange Server, you can use the [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-archiveconnectivity) cmdlet to test connectivity to a specified user's on-premises or cloud-based archive mailbox.
