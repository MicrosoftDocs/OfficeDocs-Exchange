---
title: 'Mailbox audit logging: Exchange 2013 Help'
TOCTitle: Mailbox audit logging
ms:assetid: 29b67d58-eef9-4ad4-863f-562405ea8794
ms:mtpsurl: https://technet.microsoft.com/library/Ff459237(v=EXCHG.150)
ms:contentKeyID: 49300455
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mailbox audit logging

_**Applies to:** Exchange Server 2013_

Because mailboxes can contain sensitive, high business impact (HBI) information and personally identifiable information (PII), it's important that you track who logs on to the mailboxes in your organization and what actions are taken. It's especially important to track access to mailboxes by users other than the mailbox owner. These users are referred to as *delegate users*.

By using *mailbox audit logging*, you can log mailbox access by mailbox owners, delegates (including administrators with full access permissions to mailboxes), and administrators.

When you enable audit logging for a mailbox, you can specify which user actions (for example, accessing, moving, or deleting a message) will be logged for a logon type (administrator, delegate user, or owner). Audit log entries also include important information such as the client IP address, host name, and process or client used to access the mailbox. For items that are moved, the entry includes the name of the destination folder.

## Mailbox audit logs

Mailbox audit logs are generated for each mailbox that has mailbox audit logging enabled. Log entries are stored in the Recoverable Items folder in the audited mailbox, in the Audits subfolder. This ensures that all audit log entries are available from a single location, regardless of which client access method was used to access the mailbox or which server or workstation an administrator used to access the mailbox audit log. If you move a mailbox to another Mailbox server, the mailbox audit logs for that mailbox are also moved because they're located in the mailbox.

By default, mailbox audit log entries are retained in the mailbox for 90 days and then deleted. You can modify this retention period by using the *AuditLogAgeLimit* parameter with the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet. If a mailbox is on In-Place Hold or Litigation Hold, audit log entries are only retained until the audit log retention period for the mailbox is reached. To retain audit log entries longer, you have to increase the retention period by changing the value for the *AuditLogAgeLimit* parameter. You can also export audit log entries before the retention period is reached. For more information, see:

  - [Export mailbox audit logs](https://docs.microsoft.com/exchange/security-and-compliance/exchange-auditing-reports/export-mailbox-audit-logs)

  - [Create a mailbox audit log search](create-a-mailbox-audit-log-search-exchange-2013-help.md)

## Enabling mailbox audit logging

Mailbox audit logging is enabled per mailbox. Use the **Set-Mailbox** cmdlet to enable or disable mailbox audit logging. For details, see [Enable or disable mailbox audit logging for a mailbox](enable-or-disable-mailbox-audit-logging-for-a-mailbox-exchange-2013-help.md).

When you enable mailbox audit logging for a mailbox, access to the mailbox and certain administrator and delegate actions are logged by default. To log actions taken by the mailbox owner, you must specify which owner actions should be audited.

## Mailbox actions logged by mailbox audit logging

The following table lists the actions logged by mailbox audit logging, including the logon types for which the action can be logged.

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
<th>Action</th>
<th>Description</th>
<th>Administrator</th>
<th>Delegate</th>
<th>Owner</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Copy</p></td>
<td><p>An item is copied to another folder.</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Create</p></td>
<td><p>An item is created in the Calendar, Contacts, Notes, or Tasks folder in the mailbox; for example, a new meeting request is created. Note that message or folder creation isn't audited.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>FolderBind</p></td>
<td><p>A mailbox folder is accessed.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes**</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>HardDelete</p></td>
<td><p>An item is deleted permanently from the Recoverable Items folder.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>MessageBind</p></td>
<td><p>An item is accessed in the reading pane or opened.</p></td>
<td><p>Yes</p></td>
<td><p>No</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>Move</p></td>
<td><p>An item is moved to another folder.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>MoveToDeletedItems</p></td>
<td><p>An item is moved to the Deleted Items folder.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>SendAs</p></td>
<td><p>A message is sent using Send As permissions.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes*</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>SendOnBehalf</p></td>
<td><p>A message is sent using Send on Behalf permissions.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>SoftDelete</p></td>
<td><p>An item is deleted from the Deleted Items folder.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>Update</p></td>
<td><p>An item's properties are updated.</p></td>
<td><p>Yes*</p></td>
<td><p>Yes*</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

\* Audited by default if auditing is enabled for a mailbox.

\*\* Entries for folder bind actions performed by delegates are consolidated. One log entry is generated for individual folder access within a time span of 24 hours.

If you no longer require certain types of mailbox actions to be audited, you should modify the mailbox's audit logging configuration to disable those actions. Existing log entries aren't purged until the age limit for audit log entries is reached.

## Searching mailbox audit log entries

You can use the following methods to search mailbox audit log entries:

- **Synchronously search a single mailbox**: You can use the [Search-MailboxAuditLog](https://docs.microsoft.com/powershell/module/exchange/Search-MailboxAuditLog) cmdlet to synchronously search mailbox audit log entries for a single mailbox. The cmdlet displays search results in the Exchange Management Shell window. For details, see [Search the mailbox audit log for a mailbox](search-the-mailbox-audit-log-for-a-mailbox-exchange-2013-help.md).

- **Asynchronously search one or more mailboxes**: You can create a mailbox audit log search to asynchronously search mailbox audit logs for one or more mailboxes, and then have the search results sent to a specified email address. The search results are sent as an XML attachment. To create the search, use the [New-MailboxAuditLogSearch](https://docs.microsoft.com/powershell/module/exchange/New-MailboxAuditLogSearch) cmdlet. For details, see [Create a mailbox audit log search](create-a-mailbox-audit-log-search-exchange-2013-help.md).

- **Use auditing reports in the Exchange admin center (EAC)**: You can use the **Auditing** tab in the EAC to run a non-owner mailbox access report or export entries from the mailbox audit log. For details, see:

  - [Run a non-owner mailbox access report](https://docs.microsoft.com/exchange/security-and-compliance/exchange-auditing-reports/non-owner-mailbox-access-report)

  - [Export mailbox audit logs](https://docs.microsoft.com/exchange/security-and-compliance/exchange-auditing-reports/export-mailbox-audit-logs)

## Mailbox audit log entries

The following table describes the fields logged in a mailbox audit log entry.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Field</th>
<th>Populated with</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Operation</strong></p></td>
<td><p>One of the following actions:</p>
<ul>
<li><p>Copy</p></li>
<li><p>Create</p></li>
<li><p>FolderBind</p></li>
<li><p>HardDelete</p></li>
<li><p>MessageBind</p></li>
<li><p>Move</p></li>
<li><p>MoveToDeletedItems</p></li>
<li><p>SendAs</p></li>
<li><p>SendOnBehalf</p></li>
<li><p>SoftDelete</p></li>
<li><p>Update</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>OperationResult</strong></p></td>
<td><p>One of the following results:</p>
<ul>
<li><p>Failed</p></li>
<li><p>PartiallySucceeded</p></li>
<li><p>Succeeded</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><strong>LogonType</strong></p></td>
<td><p>Logon type of the user who performed the operation. Logon types include:</p>
<ul>
<li><p>Owner</p></li>
<li><p>Delegate</p></li>
<li><p>Admin</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>DestFolderId</strong></p></td>
<td><p>Destination folder GUID for move operations.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DestFolderPathName</strong></p></td>
<td><p>Destination folder path for move operations.</p></td>
</tr>
<tr class="even">
<td><p><strong>FolderId</strong></p></td>
<td><p>Folder GUID.</p></td>
</tr>
<tr class="odd">
<td><p><strong>FolderPathName</strong></p></td>
<td><p>Folder path.</p></td>
</tr>
<tr class="even">
<td><p><strong>ClientInfoString</strong></p></td>
<td><p>Details that identify which client or Exchange component performed the operation.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ClientIPAddress</strong></p></td>
<td><p>Client computer IP address.</p></td>
</tr>
<tr class="even">
<td><p><strong>ClientMachineName</strong></p></td>
<td><p>Client computer name.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ClientProcessName</strong></p></td>
<td><p>Name of the client application process.</p></td>
</tr>
<tr class="even">
<td><p><strong>ClientVersion</strong></p></td>
<td><p>Client application version.</p></td>
</tr>
<tr class="odd">
<td><p><strong>InternalLogonType</strong></p></td>
<td><p>Logon type of the user who performed the operation. Logon types include:</p>
<ul>
<li><p>Owner</p></li>
<li><p>Delegate</p></li>
<li><p>Admin</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>MailboxOwnerUPN</strong></p></td>
<td><p>Mailbox owner user principal name (UPN).</p></td>
</tr>
<tr class="odd">
<td><p><strong>MailboxOwnerSid</strong></p></td>
<td><p>Mailbox owner security identifier (SID).</p></td>
</tr>
<tr class="even">
<td><p><strong>DestMailboxOwnerUPN</strong></p></td>
<td><p>Destination mailbox owner UPN, logged for cross-mailbox operations.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DestMailboxOwnerSid</strong></p></td>
<td><p>Destination mailbox owner SID, logged for cross-mailbox operations.</p></td>
</tr>
<tr class="even">
<td><p><strong>DestMailboxOwnerGuid</strong></p></td>
<td><p>Destination mailbox owner GUID.</p></td>
</tr>
<tr class="odd">
<td><p><strong>CrossMailboxOperation</strong></p></td>
<td><p>Information about whether the operation logged is a cross-mailbox operation (for example, copying or moving messages between mailboxes).</p></td>
</tr>
<tr class="even">
<td><p><strong>LogonUserDisplayName</strong></p></td>
<td><p>Display name of user who is logged on.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DelegateUserDisplayName</strong></p></td>
<td><p>Delegate user display name.</p></td>
</tr>
<tr class="even">
<td><p><strong>LogonUserSid</strong></p></td>
<td><p>SID of user who is logged on.</p></td>
</tr>
<tr class="odd">
<td><p><strong>SourceItems</strong></p></td>
<td><p>ItemID of mailbox items on which the logged action is performed (for example, move or delete). For operations performed on a number of items, this field is returned as a collection of items.</p></td>
</tr>
<tr class="even">
<td><p><strong>SourceFolders</strong></p></td>
<td><p>Source folder GUID.</p></td>
</tr>
<tr class="odd">
<td><p><strong>ItemId</strong></p></td>
<td><p>Item ID.</p></td>
</tr>
<tr class="even">
<td><p><strong>ItemSubject</strong></p></td>
<td><p>Item subject.</p></td>
</tr>
<tr class="odd">
<td><p><strong>MailboxGuid</strong></p></td>
<td><p>Mailbox GUID.</p></td>
</tr>
<tr class="even">
<td><p><strong>MailboxResolvedOwnerName</strong></p></td>
<td><p>Mailbox user resolved name in the format <em>DOMAIN</em>\<em>SamAccountName</em>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>LastAccessed</strong></p></td>
<td><p>Time when the operation was performed.</p></td>
</tr>
<tr class="even">
<td><p><strong>Identity</strong></p></td>
<td><p>Audit log entry ID.</p></td>
</tr>
</tbody>
</table>

## More information

  - **Administrator access to mailboxes**: Mailboxes are considered to be accessed by an administrator only in the following scenarios:

      - [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery) is used to search a mailbox.

      - The [New-MailboxExportRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxExportRequest) cmdlet is used to export a mailbox.

      - [Microsoft Exchange Server MAPI Client and Collaboration Data Objects](https://www.microsoft.com/download/details.aspx?id=39045) is used to access the mailbox.

  - **Bypassing mailbox auditing logging**: Mailbox access by authorized automated processes such as accounts used by third-party tools or accounts used for lawful monitoring can create a large number of mailbox audit log entries and may not be of interest to your organization. You can configure such accounts to bypass mailbox audit logging. For details, see [Bypass a user account from mailbox audit logging](bypass-a-user-account-from-mailbox-audit-logging-exchange-2013-help.md).

  - **Logging mailbox owner actions**: For mailboxes such as the Discovery Search Mailbox, which may contain more sensitive information, consider enabling mailbox audit logging for mailbox owner actions such as message deletion.
