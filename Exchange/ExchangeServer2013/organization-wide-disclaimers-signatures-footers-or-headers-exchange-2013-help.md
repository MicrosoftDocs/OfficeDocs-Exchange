---
title: 'Organization-wide disclaimers, signatures, footers, or headers'
TOCTitle: Organization-wide disclaimers, signatures, footers, or headers
ms:assetid: e45e33c9-e53b-427c-ada5-70901bc399b8
ms:mtpsurl: https://technet.microsoft.com/library/Dn600437(v=EXCHG.150)
ms:contentKeyID: 61071241
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Organization-wide disclaimers, signatures, footers, or headers in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can add an email disclaimer, legal disclaimer, disclosure statement, signature, or other information to the top or bottom of email messages that enter or leave your organization. This might be needed for legal, business, or regulatory requirements, to identify potentially unsafe e-mail messages, or for other reasons unique to your organization.

To set up a disclaimer, you create a transport rule that includes the conditions, such when the sender is in a specific group or when the message includes specific text patterns, and the text to add. To apply multiple disclaimers to a single email message, you use multiple transport rules.

> [!IMPORTANT]
> <UL>
> <LI>
> <P>If you want the information to be added only to outgoing messages, you must add a condition such as recipients located outside the organization. By default, transport rules are applied to both incoming and outgoing messages.</P></LI></UL>

## Examples

Here are a few ideas for how to use disclaimers.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Sample text added</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Legal - outgoing messages</p></td>
<td><p>This email and any files transmitted with it are confidential and intended solely for the use of the individual or entity to whom they are addressed. If you have received this email in error, please notify the system manager.</p></td>
</tr>
<tr class="even">
<td><p>Legal - incoming messages</p></td>
<td><p>Employees are expressly required not to make defamatory statements and not to infringe or authorize any infringement of copyright or any other legal right by email communications. Employees who receive such an email must notify their supervisor immediately.</p></td>
</tr>
<tr class="odd">
<td><p>Notice that message was sent to an alias</p></td>
<td><p>This message was sent to the Sales discussion group.</p></td>
</tr>
<tr class="even">
<td><p>Signature - pulls in data for each employee</p></td>
<td><p>Kathleen Mayer<br/>
Sales Department<br/>
Contoso<br/>
www.contoso.com<br/>
kathleen@contoso.com<br/>
cell: 111-222-1234</p></td>
</tr>
<tr class="odd">
<td><p>Advertisement</p></td>
<td><p>Click here for March specials</p></td>
</tr>
</tbody>
</table>

The examples in this article are not intended for use as-is. Modify them for your needs.

## Scoping your disclaimer

As you work on your disclaimers, consider which messages they should apply to. For example, you might want different disclaimers for internal and external messages or for messages sent by users in specific departments. To make sure only the first message in a conversation gets a disclaimer, add an exception that looks for unique text in your disclaimer.

Here are some examples of the conditions and exceptions you can use.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Description</th>
<th>Conditions and exceptions in EAC</th>
<th>Conditions and exceptions in the Shell</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outside your organization, if the original message doesn't include text from your disclaimer, such as "CONTOSO LEGAL NOTICE"</p></td>
<td><p>Condition: <strong>The recipient is located</strong> &gt; <strong>Outside the organization</strong></p>
<p>Exception: <strong>The subject or body</strong> &gt; <strong>Subject or body matches these text patterns</strong> &gt; <strong>CONTOSO LEGAL NOTICE</strong></p></td>
<td>

```powershell
-FromScope NotInOrganization -ExceptIf -SubjectOrBodyMatches "CONTOSO LEGAL NOTICE"
```

</td>
</tr>
<tr class="even">
<td><p>Incoming messages with executable attachments</p></td>
<td><p>Condition 1: <strong>The sender is located</strong> &gt; <strong>Outside the organization</strong></p>
<p>Condition 2: <strong>Any attachment</strong> &gt; <strong>has executable content</strong></p></td>
<td>

```powershell
-FromScope NotInOrganization -AttachmentHasExecutableContent
```

</td>
</tr>
<tr class="odd">
<td><p>Sender is in the marketing department</p></td>
<td><p>Condition: <strong>The sender</strong> &gt; <strong>is a member of this group</strong> &gt; <strong>group name</strong></p></td>
<td>

```powershell
-FromMemberOf "Marketing Team"
```

</td>
</tr>
<tr class="even">
<td><p>Every message that comes from an external sender to the sales discussion group</p></td>
<td><p>Condition 1: <strong>The sender is located</strong> &gt; <strong>Outside the organization</strong></p>
<p>Condition 2: <strong>The message</strong> &gt; <strong>To or Cc box contains this person</strong> &gt; <strong>group name</strong></p></td>
<td>

```powershell
-FromScope NotInOrganization -SentTo "Sales Discussion Group" -PrependSubject "Sent to Sales Discussion Group: "
```

</td>
</tr>
<tr class="odd">
<td><p>Prepend an advertisement to outgoing messages for one month</p></td>
<td><p>Condition 1: <strong>The recipient is located</strong> &gt; <strong>Outside the organization</strong></p>
<p>Specify the dates at the bottom of the <strong>New rule</strong> dialog.</p></td>
<td>

```powershell
-ApplyHtmlDisclaimerLocation 'Prepend' -SentToScope 'NotInOrganization' -ActivationDate '03/1/2014' -ExpiryDate '03/31/2014'
```

</td>
</tr>
</tbody>
</table>

For a complete list of transport rule conditions you can use to target the disclaimer, see [Transport rule conditions (predicates)](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md).

## Formatting your disclaimer

You can format your disclaimer as needed. Here's what can be included in your disclaimer text.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type of information</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Text</p></td>
<td><p>The maximum length is 5,000 characters, including any HTML tags and inline Cascading Style Sheets (CSS).</p></td>
</tr>
<tr class="even">
<td><p>HTML and inline CSS</p></td>
<td><p>You can use HTML and inline CSS styles to format the text. For example, use the <code>&lt;HR&gt;</code> tag to add a line before the disclaimer.</p>
<p>HTML in a disclaimer is ignored if the disclaimer is added to a plain text message.</p></td>
</tr>
<tr class="odd">
<td><p>Add Images</p></td>
<td><p>Use the <code>&lt;IMG&gt;</code> tag to point to an image available on the internet. For example, <code>&lt;IMG src=&quot;http://contoso.com/images/companylogo.gif&quot; alt=&quot;Contoso logo&quot;&gt;</code></p>
<p>Keep in mind that Outlook Web App and Outlook block external web content, including images, by default. Users may need to perform a specific action if they want to view the blocked external content. This means that images added by using the <code>IMG</code> tag may not be visible by default. We recommend that you test a disclaimer with <code>IMG</code> tags in the email clients your recipients are likely to use to make sure it displays well.</p></td>
</tr>
<tr class="even">
<td><p>Add information for personalized signatures</p></td>
<td><p>If you want everyone to have signatures formatted the same way with the same information, you can add unique information for each employee, such as <code>DisplayName</code>, <code>FirstName</code>, <code>LastName</code>, <code>PhoneNumber</code>, <code>Email</code>, <code>FaxNumber</code>, and <code>Department</code>. This information must be enclosed in two percent signs (%%) on each side of the information. For example, to use <code>DisplayName</code>, you must use <strong>%%DisplayName%%</strong> in your disclaimer.</p>
<p>When a disclaimer rule is triggered, the corresponding values for that user are inserted. The data comes from the sender's Active Directory user account (for on-premises Exchange Server), or from the sender's Office 365 account for Exchange Online.</p>
<p>For a complete list of attributes that can be used in disclaimers and personalized signatures, see the description for the <code>ADAttribute</code> property in <a href="mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md">Transport rule conditions (predicates)</a> (Exchange Server) or <a href="https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/conditions-and-exceptions">Mail flow rule conditions and exceptions (predicates) in Exchange Online</a>.</p></td>
</tr>
</tbody>
</table>

For example, here's an example of an HTML disclaimer that includes a signature, an `IMG` tag, and embedded CSS.

```HTML
<div style="font-size:9pt;  font-family: 'Calibri',sans-serif;">
%%displayname%%<br/>
%%title%%<br/>
%%company%%<br/>
%%street%%<br/>
%%city%%, %%state%% %%zipcode%%</div>
&nbsp;<br/>
<div style="background-color:#D5EAFF; border:1px dotted #003333; padding:.8em; ">
<div><img alt="Fabrikam"  src="http://fabrikam.com/images/fabrikamlogo.png"></div>
<span style="font-size:12pt;  font-family: 'Cambria','times new roman','garamond',serif; color:#ff0000;">HTML Disclaimer Title</span><br/>
<p style="font-size:8pt; line-height:10pt; font-family: 'Cambria','times roman',serif;">This message contains confidential information and is intended only for the individual(s) addressed in the message. If you are not the named addressee, you should not disseminate, distribute, or copy this e-mail. If you are not the intended recipient, you are notified that disclosing, distributing, or copying this e-mail is strictly prohibited.  </p>
<span style="padding-top:10px; font-weight:bold; color:#CC0000; font-size:10pt; font-family: 'Calibri',Arial,sans-serif; "><a href="http://www.fabrikam.com">Fabrikam, Inc. </a></span><br/><br/>
</div>
```

## Fallback options if the disclaimer can't be added

Some messages, such as encrypted messages, prevent Exchange from modifying the content of the original message. You can control how your organization handles these messages. You specify whether to wrap a message that can't be modified in a message envelope that contains the disclaimer, reject the message if a disclaimer can't be added, or ignore the disclaimer action and deliver the message without a disclaimer.

The following list describes each fallback action:

- **Wrap**: If the disclaimer can't be inserted into the original message, Exchange encloses, or "wraps," the original message in a new message envelope. Then the disclaimer is inserted into the new message. If the original message can't be wrapped in a new message envelope, the original message is not delivered. The sender of the message receives a non-delivery report (NDR) that explains why the message was not delivered.

  > [!IMPORTANT]
  > If an original message is wrapped in a new message envelope, subsequent transport rules are applied to the new message envelope, not to the original message. Therefore, you must configure transport rules with disclaimer actions that wrap original messages in a new message body after you configure other transport rules.

- **Reject**: If the disclaimer can't be inserted into the original message, Exchange doesn't deliver the message. The sender of the message receives an NDR that explains why the message wasn't delivered.

- **Ignore**: If the disclaimer can't be inserted into the original message, Exchange delivers the original message unmodified. No disclaimer is added.

## For more information

[Transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)
