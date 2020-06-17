---
title: 'In-Place Archiving in Exchange 2013: Exchange 2013 Help'
TOCTitle: In-Place Archiving in Exchange 2013
ms:assetid: b5e4c0e9-0558-4b90-bc12-f67adbfb59ac
ms:mtpsurl: https://technet.microsoft.com/library/Dd979800(v=EXCHG.150)
ms:contentKeyID: 48385465
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# In-Place Archiving in Exchange 2013

_**Applies to:** Exchange Server 2013_

*In-Place Archiving* helps you regain control of your organization's messaging data by eliminating the need for personal store (.pst) files and allowing users to store messages in an *archive mailbox* accessible in Microsoft Outlook 2010 and later and Microsoft Office Outlook Web App.

In Microsoft Exchange Server 2013, In-Place Archiving provides users with an alternate storage location in which to store historical messaging data. An In-Place Archive is an additional mailbox (called an archive mailbox) enabled for a mailbox user. Outlook 2007 and later and Outlook Web App users have seamless access to their archive mailbox. Using either of these client applications, users can view an archive mailbox and move or copy messages between their primary mailbox and the archive. In-Place Archiving presents a consistent view of messaging data to users and eliminates the user overhead required to manage .pst files.

You can provision a user's archive on the same mailbox database as the user's primary mailbox, another mailbox database on the same Mailbox server, or a mailbox database on another Mailbox server in the same Active Directory site. In hybrid deployments of Exchange 2010 and later, you can also provision a cloud-based archive for mailboxes located on your on-premises Mailbox servers.

## Messaging data and .pst files

Outlook uses .pst files to store data locally on users' computers or network shares. Unlike offline store (.ost) files (which are used by Outlook in Cached Exchange Mode to store a copy of the mailbox for offline access), .pst files aren't synchronized with the user's Exchange mailbox. If a user moves messages to a .pst file, those messages are removed from the mailbox.

Using .pst files to manage messaging data can result in the following issues:

- **Unmanaged files**: Generally, .pst files are created by users and reside on their computers or network shares. They aren't managed by your organization. As a result, users can create several .pst files containing the same or different messages and store them in different locations, with no organizational control.

- **Increased discovery costs**: Lawsuits and some business or regulatory requirements sometimes result in discovery requests. Locating messaging data that resides in .pst files on users' computers can be a costly manual effort. Because tracking unmanaged .pst files can be difficult, .pst data may be undiscoverable in many cases. This could possibly expose your organization to legal and financial risks.

- **Inability to apply messaging retention policies**: Messaging retention policies can't be applied to messages located in .pst files. As a result, depending on business or applicable regulations, your organization may not be in compliance.

- **Risk of data theft**: Messaging data stored in .pst files is vulnerable to data theft. For example, .pst files are often stored in portable devices such as laptops, removable hard drives, and portable media such as USB drives, CDs, and DVDs.

- **Fragmented view of messaging data**: Users who store information in .pst files don't get a uniform view of their data. Messages stored in .pst files are generally available only on the computer where the .pst file resides. As a result, if users access their mailboxes using Outlook Web App or Outlook on another computer, the messages stored in their .pst files are inaccessible.

## Client access to archive mailboxes

The following table lists the client applications that can be used to access archive mailboxes.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Client</th>
<th>Access to archive mailbox</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2013, Outlook 2010, Outlook 2007, and Outlook Web App</p></td>
<td><p>Yes. Outlook 2013, Outlook 2010, Outlook 2007 and Outlook Web App users can copy or move items from their primary mailbox to their archive mailbox, and can also use retention policies to move items to the archive.</p>

> [!NOTE]
> Outlook 2010 and later and Outlook 2007 users can also copy or move items from .pst files to their archive mailbox. Outlook 2007 users require the Office 2007 Cumulative Update for February 2011. Some differences in archive support exist between Outlook 2010 and later and Outlook 2007. For more information, see Exchange Team Blog article, see <A href="https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Yes-Virginia-there-is-Exchange-2010-archive-support-in-Outlook/ba-p/588351">Yes Virginia, there is Exchange 2010 archive support in Outlook 2007</A>.

</td>
</tr>
<tr class="even">
<td><p>Exchange ActiveSync</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

> [!NOTE]
> In-Place Archiving is a premium feature and requires an Exchange Enterprise client access license (CAL). For details about how to license Exchange, see <A href="https://go.microsoft.com/fwlink/?linkid=237292">Exchange Server Licensing</A>. For details about the versions of Outlook required to access an archive mailbox, see <A href="https://support.microsoft.com/office/46b6b7c5-c3ca-43e5-8424-1e2807917c99">License requirements for Personal Archive and retention policies</A>.

Outlook doesn't create a local copy of the archive mailbox on a user's computer, even if it's configured to use Cached Exchange Mode. Users can access an archive mailbox in online mode only.

## Delegate access

*Delegate access* is when a user or set of users is provided access to another user's mailbox. There are several scenarios for providing delegate access, including:

- Providing one or more users with access to the mailbox of a user who is no longer employed by the organization. In this case, users who may be given delegate access include the departed user's manager or supervisor or another user who will assume the departed user's responsibilities.

- Providing one or more users with access to a shared mailbox.

- Providing executive assistants with access to the mailboxes of the executives they're assisting.

In Exchange 2013, when you assign Full Access permissions to a mailbox, the delegate to which you assign the permissions can also access the user's archive. Delegates must use Outlook to access the mailbox, and they must connect to an Exchange 2010 SP1 or later Client Access server for Autodiscover purposes. Autodiscover is an Exchange service that provides configuration settings to automatically configure Outlook clients. When delegates use Outlook to access an Exchange 2013 mailbox, both the primary mailbox and the archive to which they have access are visible from Outlook.

## Moving messages to the archive mailbox

There are several ways to move messages to archive mailboxes:

- **Move or copy messages manually**: Mailbox users can manually move or copy messages from their primary mailbox or a .pst file to their archive mailbox. The archive mailbox appears as another mailbox or .pst file in Outlook and Outlook Web App.

- **Move or copy messages using Inbox rules**: Mailbox users can create Inbox rules in Outlook or Outlook Web App to automatically move messages to a folder in their archive mailbox. To learn more, see [Manage email messages by using rules](https://support.office.com/article/c24f5dea-9465-4df4-ad17-a50704d66c59).

- **Move messages using retention policies**: You can use retention policies to automatically move messages to the archive. Users can also apply a personal tag to move messages to the archive. For details about archive and retention policies, see Archive and retention policies later in this topic.

    > [!NOTE]
    > Personal tags are available only in Outlook Web App and Outlook 2010 and later.

- **Import messages from .pst files**: In Exchange 2013, you can use a mailbox import request to import messages from a .pst file to a user's archive or primary mailbox. For details, see [Mailbox import and export requests](mailbox-import-and-export-requests-exchange-2013-help.md). You can also use the PST Capture tool to search for .pst files on computers in your organization and import .pst file data to users' archives.

## Archive and retention policies

In Exchange 2013, you can apply archive policies to a mailbox to automatically move messages from a user's primary mailbox to the archive mailbox after a specified period. Archive policies are implemented by creating retention tags that use the **Move to Archive** retention action.

Messages are moved to a folder in the archive mailbox that has the same name as the source folder in the primary mailbox. If a folder with the same name doesn't exist in the archive mailbox, it's created when the Managed Folder Assistant moves a message. Re-creating the same folder hierarchy in the archive mailbox allows users to find messages easily.

To learn more about retention policies, retention tags, and the **Move to Archive** retention action, see [Retention tags and retention policies](https://docs.microsoft.com/exchange/security-and-compliance/messaging-records-management/retention-tags-and-policies).

## Default MRM policy

Exchange 2013 Setup creates a default archive and retention policy named **Default MRM Policy**. This policy contains retention tags that have the **Move to Archive** action, as shown in the following table.

> [!NOTE]
> In Exchange 2010, the default archive and retention policy created by Exchange Setup is named <STRONG>Default Archive and Retention Policy</STRONG>.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Retention tag name</th>
<th>Tag type</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Default 2 year move to archive</strong></p></td>
<td><p>Default (DPT)</p></td>
<td><p>Messages are automatically moved to the archive mailbox after two years. Applies to items in the entire mailbox that don't have a retention tag applied explicitly or inherited from the folder.</p></td>
</tr>
<tr class="even">
<td><p><strong>Personal 1 year move to archive</strong></p></td>
<td><p>Personal</p></td>
<td><p>Messages are automatically moved to the archive mailbox after one year.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Personal 5 year move to archive</strong></p></td>
<td><p>Personal</p></td>
<td><p>Messages are automatically moved to the archive mailbox after five years.</p></td>
</tr>
<tr class="even">
<td><p><strong>Personal never move to archive</strong></p></td>
<td><p>Personal</p></td>
<td><p>Messages are never moved to the archive mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Recoverable Items 14 days move to archive</strong></p></td>
<td><p>Recoverable Items Folder</p></td>
<td><p>Messages are moved from the Recoverable Items folder of the user's primary mailbox to the Recoverable Items folder of the archive mailbox. Users attempting to recover deleted items in the archive must use the Recover Deleted Items feature on the archive mailbox.</p></td>
</tr>
</tbody>
</table>

If you enable an In-Place Archive for a mailbox user and the mailbox doesn't already have a retention policy assigned, the default archive and retention policy is automatically assigned. After the Managed Folder Assistant processes the mailbox, these tags become available to the user, who can then tag folders or messages to be moved to the archive mailbox. By default, email messages from the entire mailbox are moved after two years.

Before provisioning archive mailboxes for your users, we recommend that you inform them about the archive policies that will be applied to their mailbox and provide subsequent training or documentation to meet their needs. This should include details about the following:

- Functionality available within the archive, the default archive, and retention policies.

- Information about when messages may be moved automatically to the archive.

- Information about the folder hierarchy created in the archive mailbox.

- How to apply personal tags (displayed in the Archive policy menu in Outlook and Outlook Web App).

> [!NOTE]
> If you apply a retention policy to users who have an archive mailbox, the retention policy replaces the default MRM policy. You can create one or more retention tags with the <STRONG>Move to Archive</STRONG> action, and then link the tags to the retention policy. You can also add the default <STRONG>Move to Archive</STRONG> tags (which are created by Setup and linked to the Default MRM Policy) to any retention policies you create.

For information about compliance and archiving in Outlook 2010 and later, see [Plan for compliance and archiving in Outlook 2010](https://go.microsoft.com/fwlink/?linkid=210902).

## Archive quotas

Archive mailboxes are designed so that users can store historical messaging data outside their primary mailbox. Often, users use .pst files due to low mailbox storage quotas and the restrictions imposed when these quotas are exceeded. For example, users can be prevented from sending messages when their mailbox size exceeds the *Prohibit send quota*. Similarly, users can be prevented from sending and receiving messages when their mailbox size exceeds the *Prohibit send and receive quota*.

To eliminate the need for .pst files, you can provide an archive mailbox with storage limits that meet the user's requirements. However, you may still want to retain some control of the storage quotas and growth of archive mailboxes to help monitor costs and expansion.

To help with this control, you can configure archive mailboxes with an *archive warning quota* and an *archive quota*. When an archive mailbox exceeds the specified archive warning quota, a warning event is logged in the Application event log. When an archive mailbox exceeds the specified archive quota, messages are no longer moved to the archive, a warning event is logged in the Application event log, and a quota message is sent to the mailbox user. By default, in Exchange 2013, the archive warning quota is set to 45 gigabytes (GB) and the archive quota is set to 50 GB.

The following table lists the events logged and warning messages sent when the archive warning quota and archive quota are met.

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Quota</th>
<th>Event ID</th>
<th>Type</th>
<th>Source</th>
<th>Category</th>
<th>Message</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Archive warning quota</p></td>
<td><p>10022</p></td>
<td><p>Warning</p></td>
<td><p>MSExchangeMailboxAssistants</p></td>
<td><p>Managed Folder Assistant</p></td>
<td><p>The archive mailbox '&lt;Display Name&gt;:&lt;GUID&gt;:&lt;Mailbox Database&gt;:&lt;Server FQDN&gt;' exceeded the archive warning quota '&lt;Archive warning quota&gt;'. Archive mailbox size is '&lt;Size&gt;' bytes.</p></td>
</tr>
<tr class="even">
<td><p>Archive quota</p></td>
<td><p>8537</p></td>
<td><p>Warning</p></td>
<td><p>MSExchangeIS</p></td>
<td><p>General</p></td>
<td><p>The archive mailbox for &lt;Legacy DN&gt; has exceeded the maximum archive mailbox size. You can't copy or move items into the archive mailbox. All message retention actions that move items to the archive mailbox will fail, and the primary mailbox may contain items with expired retention tags until the archive mailbox is within the maximum size limit. The mailbox owner should be notified about the condition of the archive mailbox.</p></td>
</tr>
</tbody>
</table>

## In-Place Archives and other Exchange features

This section explains the functionality between In-Place Archives and various Exchange features:

- **Exchange Search**: The ability to quickly search messages becomes even more critical with archive mailboxes. For Exchange Search, there's no difference between the primary and archive mailbox. Content in both mailboxes is indexed. Because the archive mailbox isn't cached on a user's computer (even when using Outlook in Cached Exchange Mode), search results for the archive are always provided by Exchange Search. When searching the entire mailbox in Outlook 2010 and later and Outlook Web App, search results include the users' primary and archive mailbox.

- **In-Place eDiscovery**: When a discovery manager performs an In-Place eDiscovery search, users' archive mailboxes are also searched. There's no option to exclude archive mailboxes when creating a discovery search from the Exchange admin center (EAC). When using the Exchange Management Shell to create a discovery search, you can exclude the archive by using the *DoNotIncludeArchive* switch. For details, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/New-MailboxSearch). To learn more, see [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery).

    > [!IMPORTANT]
    > You can't use In-Place eDiscovery to search a disconnected mailbox.

- **In-Place Hold and litigation hold**: When you put a mailbox on In-Place Hold or litigation hold, the hold is placed on both the primary and the archive mailbox. To learn more about In-Place Hold and litigation hold, see [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds).

- **Recoverable Items folder**: The archive mailbox contains its own Recoverable Items folder and is subject to the same Recoverable Items folder quotas as the primary mailbox. To learn more about recoverable items, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

- **Archiving Lync content in Exchange**: You can archive instant messaging conversations and shared online meeting documents in the user's primary mailbox. The mailbox must reside on an Exchange 2013 Mailbox server and you must have Microsoft Lync 2013 deployed in your organization. For details, see [Integration with SharePoint and Lync](integration-with-sharepoint-and-lync-exchange-2013-help.md).

## Managing archive mailboxes

In Exchange 2013, creating and managing archive mailboxes is integrated with common mailbox management tasks, including:

- **Creating an archive mailbox**: You can create an archive mailbox when creating a mailbox, or you can enable an archive mailbox for an existing mailbox. For details, see [Manage In-Place Archives in Exchange 2013](manage-in-place-archives-in-exchange-2013-exchange-2013-help.md).

- **Moving an archive mailbox**: You can move a user's archive mailbox to another mailbox database on the same Mailbox server or to another server, independent of the primary mailbox. To move a user's archive mailbox, you must create a mailbox move request. For details, see [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).

    > [!IMPORTANT]
    > Locating a user's mailbox and archive on different versions of Exchange Server is not supported.

- **Disabling an archive mailbox**: You may want to disable a user's archive mailbox for troubleshooting purposes or if you're moving the primary mailbox to a version of Exchange that doesn't support In-Place Archiving. Disabling an archive is similar to disabling a primary mailbox. For details, see [Manage In-Place Archives in Exchange 2013](manage-in-place-archives-in-exchange-2013-exchange-2013-help.md). In on-premises deployments, a disabled archive mailbox is retained in the mailbox database until the deleted mailbox retention period for that database is reached. During this period, you can reconnect the archive to a mailbox user. When the deleted mailbox retention period is reached, the disconnected archive mailbox is purged from the mailbox database.

- **Retrieving mailbox statistics and folder statistics**: You can retrieve mailbox statistics and mailbox folder statistics for a user's archive mailbox by using the *Archive* switch with the [Get-MailboxStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxStatistics) and [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxFolderStatistics) cmdlets.

- **Test archive connectivity**: In Exchange 2013, you can use the [Test-ArchiveConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-ArchiveConnectivity) cmdlet to test connectivity to a specified user's on-premises or cloud-based archive.
