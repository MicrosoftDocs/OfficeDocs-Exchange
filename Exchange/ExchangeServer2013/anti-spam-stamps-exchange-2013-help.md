---
title: 'Anti-spam stamps: Exchange 2013 Help'
TOCTitle: Anti-spam stamps
ms:assetid: 28d3a5c2-8509-4b25-9876-763536e77c27
ms:mtpsurl: https://technet.microsoft.com/library/Aa996878(v=EXCHG.150)
ms:contentKeyID: 49248677
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Anti-spam stamps

_**Applies to:** Exchange Server 2013_

Anti-spam stamps help you diagnose spam-related problems by applying diagnostic metadata, or stamps, such as sender-specific information, puzzle validation results, and content filtering results, to messages as they pass through the anti-spam features that filter inbound messages from the Internet. There are three anti-spam stamps: the phishing confidence level stamp, the spam confidence level stamp, and the Sender ID stamp.

You can use anti-spam stamps as diagnostic tools to determine what actions to take on false-positives and on suspected spam messages that individuals receive in their mailboxes.

> [!NOTE]
> On November 1, 2016, Microsoft stopped producing spam definition updates for the SmartScreen filters in Exchange and Outlook. The existing SmartScreen spam definitions will be left in place, but their effectiveness will likely degrade over time. For more information, see <A href="https://techcommunity.microsoft.com/t5/exchange-team-blog/deprecating-support-for-smartscreen-in-outlook-and-exchange/ba-p/605332">Deprecating support for SmartScreen in Outlook and Exchange</A>.

## Viewing anti-spam stamps

You can view anti-spam stamps by using Microsoft Outlook. For more information, see [View anti-spam stamps in Outlook](view-anti-spam-stamps-in-outlook-exchange-2013-help.md).

## Understanding the anti-spam report

The anti-spam report is a summary report of the anti-spam filter results that have been applied to an email message. The Content Filter agent applies this stamp to the message envelope in the form of an X-header as follows.

```powershell
X-MS-Exchange-Organization-Antispam-Report: DV:<DATVersion>;CW:CustomList;PCL:PhishingVerdict <verdict>;P100:PhishingBlock;PP:Presolve;SID:SenderIDStatus <status>;TIME:<SendReceiveDelta>;MIME:MimeCompliance
```

The following table describes the filter information that can appear in an anti-spam report.

> [!NOTE]
> The anti-spam report only displays information from the filters that were applied to the specific message. An anti-spam report doesn't usually contain all the information listed in the following table. For example, you may receive the following anti-spam report: <CODE>DV:3.1.3924.1409;SID:SenderIDStatus Fail;PCL:PhishingLevel SUSPICIOUS;CW:CustomList;PP:Presolved;TIME:TimeBasedFeatures</CODE>.

### Filter information in an anti-spam report

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Stamp</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SID</p></td>
<td><p>The Sender ID (SID) stamp is based on the sender policy framework (SPF) that authorizes the use of domains in email. The SPF is displayed in the message envelope as <code>Received-SPF</code>. The Sender ID evaluation process generates a Sender ID status for the message. This status can be returned as one of the following values:</p>
<ul>
<li><p><strong>Pass</strong>   Both the IP address and Purported Responsible Address (PRA) passed the Sender ID verification check.</p></li>
<li><p><strong>Neutral</strong>   Published Sender ID data is explicitly inconclusive.</p></li>
<li><p><strong>Soft fail</strong>   The IP address for the PRA may be in the not permitted set.</p></li>
<li><p><strong>Fail</strong>   The IP Address is not permitted; no PRA is found in the incoming mail or the sending domain does not exist.</p></li>
<li><p><strong>None</strong>   No published SPF data exists in the sender's DNS.</p></li>
<li><p><strong>TempError</strong>   A temporary DNS failure occurred, such as an unavailable DNS server.</p></li>
<li><p><strong>PermError</strong>   The DNS record is invalid, such as an error in the record format.</p></li>
</ul>
<p>The Sender ID stamp is displayed as an X-Header in the message envelope as follows:</p>

<code>X-MS-Exchange-Organization-SenderIdResult:\<status\></code>
<p>For more information about Sender ID, see <a href="sender-id-exchange-2013-help.md">Sender ID</a>.</p></td>
</tr>
<tr class="even">
<td><p>DV</p></td>
<td><p>The DAT version (DV) stamp indicates the version of the spam definition file that was used when scanning the message.</p></td>
</tr>
<tr class="odd">
<td><p>SA</p></td>
<td><p>The signature action (SA) stamp indicates that the message was either recovered or deleted because of a signature that was found in the message.</p></td>
</tr>
<tr class="even">
<td><p>SV</p></td>
<td><p>The signature DAT version (SV) stamp indicates the version of the signature file that was used when scanning the message.</p></td>
</tr>
<tr class="odd">
<td><p>PCL</p></td>
<td><p>The phishing confidence level (PCL) stamp displays the rating of the message based on its content and is applied when the message is processed by the Content Filter agent. This status can be returned as one of the following values:</p>
<ul>
<li><p><strong>Neutral</strong>   The message's content isn't likely to be phishing.</p></li>
<li><p><strong>Suspicious</strong>   The message's content is likely to be phishing.</p></li>
</ul>
<p>The PCL value can range from 1 through 8. A PCL rating from 1 through 3 returns a status of <code>Neutral</code>. This means that the message's content isn't likely to be phishing. A PCL rating from 4 through 8 returns a status of <code>Suspicious</code>. This means that the message is likely to be phishing.</p>
<p>The values are used to determine what action Outlook takes on messages. Outlook uses the PCL stamp to block the content of suspicious messages.</p>
<p>The PCL stamp is displayed as an X-header in the message envelope as follows:</p>
<code>X-MS-Exchange-Organization-PCL:\<status\></code>
</td>
</tr>
<tr class="even">
<td><p>SCL</p></td>
<td><p>The spam confidence level (SCL) stamp of the message displays the rating of the message based on its content. The Content Filter agent uses Microsoft SmartScreen technology to assess the contents of a message and to assign an SCL rating to each message. The SCL value is from 0 through 9, where 0 is considered less likely to be spam, and 9 is considered more likely to be spam. The actions that Exchange and Outlook take depend on your SCL threshold settings.</p>
<p>The SCL stamp is displayed as an X-header in the message envelope as follows:</p>
<code>X-MS-Exchange-Organization-SCL:\<status\></code>
<p>For more information about SCL thresholds and actions, see <a href="spam-confidence-level-threshold-exchange-2013-help.md">Spam Confidence Level Threshold</a>.</p></td>
</tr>
<tr class="odd">
<td><p>CW</p></td>
<td><p>The custom weight (CW) stamp of a message indicates that the message contains an unapproved word or phrase and that the SCL value, or weight, of that unapproved word or phrase was applied to the final SCL score:</p>
<ul>
<li><p>Unapproved phrases, or Block phrases, have maximum weight and change the SCL score to 9.</p></li>
<li><p>Approved words or phrases, or Allow phrases, have minimum weight and change the SCL score to 0.</p></li>
</ul>
<p>For more information about how to add approved and unapproved words or phrases to the Content Filtering agent, see <a href="manage-content-filtering-exchange-2013-help.md">Manage content filtering</a>.</p></td>
</tr>
<tr class="even">
<td><p>PP</p></td>
<td><p>The presolved puzzle (PP) stamp indicates that if a sender's message contains a valid, solved computational postmark, based on Outlook E-mail Postmark validation functionality, it's unlikely that the sender is a malicious sender. In this case, the Content Filter agent would reduce the SCL rating.</p>
<p>The Content Filter agent doesn't change the SCL rating if the E-mail Postmark validation feature is enabled and either of the following conditions is true:</p>
<ul>
<li><p>An inbound message doesn't contain a computational postmark header.</p></li>
<li><p>The computational postmark header isn't valid.</p></li>
</ul>
<p>For more information about the postmark validation feature, see <a href="content-filtering-exchange-2013-help.md">Content filtering</a>.</p></td>
</tr>
<tr class="odd">
<td><p>TIME:TimeBasedFeatures</p></td>
<td><p>The TIME stamp indicates that there was a significant time delay between the time that the message was sent and the time that the message was received. The TIME stamp is used to determine the final SCL rating for the message.</p></td>
</tr>
<tr class="even">
<td><p>MIME:MIMECompliance</p></td>
<td><p>The MIME stamp indicates that the email message isn't MIME compliant.</p></td>
</tr>
<tr class="odd">
<td><p>P100:PhishingBlock</p></td>
<td><p>The P100 stamp indicates that the message contains a URL that's present in a phishing definition file.</p></td>
</tr>
<tr class="even">
<td><p>IPOnAllowList</p></td>
<td><p>The IPOnAllowList stamp indicates that the sender's IP address is on the IP Allow list. For more information about the IP Allow list, see <a href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124320(v=exchg.141)">Understanding Connection Filtering</a>.</p></td>
</tr>
<tr class="odd">
<td><p>MessageSecurityAntispamBypass</p></td>
<td><p>The MessageSecurityAntispamBypass stamp indicates that the message wasn't filtered for content and that the sender has been granted permission to bypass the anti-spam filters.</p></td>
</tr>
<tr class="even">
<td><p>SenderBypassed</p></td>
<td><p>The SenderBypassed stamp indicates that the Content Filter agent doesn't process any content filtering for messages that are received from this sender. For more information, see <a href="manage-content-filtering-exchange-2013-help.md">Manage content filtering</a>.</p></td>
</tr>
<tr class="odd">
<td><p>AllRecipientsBypassed</p></td>
<td><p>The AllRecipientsBypassed stamp indicates that one of the following conditions was met for all recipients listed in the message:</p>
<ul>
<li><p>The <em>AntispamBypassedEnabled</em> parameter on the recipient's mailbox is set to <code>$true</code>. This is a per-recipient setting that can only be set by an administrator using the <strong>Set-Mailbox</strong> cmdlet.</p></li>
<li><p>The message sender is in the recipient's Outlook Safe Senders List. For more information about the Safe Senders List, see <a href="manage-safelist-aggregation-exchange-2013-help.md">Manage safelist aggregation</a>.</p></li>
<li><p>The Content Filter agent doesn't process any content filtering for messages that are sent to this recipient. For more information about recipient exceptions, see <a href="manage-content-filtering-exchange-2013-help.md">Manage content filtering</a>.</p></li>
</ul></td>
</tr>
</tbody>
</table>
