---
title: 'Transport rule conditions and exceptions (predicates) in Exchange 2013'
TOCTitle: Transport rule conditions and exceptions (predicates)
ms:assetid: c918ea00-1e68-4b8b-8d51-6966b4432e2d
ms:mtpsurl: https://technet.microsoft.com/library/Dd638183(v=EXCHG.150)
ms:contentKeyID: 49361079
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Transport rule conditions and exceptions (predicates) in Exchange 2013

_**Applies to:** Exchange Server 2013_

Conditions and exceptions in transport rules identify the messages that the rule is applied to or not applied to. For example, if the rule adds a disclaimer to messages, you can configure the rule to only apply to messages that contain specific words, messages sent by specific users, or to all messages except those sent by the members of a specific group. Collectively, the conditions and exceptions in transport rules are also known as *predicates*, because for every condition, there's a corresponding exception that uses the exact same settings and syntax. The only difference is conditions specify messages to include, while exceptions specify messages to exclude.

Most conditions and exceptions have one property that requires one or more values. For example, the **The sender is** condition requires the sender of the message. Some conditions have two properties. For example, the **A message header includes any of these words** condition requires one property to specify the message header field, and a second property to specify the text to look for in the header field. Some conditions or exceptions don't have any properties. For example, the **Any attachment has executable content** condition simply looks for attachments in messages that have executable content.

For more information about transport rules in Exchange Server 2013, see [Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

For more information about conditions and exceptions in transport rules in Exchange Online Protection or Exchange Online, see [Mail flow rule conditions and exceptions (predicates) in Exchange Online](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/conditions-and-exceptions).

## Conditions and exceptions for transport rules on Mailbox servers

The tables in the following sections describe the conditions and exceptions that are available in transport rules on Mailbox servers. The properties types are described in the Property types section.

Senders

Recipients

Message subject or body

Attachments

Any recipients

Message sensitive information types, To and Cc values, size, and character sets

Sender and recipient

Message properties

Message headers

**Notes**:

  - After you select a condition or exception in the Exchange admin center (EAC), the value that's ultimately shown in the **Apply this rule if** or **Except if** field is often different (shorter) than the click path value you selected. Also, when you create new rules based on a template (a filtered list of scenarios), you can often select a short condition name instead of following the complete click path. The short names and full click path values are shown in the EAC column in the tables.

  - If you select **\[Apply to all messages\]** in the EAC, you can't specify any other conditions. The equivalent in the Exchange Management Shell is to create a rule without specifying any condition parameters.

  - The settings and properties are the same in conditions and exceptions, so the output of the **Get-TransportRulePredicate** cmdlet doesn't list exceptions separately. Also, the names of some of the predicates that are returned by this cmdlet are different than the corresponding parameter names, and a predicate might require multiple parameters.

## Senders

For conditions and exceptions that examine the sender's address, you can specify where rule looks for the sender's address.

In the EAC, in the **Properties of this rule** section, click **Match sender address in message**. Note that you might need to click **More options** to see this setting. In the Exchange Management Shell, the parameter is *SenderAddressLocation*. The available values are:

  - **Header**: Only examine senders in the message headers (for example, the **From**, **Sender**, or **Reply-To** fields). This is the default value, and is the way transport rules worked before Exchange 2013 Cumulative Update 1 (CU1).

  - **Envelope**: Only examine senders from the message envelope (the **MAIL FROM** value that was used in the SMTP transmission, which is typically stored in the **Return-Path** field). Note that message envelope searching is only available for the following conditions (and the corresponding exceptions):

      - **The sender is** (*From*)

      - **The sender is a member of** (*FromMemberOf*)

      - **The sender address includes** (*FromAddressContainsWords*)

      - **The sender address matches** (*FromAddressMatchesPatterns*)

      - **The sender's domain is** (*SenderDomainIs*)

  - **Header or envelope** (`HeaderOrEnvelope`)   Examine senders in the message header and the message envelope.

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The sender is</strong></p>
<p><strong>The sender</strong> &gt; <strong>is this person</strong></p></td>
<td><p><em>From</em></p>
<p><em>ExceptIfFrom</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages that are sent by the specified mailboxes, mail users, or mail contacts in the Exchange organization.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender is located</strong></p>
<p><strong>The sender</strong> &gt; <strong>is external/internal</strong></p></td>
<td><p><em>FromScope</em></p>
<p><em>ExceptIfFromScope</em></p></td>
<td><p><code>UserScopeFrom</code></p></td>
<td><p>Messages that are sent by either internal senders or external senders.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The sender is a member of</strong></p>
<p><strong>The sender</strong> &gt; <strong>is a member of this group</strong></p></td>
<td><p><em>FromMemberOf</em></p>
<p><em>ExceptIfFromMemberOf</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages that are sent by a member of the specified group.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender address includes</strong></p>
<p><strong>The sender</strong> &gt; <strong>address includes any of these words</strong></p></td>
<td><p><em>FromAddressContainsWords</em></p>
<p><em>ExceptIfFromAddressContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the sender's email address.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The sender address matches</strong></p>
<p><strong>The sender</strong> &gt; <strong>address matches any of these text patterns</strong></p></td>
<td><p><em>FromAddressMatchesPatterns</em></p>
<p><em>ExceptIfFromAddressMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the sender's email address contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender's specified properties include any of these words</strong></p>
<p><strong>The sender</strong> &gt; <strong>has specific properties including any of these words</strong></p></td>
<td><p><em>SenderADAttributeContainsWords</em></p>
<p><em>ExceptIfSenderADAttributeContainsWords</em></p></td>
<td><p>First property: <code>ADAttribute</code></p>
<p>Second property: <code>Words</code></p></td>
<td><p>Messages where the specified Active Directory attribute of the sender contains any of the specified words.</p>
<p>Note that the <strong>Country</strong> attribute requires the two-letter country code value (for example, DE for Germany).</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The sender's specified properties match these text patterns</strong></p>
<p><strong>The sender</strong> &gt; <strong>has specific properties matching these text patterns</strong></p></td>
<td><p><em>SenderADAttributeMatchesPatterns</em></p>
<p><em>ExceptIfSenderADAttributeMatchesPatterns</em></p></td>
<td><p>First property: <code>ADAttribute</code></p>
<p>Second property: <code>Patterns</code></p></td>
<td><p>Messages where the specified Active Directory attribute of the sender contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender has overridden the Policy Tip</strong></p>
<p><strong>The sender</strong> &gt; <strong>has overridden the Policy Tip</strong></p></td>
<td><p><em>HasSenderOverride</em></p>
<p><em>ExceptIfHasSenderOverride</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages where the sender has chosen to override a data loss prevention (DLP) policy. For more information about DLP policies, see <a href="https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention">Data loss prevention</a>.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Sender's IP address is in the range</strong></p>
<p><strong>The sender</strong> &gt; <strong>IP address is in any of these ranges or exactly matches</strong></p></td>
<td><p><em>SenderIPRanges</em></p>
<p><em>ExceptIfSenderIPRanges</em></p></td>
<td><p><code>IPAddressRanges</code></p></td>
<td><p>Messages where the sender's IP address matches the specified IP address, or falls within the specified IP address range.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender's domain is</strong></p>
<p><strong>The sender</strong> &gt; <strong>domain is</strong></p></td>
<td><p><em>SenderDomainIs</em></p>
<p><em>ExceptIfSenderDomainIs</em></p></td>
<td><p><code>DomainName</code></p></td>
<td><p>Messages where the domain of the sender's email address matches the specified value.</p>
<p>If you need to find sender domains that <em>contain</em> the specified domain (for example, any subdomain of a domain), use <strong>The sender address matches</strong> (<em>FromAddressMatchesPatterns</em>) condition and specify the domain by using the syntax: <code>'@domain\.com$'</code>.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Recipients

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The recipient is</strong></p>
<p><strong>The recipient</strong> &gt; <strong>is this person</strong></p></td>
<td><p><em>SentTo</em></p>
<p><em>ExceptIfSentTo</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where one of the recipients is the specified mailbox, mail user, or mail contact in the Exchange organization. The recipients can be in the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields of the message.</p>
<p><strong>Note:</strong> You can't specify distribution groups or mail-enabled security groups. If you need to take action on messages that are sent to a group, use the <strong>To box contains</strong> (<em>AnyOfToHeader</em>) condition instead.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The recipient is located</strong></p>
<p><strong>The recipient</strong> &gt; <strong>is external/external</strong></p></td>
<td><p><em>SentToScope</em></p>
<p><em>ExceptIfSentToScope</em></p></td>
<td><p><code>UserScopeTo</code></p></td>
<td><p>Messages that are sent to internal recipients, external recipients, external recipients in partner organizations, or external recipients in non-partner organizations.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The recipient is a member of</strong></p>
<p><strong>The recipient</strong> &gt; <strong>is a member of this group</strong></p></td>
<td><p><em>SentToMemberOf</em></p>
<p><em>ExceptIfSentToMemberOf</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages that contain recipients who are members of the specified group. The group can be in the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields of the message.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The recipient address includes</strong></p>
<p><strong>The recipient</strong> &gt; <strong>address includes any of these words</strong></p></td>
<td><p><em>RecipientAddressContainsWords</em></p>
<p><em>ExceptIfRecipientAddressContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the recipient's email address.</p>
<p><strong>Note:</strong> This condition doesn't consider messages that are sent to recipient proxy addresses. It only matches messages that are sent to the recipient's primary email address.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The recipient address matches</strong></p>
<p><strong>The recipient</strong> &gt; <strong>address matches any of these text patterns</strong></p></td>
<td><p><em>RecipientAddressMatchesPatterns</em></p>
<p><em>ExceptIfRecipientAddressMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where a recipient's email address contains text patterns that match the specified regular expressions.</p>
<p><strong>Note:</strong> This condition doesn't consider messages that are sent to recipient proxy addresses. It only matches messages that are sent to the recipient's primary email address.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The recipient's specified properties include any of these words</strong></p>
<p><strong>The recipient</strong> &gt; <strong>has specific properties including any of these words</strong></p></td>
<td><p><em>RecipientADAttributeContainsWords</em></p>
<p><em>ExceptIfRecipientADAttributeContainsWords</em></p></td>
<td><p>First property: <code>ADAttribute</code></p>
<p>Second property: <code>Words</code></p></td>
<td><p>Messages where the specified Active Directory attribute of a recipient contains any of the specified words.</p>
<p>Note that the <strong>Country</strong> attribute requires the two-letter country code value (for example, DE for Germany).</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The recipient's specified properties match these text patterns</strong></p>
<p><strong>The recipient</strong> &gt; <strong>has specific properties matching these text patterns</strong></p></td>
<td><p><em>RecipientADAttributeMatchesPatterns</em></p>
<p><em>ExceptIfRecipientADAttributeMatchesPatterns</em></p></td>
<td><p>First property: <code>ADAttribute</code></p>
<p>Second property: <code>Patterns</code></p></td>
<td><p>Messages where the specified Active Directory attribute of a recipient contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>A recipient's domain is</strong></p>
<p><strong>The recipient</strong> &gt; <strong>domain is</strong></p></td>
<td><p><em>RecipientDomainIs</em></p>
<p><em>ExceptIfRecipientDomainIs</em></p></td>
<td><p><code>DomainName</code></p></td>
<td><p>Messages where the domain of a recipient's email address matches the specified value.</p>
<p>If you need to find recipient domains that <em>contain</em> the specified domain (for example, any subdomain of a domain), use <strong>The recipient address matches</strong> (<em>RecipientAddressMatchesPatterns</em>) condition, and specify the domain by using the syntax <code>'@domain\.com$'</code>.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Message subject or body

> [!NOTE]
> The search for words or text patterns in the subject or other header fields in the message occurs <EM>after</EM> the message has been decoded from the MIME content transfer encoding method that was used to transmit the binary message between SMTP servers in ASCII text. You can't use conditions or exceptions to search for the raw (typically, Base64) encoded values of the subject or other header fields in messages.

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The subject or body includes</strong></p>
<p><strong>The subject or body</strong> &gt; <strong>subject or body includes any of these words</strong></p></td>
<td><p><em>SubjectOrBodyContainsWords</em></p>
<p><em>ExceptIfSubjectOrBodyContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that have the specified words in the <strong>Subject</strong> field or message body.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The subject or body matches</strong></p>
<p><strong>The subject or body</strong> &gt; <strong>subject or body matches these text patterns</strong></p></td>
<td><p><em>SubjectOrBodyMatchesPatterns</em></p>
<p><em>ExceptIfSubjectOrBodyMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>Subject</strong> field or message body contain text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The subject includes</strong></p>
<p><strong>The subject or body</strong> &gt; <strong>subject includes any of these words</strong></p></td>
<td><p><em>SubjectContainsWords</em></p>
<p><em>ExceptIfSubjectContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that have the specified words in the <strong>Subject</strong> field.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The subject matches</strong></p>
<p><strong>The subject or body</strong> &gt; <strong>subject matches these text patterns</strong></p></td>
<td><p><em>SubjectMatchesPatterns</em></p>
<p><em>ExceptIfSubjectMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>Subject</strong> field contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
</tbody>
</table>

## Attachments

For more information about how transport rules inspect message attachments, see [Use transport rules to inspect message attachments in Exchange 2013](use-transport-rules-to-inspect-message-attachments-exchange-2013-help.md).

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Any attachment's content includes</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>content includes any of these words</strong></p></td>
<td><p><em>AttachmentContainsWords</em></p>
<p><em>ExceptIfAttachmentContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages where an attachment contains the specified words.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachments content matches</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>content matches these text patterns</strong></p></td>
<td><p><em>AttachmentMatchesPatterns</em></p>
<p><em>ExceptIfAttachmentMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where an attachment contains text patterns that match the specified regular expressions.</p>
<p><strong>Note:</strong> Only the first 150 kilobytes (KB) of the attachments are scanned.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Any attachment's content can't be inspected</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>content can't be inspected</strong></p></td>
<td><p><em>AttachmentIsUnsupported</em></p>
<p><em>ExceptIfAttachmentIsUnsupported</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages where an attachment isn't natively recognized by Exchange, and the required IFilter isn't installed on the Mailbox server. For more information, see <a href="register-filter-pack-ifilters-with-exchange-2013-exchange-2013-help.md">Register Filter Pack IFilters with Exchange 2013</a>.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment's file name matches</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>file name matches these text patterns</strong></p></td>
<td><p><em>AttachmentNameMatchesPatterns</em></p>
<p><em>ExceptIfAttachmentNameMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where an attachment's file name contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Any attachment's file extension matches</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>file extension includes these words</strong></p></td>
<td><p><em>AttachmentExtensionMatchesWords</em></p>
<p><em>ExceptIfAttachmentExtensionMatchesWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages where an attachment's file extension matches any of the specified words.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment is greater than or equal to</strong></p>
<p><strong>Any attachment &gt; size is greater than or equal to</strong></p></td>
<td><p><em>AttachmentSizeOver</em></p>
<p><em>ExceptIfAttachmentSizeOver</em></p></td>
<td><p><code>Size</code></p></td>
<td><p>Messages where any attachment is greater than or equal to the specified value.</p>
<p>In the EAC, you can only specify the size in kilobytes (KB).</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The message didn't complete scanning</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>didn't complete scanning</strong></p></td>
<td><p><em>AttachmentProcessingLimitExceeded</em></p>
<p><em>ExceptIfAttachmentProcessingLimitExceeded</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages where the rules engine couldn't complete the scanning of the attachments. You can use this condition to create rules that work together to identify and process messages where the content couldn't be fully scanned.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment has executable content</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>has executable content</strong></p></td>
<td><p><em>AttachmentHasExecutableContent</em></p>
<p><em>ExceptIfAttachmentHasExecutableContent</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages where an attachment is an executable file. The system inspects the file's properties rather than relying on the file's extension.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>Any attachment is password protected</strong></p>
<p><strong>Any attachment</strong> &gt; <strong>is password protected</strong></p></td>
<td><p><em>AttachmentIsPasswordProtected</em></p>
<p><em>ExceptIfAttachmentIsPasswordProtected</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages where an attachment is password protected (and therefore can't be scanned). Password detection only works for Office documents and .zip files.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Any recipients

The conditions and exceptions in this section provide a unique capability that affects *all* recipients when the message contains at least one of the specified recipients. For example, let's say you have a rule that rejects messages. If you use a recipient condition from the Recipients section, the message is only rejected for those specified recipients. For example, if the rule finds the specified recipient in a message, but the message contains five other recipients. The message is rejected for that one recipient, and is delivered to the five other recipients.

If you add a recipient condition from this section, that same message is rejected for the detected recipient and the five other recipients.

Conversely, a recipient exception from this section *prevents* the rule action from being applied to *all* recipients of the message, not just for the detected recipients.

**Note**: This condition doesn't consider messages that are sent to recipient proxy addresses. It only matches messages that are sent to the recipient's primary email address.

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Any recipient address includes</strong></p>
<p><strong>Any recipient</strong> &gt; <strong>address includes any of these words</strong></p></td>
<td><p><em>AnyOfRecipientAddressContainsWords</em></p>
<p><em>ExceptIfAnyOfRecipientAddressContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields of the message.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>Any recipient address matches</strong></p>
<p><strong>Any recipient</strong> &gt; <strong>address matches any of these text patterns</strong></p></td>
<td><p><em>AnyOfRecipientAddressMatchesPatterns</em></p>
<p><em>ExceptIfAnyOfRecipientAddressMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields contain text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Message sensitive information types, To and Cc values, size, and character sets

The conditions in this section that look for values in the **To** and **Cc** fields behave like the conditions in the Any recipients section (*all* recipients of the message are affected by the rule, not just the detected recipients).

**Note**: This condition doesn't consider messages that are sent to recipient proxy addresses. It only matches messages that are sent to the recipient's primary email address.

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The message contains sensitive information</strong></p>
<p><strong>The message</strong> &gt; <strong>contains any of these types of sensitive information</strong></p></td>
<td><p><em>MessageContainsDataClassifications</em></p>
<p><em>ExceptIfMessageContainsDataClassifications</em></p></td>
<td><p><code>SensitiveInformationTypes</code></p></td>
<td><p>Messages that contain sensitive information as defined by data loss prevention (DLP) policies.</p>
<p>This condition is required for rules that use the <strong>Notify the sender with a Policy Tip</strong> (<em>NotifySender</em>) action.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The To box contains</strong></p>
<p><strong>The message</strong> &gt; <strong>To box contains this person</strong></p></td>
<td><p><em>AnyOfToHeader</em></p>
<p><em>ExceptIfAnyOfToHeader</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>To</strong> field includes any of the specified recipients.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The To box contains a member of</strong></p>
<p><strong>The message</strong> &gt; <strong>To box contains a member of this group</strong></p></td>
<td><p><em>AnyOfToHeaderMemberOf</em></p>
<p><em>ExceptIfAnyOfToHeaderMemberOf</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>To</strong> field contains a recipient who is a member of the specified group.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The Cc box contains</strong></p>
<p><strong>The message</strong> &gt; <strong>Cc box contains this person</strong></p></td>
<td><p><em>AnyOfCcHeader</em></p>
<p><em>ExceptIfAnyOfCcHeader</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>Cc</strong> field includes any of the specified recipients.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The Cc box contains a member of</strong></p>
<p><strong>The message</strong> &gt; <strong>contains a member of this group</strong></p></td>
<td><p><em>AnyOfCcHeaderMemberOf</em></p>
<p><em>ExceptIfAnyOfCcHeaderMemberOf</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>Cc</strong> field contains a recipient who is a member of the specified group.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The To or Cc box contains</strong></p>
<p><strong>The message</strong> &gt; <strong>To or Cc box contains this person</strong></p></td>
<td><p><em>AnyOfToCcHeader</em></p>
<p><em>ExceptIfAnyOfToCcHeader</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>To</strong> or <strong>Cc</strong> fields contain any of the specified recipients.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The To or Cc box contains a member of</strong></p>
<p><strong>The message</strong> &gt; <strong>To or Cc box contains a member of this group</strong></p></td>
<td><p><em>AnyOfToCcHeaderMemberOf</em></p>
<p><em>ExceptIfAnyOfToCcHeaderMemberOf</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages where the <strong>To</strong> or <strong>Cc</strong> fields contain a recipient who is a member of the specified group.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The message size is greater than or equal to</strong></p>
<p><strong>The message</strong> &gt; <strong>size is greater than or equal to</strong></p></td>
<td><p><em>MessageSizeOver</em></p>
<p><em>ExceptIfMessageSizeOver</em></p></td>
<td><p><code>Size</code></p></td>
<td><p>Messages where the total size (message plus attachments) is greater than or equal to the specified value.</p>
<p>In the EAC, you can only specify the size in kilobytes (KB).</p>
<p><strong>Note:</strong> Message size limits on mailboxes are evaluated before transport rules. A message that's too large for a mailbox will be rejected before a rule with this condition is able to act on the message.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The message character set name includes any of these words</strong></p>
<p><strong>The message</strong> &gt; <strong>character set name includes any of these words</strong></p></td>
<td><p><em>ContentCharacterSetContainsWords</em></p>
<p><em>ExceptIfContentCharacterSetContainsWords</em></p></td>
<td><p><code>CharacterSets</code></p></td>
<td><p>Messages that have any of the specified character set names.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
</tbody>
</table>

## Sender and recipient

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The sender is one of the recipient's</strong></p>
<p><strong>The sender and the recipient</strong> &gt; <strong>the sender's relationship to a recipient is</strong></p></td>
<td><p><em>SenderManagementRelationship</em></p>
<p><em>ExceptIfSenderManagementRelationship</em></p></td>
<td><p><code>ManagementRelationship</code></p></td>
<td><p>Messages where the either sender is the manager of a recipient, or the sender is managed by a recipient.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The message is between members of these groups</strong></p>
<p><strong>The sender and the recipient</strong> &gt; <strong>the message is between members of these groups</strong></p></td>
<td><p><em>BetweenMemberOf1</em> and <em>BetweenMemberOf2</em></p>
<p><em>ExceptIfBetweenMemberOf1</em> and <em>ExceptIfBetweenMemberOf2</em></p></td>
<td><p><code>Addresses</code></p></td>
<td><p>Messages that are sent between members of the specified groups.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The manager of the sender or recipient is</strong></p>
<p><strong>The sender and the recipient</strong> &gt; <strong>the manager of the sender or recipient is this person</strong></p></td>
<td><p><em>ManagerForEvaluatedUser</em> and <em>ManagerAddress</em></p>
<p><em>ExceptIfManagerForEvaluatedUser</em> and <em>ExceptIfManagerAddress</em></p></td>
<td><p>First property: <code>EvaluatedUser</code></p>
<p>Second property: <code>Addresses</code></p></td>
<td><p>Messages where either a specified user is the manager of the sender, or a specified user is the manager of a recipient.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The sender's and any recipient's property compares as</strong></p>
<p><strong>The sender and the recipient</strong> &gt; <strong>the sender and recipient property compares as</strong></p></td>
<td><p><em>ADAttributeComparisonAttribute</em> and <em>ADComparisonOperator</em></p>
<p><em>ExceptIfADAttributeComparisonAttribute</em> and <em>ExceptIfADComparisonOperator</em></p></td>
<td><p>First property: <code>ADAttribute</code></p>
<p>Second property: <code>Evaluation</code></p></td>
<td><p>Messages where the specified Active Directory attribute for the sender and recipient either match or don't match.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
</tbody>
</table>

## Message properties

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>The message type is</strong></p>
<p><strong>The message properties</strong> &gt; <strong>include the message type</strong></p></td>
<td><p><em>MessageTypeMatches</em></p>
<p><em>ExceptIfMessageTypeMatches</em></p></td>
<td><p><code>MessageType</code></p></td>
<td><p>Messages of the specified type.</p>

> [!NOTE]
> When Outlook or Outlook Web App is configured to forward a message, the <STRONG>ForwardingSmtpAddress</STRONG> property is added to the message. The message type isn't changed to <CODE>AutoForward</CODE>.

</td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The message is classified as</strong></p>
<p><strong>The message properties</strong> &gt; <strong>include this classification</strong></p></td>
<td><p><em>HasClassification</em></p>
<p><em>ExceptIfHasClassification</em></p></td>
<td><p><code>MessageClassification</code></p></td>
<td><p>Messages that have the specified message classification. This is a custom message classification that you can create in your organization by using the <strong>New-MessageClassification</strong> cmdlet.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The message isn't marked with any classifications</strong></p>
<p><strong>The message properties</strong> &gt; <strong>don't include any classification</strong></p></td>
<td><p><em>HasNoClassification</em></p>
<p><em>ExceptIfHasNoClassification</em></p></td>
<td><p>n/a</p></td>
<td><p>Messages that don't have a message classification.</p></td>
<td><p>Exchange 2010 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>The message has an SCL greater than or equal to</strong></p>
<p><strong>The message properties</strong> &gt; <strong>include an SCL greater than or equal to</strong></p></td>
<td><p><em>SCLOver</em></p>
<p><em>ExceptIfSCLOver</em></p></td>
<td><p><code>SCLValue</code></p></td>
<td><p>Messages that are assigned a spam confidence level (SCL) that's greater than or equal to the specified value.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><strong>The message importance is set to</strong></p>
<p><strong>The message properties</strong> &gt; <strong>include the importance level</strong></p></td>
<td><p><em>WithImportance</em></p>
<p><em>ExceptIfWithImportance</em></p></td>
<td><p><code>Importance</code></p></td>
<td><p>Messages that are marked with the specified Importance level.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
</tbody>
</table>

## Message headers

> [!NOTE]
> The search for words or text patterns in the subject or other header fields in the message occurs <EM>after</EM> the message has been decoded from the MIME content transfer encoding method that was used to transmit the binary message between SMTP servers in ASCII text. You can't use conditions or exceptions to search for the raw (typically, Base64) encoded values of the subject or other header fields in messages.

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
<th>Condition or exception in the EAC</th>
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>A message header includes</strong></p>
<p><strong>A message header</strong> &gt; <strong>includes any of these words</strong></p></td>
<td><p><em>HeaderContainsMessageHeader</em> and <em>HeaderContainsWords</em></p>
<p><em>ExceptIfHeaderContainsMessageHeader</em> and <em>ExceptIfHeaderContainsWords</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>Words</code></p></td>
<td><p>Messages that contain the specified header field, and the value of that header field contains the specified words.</p>
<p>The name of the header field and the value of the header field are always used together.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><strong>A message header matches</strong></p>
<p><strong>A message header</strong> &gt; <strong>matches these text patterns</strong></p></td>
<td><p><em>HeaderMatchesMessageHeader</em> and <em>HeaderMatchesPatterns</em></p>
<p><em>ExceptIfHeaderMatchesMessageHeader</em> and <em>ExceptIfHeaderMatchesPatterns</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>Patterns</code></p></td>
<td><p>Messages that contain the specified header field, and the value of that header field contains the specified regular expressions.</p>
<p>The name of the header field and the value of the header field are always used together.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
</tbody>
</table>

## Conditions and exceptions for transport rules on Edge Transport servers

The conditions and exceptions that are available in transport rules on Edge Transport servers are a small subset of what's available on Mailbox servers. There's no EAC on Edge Transport servers, so you can only manage transport rules in the Exchange Management Shell on the local Edge Transport server. The conditions and exceptions are described in the following table. The properties types are described in the Property types section.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Condition and exception parameters in the Exchange Management Shell</th>
<th>Property type</th>
<th>Description</th>
<th>Available in</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>AnyOfRecipientAddressContainsWords</em></p>
<p><em>ExceptIfAnyOfRecipientAddressContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields.</p>
<p>When a message contains the specified recipient, the rule action is applied (or not applied) to <em>all</em> recipients of the message. For example, the message is rejected for all recipients of the message, not just for the specified recipient.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><em>AnyOfRecipientAddressMatchesPatterns</em></p>
<p><em>ExceptIfAnyOfRecipientAddressMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>To</strong>, <strong>Cc</strong>, or <strong>Bcc</strong> fields contain text patterns that match the specified regular expressions.</p>
<p>When a message contains the specified recipient, the rule action is applied (or not applied) to <em>all</em> recipients of the message. For example, the message is rejected for all recipients of the message, not just for the specified recipient.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>AttachmentSizeOver</em></p>
<p><em>ExceptIfAttachmentSizeOver</em></p></td>
<td><p><code>Size</code></p></td>
<td><p>Messages with attachments where any attachment is greater than or equal to the specified value.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>FromAddressContainsWords</em></p>
<p><em>ExceptIfFromAddressContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the sender's email address.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>FromAddressMatchesPatterns</em></p>
<p><em>ExceptIfFromAddressMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the sender's email address contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>FromScope</em></p>
<p><em>ExceptIfFromScope</em></p></td>
<td><p><code>UserScopeFrom</code></p></td>
<td><p>Messages that are sent by either internal senders or external senders.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>HeaderContainsMessageHeader</em> and <em>HeaderContainsWords</em></p>
<p><em>ExceptIfHeaderContainsMessageHeader</em> and <em>ExceptIfHeaderContainsWords</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>Words</code></p></td>
<td><p>Messages that contain the specified header field, and the value of that header field contains the specified words.</p>
<p>The name of the header field and the value of the header field are always used together.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>HeaderMatchesMessageHeader</em> and <em>HeaderMatchesPatterns</em></p>
<p><em>ExceptIfHeaderMatchesMessageHeader</em> and <em>ExceptIfHeaderMatchesPatterns</em></p></td>
<td><p>First property: <code>MessageHeaderField</code></p>
<p>Second property: <code>Patterns</code></p></td>
<td><p>Messages that contain the specified header field, and the value of that header field contains the specified regular expressions.</p>
<p>The name of the header field and the value of the header field are always used together.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>MessageSizeOver</em></p>
<p><em>ExceptIfMessageSizeOver</em></p></td>
<td><p><code>Size</code></p></td>
<td><p>Messages where the total size (message plus attachments) is greater than or equal to the specified value.</p></td>
<td><p>Exchange 2013 or later</p></td>
</tr>
<tr class="even">
<td><p><em>SCLOver</em></p>
<p><em>ExceptIfSCLOver</em></p></td>
<td><p><code>SCLValue</code></p></td>
<td><p>Messages that are assigned an SCL that's greater than or equal to the specified value.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>SubjectContainsWords</em></p>
<p><em>ExceptIfSubjectContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the <strong>Subject</strong> field.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>SubjectMatchesPatterns</em></p>
<p><em>ExceptIfSubjectMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>Subject</strong> field contains text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="odd">
<td><p><em>SubjectOrBodyContainsWords</em></p>
<p><em>ExceptIfSubjectOrBodyContainsWords</em></p></td>
<td><p><code>Words</code></p></td>
<td><p>Messages that contain the specified words in the <strong>Subject</strong> field or message body.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
<tr class="even">
<td><p><em>SubjectOrBodyMatchesPatterns</em></p>
<p><em>ExceptIfSubjectOrBodyMatchesPatterns</em></p></td>
<td><p><code>Patterns</code></p></td>
<td><p>Messages where the <strong>Subject</strong> field or message body contain text patterns that match the specified regular expressions.</p></td>
<td><p>Exchange 2007 or later</p></td>
</tr>
</tbody>
</table>

## Property types

The property types that are used in conditions and exceptions are described in the following table.

> [!NOTE]
> If the property is a string, trailing spaces are not allowed.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Property type</th>
<th>Valid values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>ADAttribute</code></p></td>
<td><p>Select from a predefined list of Active Directory attributes</p></td>
<td><p>UNRESOLVED_TOKENBLOCK_VAL(PD_Transport_Rules_ADAttributes_Snippet)</p>
<p>In the EAC, to specify multiple words or text patterns for the same attribute, separate the values with commas. For example, the value <strong>San Francisco,Palo Alto</strong> for the <strong>City</strong> attribute looks for &quot;City equals San Francisco&quot; or City equals Palo Alto&quot;.</p>
<p>In the Exchange Management Shell, use the syntax <code>&quot;AttributeName1:Value1,Value 2 with spaces,Value3...&quot;,&quot;AttributeName2:Word4,Value 5 with spaces,Value6...&quot;</code>, where <code>Value</code> is the word or text pattern that you want to match.</p>
<p>For example, <code>&quot;City:San Francisco,Palo Alto&quot;</code> or <code>&quot;City:San Francisco,Palo Alto&quot;</code>,<code>&quot;Department:Sales,Finance&quot;</code>.</p>
<p>When you specify multiple attributes, or multiple values for the same attribute, the <strong>or</strong> operator is used. Don't use values with leading or trailing spaces.</p>
<p>Note that the <strong>Country</strong> attribute requires the two-letter ISO 3166-1 country code value (for example, DE for Germany). To search for values, see <a href="https://go.microsoft.com/fwlink/p/?linkid=331680">https://go.microsoft.com/fwlink/p/?LinkId=331680</a>.</p></td>
</tr>
<tr class="even">
<td><p><code>Addresses</code></p></td>
<td><p>Exchange recipients</p></td>
<td><p>Depending on the nature of the condition or exception, you might be able to specify any mail-enabled object in the organization (for example, recipient-related conditions), or you might be limited to a specific object type (for example, groups for group membership conditions). And, the condition or exception might require one value, or allow multiple values.</p>
<p>In the Exchange Management Shell, separate multiple values by commas.</p>
<p><strong>Note:</strong> This condition doesn't consider messages that are sent to recipient proxy addresses. It only matches messages that are sent to the recipient's primary email address.</p></td>
</tr>
<tr class="odd">
<td><p><code>CharacterSets</code></p></td>
<td><p>Array of character set names</p></td>
<td><p>One or more content character sets that exist in a message. For example:</p>
<ul>
<li><p><code>Arabic/iso-8859-6</code></p></li>
<li><p><code>Chinese/big5</code></p></li>
<li><p><code>Chinese/euc-cn</code></p></li>
<li><p><code>Chinese/euc-tw</code></p></li>
<li><p><code>Chinese/gb2312</code></p></li>
<li><p><code>Chinese/iso-2022-cn</code></p></li>
<li><p><code>Cyrillic/iso-8859-5</code></p></li>
<li><p><code>Cyrillic/koi8-r</code></p></li>
<li><p><code>Cyrillic/windows-1251</code></p></li>
<li><p><code>Greek/iso-8859-7</code></p></li>
<li><p><code>Hebrew/iso-8859-8</code></p></li>
<li><p><code>Japanese/euc-jp</code></p></li>
<li><p><code>Japanese/iso-022-jp</code></p></li>
<li><p><code>Japanese/shift-jis</code></p></li>
<li><p><code>Korean/euc-kr</code></p></li>
<li><p><code>Korean/johab</code></p></li>
<li><p><code>Korean/ks_c_5601-1987</code></p></li>
<li><p><code>Turkish/windows-1254</code></p></li>
<li><p><code>Turkish/iso-8859-9</code></p></li>
<li><p><code>Vietnamese/tcvn</code></p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><code>DomainName</code></p></td>
<td><p>Array of SMTP domains</p></td>
<td><p>For example, <code>contoso.com</code> or <code>eu.contoso.com</code>.</p>
<p>In the Exchange Management Shell, you can specify multiple domains separated by commas.</p></td>
</tr>
<tr class="odd">
<td><p><code>EvaluatedUser</code></p></td>
<td><p>Single value of <strong>Sender</strong> or <strong>Recipient</strong></p></td>
<td><p>Specifies whether the rule is looking for the manager of the sender or the manager of the recipient.</p></td>
</tr>
<tr class="even">
<td><p><code>Evaluation</code></p></td>
<td><p>Single value of <strong>Equal</strong> or <strong>Not equal</strong> (<code>NotEqual</code>)</p></td>
<td><p>When comparing the Active Directory attribute of the sender and recipients, this specifies whether the values should match, or not match.</p></td>
</tr>
<tr class="odd">
<td><p><code>Importance</code></p></td>
<td><p>Single value of <strong>Low</strong>, <strong>Normal</strong>, or <strong>High</strong></p></td>
<td><p>The Importance level that was assigned to the message by the sender in Outlook or Outlook Web App.</p></td>
</tr>
<tr class="even">
<td><p><code>IPAddressRanges</code></p></td>
<td><p>Array of IP addresses or address ranges</p></td>
<td><p>You enter the IPv4 addresses using the following syntax:</p>
<ul>
<li><p><strong>Single IP address</strong>   For example, <code>192.168.1.1</code>.</p></li>
<li><p><strong>IP address range</strong>   For example, <code>192.168.0.1-192.168.0.254</code>.</p></li>
<li><p><strong>Classless InterDomain Routing (CIDR) IP address range</strong>   For example, <code>192.168.0.1/25</code>.</p></li>
</ul>
<p>In the Exchange Management Shell, you can specify multiple IP addresses or ranges separated by commas.</p></td>
</tr>
<tr class="odd">
<td><p><code>ManagementRelationship</code></p></td>
<td><p>Single value of <strong>Manager</strong> or <strong>Direct report</strong>(<code>DirectReport</code>)</p></td>
<td><p>Specifies the relationship between the sender and any of the recipients. The rule checks the <strong>Manager</strong> attribute in Active Directory to see if the sender is the manager of a recipient, or if the sender is managed by a recipient.</p></td>
</tr>
<tr class="even">
<td><p><code>MessageClassification</code></p></td>
<td><p>Single message classification</p></td>
<td><p>In the EAC, you select from the list of message classifications that you've created.</p>
<p>In the Exchange Management Shell, you use the <strong>Get-MessageClassification</strong> cmdlet to identify the message classification. For example, use the following command to search for messages with the <code>Company Internal</code> classification and prepend the message subject with the value <code>CompanyInternal</code>.</p>
<p><code>New-TransportRule &quot;Rule Name&quot; -HasClassification @(Get-MessageClassification &quot;Company Internal&quot;).Identity -PrependSubject &quot;CompanyInternal&quot;</code></p></td>
</tr>
<tr class="odd">
<td><p><code>MessageHeaderField</code></p></td>
<td><p>Single string</p></td>
<td><p>Specifies the name of the header field. The name of the header field is always paired with the value in the header field (word or text pattern match).</p>
<p>The <em>message header</em> is a collection of required and optional header fields in the message. Examples of header fields are <strong>To</strong>, <strong>From</strong>, <strong>Received</strong>, and <strong>Content-Type</strong>. Official header fields are defined in RFC 5322. Unofficial header fields start with <strong>X-</strong> and are known as <em>X-headers</em>.</p></td>
</tr>
<tr class="even">
<td><p><code>MessageType</code></p></td>
<td><p>Single message type value</p></td>
<td><p>Specifies one of the following message types:</p>
<ul>
<li><p><strong>Automatic reply</strong> (<code>OOF</code>)</p></li>
<li><p><strong>Auto-forward</strong> (<code>AutoForward</code>)</p></li>
<li><p><strong>Encrypted</strong></p></li>
<li><p><strong>Calendaring</strong></p></li>
<li><p><strong>Permission controlled</strong> (<code>PermissionControlled</code>)</p></li>
<li><p><strong>Voicemail</strong></p></li>
<li><p><strong>Signed</strong></p></li>
<li><p><strong>Approval request</strong> (<code>ApprovalRequest</code>)</p></li>
<li><p><strong>Read receipt</strong> (<code>ReadReceipt</code>)</p></li>
</ul>

> [!NOTE]
> When Outlook or Outlook Web App is configured to forward a message, the <STRONG>ForwardingSmtpAddress</STRONG> property is added to the message. The message type isn't changed to <CODE>AutoForward</CODE>.

</td>
</tr>
<tr class="odd">
<td><p><code>Patterns</code></p></td>
<td><p>Array of regular expressions</p></td>
<td><p>Specifies one or more regular expressions that are used to identify text patterns in values. For more information, see <a href="https://go.microsoft.com/fwlink/p/?linkid=180327">Regular Expression Syntax</a>.</p>
<p>In the Exchange Management Shell, you specify multiple regular expressions separated by commas, and you enclose each regular expression in quotation marks (&quot;).</p></td>
</tr>
<tr class="even">
<td><p><code>SCLValue</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>Bypass spam filtering</strong> (<code>-1</code>)</p></li>
<li><p>Integers 0 through 9</p></li>
</ul></td>
<td><p>Specifies the spam confidence level (SCL) that's assigned to a message. A higher SCL value indicates that a message is more likely to be spam.</p></td>
</tr>
<tr class="odd">
<td><p><code>SensitiveInformationTypes</code></p></td>
<td><p>Array of sensitive information types</p></td>
<td><p>Specifies one or more sensitive information types that are defined in your organization. For a list of built-in sensitive information types, see <a href="what-the-sensitive-information-types-in-exchange-look-for-exchange-2013-help.md">What the sensitive information types in Exchange 2013 look for</a>.</p>
<p>In the Exchange Management Shell, use the syntax <code>@{&lt;SensitiveInformationType1&gt;},@{&lt;SensitiveInformationType2&gt;},...</code>. For example, to look for content that contains at least two credit card numbers, and at least one ABA routing number, use the value <code>@{Name=&quot;Credit Card Number&quot;; minCount=&quot;2&quot;},@{Name=&quot;ABA Routing Number&quot;; minCount=&quot;1&quot;}</code>.</p></td>
</tr>
<tr class="even">
<td><p><code>Size</code></p></td>
<td><p>Single size value</p></td>
<td><p>Specifies the size of an attachment or the whole message.</p>
<p>In the EAC, you can only specify the size in kilobytes (KB).</p>
<p>In the Exchange Management Shell, when you enter a value, qualify the value with one of the following units:</p>
<ul>
<li><p><code>B</code> (bytes)</p></li>
<li><p><code>KB</code> (kilobytes)</p></li>
<li><p><code>MB</code> (megabytes)</p></li>
<li><p><code>GB</code> (gigabytes)</p></li>
</ul>
<p>For example, <code>20MB</code></p>
<p>Unqualified values are typically treated as bytes, but small values may be rounded up to the nearest kilobyte.</p></td>
</tr>
<tr class="odd">
<td><p><code>UserScopeFrom</code></p></td>
<td><p>Single value of <strong>Inside the organization</strong> (<code>InOrganization</code>) or <strong>Outside the organization</strong> (<code>NotInOrganization</code>)</p></td>
<td><p>A sender is considered to be inside the organization if either of the following conditions is true:</p>
<ul>
<li><p>The sender is a mailbox, mail user, group, or mail-enabled public folder that exists in the organization's Active Directory.</p></li>
<li><p>The sender's email address is in an accepted domain that's configured as an authoritative domain or an internal relay domain, <strong>and</strong> the message was sent or received over an authenticated connection. For more information about accepted domains, see <a href="accepted-domains-exchange-2013-help.md">Accepted domains</a>.</p></li>
</ul>
<p>A sender is considered to be outside the organization if either of the following conditions is true:</p>
<ul>
<li><p>The sender's email address isn't in an accepted domain.</p></li>
<li><p>The sender's email address is in an accepted domain that's configured as an external relay domain.</p></li>
</ul>

> [!NOTE]
> To determine whether mail contacts are considered to be inside or outside the organization, the sender's address is compared with the organization's accepted domains.

</td>
</tr>
<tr class="even">
<td><p><code>UserScopeTo</code></p></td>
<td><p>One of the following values:</p>
<ul>
<li><p><strong>Inside the organization</strong> (<code>InOrganization</code>)</p></li>
<li><p><strong>Outside the organization</strong> (<code>NotInOrganization</code>)</p></li>
<li><p><strong>In an external partner organization</strong> (<code>ExternalPartner</code>)</p></li>
<li><p><strong>In an external non-partner organization</strong> (<code>ExternalNonPartner</code>)</p></li>
</ul></td>
<td><p>A recipient is considered to be inside the organization if either of the following conditions is true:</p>
<ul>
<li><p>The recipient is a mailbox, mail user, group, or mail-enabled public folder that exists in the organization's Active Directory.</p></li>
<li><p>The recipient's email address is in an accepted domain that's configured as an authoritative domain or an internal relay domain, <strong>and</strong> the message was sent or received over an authenticated connection.</p></li>
</ul>
<p>A recipient is considered to be outside the organization if either of the following conditions is true:</p>
<ul>
<li><p>The recipient's email address isn't in an accepted domain.</p></li>
<li><p>The recipient's email address is in an accepted domain that's configured as an external relay domain.</p></li>
</ul>
<p>External partner organizations are external domains where you've configured Domain Security (mutual TLS authentication) to send mail.</p>
<p>External non-partner organizations are all other external domains that aren't considered partner domains.</p></td>
</tr>
<tr class="odd">
<td><p><code>Words</code></p></td>
<td><p>Array of strings</p></td>
<td><p>Specifies one or more words to look for. The words aren't case-sensitive, and can be surrounded by spaces and punctuation marks. Wildcards and partial matches aren't supported.</p>
<p>For example, &quot;contoso&quot; matches &quot; Contoso.&quot;. However, if the text is surrounded by other characters, it isn't considered a match. For example, &quot;contoso&quot; doesn't match the following values:</p>
<ul>
<li><p>Acontoso</p></li>
<li><p>Contosoa</p></li>
<li><p>Acontosob</p></li>
</ul>
<p>The asterisk (*) is treated as a literal character, and isn't used as a wildcard character.</p></td>
</tr>
</tbody>
</table>

## For more information

[Transport rules in Exchange 2013](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)

[Transport rule actions in Exchange 2013](mail-flow-rule-actions-in-exchange-2013-exchange-2013-help.md)

[Transport rule procedures in Exchange 2013](mail-flow-or-transport-rule-procedures-exchange-2013-help.md)

[Mail flow rule conditions and exceptions (predicates) in Exchange Online](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/conditions-and-exceptions)
