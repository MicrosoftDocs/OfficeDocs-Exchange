---
localization_priority: Normal
description: Admins can learn how to use the Exchange admin center (EAC) to export mailbox audit logs in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b458a95a-3321-4647-8884-cf97f8e7186a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Export mailbox audit logs
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Export mailbox audit logs in Exchange Online

When mailbox auditing is enabled for a mailbox, Exchange Online logs information in the mailbox audit log whenever a user other than the owner accesses the mailbox. Each log entry includes information about who accessed the mailbox and when, the actions performed by the non-owner, and whether the action was successful. Entries in the mailbox audit log are retained for 90 days by default. You can use the mailbox audit log to determine if a user other than the owner has accessed a mailbox.

When you export entries from mailbox audit logs, Exchange Online saves the entries in an XML file and attaches it to an email message sent to the specified recipients.

## Before you begin

- Estimated time to complete each procedure: varies. The mailbox audit log is sent within a few days after you export it.

- As of January 2019, mailbox audit logging on by default is enabled for all Exchange Online organizations. For more information, see [Manage mailbox auditing](https://docs.microsoft.com/office365/securitycompliance/enable-mailbox-auditing).

- If you're going to use Outlook on the web (formerly known as Outlook Web App) to view the exported entries, you need to enable .xml attachments in Outlook on the web. For details, see [Configure Outlook on the web to allow XML attachments](exchange-auditing-reports.md#configure-outlook-on-the-web-to-allow-xml-attachments).

- To find and open the Exchange admin center (EAC), see, [Exchange admin center in Exchange Online](../../exchange-admin-center.md).

- You need to be assigned permissions before you can perform this procedure. To see what permissions you need, see the "View reports" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Export the mailbox audit log

1. In the EAC, go to **Compliance Management** \> **Auditing**.

2. Click **Export mailbox audit logs**.

3. Configure the following search criteria for exporting the entries from the mailbox audit log:

   - **Start and end dates**: Select the date range for the entries to include in the exported file.

   - **Mailboxes to search audit log for**: Select the mailboxes to retrieve audit log entries for.

   - **Type of non-owner access**: Select one of the following options to define the type of non-owner access to retrieve entries for:

     - **All non-owners**: Search for access by admins and delegated users inside your organization, and by Microsoft datacenter administrators in Exchange Online.

     - **External users**: Search for access by Microsoft datacenter administrators.

     - **Administrators and delegated users**: Search for access by admins and delegated users inside your organization.

     - **Administrators**: Search for access by admins in your organization.

   - **Recipients**: Select the users to send the mailbox audit log to.

4. Click **Export**.

Exchange Online retrieves entries in the mailbox audit log that meet your search criteria, saves them to a file named SearchResult.xml, and then attaches the XML file to an email message sent to the recipients that you specified.

### How do you know this worked?

Sign in to the mailbox where the mailbox audit log was sent. If you've successfully exported the audit log, you'll receive a message sent from Exchange. It mmight take a few days to receive this message. The mailbox audit log (named SearchResult.xml) will be attached to this message. If you've correctly configured Outlook on the web to allow XML attachments, you can download the attached XML file.

## View the mailbox audit log

To save and view the SearchResult.xml file:

1. Sign in to the mailbox where the mailbox audit log was sent.

2. In the Inbox, open the message with the XML file attachment sent by Exchange Online. Notice that the body of the email message contains the search criteria.

3. Click the attachment and select to download the XML file.

4. Open the SearchResult.xml in Microsoft Excel.

## More information

### Entries in the mailbox audit log

The following example shows an entry from the mailbox audit log contained in the SearchResult.xml file. Each entry is preceded by the **\<Event\>** XML tag and ends with the **\</Event\>** XML tag. This entry shows that the admin purged the message with the subject, "Notification of litigation hold" from the Recoverable Items folder in David's mailbox on April 30, 2010.

```XML
<Event MailboxGuid="6d4fbdae-e3ae-4530-8d0b-f62a14687939"
  Owner="PPLNSL-dom\david50001-1363917750"
  LastAccessed="2010-04-30T11:01:55.140625-07:00"
  Operation="HardDelete"
  OperationResult="Succeeded"
  LogonType="Admin"
 FolderId="0000000073098C3277988F4CB882F5B82EBF64610100A7C317F68C24304BBD18ABE1F185E79B00000026BD4F0000"
  FolderPathName="\Recoverable Items\Deletions"
  ClientInfoString="Client=OWA;Action=ViaProxy"
  ClientIPAddress="10.196.241.168"
  InternalLogonType="Owner"
  MailboxOwnerUPN="david@contoso.com"
  MailboxOwnerSid="S-1-5-21-290112810-296651436-1966561949-1151"
  CrossMailboxOperation="false"
  LogonUserDN="Administraor"
  LogonUserSid="S-1-5-21-290112810-296651436-1966561949-1149">
<SourceItems>
<ItemId="0000000073098C3277988F4CB882F5B82EBF64610700A7C317F68C24304BBD18ABE1F185E79B00000026BD4F0000A7C317F68C24304BBD18ABE1F185E79B00000026BD540"
   Subject="Notification of litigation hold"
   FolderPathName="\Recoverable Items\Deletions" />
</SourceItems>
</Event>
```

### Useful fields in the mailbox audit log

Here's a description of useful fields in the mailbox audit log. They can help you identify specific information about each instance of non-owner access of a mailbox.

|**Field**|**Description**|
|:-----|:-----|
|Owner|The owner of the mailbox that was accessed by a non-owner.|
|LastAccessed|The date and time when the mailbox was accessed.|
|Operation|The action that was performed by the non-owner. For more information, see the "What gets logged in the mailbox audit log?" section in [Run a non-owner mailbox access report in Exchange Online](non-owner-mailbox-access-report.md).|
|OperationResult|Whether the action performed by the non-owner succeeded or failed.|
|LogonType|The type of non-owner access. These include admin, delegate, and external.|
|FolderPathName|The name of the folder that contained the message that was affected by the non-owner.|
|ClientInfoString|Information about the mail client used by the non-owner to access the mailbox.|
|ClientIPAddress|The IP address of the computer used by the non-owner to access the mailbox.|
|InternalLogonType|The logon type of the account used by the non-owner to access this mailbox.|
|MailboxOwnerUPN|The email address of the mailbox owner.|
|LogonUserDN|The display name of the non-owner.|
|Subject|The subject line of the email message that was affected by the non-owner.|
