---
localization_priority: Normal
description: 'Summary: Learn about the specific logon types and user actions that can be audited when you enable mailbox audit logging for user mailboxes in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 29b67d58-eef9-4ad4-863f-562405ea8794
ms.date: 7/8/2018
ms.reviewer: 
title: Mailbox audit logging in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Mailbox audit logging in Exchange Server

Because mailboxes can contain sensitive, high business impact (HBI) information and personally identifiable information (PII), it's important that you track who logs on to the mailboxes in your organization and what actions are taken. It's especially important to track access to mailboxes by users other than the mailbox owner. These users are referred to as *delegate users*.

By using *mailbox audit logging*, you can log mailbox access by mailbox owners, delegates (including administrators with full access permissions to mailboxes), and administrators.

When you enable audit logging for a mailbox, you can specify which user actions (for example, accessing, moving, or deleting a message) will be logged for a logon type (administrator, delegate user, or owner). Audit log entries also include important information such as the client IP address, host name, and process or client used to access the mailbox. For items that are moved, the entry includes the name of the destination folder.

## Mailbox audit logs
<a name="mbxauditlogs"> </a>

Mailbox audit logs are generated for each mailbox that has mailbox audit logging enabled. Log entries are stored in the Recoverable Items folder in the audited mailbox, in the Audits subfolder. This ensures that all audit log entries are available from a single location, regardless of which client access method was used to access the mailbox or which server or computer an administrator uses to access the mailbox audit log. If you move a mailbox to another Mailbox server, the mailbox audit logs for that mailbox are also moved because they're located in the mailbox.

By default, mailbox audit log entries are retained in the mailbox for 90 days and then deleted. You can modify this retention period by using the _AuditLogAgeLimit_ parameter with the [Set-Mailbox](http://technet.microsoft.com/library/a0d413b9-d949-4df6-ba96-ac0906dedae2.aspx) cmdlet. If a mailbox is on In-Place Hold or Litigation Hold, audit log entries are only retained until the audit log retention period for the mailbox is reached. To retain audit log entries longer, you have to increase the retention period by changing the value for the _AuditLogAgeLimit_ parameter. You can also export audit log entries before the retention period is reached. For more information, see:

- [Export Mailbox Audit Logs](http://technet.microsoft.com/library/b458a95a-3321-4647-8884-cf97f8e7186a.aspx)

- [Create a Mailbox Audit Log Search](http://technet.microsoft.com/library/48ba22cf-b1f2-4dbc-98fc-fed22d97db14.aspx)

## Enabling mailbox audit logging
<a name="Enable"> </a>

Mailbox audit logging is enabled per mailbox. Use the **Set-Mailbox** cmdlet to enable or disable mailbox audit logging. For details, see [Enable or disable mailbox audit logging for a mailbox](enable-or-disable.md).

When you enable mailbox audit logging for a mailbox, access to the mailbox and certain administrator and delegate actions are logged by default. To log actions taken by the mailbox owner, you must specify which owner actions should be audited.

## Mailbox actions logged by mailbox audit logging
<a name="actions"> </a>

The following table lists the actions logged by mailbox audit logging, including the logon types for which the action can be logged. Note that an administrator who has been assigned the Full Access permission to a user's mailbox is considered a delegate user.

If you no longer require certain types of mailbox actions to be audited, you should modify the mailbox's audit logging configuration to disable those actions. Existing log entries aren't purged until the age limit for audit log entries is reached.

|**Action**|**Description**|**Admin**|**Delegate**|**Owner**|
|:-----|:-----|:-----|:-----|:-----|
|Copy|An item is copied to another folder.|Yes|No|No|
|Create|An item is created in the Calendar, Contacts, Notes, or Tasks folder in the mailbox; for example, a new meeting request is created. Note that message or folder creation isn't audited.|Yes<sup>1</sup>|Yes<sup>1</sup>|Yes|
|FolderBind|A mailbox folder is accessed.|Yes<sup>1</sup>|Yes<sup>2</sup>|No|
|HardDelete|An item is deleted permanently from the Recoverable Items folder.|Yes<sup>1</sup>|Yes<sup>1</sup>|Yes|
|MailboxLogin|The user signed in to their mailbox.|No|No|Yes<sup>3</sup>|
|MessageBind|An item is accessed in the reading pane or opened.|Yes|No|No|
|Move|An item is moved to another folder.|Yes<sup>1</sup>|Yes|Yes|
|MoveToDeletedItems|An item is moved to the Deleted Items folder.|Yes<sup>1</sup>|Yes|Yes|
|SendAs|A message is sent using Send As permissions.|Yes<sup>1</sup>|Yes<sup>1</sup>|No|
|SendOnBehalf|A message is sent using Send on Behalf permissions.|Yes<sup>1</sup>|Yes|No|
|SoftDelete|An item is deleted from the Deleted Items folder.|Yes<sup>1</sup>|Yes<sup>1</sup>|Yes|
|Update|An item's properties are updated.|Yes<sup>1</sup>|Yes<sup>1</sup>|Yes|

<sup>1</sup> Audited by default if auditing is enabled for a mailbox.

<sup>2</sup> Entries for folder bind actions performed by delegates are consolidated. One log entry is generated for individual folder access within a time span of 24 hours.

<sup>3</sup> Auditing for owner logins to a mailbox works only for POP3, IMAP4, or OAuth logins. It doesn't work for NTLM or Kerberos logins to the mailbox.

## Searching the mailbox audit log
<a name="Search"> </a>

You can use the following methods to search mailbox audit log entries:

- **Synchronously search a single mailbox**: You can use the [Search-MailboxAuditLog](http://technet.microsoft.com/library/2cc4598a-8a8c-45a0-901c-73e1079d193e.aspx) cmdlet to synchronously search mailbox audit log entries for a single mailbox. The cmdlet displays search results in the Exchange Management Shell window. For details, see [Search Mailbox Audit Log for a Mailbox](http://technet.microsoft.com/library/5b518a08-3b51-4ba3-bfbd-0e35cc5ff374.aspx).

- **Asynchronously search one or more mailboxes**: You can create a mailbox audit log search to asynchronously search mailbox audit logs for one or more mailboxes, and then have the search results sent to a specified email address. The search results are sent as an XML attachment. To create the search, use the [New-MailboxAuditLogSearch](http://technet.microsoft.com/library/95365cab-bbb2-4a64-8e8f-1c89fa9e0352.aspx) cmdlet. For details, see [Create a Mailbox Audit Log Search](http://technet.microsoft.com/library/48ba22cf-b1f2-4dbc-98fc-fed22d97db14.aspx).

- **Use auditing reports in the Exchange admin center (EAC)**: You can use the **Auditing** tab in the EAC to run a non-owner mailbox access report (contains entries for admin and delete actions) or export non-owner entries from the mailbox audit log. For details, see:

  - [Run a non-owner mailbox access report](../../policy-and-compliance/non-owner-mailbox-access-reports.md)

  - [Export Mailbox Audit Logs](http://technet.microsoft.com/library/b458a95a-3321-4647-8884-cf97f8e7186a.aspx)

## Mailbox audit log entries
<a name="Mailbox"> </a>

The following table describes the fields logged in a mailbox audit log entry.

|**Field**|**Populated with**|
|:-----|:-----|
|**Operation**|One of the following actions:  <br/> Copy  <br/> Create  <br/> FolderBind  <br/> HardDelete  <br/> MailboxLogin  <br/> MessageBind  <br/> Move  <br/> MoveToDeletedItems  <br/> SendAs  <br/> SendOnBehalf  <br/> SoftDelete  <br/> Update|
|**OperationResult**|One of the following results:  <br/> Failed  <br/> PartiallySucceeded  <br/> Succeeded|
|**LogonType**|Logon type of the user who performed the operation. Logon types include:  <br/> Owner  <br/> Delegate  <br/> Admin|
|**DestFolderId**|Destination folder GUID for move operations.|
|**DestFolderPathName**|Destination folder path for move operations.|
|**FolderId**|Folder GUID.|
|**FolderPathName**|Folder path.|
|**ClientInfoString**|Details that identify which client or Exchange component performed the operation.|
|**ClientIPAddress**|Client computer IP address.|
|**ClientMachineName**|Client computer name.|
|**ClientProcessName**|Name of the client application process.|
|**ClientVersion**|Client application version.|
|**InternalLogonType**|The type of internal user (a person in your organization) who performed the operation. The possible values for this field are the same ones as the **LogonType** field.|
|**MailboxOwnerUPN**|Mailbox owner user principal name (UPN).|
|**MailboxOwnerSid**|Mailbox owner security identifier (SID).|
|**DestMailboxOwnerUPN**|Destination mailbox owner UPN, logged for cross-mailbox operations.|
|**DestMailboxOwnerSid**|Destination mailbox owner SID, logged for cross-mailbox operations.|
|**DestMailboxOwnerGuid**|Destination mailbox owner GUID.|
|**CrossMailboxOperation**|Information about whether the operation logged is a cross-mailbox operation (for example, copying or moving messages between mailboxes).|
|**LogonUserDisplayName**|Display name of user who is logged on.|
|**DelegateUserDisplayName**|Delegate user display name.|
|**LogonUserSid**|SID of user who is logged on.|
|**SourceItems**|ItemID of mailbox items on which the logged action is performed (for example, move or delete). For operations performed on a number of items, this field is returned as a collection of items.|
|**SourceFolders**|Source folder GUID.|
|**ItemId**|Item ID.|
|**ItemSubject**|Item subject.|
|**MailboxGuid**|Mailbox GUID.|
|**MailboxResolvedOwnerName**|Mailbox user resolved name in the format _DOMAIN_\ _SamAccountName_.|
|**LastAccessed**|Time when the operation was performed.|
|**Identity**|Audit log entry ID.|

## More information
<a name="moreinfo"> </a>

- **Administrator access to mailboxes**: Mailboxes are considered to be accessed by an administrator only in the following scenarios:

  - In-Place eDiscovery is used to search a mailbox.

  - The [New-MailboxExportRequest](http://technet.microsoft.com/library/1625c25a-7cc9-459c-97ea-281ac421bbce.aspx) cmdlet is used to export a mailbox.

  - [Microsoft Exchange Server MAPI Editor](https://go.microsoft.com/fwlink/p/?linkId=204086) is used to access the mailbox.

- **Bypassing mailbox auditing logging**: Mailbox access by authorized automated processes such as accounts used by third-party tools or accounts used for lawful monitoring can create a large number of mailbox audit log entries and may not be of interest to your organization. You can configure such accounts to bypass mailbox audit logging. For details, see [Bypass a User Account From Mailbox Audit Logging](http://technet.microsoft.com/library/98a87071-fe31-4b67-beb8-a73799e54df2.aspx).

- **Logging mailbox owner actions**: For mailboxes such as the Discovery Search Mailbox, which may contain more sensitive information, consider enabling mailbox audit logging for mailbox owner actions such as message deletion.
