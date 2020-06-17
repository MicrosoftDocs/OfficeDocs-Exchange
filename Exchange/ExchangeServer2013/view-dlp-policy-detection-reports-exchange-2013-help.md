---
title: 'View DLP policy detection reports: Exchange 2013 Help'
TOCTitle: View DLP policy detection reports
ms:assetid: 5c3f1cf6-d8c7-4d83-bb24-641ea9d50cbc
ms:mtpsurl: https://technet.microsoft.com/library/JJ150520(v=EXCHG.150)
ms:contentKeyID: 47560019
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View DLP policy detection reports

_**Applies to:** Exchange Server 2013_

Data loss prevention (DLP) policy detection management broadly defines the activities that an organization performs in order to identify, investigate, and resolve DLP policy violations. In order to manage incidents, you need access to information that identifies what was detected by your DLP policies. This detection information is integrated with existing Microsoft Exchange Server 2013 data and log formats so that you can leverage an existing rich system of data to manage your mail flow incidents.

For information about creating an incident report along with a single policy detection event, see [Create incident reports for DLP policy detections](create-incident-reports-for-dlp-policy-detections-exchange-2013-help.md). For more information about message logs, see [Track messages with delivery reports](track-messages-with-delivery-reports-exchange-2013-help.md).

> [!NOTE]
> Exchange 2013: DLP is a premium feature that requires an Exchange Enterprise Client Access License (CAL). For more information about CALs and server licensing, see <A href="https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview">Exchange licensing FAQs</A>.

## Audit information

Data related to DLP detection management in Exchange is integrated into the message tracking logs, also known as delivery reports. The capabilities reuse much of the existing logging framework available in the system. For general information, including understanding the structure of the message tracking log files, please review existing content in [Understanding Message Tracking](message-tracking-exchange-2013-help.md) or [Track messages with delivery reports](track-messages-with-delivery-reports-exchange-2013-help.md).

The delivery report is a detailed log of all message activity as messages are transferred to and from a computer that is running the Transport service on a Mailbox server. The message tracking log can be accessed through the Exchange Management Shell by using the **Get-MessageTrackingLog** cmdlet. DLP data is integrated into the delivery report following existing data formats and conventions.

## Data logging format

Message tracking logs contain data from the agents involved in processing the mail flow content. For DLP, the transport rule agent (TRA) is used to invoke deep message content scanning and to apply the policies defined as part of the ETRs. The existing AgentInfo Event is used to add DLP related entries in the message tracking log.

The agent name will be **TRA** or **Transport Rule Agent** in the AgentInfo event. A single AgentInfo event will be logged per message describing the DLP processing applied to the message. The **CustomData** field of the message tracking log entry field is where the DLP data logged by the transport rule agent will appear. This field may contain multiple entries: one data classification and client information line for each data classification found in the message, one rule line for each rule that applies to the message, and one health monitoring line for each rule that exceeds the load or execution time threshold.

An example of the DLP log entry is displayed here. The output has been formatted to display strings in separate lines with new lines between.

Source: AGENT

EventId: AGENTINFO

CustomData: S:TRA=DC|dcid=41BFDBC6C9D811E0816A3CD34824019B|count=10|conf=77;

S:TRA=DC|dcid=C7ECCBA0CA0011E0B6C00B124924019B|count=3|conf=81;

S:TRA=CI|sndOverride=or|just=Business Reason;

S:TRA=CI|sndOverride=fp;

S:TRA=ETR|ruleId=FC2AA60C9D811E0AFC076D34824019B|dlpid=1B81CC82C9DB11E09052C5D64824019B|st=2010-11-03 15:30T|action=PrependSubject|action=Encrypt|sev=2|mode=audit|dcid=41BFDBC6C9D811E0816A3CD34824019B|sndOverride=or;

S:TRA=ETR|ruleId=AB2AA60C9D811E0AFC076D34824019B|dlpid=1B81CC82C9DB11E09052C5D64824019B|st=2010-11-03 15:30T|action=Encrypt|sev=1|mode=enabled|dcid=C7ECCBA0CA0011E0B6C00B124924019B|sndOverride=fp;

S:TRA=ETRP|ruleId=C27D21EECA0311E0BCB896154924019B|LoadW=200|LoadC=100|ExecW=5500|ExecC=200;

The Transport Rule Agent requires grouping of the rule ID, DLP Policy ID (optional), last modified date, action, severity, mode, detected data classification (optional), and sender override (optional) based on rule ID (indicated by "TRA=ETR" in the log line). It also requires the data classification ID, count, and confidence level of classifications to be grouped by classification name (indicated by "TRA=DC" in the log line).

Additional groupings include data classification ID, sender override (optional), and override justification (optional) based on data classification ID for all classifications that were detected on the client (indicated by "TRA=CI" in the log line). The Transport Rule Agent also requires the rule ID, load Wall clock (optional), load CPU clock (optional), execution Wall clock (optional), and execution CPU clock (optional) be grouped by rule ID for all rules that exceed the load or execution Wall or CPU clock thresholds (indicated by "TRA=ETRP" in the log line).

The following is a complete list of the data fields. All data in the MTL is type string. Format column describes how to recognize each field in the Message Tracking Log. Optional Field column specifies what fields might not be logged when a rule matches. DLP Specific column shows what fields are specific to the DLP feature.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Field name</strong></p></td>
<td><p><strong>Description</strong></p></td>
<td><p><strong>Format</strong></p></td>
<td><p><strong>Optional field</strong></p></td>
<td><p><strong>DLP specific</strong></p></td>
</tr>
<tr class="even">
<td><p>TRA</p></td>
<td><p>Transport Rule Agent; type AgentName</p></td>
<td><p>TRA=DC, ETR, CI, or ETRP</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>DC</p></td>
<td><p>Data Classification; type groupName</p></td>
<td><p>TRA=DC</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>ETR</p></td>
<td><p>Exchange Transport Rule; type groupName</p></td>
<td><p>TRA=ETR</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>CI</p></td>
<td><p>Client Information, type groupName</p></td>
<td><p>TRA=CI</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>ETRP</p></td>
<td><p>Exchange Transport Rule Performance; type groupName</p></td>
<td><p>TRA=ETRP</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>dcid</p></td>
<td><p>ID of the Data Classification</p></td>
<td><p>dcid=GUID</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>count</p></td>
<td><p>Count of the Data Classification</p></td>
<td><p>count=Integer</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>conf</p></td>
<td><p>Confidence level of the Data Classification</p></td>
<td><p>conf=Integer (Percent)</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>sndOverride</p></td>
<td><p>Sender override; the field is optional.</p>
<p>In the TRA=CI line, when field is set to "or" signifies the data classification was overridden. If the field is set to "fp" signifies the data classification was reported as a false positive.</p>
<p>In the TRA=ETR line, when the field is set to "or" signifies the rule or part of the rule was overridden. If the field is set to "fp" signifies the rule or part of the rule was reported as a false positive.</p></td>
<td><p>sndOverride=or or fp</p>
<p>Where "or" represents override and "fp" means false positive. The sndOverride field is present when an end-user had reported either an override or false positive for a rule.</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>just</p></td>
<td><p>Justification; the field is optional and only available when the sender override field is equal to "or" in the TRA=CI line. Justification text provided by the end user as the reason the data classification should be overridden.</p></td>
<td><p>just=IW input justification string</p>
<p>Justification field is only logged when end user reports an override.</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>ruleId</p></td>
<td><p>ID for a rule</p></td>
<td><p>ruleId=GUID</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>dlpId</p></td>
<td><p>ID for a DLP Policy. The field is optional; if there is no dlpId then the rule doesn't belong to a DLP Policy.</p></td>
<td><p>dlpId=GUID</p></td>
<td><p>Optional</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>st</p></td>
<td><p>Last Modified Date of a rule</p></td>
<td><p>st=UTC date-time</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>action</p></td>
<td><p>Action taken by a rule; could have multiple actions per rule</p></td>
<td><p>action=single action</p>
<p>If there are multiple actions applied for a rule, there will be multiple action fields.</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>sev</p></td>
<td><p>Audit severity of the rule</p></td>
<td><p>sev=1, 2, or 3</p>
<p>Where 1 represents low, 2 is medium, and 3 means high.</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>mode</p></td>
<td><p>State of the rule when it was hit (enforcement, audit, or auditandnotify).</p></td>
<td><p>mode=audit, auditandnotify, or enforcement</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>loadW</p></td>
<td><p>Load Wall Clock; the field is optional</p></td>
<td><p>loadW=time in ms</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>loadC</p></td>
<td><p>Load CPU Clock; the field is optional</p></td>
<td><p>loadC=time in ms</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>execW</p></td>
<td><p>Execute Wall Clock; the field is optional</p></td>
<td><p>execW=time in ms</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>execC</p></td>
<td><p>Execute CPU Clock; the field is optional</p></td>
<td><p>execC=time in ms</p></td>
<td><p>Optional</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>message-id</p></td>
<td><p>ID of the message</p></td>
<td><p>message-id=ID of message</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>date-time</p></td>
<td><p>Date and time the message was sent in universal time</p></td>
<td><p>date-time=UTC date-time</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>sender-address</p></td>
<td><p>Email address specified in the sender field</p></td>
<td><p>sender-address=Email address</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="odd">
<td><p>recipient-address</p></td>
<td><p>Email address(es) of the message's recipient(s)</p></td>
<td><p>recipient-address=Email address</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>message-subject</p></td>
<td><p>Data found in the subject field of the message</p></td>
<td><p>message-subject=end-user input subject string</p></td>
<td><p>Mandatory</p></td>
<td><p>No</p></td>
</tr>
</tbody>
</table>

## For more information

[Data loss prevention](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention)

[Create incident reports for DLP policy detections](create-incident-reports-for-dlp-policy-detections-exchange-2013-help.md)

[Track messages with delivery reports](track-messages-with-delivery-reports-exchange-2013-help.md)
