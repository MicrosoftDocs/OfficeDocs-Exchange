---
title: 'Message tracking: Exchange 2013 Help'
TOCTitle: Message tracking
ms:assetid: bada2ea7-6d7c-4630-b7f1-67f56818f0ff
ms:mtpsurl: https://technet.microsoft.com/library/Bb124375(v=EXCHG.150)
ms:contentKeyID: 50646522
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Message tracking

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, the message tracking log is a detailed record of all message activity as messages are transferred to and from the Transport service on Mailbox servers, mailboxes on Mailbox servers, and Edge Transport servers. You can use message tracking logs for message forensics, mail flow analysis, reporting, and troubleshooting.

In Exchange 2013, you can use the **Set-TransportService** cmdlet or the **Set-MailboxServer** cmdlet for all message tracking configuration tasks, because the Exchange 2013 Mailbox server holds the Transport service and the mailboxes. You can use either of these cmdlets to make the following message tracking configuration changes:

- Enable or disable message tracking. The default is enabled.

- Specify the location of the message tracking log files.

- Specify a maximum size for the individual message tracking log files. The default is 10 MB.

- Specify a maximum size for the directory that contains the message tracking log files: The default is 1000 MB.

- Specify maximum age for the message tracking log files: The default is 30 days.

- Enable or disable message subject logging in the message tracking logs. The default is enabled.

> [!NOTE]
> You can also use the Exchange admin center (EAC) to enable or disable message tracking, and to specify the location of the message tracking log files.

By default, Exchange uses circular logging to limit the message tracking logs based on file size and file age to help control the hard disk space used by the message tracking log files.

## Search the message tracking log

Message tracking logs contain vast amounts of data as messages move through an Exchange 2013 Mailbox server. When it comes to searching the message tracking logs, you have different options.

- **Get-MessageTrackingLog**: Administrators can use this cmdlet to search the message tracking log for information about messages using a wide range of filter criteria. For more information, see [Search message tracking logs](search-message-tracking-logs-exchange-2013-help.md).

- **Delivery reports for administrators**: Administrators can use the **Delivery reports** tab in the Exchange admin center (EAC) or the underlying **Search-MessageTrackingReport** and **Get-MesageTrackingReport** cmdlets to search the message tracking logs for information about messages sent by or received by a specific mailbox in the organization. For more information see [Delivery reports for administrators](delivery-reports-for-administrators-exchange-2013-help.md).

## Structure of the message tracking log files

By default, the message tracking log files exist in %ExchangeInstallPath%TransportRoles\\Logs\\MessageTracking.

The naming convention for log files in the message tracking log directory is `MSGTRK`*yyyymmdd-nnnn*`.log`, `MSGTRKMA`*yyyymmdd-nnnn*`.log`, `MSGTRKMD`*yyyymmdd-nnnn*`.log`, and `MSGTRKMS`*yyyymmdd-nnnn*`.log` . The different logs are used by the following services:

- **MSGTRK**: These logs are associated with the Transport service.

- **MSGTRKMA**: These logs are associated with the approvals and rejections used by moderated transport. For more information, see [Manage message approval](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/manage-message-approval).

- **MSGTRKMD**: These logs are associated with messages delivered to mailboxes by the Mailbox Transport Delivery service.

- **MSGTRKMS**: These logs are associated with messages sent from mailboxes by the Mailbox Transport Submission service.

The placeholders in the log file names represent the following information:

- The placeholder *yyyymmdd* is the coordinated universal time (UTC) date on which the log file was created. *yyyy* = year, *mm* = month, and *dd* = day.

- The placeholder *nnnn* is an instance number that starts at the value of 1 daily for each message tracking log file name prefix.

Information is written to each log file until the file size reaches its maximum specified value for each log file. Then, a new log file that has an incremented instance number is opened. This process is repeated throughout the day. The log file rotation functionality deletes the oldest log files when either of the following conditions is true:

- A log file reaches its maximum specified age.

- The message tracking log directory reaches its maximum specified size.

  > [!IMPORTANT]
  > The maximum size of the message tracking log directory is calculated as the total size of all log files that have the same name prefix. Other files that do not follow the name prefix convention are not counted in the total directory size calculation. Renaming old log files or copying other files into the message tracking log directory could cause the directory to exceed its specified maximum size.<BR>On Exchange 2013 Mailbox servers, the maximum size of the message tracking log directory is three times the specified value. Although the message tracking log files that are generated by the four different services have four different name prefixes, the amount and frequency of data written to the <STRONG>MSGTRKMA</STRONG> log files is negligible compared to the three other log file prefixes.

The message tracking log files are text files that contain data in the comma-separated value (CSV) format. Each message tracking log file has a header that contains the following information:

- **\#Software:**: Name of the software that created the message tracking log file. Typically, the value is Microsoft Exchange Server.

- **\#Version:**: Version number of the software that created the message tracking log file. Currently, the value is 15.0.0.0.

- **\#Log-Type:**: Log type value, which is Message Tracking Log.

- **\#Date:**: The UTC date-time when the log file was created. The UTC date-time is represented in the ISO 8601 date-time format: *yyyy-mm-dd*T*hh:mm:ss.fff*Z, where *yyyy* = year, *mm* = month, *dd* = day, T indicates the beginning of the time component, *hh* = hour, *mm* = minute, *ss* = second, *fff* = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.

- **\#Fields:**: Comma-delimited field names used in the message tracking log files.

## Fields in the message tracking log files

The message tracking log stores each message event on a single line in the log. The message event information is organized by fields, and these fields are separated by commas. The field name is generally descriptive enough to determine the type of information that it contains. However, some fields may be blank, or the type of information that is stored in the field may change based on the message event type and the type of message tracking log file where the event was recorded. General descriptions of the fields that are used to classify each message tracking event are explained in the following table.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Field name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>date-time</strong></p></td>
<td><p>The UTC date-time of the message tracking event. The UTC date-time is represented in the ISO 8601 date-time format: <em>yyyy-mm-dd</em>T<em>hh:mm:ss.fff</em>Z, where <em>yyyy</em> = year, <em>mm</em> = month, <em>dd</em> = day, T indicates the beginning of the time component, <em>hh</em> = hour, <em>mm</em> = minute, <em>ss</em> = second, <em>fff</em> = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.</p></td>
</tr>
<tr class="even">
<td><p><strong>client-ip</strong></p></td>
<td><p>The IPv4 or IPv6 address of the messaging server or messaging client that submitted the message.</p></td>
</tr>
<tr class="odd">
<td><p><strong>client-hostname</strong></p></td>
<td><p>The host name or FQDN of the messaging server or messaging client that submitted the message.</p></td>
</tr>
<tr class="even">
<td><p><strong>server-ip</strong></p></td>
<td><p>The IPv4 or IPv6 address of the source or destination Exchange server.</p></td>
</tr>
<tr class="odd">
<td><p><strong>server-hostname</strong></p></td>
<td><p>The host name or FQDN of the destination server.</p></td>
</tr>
<tr class="even">
<td><p><strong>source-context</strong></p></td>
<td><p>Extra information associated with the <strong>source</strong> field. For example, transport agent information.</p></td>
</tr>
<tr class="odd">
<td><p><strong>connector-id</strong></p></td>
<td><p>The name of the source or destination Send connector or Receive connector. For example, <em>ServerName</em>\<em>ConnectorName</em> or <em>ConnectorName</em>.</p></td>
</tr>
<tr class="even">
<td><p><strong>source</strong></p></td>
<td><p>The Exchange transport component responsible for the message tracking event. The values found in this field are described in the Source values in the message tracking log section later in this topic.</p></td>
</tr>
<tr class="odd">
<td><p><strong>event-id</strong></p></td>
<td><p>The message event type. The event types are described in the Event types in the message tracking log section later in this topic.</p></td>
</tr>
<tr class="even">
<td><p><strong>internal-message-id</strong></p></td>
<td><p>A message identifier assigned by the Exchange server currently processing the message.</p>
<p>A specific message's value of <strong>internal-message-id</strong> is different in the message tracking log of every Exchange server that's involved in the transmission of the message. An example value is <code>73014444033</code>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>message-id</strong></p></td>
<td><p>The value of the <strong>Message-Id:</strong> header field found in the message header. If the <strong>Message-Id:</strong> header field does not exist or is blank, an arbitrary value is assigned. This value is constant for the lifetime of the message. For messages created in Exchange, the value is in the format <code>&lt;GUID@ServerFQDN&gt;</code>, including the angle brackets (<code>&lt; &gt;</code>). For example, <code>&lt;4867a3d78a50438bad95c0f6d072fca5@mailbox01.contoso.com&gt;</code>. Other messaging systems may use different syntax or values.</p></td>
</tr>
<tr class="even">
<td><p><strong>network-message-id</strong></p></td>
<td><p>A unique message ID value that persists across copies of the message that may be created due to bifurcation or distribution group expansion. An example value is <code>1341ac7b13fb42ab4d4408cf7f55890f</code>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>recipient-address</strong></p></td>
<td><p>The email addresses of the message's recipients. Multiple email addresses are separated by the semicolon character (;).</p></td>
</tr>
<tr class="even">
<td><p><strong>recipient-status</strong></p></td>
<td><p>This field contains the recipient status for each recipient separated by the semicolon character (;). The status values are presented for the recipients in the same order as the values in the <strong>recipient-address</strong> field. Example status values include <code>250 2.1.5 Recipient OK</code> or <code>550 4.4.7 QUEUE.Expired;&lt;ErrorText&gt;</code>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>total-bytes</strong></p></td>
<td><p>The size of the message that includes attachments, in bytes.</p></td>
</tr>
<tr class="even">
<td><p><strong>recipient-count</strong></p></td>
<td><p>The number of recipients in the message.</p></td>
</tr>
<tr class="odd">
<td><p><strong>related-recipient-address</strong></p></td>
<td><p>This field is used with <strong>EXPAND</strong>, <strong>REDIRECT</strong>, and <strong>RESOLVE</strong> events to display other recipient email addresses associated with the message.</p></td>
</tr>
<tr class="even">
<td><p><strong>reference</strong></p></td>
<td><p>This field contains additional information for specific types of events. For example:</p>
<p><strong>DSN</strong>   Contains the report link, which is the <strong>Message-Id</strong> value of the associated delivery status notification (DSN) if a DSN is generated subsequent to this event. If this is a DSN message, the <strong>Reference</strong> field contains the <strong>Message-Id</strong> value of the original message for which this DNS was generated.</p>
<p><strong>EXPAND</strong>   The Reference field contains the <strong>related-recipient-address</strong> value of the related messages.</p>
<p><strong>RECEIVE</strong>   The Reference field may contain the <strong>Message-Id</strong> value of the related message if the message was generated by other processes, for example, journaling or Inbox rules.</p>
<p><strong>SEND</strong>   The Reference field contains the <strong>Internal-Message-Id</strong> value of any DSN messages.</p>
<p><strong>THROTTLE</strong>   The Reference field contains the reason why the message was throttled.</p>
<p><strong>TRANSFER</strong>   The Reference field contains the Internal-Message-Id of the message that is being forked.</p>
<p>For messages generated by inbox rules, the <strong>Reference</strong> field contains the <strong>Internal-Message-Id</strong> value of the inbound message that caused the inbox rule to generate the outbound message.</p>
<p>For other types of events, the <strong>Reference</strong> field may contain the <strong>Internal-Message-Id</strong> value for forked messages.</p>
<p>For other types of events, the <strong>Reference</strong> field is usually blank.</p></td>
</tr>
<tr class="odd">
<td><p><strong>message-subject</strong></p></td>
<td><p>The message's subject found in the <code>Subject:</code> header field. The tracking of message subjects is controlled by the <em>MessageTrackingLogSubjectLoggingEnabled</em> parameter in the <strong>Set-TransportService</strong> or <strong>Set-MailboxServer</strong> cmdlets. By default, message subject tracking is enabled.</p></td>
</tr>
<tr class="even">
<td><p><strong>sender-address</strong></p></td>
<td><p>The email address specified in the <code>Sender:</code> header field, or the <code>From:</code> header field if <code>Sender:</code> is not present.</p></td>
</tr>
<tr class="odd">
<td><p><strong>return-path</strong></p></td>
<td><p>The return email address specified by <code>MAIL FROM:</code> in the message envelope. Although this field is never empty, it can have the null sender address value represented as <code>&lt;&gt;</code>.</p></td>
</tr>
<tr class="even">
<td><p><strong>message-info</strong></p></td>
<td><p>Additional information about the message. For example:</p>
<ul>
<li><p>The message origination UTC date-time for <strong>DELIVER</strong> and <strong>SEND</strong> events. The origination date-time is the time when the message first entered the Exchange organization. The UTC date-time is represented in the ISO 8601 date-time format: <em>yyyy-mm-dd</em>T<em>hh:mm:ss.fff</em>Z, where <em>yyyy</em> = year, <em>mm</em> = month, <em>dd</em> = day, T indicates the beginning of the time component, <em>hh</em> = hour, <em>mm</em> = minute, <em>ss</em> = second, <em>fff</em> = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.</p></li>
<li><p>Authentication errors. For example you may see the value <code>11a</code> and the type of authentication used when authentication errors occur.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><strong>directionality</strong></p></td>
<td><p>The direction of the message. Example values include <code>Incoming</code>, <code>Undefined</code>, and <code>Originating</code>.</p></td>
</tr>
<tr class="even">
<td><p><strong>tenant-id</strong></p></td>
<td><p>This field isn't used in on-premises Exchange 2013 organizations.</p></td>
</tr>
<tr class="odd">
<td><p><strong>original-client-ip</strong></p></td>
<td><p>The IPv4 or IPv6 address of the original client.</p></td>
</tr>
<tr class="even">
<td><p><strong>original-server-ip</strong></p></td>
<td><p>The IPv4 or IPv6 address of the original server.</p></td>
</tr>
<tr class="odd">
<td><p><strong>custom-data</strong></p></td>
<td><p>This field contains data related to a specific event types. For example, the Transport Rule agent uses this field to record the GUID of the transport rule or DLP policy that acted on the message. For more information about these Transport Rule agent values, see the &quot;Data logging&quot; section in the <a href="view-dlp-policy-detection-reports-exchange-2013-help.md">View DLP policy detection reports</a> topic,</p></td>
</tr>
</tbody>
</table>

## Event types in the message tracking log

Various event types in the **event-id** field are used to classify the message events in the message tracking log. Some message events appear in only one type of message tracking log file, and some message events appear in all types of message tracking log files. The events types that are used to classify each message event are explained in the following table.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Event name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>AGENTINFO</strong></p></td>
<td><p>This event is used by transport agents to log custom data.</p></td>
</tr>
<tr class="even">
<td><p><strong>BADMAIL</strong></p></td>
<td><p>A message submitted by the Pickup directory or the Replay directory that can't be delivered or returned.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DEFER</strong></p></td>
<td><p>Message delivery was delayed.</p></td>
</tr>
<tr class="even">
<td><p><strong>DELIVER</strong></p></td>
<td><p>A message was delivered to a local mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DROP</strong></p></td>
<td><p>A message was dropped without a delivery status notification (also known as a DSN, bounce message, non-delivery report, or NDR). For example:</p>
<ul>
<li><p>Completed moderation approval request messages.</p></li>
<li><p>Spam messages that were silently dropped without an NDR.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>DSN</strong></p></td>
<td><p>A delivery status notification (DSN) was generated.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DUPLICATEDELIVER</strong></p></td>
<td><p>A duplicate message was delivered to the recipient. Duplication may occur if a recipient is a member of multiple nested distribution groups. Duplicate messages are detected and removed by the information store.</p></td>
</tr>
<tr class="even">
<td><p><strong>DUPLICATEEXPAND</strong></p></td>
<td><p>During the expansion of the distribution group, a duplicate recipient was detected.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DUPLICATEREDIRECT</strong></p></td>
<td><p>An alternate recipient for the message was already a recipient.</p></td>
</tr>
<tr class="even">
<td><p><strong>EXPAND</strong></p></td>
<td><p>A distribution group was expanded.</p></td>
</tr>
<tr class="odd">
<td><p><strong>FAIL</strong></p></td>
<td><p>Message delivery failed. Sources include <strong>SMTP</strong>, <strong>DNS</strong>, <strong>QUEUE</strong>, and <strong>ROUTING</strong>.</p></td>
</tr>
<tr class="even">
<td><p><strong>HADISCARD</strong></p></td>
<td><p>A shadow message was discarded after the primary copy was delivered to the next hop. For more information, see <a href="shadow-redundancy-exchange-2013-help.md">Shadow redundancy</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>HARECEIVE</strong></p></td>
<td><p>A shadow message was received by the server in the local database availability group (DAG) or Active Directory site.</p></td>
</tr>
<tr class="even">
<td><p><strong>HAREDIRECT</strong></p></td>
<td><p>A shadow message was created.</p></td>
</tr>
<tr class="odd">
<td><p><strong>HAREDIRECTFAIL</strong></p></td>
<td><p>A shadow message failed to be created. The details are stored in the <strong>source-context</strong> field.</p></td>
</tr>
<tr class="even">
<td><p><strong>INITMESSAGECREATED</strong></p></td>
<td><p>A message was sent to a moderated recipient, so the message was sent to the arbitration mailbox for approval. For more information, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/manage-message-approval">Manage message approval</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>LOAD</strong></p></td>
<td><p>A message was successfully loaded at boot.</p></td>
</tr>
<tr class="even">
<td><p><strong>MODERATIONEXPIRE</strong></p></td>
<td><p>A moderator for a moderated recipient never approved or rejected the message, so the message expired. For more information about moderated recipients, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/manage-message-approval">Manage message approval</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>MODERATORAPPROVE</strong></p></td>
<td><p>A moderator for a moderated recipient approved the message, so the message was delivered to the moderated recipient.</p></td>
</tr>
<tr class="even">
<td><p><strong>MODERATORREJECT</strong></p></td>
<td><p>A moderator for a moderated recipient rejected the message, so the message wasn't delivered to the moderated recipient.</p></td>
</tr>
<tr class="odd">
<td><p><strong>MODERATORSALLNDR</strong></p></td>
<td><p>All approval requests sent to all moderators of a moderated recipient were undeliverable, and resulted in non-delivery reports (NDRs).</p></td>
</tr>
<tr class="even">
<td><p><strong>NOTIFYMAPI</strong></p></td>
<td><p>A message was detected in the Outbox of a mailbox on the local server.</p></td>
</tr>
<tr class="odd">
<td><p><strong>NOTIFYSHADOW</strong></p></td>
<td><p>A message was detected in the Outbox of a mailbox on the local server, and a shadow copy of the message needs to be created.</p></td>
</tr>
<tr class="even">
<td><p><strong>POISONMESSAGE</strong></p></td>
<td><p>A message was put in the poison message queue or removed from the poison message queue.</p></td>
</tr>
<tr class="odd">
<td><p><strong>PROCESS</strong></p></td>
<td><p>The message was successfully processed.</p></td>
</tr>
<tr class="even">
<td><p><strong>PROCESSMEETINGMESSAGE</strong></p></td>
<td><p>A meeting message was processed by the Mailbox Transport Delivery service.</p></td>
</tr>
<tr class="odd">
<td><p><strong>RECEIVE</strong></p></td>
<td><p>A message was received by the SMTP receive component of the transport service or from the Pickup or Replay directories (source: <code>SMTP</code>), or a message was submitted from a mailbox to the Mailbox Transport Submission service (source: <code>STOREDRIVER</code>).</p></td>
</tr>
<tr class="even">
<td><p><strong>REDIRECT</strong></p></td>
<td><p>A message was redirected to an alternative recipient after an Active Directory lookup.</p></td>
</tr>
<tr class="odd">
<td><p><strong>RESOLVE</strong></p></td>
<td><p>A message's recipients were resolved to a different email address after an Active Directory lookup.</p></td>
</tr>
<tr class="even">
<td><p><strong>RESUBMIT</strong></p></td>
<td><p>A message was automatically resubmitted from Safety Net. For more information, see <a href="safety-net-exchange-2013-help.md">Safety Net</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>RESUBMITDEFER</strong></p></td>
<td><p>A message resubmitted from Safety Net was deferred.</p></td>
</tr>
<tr class="even">
<td><p><strong>RESUBMITFAIL</strong></p></td>
<td><p>A message resubmitted from Safety Net failed.</p></td>
</tr>
<tr class="odd">
<td><p><strong>SEND</strong></p></td>
<td><p>A message was sent by SMTP between transport services.</p></td>
</tr>
<tr class="even">
<td><p><strong>SUBMIT</strong></p></td>
<td><p>The Mailbox Transport Submission service successfully transmitted the message to the Transport service. For <strong>SUBMIT</strong> events, the <strong>source-context</strong> property contains the following details:</p>
<ul>
<li><p><strong>MDB</strong>   The mailbox database GUID.</p></li>
<li><p><strong>Mailbox</strong>   The mailbox GUID.</p></li>
<li><p><strong>Event</strong>   The event sequence number.</p></li>
<li><p><strong>MessageClass</strong>   The type of message. For example, <code>IPM.Note</code>.</p></li>
<li><p><strong>CreationTime</strong>   Date-time of the message submission.</p></li>
<li><p><strong>ClientType</strong>   For example, <code>User</code>, <code>OWA</code> ,or <code>ActiveSync</code>.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><strong>SUBMITDEFER</strong></p></td>
<td><p>The message transmission from the Mailbox Transport Submission service to the Transport service was deferred.</p></td>
</tr>
<tr class="even">
<td><p><strong>SUBMITFAIL</strong></p></td>
<td><p>The message transmission from the Mailbox Transport Submission service to the Transport service failed.</p></td>
</tr>
<tr class="odd">
<td><p><strong>SUPPRESSED</strong></p></td>
<td><p>The message transmission was suppressed.</p></td>
</tr>
<tr class="even">
<td><p><strong>THROTTLE</strong></p></td>
<td><p>The message was throttled.</p></td>
</tr>
<tr class="odd">
<td><p><strong>TRANSFER</strong></p></td>
<td><p>Recipients were moved to a forked message because of content conversion, message recipient limits, or agents. Sources include <strong>ROUTING</strong> or <strong>QUEUE</strong>.</p></td>
</tr>
</tbody>
</table>

## Source values in the message tracking log

The values in the **source** field in the message tracking log indicate the transport component that's responsible for the message tracking event. The following table describes the values of the **source** field.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Source value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>ADMIN</strong></p></td>
<td><p>The event source was human intervention. For example, an administrator used Queue Viewer to delete a message, or submitted message files using the Replay directory.</p></td>
</tr>
<tr class="even">
<td><p><strong>AGENT</strong></p></td>
<td><p>The event source was a transport agent.</p></td>
</tr>
<tr class="odd">
<td><p><strong>APPROVAL</strong></p></td>
<td><p>The event source was the approval framework that's used with moderated recipients. For more information, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/manage-message-approval">Manage message approval</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>BOOTLOADER</strong></p></td>
<td><p>The event source was unprocessed messages that exist on the server at boot time. This is related to the <strong>LOAD</strong> event type.</p></td>
</tr>
<tr class="odd">
<td><p><strong>DNS</strong></p></td>
<td><p>The event source was DNS.</p></td>
</tr>
<tr class="even">
<td><p><strong>DSN</strong></p></td>
<td><p>The event source was a delivery status notification (DSN). For example, a non-delivery report (NDR).</p></td>
</tr>
<tr class="odd">
<td><p><strong>GATEWAY</strong></p></td>
<td><p>The event source was a Foreign connector. For more information, see <a href="foreign-connectors-exchange-2013-help.md">Foreign connectors</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>MAILBOXRULE</strong></p></td>
<td><p>The event source was an Inbox rule. For more information, see <a href="https://support.microsoft.com/office/edea3d17-00c9-434b-b9b7-26ee8d9f5622">Inbox rules</a>.</p></td>
</tr>
<tr class="odd">
<td><p><strong>MEETINGMESSAGEPROCESSOR</strong></p></td>
<td><p>The event source was the meeting message processor, which updates calendars based on meeting updates.</p></td>
</tr>
<tr class="even">
<td><p><strong>ORAR</strong></p></td>
<td><p>The event source was an Originator Requested Alternate Recipient (ORAR). You can enable or disable support for ORAR on Receive connectors using the <em>OrarEnabled</em> parameter on the <strong>New-ReceiveConnector</strong> or <strong>Set-ReceiveConnector</strong> cmdlets.</p></td>
</tr>
<tr class="odd">
<td><p><strong>PICKUP</strong></p></td>
<td><p>The event source was the Pickup directory. For more information, see <a href="pickup-directory-and-replay-directory-exchange-2013-help.md">Pickup directory and Replay directory</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>POISONMESSAGE</strong></p></td>
<td><p>The event source was the poison message identifier. For more information about poison messages and the poison message queue, see <a href="queues-exchange-2013-help.md">Queues</a></p></td>
</tr>
<tr class="odd">
<td><p><strong>PUBLICFOLDER</strong></p></td>
<td><p>The event source was a mail-enabled public folder.</p></td>
</tr>
<tr class="even">
<td><p><strong>QUEUE</strong></p></td>
<td><p>The event source was a queue.</p></td>
</tr>
<tr class="odd">
<td><p><strong>REDUNDANCY</strong></p></td>
<td><p>The event source was Shadow Redundancy. For more information, see <a href="shadow-redundancy-exchange-2013-help.md">Shadow redundancy</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>ROUTING</strong></p></td>
<td><p>The event source was the routing resolution component of the categorizer in the Transport service.</p></td>
</tr>
<tr class="odd">
<td><p><strong>SAFETYNET</strong></p></td>
<td><p>The event source was Safety Net. For more information, see <a href="safety-net-exchange-2013-help.md">Safety Net</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>SMTP</strong></p></td>
<td><p>The message was submitted by the SMTP send or SMTP receive component of the transport service.</p></td>
</tr>
<tr class="odd">
<td><p><strong>STOREDRIVER</strong></p></td>
<td><p>The event source was a MAPI submission from a mailbox on the local server.</p></td>
</tr>
</tbody>
</table>

## Example entries in the message tracking log

An uneventful message sent between two users generates several entries in the message tracking log. You can see the results using the **Get-MessageTrackingLog** cmdlet. For more information, see [Search message tracking logs](search-message-tracking-logs-exchange-2013-help.md).

This is a condensed example of the message tracking log entries created when the user chris@contoso.com successfully sends a test message to the user michelle@contoso.com. Both users have mailboxes on the same server.

```powershell
EventId    Source      Sender            Recipients             MessageSubject
-------    ------      ------            ----------             --------------
NOTIFYMAPI STOREDRIVER                   {}
RECEIVE    STOREDRIVER chris@contoso.com {michelle@contoso.com} test
SUBMIT     STOREDRIVER chris@contoso.com {michelle@contoso.com} test
HAREDIRECT SMTP        chris@contoso.com {michelle@contoso.com} test
RECEIVE    SMTP        chris@contoso.com {michelle@contoso.com} test
AGENTINFO  AGENT       chris@contoso.com {michelle@contoso.com} test
SEND       SMTP        chris@contoso.com {michelle@contoso.com} test
DELIVER    STOREDRIVER chris@contoso.com {michelle@contoso.com} test
```

## Security concerns for the message tracking log

No message content is stored in the message tracking log. By default, the subject line of an email message is stored in the message tracking log. You may want to disable message subject logging to comply with increased security or privacy requirements. Before you enable or disable message subject logging, make sure that you verify your organization's policy about revealing subject line information. For more information, see [Configure message tracking](configure-message-tracking-exchange-2013-help.md).
