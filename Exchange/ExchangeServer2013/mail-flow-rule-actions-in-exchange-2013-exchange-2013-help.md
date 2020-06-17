---
title: 'Transport rule actions in Exchange 2013: Exchange 2013 Help'
TOCTitle: Transport rule actions
ms:assetid: 5d11a955-b1cc-4150-a0b9-a8cc48ba9bde
ms:mtpsurl: https://technet.microsoft.com/library/Aa998315(v=EXCHG.150)
ms:contentKeyID: 49361077
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Transport rule actions in Exchange 2013

_**Applies to:** Exchange Server 2013_

Actions in transport rules specify what you want to do to messages that match conditions of the rule. For example, you can create a rule that forwards message from specific senders to a moderator, or adds a disclaimer or personalized signature to all outbound messages.

Actions typically require additional properties. For example, when the rule redirects a message, you need to specify where to redirect the message. Some actions have multiple properties that are available or required. For example, when the rule adds a header field to the message header, you need to specify both the name and value of the header. When the rule adds a disclaimer to messages, you need to specify the disclaimer text, but you can also specify where to insert the text, or what to do if the disclaimer can't be added to the message. Typically, you can configure multiple actions in a rule, but some actions are exclusive. For example, one rule can't reject and redirect the same message.

For more information about transport rules in Exchange Server 2013, see [Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

For more information about conditions and exceptions in transport rules, see [Transport rule conditions and exceptions (predicates) in Exchange 2013](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md).

For more information about actions in transport rules in Exchange Online or Exchange Online Protection, see [Mail flow rule actions in Exchange Online](https://docs.microsoft.com/-exchange/security-and-compliance/mail-flow-rules/mail-flow-rule-actions).

## Actions for transport rules on Mailbox servers

The actions that are available in transport rules on Mailbox servers are described in the following table. Valid values for each property are described in the Property values section.

**Notes**:

- After you select an action in the Exchange admin center (EAC), the value that's ultimately shown in the **Do the following** field is often different from the click path you selected. Also, when you create new rules, you can sometimes (depending on the selections you make) select a short action name from a template (a filtered list of actions) instead of following the complete click path. The short names and full click path values are shown in the EAC column in the table.

- The names of some of the actions that are returned by the **Get-TransportRuleAction** cmdlet are different than the corresponding parameter names, and multiple parameters might be required for an action.

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
<th>Action in the EAC</th>
<th>Action parameter in the Exchange Management Shell</th>
<th>Property</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Forward the message for approval to these people</strong></p>
<p><strong>Forward the message for approval</strong> &gt; <strong>to these people</strong></p></td>
<td><p><em>ModerateMessageByUser</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Forwards the message to the specified moderators as an attachment wrapped in an approval request. For more information, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/common-message-approval-scenarios">Common message approval scenarios</a>. You can't use a distribution group as a moderator.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Forward the message for approval to the sender's manager</strong></p>
<p><strong>Forward the message for approval</strong> &gt; <strong>to the sender's manager</strong></p></td>
<td><p><em>ModerateMessageByManager</em></p></td>
<td><p>n/a</p></td>
<td><p>Forwards the message to the sender's manager for approval.</p>
<p>This action only works if the sender's <strong>Manager</strong> attribute is defined in Active Directory. Otherwise, the message is delivered to the recipients without moderation.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Redirect the message to these recipients</strong></p>
<p><strong>Redirect the message to</strong> &gt; <strong>these recipients</strong></p></td>
<td><p><em>RedirectMessageTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Redirects the message to the specified recipients. The message isn't delivered to the original recipients, and no notification is sent to the sender or the original recipients.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Reject the message with the explanation</strong></p>
<p><strong>Block the message</strong> &gt; <strong>reject the message and include an explanation</strong></p></td>
<td><p><em>RejectMessageReasonText</em></p></td>
<td><p><code>String</code></p></td>
<td><p>Returns the message to the sender in a non-delivery report (also known as an NDR or bounce message) with the specified text as the rejection reason. The recipient doesn't receive the original message or notification.</p>
<p>The default enhanced status code that's used is <code>5.7.1</code>.</p>
<p>When you create or modify the rule in the Exchange Management Shell, you can specify the DSN code by using the <em>RejectMessageEnhancedStatusCode</em> parameter.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Reject the message with the enhanced status code</strong></p>
<p><strong>Block the message</strong> &gt; <strong>reject the message with the enhanced status code of</strong></p></td>
<td><p><em>RejectMessageEnhancedStatusCode</em></p></td>
<td><p><code>DSNEnhancedStatusCode</code></p></td>
<td><p>Returns the message to the sender in an NDR with the specified enhanced delivery status notification (DSN) code. The recipient doesn't receive the original message or notification.</p>
<p>Valid DSN codes are <code>5.7.1</code> or <code>5.7.900</code> through <code>5.7.999</code>.</p>
<p>The default reason text that's used is <code>Delivery not authorized, message refused</code>.</p>
<p>When you create or modify the rule in the Exchange Management Shell, you can specify the rejection reason text by using the <em>RejectMessageReasonText</em> parameter.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Delete the message without notifying anyone</strong></p>
<p><strong>Block the message</strong> &gt; <strong>delete the message without notifying anyone</strong></p></td>
<td><p><em>DeleteMessage</em></p></td>
<td><p>n/a</p></td>
<td><p>Silently drops the message without sending a notification to the recipient or the sender.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Add recipients to the Bcc box</strong></p>
<p><strong>Add recipients</strong> &gt; <strong>to the Bcc box</strong></p></td>
<td><p><em>BlindCopyTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>Bcc</strong> field of the message. The original recipients aren't notified, and they can't see the additional addresses.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Add recipients to the To box</strong></p>
<p><strong>Add recipients</strong> &gt; <strong>to the To box</strong></p></td>
<td><p><em>AddToRecipients</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>To</strong> field of the message. The original recipients can see the additional addresses.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Add recipients to the Cc box</strong></p>
<p><strong>Add recipients</strong> &gt; <strong>to the Cc box</strong></p></td>
<td><p><em>CopyTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>Cc</strong> field of the message. The original recipients can see the additional address.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Add the sender's manager as a recipient</strong></p>
<p><strong>Add recipients</strong> &gt; <strong>add the sender's manager as a recipient</strong></p></td>
<td><p><em>AddManagerAsRecipientType</em></p></td>
<td><p><code>AddedManagerAction</code></p></td>
<td><p>Adds the sender's manager to the message as the specified recipient type (<strong>To</strong>, <strong>Cc</strong>, <strong>Bcc</strong>), or redirects the message to the sender's manager without notifying the sender or the recipient.</p>
<p>This action only works if the sender's <strong>Manager</strong> attribute is defined in Active Directory.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Append the disclaimer</strong></p>
<p><strong>Apply a disclaimer to the message</strong> &gt; <strong>append a disclaimer</strong></p></td>
<td><p><em>ApplyHtmlDisclaimerText</em></p>
<p><em>ApplyHtmlDisclaimerFallbackAction</em></p>
<p><em>ApplyHtmlDisclaimerTextLocation</em></p></td>
<td><p>First property: <code>DisclaimerText</code></p>
<p>Second property: <code>DisclaimerFallbackAction</code></p>
<p>Third property (Exchange Management Shell only): <code>DisclaimerTextLocation</code></p></td>
<td><p>Applies the specified HTML disclaimer to the end of the message.</p>
<p>When you create or modify the rule in the Exchange Management Shell, use the <em>ApplyHtmlDisclaimerTextLocation</em> parameter with the value <code>Append</code>.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Prepend the disclaimer</strong></p>
<p><strong>Apply a disclaimer to the message</strong> &gt; <strong>prepend a disclaimer</strong></p></td>
<td><p><em>ApplyHtmlDisclaimerText</em></p>
<p><em>ApplyHtmlDisclaimerFallbackAction</em></p>
<p><em>ApplyHtmlDisclaimerTextLocation</em></p></td>
<td><p>First property: <code>DisclaimerText</code></p>
<p>Second property: <code>DisclaimerFallbackAction</code></p>
<p>Third property (Exchange Management Shell only): <code>DisclaimerTextLocation</code></p></td>
<td><p>Applies the specified HTML disclaimer to the beginning of the message.</p>
<p>When you create or modify the rule in the Exchange Management Shell, use the <em>ApplyHtmlDisclaimerTextLocation</em> parameter with the value <code>Prepend</code>.</p>
<p></p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Remove this header</strong></p>
<p><strong>Modify the message properties</strong> &gt; <strong>remove a message header</strong></p></td>
<td><p><em>RemoveHeader</em></p></td>
<td><p><code>MessageHeaderField</code></p></td>
<td><p>Removes the specified header field from the message header.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Set the message header to this value</strong></p>
<p><strong>Modify the message properties</strong> &gt; <strong>set a message header</strong></p></td>
<td><p><em>SetHeaderName</em></p>
<p><em>SetHeaderValue</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>String</code></p></td>
<td><p>Adds or modifies the specified header field in the message header, and sets the header field to the specified value.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Apply a message classification</strong></p>
<p><strong>Modify the message properties</strong> &gt; <strong>apply a message classification</strong></p></td>
<td><p><em>ApplyClassification</em></p></td>
<td><p><code>MessageClassification</code></p></td>
<td><p>Applies the specified message classification to the message.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Set the spam confidence level (SCL) to</strong></p>
<p><strong>Modify the message properties</strong> &gt; <strong>set the spam confidence level (SCL)</strong></p></td>
<td><p><em>SetSCL</em></p></td>
<td><p><code>SCLValue</code></p></td>
<td><p>Sets the spam confidence level (SCL) of the message to the specified value.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Apply rights protection to the message with</strong></p>
<p><strong>Modify the message security</strong> &gt; <strong>apply rights protection</strong></p></td>
<td><p><em>ApplyRightsProtectionTemplate</em></p></td>
<td><p><code>RMSTemplate</code></p></td>
<td><p>Applies the specified Rights Management Services (RMS) template to the message.</p>
<p>RMS requires Exchange Enterprise client access licenses (CALs) for each mailbox. For more information about CALs, see <a href="https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview">Exchange licensing FAQs</a>.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Require TLS encryption</strong></p>
<p><strong>Modify the message security</strong> &gt; <strong>require TLS encryption</strong></p></td>
<td><p><em>RouteMessageOutboundRequireTls</em></p></td>
<td><p><code>n/a</code></p></td>
<td><p>Forces the outbound messages to be routed over a TLS encrypted connection.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Prepend the subject of the message with</strong></p></td>
<td><p><em>PrependSubject</em></p></td>
<td><p><code>String</code></p></td>
<td><p>Adds the specified text to the beginning of the <strong>Subject</strong> field of the message. Consider using a space or a colon (:) as the last character of the specified text to differentiate it from the original subject text.</p>
<p>To prevent the same string from being added to messages that already contain the text in the subject (for example, replies), add the <strong>The subject includes</strong> (<em>ExceptIfSubjectContainsWords</em>) exception to the rule.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Notify the sender with a Policy Tip</strong></p></td>
<td><p><em>NotifySender</em></p>
<p><em>RejectMessageReasonText</em></p>
<p><em>RejectMessageEnhancedStatusCode</em> (Exchange Management Shell only)</p></td>
<td><p>First property: <code>NotifySenderType</code></p>
<p>Second property: <code>String</code></p>
<p>Third property (Exchange Management Shell only): <code>DSNEnhancedStatusCode</code></p></td>
<td><p>Notifies the sender or blocks the message when the message matches a DLP policy.</p>
<p>When you use this action, you need to use the <strong>The message contains sensitive information</strong> (<em>MessageContainsDataClassification</em> condition.</p>
<p>When you create or modify the rule in the Exchange Management Shell, the <em>RejectMessageReasonText</em> parameter is optional. If you don't use this parameter, the default text <code>Delivery not authorized, message refused</code> is used.</p>
<p>In the Exchange Management Shell, you can also use the <em>RejectMessageEnhancedStatusCode</em> parameter to specify the enhanced status code. If you don't use this parameter, the default enhanced status code <code>5.7.1</code> is used.</p>
<p>This action limits the other conditions, exceptions, and actions that you can configure in the rule.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Generate incident report and send it to</strong></p></td>
<td><p><em>GenerateIncidentReport</em></p>
<p><em>IncidentReportContent</em></p></td>
<td><p>First property: <code>Addresses</code></p>
<p>Second property: <code>IncidentReportContent</code></p></td>
<td><p>Sends an incident report that contains the specified content to the specified recipients.</p>
<p>An incident report is generated for messages that match data loss prevention (DLP) policies in your organization.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Properties of this rule</strong> section &gt; <strong>Audit this rule with severity level</strong></p></td>
<td><p><em>SetAuditSeverity</em></p></td>
<td><p><code>AuditSeverityLevel</code></p></td>
<td><p>Specifies whether to:</p>
<ul>
<li><p>Prevent the generation of an incident report and the corresponding entry in the message tracking log.</p></li>
<li><p>Generate an incident report and the corresponding entry in the message tracking log with the specified severity level (low, medium, or high).</p></li>
</ul></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Properties of this rule</strong> section &gt; <strong>Stop processing more rules</strong></p>
<p><strong>More options</strong> &gt; <strong>Properties of this rule</strong> section &gt; <strong>Stop processing more rules</strong></p></td>
<td><p><em>StopRuleProcessing</em></p></td>
<td><p>n/a</p></td>
<td><p>Specifies that after the message is affected by the rule, the message is exempt from processing by other rules.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Actions for transport rules on Edge Transport servers

A small subset of actions that are available on Mailbox servers are also available on Edge Transport servers, but there are also some actions that are only available on Edge Transport servers. There's no EAC on Edge Transport servers, so you can only manage transport rules in the Exchange Management Shell on the local Edge Transport server. The actions are described in the following table. The properties types are described in the Property values section.

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
<th>Action parameter in the Exchange Management Shell</th>
<th>Property</th>
<th>Description</th>
<th>Available on</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>AddToRecipients</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>To</strong> field of the message. The original recipients can see the additional addresses.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>BlindCopyTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>Bcc</strong> field of the message. The original recipients aren't notified, and they can't see the additional addresses.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>CopyTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Adds one or more recipients to the <strong>Cc</strong> field of the message. The original recipients can see the additional address.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>DeleteMessage</em></p></td>
<td><p>n/a</p></td>
<td><p>Silently drops the message without sending a notification to the recipient or the sender.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>Disconnect</em></p></td>
<td><p>n/a</p></td>
<td><p>Ends the SMTP connection between the sending server and the Edge Transport server without generating an NDR.</p></td>
<td><p>Edge Transport servers only</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>LogEventText</em></p></td>
<td><p><code>String</code></p></td>
<td><p>Generates an event with the specified text in the Application log of the local Edge Transport server. The entry contains the following information:</p>
<ul>
<li><p><strong>Level</strong>   <code>Information</code></p></li>
<li><p><strong>Source</strong>   <code>MSExchange Messaging Policies</code></p></li>
<li><p><strong>Event ID</strong>   <code>4000</code></p></li>
<li><p><strong>Task Category</strong>   <code>Rules</code></p></li>
<li><p><strong>EventData</strong>   <code>The following message is logged by an action in the rules: &lt;text you specify&gt;.</code></p></li>
</ul></td>
<td><p>Edge Transport servers only</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>PrependSubject</em></p></td>
<td><p><code>String</code></p></td>
<td><p>Adds the specified text to the beginning of the <strong>Subject</strong> field of the message. Consider using a space or a colon (:) as the last character of the specified text to differentiate it from the original subject.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>Quarantine</em></p></td>
<td><p>n/a</p></td>
<td><p>Delivers the message to the quarantine mailbox that's defined in the content filtering configuration on the Edge Transport server. For more information, see <a href="configure-a-spam-quarantine-mailbox-exchange-2013-help.md">Configure a spam quarantine mailbox</a>.</p>
<p>If the quarantine mailbox isn't configured, the message is returned to the sender in an NDR.</p></td>
<td><p>Edge Transport servers only</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>RedirectMessageTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Redirects the message to the specified recipients. The message isn't delivered to the original recipients, and no notification is sent to the sender or the original recipients.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>RemoveHeader</em></p></td>
<td><p><code>MessageHeaderField</code></p></td>
<td><p>Removes the specified header field from the message header.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>SetHeaderName</em></p>
<p><em>SetHeaderValue</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>String</code></p></td>
<td><p>Adds or modifies the specified header field in the message header, and sets the header field to the specified value.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>SetSCL</em></p></td>
<td><p><code>SCLValue</code></p></td>
<td><p>Sets the SCL of the message to the specified value.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>SmtpRejectMessageRejectText</em></p>
<p><em>SmtpRejectMessageRejectStatusCode</em></p></td>
<td><p>First property: <code>String</code></p>
<p>Second property: <code>SMTPStatusCode</code></p></td>
<td><p>Ends the SMTP connection between the sending server and the Edge Transport server with the specified SMTP status code and the specified rejection text. The recipient doesn't receive the original message or notification.</p>
<p>Valid values for the SMTP status code are integers from <code>400</code> through <code>500</code> as defined in RFC 3463.</p>
<p>If you specify the rejection text without specifying the SMTP status code, the default code <code>550</code> is used.</p>
<p>If you specify the SMTP status code without specifying the rejection text, the text that's used is <code>Delivery not authorized, message refused</code>.</p></td>
<td><p>Edge Transport servers only</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>StopRuleProcessing</em></p></td>
<td><p>n/a</p></td>
<td><p>Specifies that after the message is affected by the rule, the message is exempt from processing by other rules.</p></td>
<td><p>Mailbox servers and Edge Transport servers</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Property values

The property values that are used for actions in transport rules are described in the following table.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Valid values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>AddedManagerAction</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>To</strong></p></li>
<li><p><strong>Cc</strong></p></li>
<li><p><strong>Bcc</strong></p></li>
<li><p><strong>Redirect</strong></p></li>
</ul></td>
<td><p>Specifies how to include the sender's manager in messages.</p>
<ul>
<li><p>If you select <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong>, the sender's manager is added as a recipient in the specified field.</p></li>
<li><p>If you select <strong>Redirect</strong>, the message is only delivered to the sender's manager without notifying the sender or the recipient.</p></li>
</ul>
<p>This action only works if the sender's <strong>Manager</strong> attribute is defined in Active Directory.</p></td>
</tr>
<tr class="even">
<td><p><code>Addresses</code></p></td>
<td><p>Exchange recipients</p></td>
<td><p>Depending on the action, you might be able to specify any mail-enabled object in the organization, or you might be limited to a specific object type. Typically, you can select multiple recipients, but you can only send an incident report to one recipient.</p></td>
</tr>
<tr class="odd">
<td><p><code>AuditSeverityLevel</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p>Uncheck <strong>Audit this rule with severity level</strong>, or select <strong>Audit this rule with severity level</strong> with the value <strong>Not specified</strong> (<code>DoNotAudit</code>)</p></li>
<li><p><strong>Low</strong></p></li>
<li><p><strong>Medium</strong></p></li>
<li><p><strong>High</strong></p></li>
</ul></td>
<td><p>The values <strong>Low</strong>, <strong>Medium</strong>, or <strong>High</strong> specify the severity level that's assigned to the incident report and to the corresponding entry in the message tracking log.</p>
<p>The other value prevents an incident report from being generated, and prevents the corresponding entry from being written to the message tracking log.</p></td>
</tr>
<tr class="even">
<td><p><code>DisclaimerFallbackAction</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>Wrap</strong></p></li>
<li><p><strong>Ignore</strong></p></li>
<li><p><strong>Reject</strong></p></li>
</ul></td>
<td><p>Specifies what to do if the disclaimer can't be applied to a message. There are situations where the contents of a message can't be altered (for example, the message is encrypted). The available fallback actions are:</p>
<ul>
<li><p><strong>Wrap</strong>   The original message is wrapped in a new message envelope, and the disclaimer text is inserted into the new message. This is the default value.</p>
<p><strong>Notes</strong>:</p>
<ul>
<li><p>Subsequent transport rules are applied to the new message envelope, not to the original message. Therefore, configure these rules with a lower priority than other rules.</p></li>
<li><p>If the original message can't be wrapped in a new message envelope, the original message isn't delivered. The message is returned to the sender in an NDR.</p></li>
</ul></li>
<li><p><strong>Ignore</strong>   The rule is ignored and the message is delivered without the disclaimer</p></li>
<li><p><strong>Reject</strong>   The message is returned to the sender in an NDR.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><code>DisclaimerText</code></p></td>
<td><p>HTML string</p></td>
<td><p>Specifies the disclaimer text, which can include HTML tags, inline cascading style sheet (CSS) tags, and images by using the IMG tag. The maximum length is 5000 characters, including tags.</p></td>
</tr>
<tr class="even">
<td><p><code>DisclaimerTextLocation</code></p></td>
<td><p>Single value: <code>Append</code> or <code>Prepend</code></p></td>
<td><p>In the Exchange Management Shell, you use the <em>ApplyHtmlDisclaimerTextLocation</em> to specify the location of the disclaimer text in the message:</p>
<ul>
<li><p><code>Append</code>   Add the disclaimer to the end of the message body. This is the default value.</p></li>
<li><p><code>Prepend</code>   Add the disclaimer to the beginning of the message body.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><code>DSNEnhancedStatusCode</code></p></td>
<td><p>Single DSN code value:</p>
<ul>
<li><p><code>5.7.1</code></p></li>
<li><p><code>5.7.900</code> through <code>5.7.999</code></p></li>
</ul></td>
<td><p>Specifies the DSN code that's used. You can create custom DSNs by using the <strong>New-SystemMessage</strong> cmdlet.</p>
<p>If you don't specify the rejection reason text along with the DSN code, the default reason text that's used is <code>Delivery not authorized, message refused</code>.</p>
<p>When you create or modify the rule in the Exchange Management Shell, you can specify the rejection reason text by using the <em>RejectMessageReasonText</em> parameter.</p></td>
</tr>
<tr class="even">
<td><p><code>IncidentReportContent</code></p></td>
<td><p>One or more of the following values:</p>
<ul>
<li><p><strong>Sender</strong></p></li>
<li><p><strong>Recipients</strong></p></li>
<li><p><strong>Subject</strong></p></li>
<li><p><strong>Cc'd recipients</strong> (<code>Cc</code>)</p></li>
<li><p><strong>Bcc'd recipients</strong> (<code>Bcc</code>)</p></li>
<li><p><strong>Severity</strong></p></li>
<li><p><strong>Sender override information</strong> (<code>Override</code>)</p></li>
<li><p><strong>Matching rules</strong> (<code>RuleDetections</code>)</p></li>
<li><p><strong>False positive reports</strong> (<code>FalsePositive</code>)</p></li>
<li><p><strong>Detected data classifications</strong> (<code>DataClassifications</code>)</p></li>
<li><p><strong>Matching content</strong> (<code>IdMatch</code>)</p></li>
<li><p><strong>Original mail</strong> (<code>AttachOriginalMail</code>)</p></li>
</ul></td>
<td><p>Specifies the original message properties to include in the incident report. You can choose to include any combination of these properties. In addition to the properties you specify, the message ID is always included. The available properties are:</p>
<ul>
<li><p><strong>Sender</strong>   The sender of the original message.</p></li>
<li><p><strong>Recipients</strong>, <strong>Cc'd recipients</strong> , and <strong>Bcc'd recipients</strong>   All recipients of the message, or only the recipients in the <strong>Cc</strong> or <strong>Bcc</strong> fields. For each property, only the first 10 recipients are included in the incident report.</p></li>
<li><p><strong>Subject</strong>   The <strong>Subject</strong> field of the original message.</p></li>
<li><p><strong>Severity</strong>   The audit severity of the rule that was triggered. Message tracking logs include all the audit severity levels, and can be filtered by audit severity. In the EAC, if you clear the <strong>Audit this rule with severity level</strong> check box (in the Exchange Management Shell, the <em>SetAuditSeverity</em> parameter value <code>DoNotAudit</code>), rule matches won't appear in the rule reports. If a message is processed by more than one rule, the highest severity is included in any incident reports.</p></li>
<li><p><strong>Sender override information</strong>   The override if the sender chose to override a Policy Tip. If the sender provided a justification, the first 100 characters of the justification are also included.</p></li>
<li><p><strong>Matching rules</strong>   The list of rules that the message triggered.</p></li>
<li><p><strong>False positive reports</strong>   The false positive if the sender marked the message as a false positive for a Policy Tip.</p></li>
<li><p><strong>Detected data classifications</strong>   The list of sensitive information types detected in the message.</p></li>
<li><p><strong>Matching content</strong>   The sensitive information type detected, the exact matched content from the message, and the 150 characters before and after the matched sensitive information.</p></li>
<li><p><strong>Original mail</strong>   The entire message that triggered the rule is attached to the incident report.</p></li>
</ul>
<p>In the Exchange Management Shell, you specify multiple values separated by commas.</p></td>
</tr>
<tr class="odd">
<td><p><code>MessageClassification</code></p></td>
<td><p>Single message classification object</p></td>
<td><p>In the EAC, you select from the list of available message classifications.</p>
<p>In the Exchange Management Shell, use the <strong>Get-MessageClassification</strong> cmdlet to see the message classification objects that are available.</p></td>
</tr>
<tr class="even">
<td><p><code>MessageHeaderField</code></p></td>
<td><p>Single string</p></td>
<td><p>Specifies the SMTP message header field to add, remove, or modify.</p>
<p>The <em>message header</em> is a collection of required and optional header fields in the message. Examples of header fields are <strong>To</strong>, <strong>From</strong>, <strong>Received</strong>, and <strong>Content-Type</strong>. Official header fields are defined in RFC 5322. Unofficial header fields start with <strong>X-</strong> and are known as <em>X-headers</em>.</p></td>
</tr>
<tr class="odd">
<td><p><code>NotificationMessageText</code></p></td>
<td><p>Any combination of plain text, HTML tags, and keywords</p></td>
<td><p>Specified the text to use in a recipient notification message.</p>
<p>In addition to plain text and HTML tags, you can specify the following keywords that use values from the original message:</p>
<ul>
<li><p><code>%%From%%</code></p></li>
<li><p><code>%%To%%</code></p></li>
<li><p><code>%%Cc%%</code></p></li>
<li><p><code>%%Subject%%</code></p></li>
<li><p><code>%%Headers%%</code></p></li>
<li><p><code>%%MessageDate%%</code></p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><code>NotifySenderType</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>Notify the sender, but allow them to send</strong> (<code>NotifyOnly</code>)</p></li>
<li><p><strong>Block the message</strong> (<code>RejectMessage</code>)</p></li>
<li><p><strong>Block the message unless it's a false positive</strong> (<code>RejectUnlessFalsePositiveOverride</code>)</p></li>
<li><p><strong>Block the message, but allow the sender to override and send</strong> (<code>RejectUnlessSilentOverride</code>)</p></li>
<li><p><strong>Block the message, but allow the sender to override with a business justification and send</strong> (<code>RejectUnlessExplicitOverride</code>)</p></li>
</ul></td>
<td><p>Specifies the type of Policy Tip that the sender receives if the message violates a DLP policy. The settings are described in the following list:</p>
<ul>
<li><p><strong>Notify the sender, but allow them to send</strong> The sender is notified, but the message is delivered normally.</p></li>
<li><p><strong>Block the message</strong> The message is rejected, and the sender is notified.</p></li>
<li><p><strong>Block the message unless it's a false positive</strong> The message is rejected unless it's marked as a false positive by the sender.</p></li>
<li><p><strong>Block the message, but allow the sender to override and send</strong> The message is rejected unless the sender has chosen to override the policy restriction.</p></li>
<li><p><strong>Block the message, but allow the sender to override with a business justification and send</strong> This is similar to <strong>Block the message, but allow the sender to override and send</strong> type, but the sender also provides a justification for overriding the policy restriction.</p></li>
</ul>
<p>When you use this action, you need to use the <strong>The message contains sensitive information</strong> (<em>MessageContainsDataClassification</em>) condition.</p></td>
</tr>
<tr class="odd">
<td><p><code>RMSTemplate</code></p></td>
<td><p>Single RMS template object</p></td>
<td><p>Specifies the Rights Management Services (RMS) template that's applied to the message.</p>
<p>In the EAC, you select the RMS template from a list.</p>
<p>In the Exchange Management Shell, use the <strong>Get-RMSTemplate</strong> cmdlet to see the RMS templates that are available.</p>
<p>RMS requires Exchange Enterprise client access licenses (CALs) for each mailbox. For more information about CALs, see <a href="https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview">Exchange licensing FAQs</a>.</p></td>
</tr>
<tr class="even">
<td><p><code>SCLValue</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>Bypass spam filtering</strong> (<code>-1</code>)</p></li>
<li><p>Integers 0 through 9</p></li>
</ul></td>
<td><p>Specifies the spam confidence level (SCL) that's assigned to the message. A higher SCL value indicates that a message is more likely to be spam.</p></td>
</tr>
<tr class="odd">
<td><p><code>String</code></p></td>
<td><p>Single string</p></td>
<td><p>Specifies the text that's applied to the specified message header field, NDR, or event log entry.</p>
<p>In the Exchange Management Shell, if the value contains spaces, enclose the value in quotation marks (&quot;).</p></td>
</tr>
</tbody>
</table>

## For more information

[Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md)

[Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)

[Transport rule conditions and exceptions (predicates) in Exchange 2013](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md)

[Organization-wide disclaimers, signatures, footers, or headers in Exchange 2013](organization-wide-disclaimers-signatures-footers-or-headers-exchange-2013-help.md)
