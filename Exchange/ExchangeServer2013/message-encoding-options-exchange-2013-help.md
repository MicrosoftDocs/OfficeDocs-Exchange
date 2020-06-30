---
title: 'Message encoding options: Exchange 2013 Help'
TOCTitle: Message encoding options
ms:assetid: c1d9edbb-d87c-41e5-881b-cd612d83d7e4
ms:mtpsurl: https://technet.microsoft.com/library/Bb310794(v=EXCHG.150)
ms:contentKeyID: 49318586
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Message encoding options

_**Applies to:** Exchange Server 2013_

The message encoding options that are available in Exchange specify message characteristics, such as MIME and non-MIME character sets, binary encoding, and attachment formats. You can specify message encoding options in the following locations:

  - Remote domain settings

  - Mail user and mail contact settings

  - Microsoft Outlook settings

      - Message format

      - Internet message format

      - Internet recipient message format

      - Message character set encoding options

**Content**

Message encoding options for messages sent to remote domains

Message encoding options for mail users and mail contacts

Message encoding options available in Outlook

Message encoding options available in Outlook Web App

Order of precedence for message encoding options

For more information

## Message encoding options for messages sent to remote domains

When you configure message encoding options for a remote domain, the specific settings are applied for all messages sent to that domain. For remote domains in your organization, you have the following configuration options for message encoding.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Setting</strong></p></td>
<td><p><strong>Available in EAC in Exchange Online Dedicated</strong></p></td>
<td><p><strong>Available in the Shell</strong></p></td>
</tr>
<tr class="even">
<td><p><strong>MIME character set</strong>   The character set that you specify will only be used for MIME messages that don't have their own character set specified. Setting this parameter won't overwrite character sets that are already specified in the outgoing mail.</p>
<p><strong>Non-MIME character set</strong>   This setting is used if either of the following conditions are true:</p>
<ul>
<li><p>Incoming messages from a remote domain are missing the value of the <em>charset=</em> setting in the MIME Content-Type: header field.</p></li>
<li><p>Outgoing messages to a remote domain are missing the value of the MIME character set.</p></li>
</ul>
<p></p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p><strong>Content type</strong>   You can specify the content type for MIME messages sent to the recipients in the remote domain. You can use of the following settings:</p>
<ul>
<li><p><strong>MimeHtmlText</strong>   All messages are converted to MIME messages that use HTML formatting, unless the original message is a text message. If the original message is a text message, the outgoing message will be a MIME message that uses text formatting. This is the default setting.</p></li>
<li><p><strong>MimeText</strong>   All messages are converted to MIME messages that use text formatting.</p></li>
<li><p><strong>MimeHtml</strong>   All messages are converted to MIME messages that use HTML formatting.</p></li>
</ul></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p><strong>Line wrap size</strong>   You can specify the maximum number of characters that can exist on a single line of text in the body of the e-mail message. Older email client applications may prefer 78 characters per line. This option is only available by using the Shell.</p>
<p></p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

## Message encoding options for mail users and mail contacts

When you configure message encoding options for a mail contact or a mail user, that option is applied to all messages sent to that specific recipient. For mail contacts and mail users in your organization, you have the following configuration options for message encoding:

  - **UsePreferMessageFormat**: This parameter specifies whether the message format settings configured for the mail contact override the global settings configured for the remote domain. If you disable this setting, Exchange ignores other message encoding options for this recipient and the message encoding is determined by the configuration of the remote domain or the settings configured by the message sender.

  - **MessageFormat**: This parameter specifies the message format. You can either specify Text or Mime as the message format. The value of this setting is dependent on the *MessageBodyFormat* parameter. If the message body format is Html or TextAndHtml, you must set this parameter to Mime.

  - **MessageBodyFormat**: This parameter specifies the message body format. You can specify Text, Html, or TextAndHtml. The value of this setting is dependent on the *MessageFormat* parameter. If the message format is Text, you must also set this parameter to Text.

  - **MacAttachmentFormat**: This parameter specifies the Apple Macintosh operating system attachment format for messages. You can specify BinHex, UuEncode, AppleSingle, or AppleDouble. The value of this setting is dependent on the *MessageFormat* parameter. If the message format is set to Text, you must set this parameter to either BinHex or UuEncode. If the message format is set to Mime, you must set this parameter to BinHex, AppleSingle or AppleDouble.

You need to use these parameters in the Exchange Management Shell to set the message encoding options for mail users and mail contacts. For more information, see the following topics:

  - [Enable-MailContact](https://docs.microsoft.com/powershell/module/exchange/Enable-MailContact)

  - [New-MailContact](https://docs.microsoft.com/powershell/module/exchange/New-MailContact)

  - [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/Set-MailContact)

  - [Enable-MailUser](https://docs.microsoft.com/powershell/module/exchange/Enable-MailUser)

  - [New-MailUser](https://docs.microsoft.com/powershell/module/exchange/New-MailUser)

  - [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/Set-MailUser)

## Message encoding options available in Outlook

As a sender, you can specify message encoding options in Outlook at any of the following stages:

  - By configuring the default message format to be either plain text or HTML.

  - By setting the message format as you're composing it to either plain text or HTML using the **Format** area in the **Options** tab.

  - By configuring the message encoding options for messages that are sent to all recipients outside the Exchange organization. These options are called *Internet message format* options. The options only apply to remote recipients, and not to recipients in the Exchange organization.

  - By configuring the message encoding options for messages that are sent to specific recipients outside the Exchange organization. These options are called *Internet recipient message format* options. The options only apply to remote recipients in your Contacts folder, and not to recipients in the Exchange organization.

By default, Outlook uses automatic character set message encoding by scanning the whole text of the outgoing message to determine the appropriate encoding to use for the message. This setting applies to messages that you send to Internet recipients and recipients in the Exchange organization. However, you can bypass this and specify a preferred encoding for outgoing messages.

## Message encoding options available in Outlook Web App

As a sender, you can specify message encoding options in Outlook Web App at any of the following stages:

  - By configuring the default message format to be either plain text or HTMLin the **Message format** section of the **Settings** \>**Options** \> **Settings** page.

  - By setting the message format as you're composing it to either plain text or HTML by using the **More options** (...) menu, and selecting **Switch to plain text** or **Switch to HTML**.

## Order of precedence for message encoding options

Exchange uses the order of precedence as described in the following list to determine the message encoding options for outgoing messages sent to recipients outside the Exchange organization:

1. Remote domain settings

2. Outlook or Outlook Web App settings

3. Mail user or mail contact settings

The list specifies the order of precedence from lowest to highest. A setting made at a higher level may override a setting made at a lower level.

The following table describes the order of precedence from lowest priority to highest priority for message character set encoding options.

### Order of precedence from lowest priority to highest priority for message character set encoding options

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Source</th>
<th>Parameter</th>
<th>Values</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Remote domain entry setting, using the EAC or <strong>Set-RemoteDomain</strong></p></td>
<td><p><em>CharacterSet</em></p></td>
<td><p>Specified</p></td>
</tr>
<tr class="even">
<td><p>Remote domain entry setting, using the EAC or <strong>Set-RemoteDomain</strong></p></td>
<td><p><em>NonMimeCharacterSet</em></p></td>
<td><p>Specified</p></td>
</tr>
<tr class="odd">
<td><p>Outlook setting</p></td>
<td><p>Message character set encoding</p></td>
<td><ul>
<li><p>Auto-select</p></li>
<li><p>Specified</p></li>
</ul></td>
</tr>
</tbody>
</table>

When you set the non-MIME character set for a remote domain, the character set is assigned to the following types of messages:

  - Outgoing messages to a configured remote domain that don't contain a specified character set.

  - Incoming messages from a configured remote domain that don't contain a specified character set.

The value of the Windows ANSI code page for the transport server is used to assign a character set to the following types of messages:

  - Internal messages that don't contain a specified character set.

  - Internal messages that contain a specified character set, but don't contain a specified server code page.

If a message contains a specified but invalid character set, the transport server tries to replace the invalid character set with a valid character set.

The following table describes the order of precedence from lowest priority to highest priority for plain text message encoding options.

### Order of precedence from lowest priority to highest priority for plain text message encoding options

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Source</th>
<th>Parameter</th>
<th>Values</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Set-RemoteDomain</strong></p></td>
<td><p><em>LineWrapSize</em></p></td>
<td><ul>
<li><p>From 0 through 132</p></li>
<li><p>unlimited</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook settings</p></td>
<td><p>Message format</p></td>
<td><p>Plain text</p></td>
</tr>
<tr class="odd">
<td><p>Outlook settings</p></td>
<td><p>Internet message format</p></td>
<td><p>Plain text options:</p>
<ul>
<li><p>Encode attachments in UuEncode format when you send a plain text message</p></li>
<li><p>Automatically wrap text at <em>nn</em> characters</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook settings</p></td>
<td><p>Internet recipient message format</p></td>
<td><p>Plain text format:</p>
<ul>
<li><p>Encode attachments in UuEncode attachment format</p></li>
<li><p>Use BinHex Mac attachment format</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>UsePreferMessageFormat</em></p></td>
<td><ul>
<li><p><code>$true</code></p></li>
<li><p><code>$false</code></p></li>
</ul>
<p>If the value is <code>$false</code> or if the recipient isn't defined as a mail user or mail contact in the Exchange organization, the mail user or mail contact settings are ignored.</p></td>
</tr>
<tr class="even">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MessageFormat</em></p></td>
<td><p>Text</p></td>
</tr>
<tr class="odd">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MessageBodyFormat</em></p></td>
<td><p>Text</p></td>
</tr>
<tr class="even">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MacAttachmentFormat</em></p></td>
<td><ul>
<li><p>BinHex</p></li>
<li><p>UuEncode</p></li>
</ul></td>
</tr>
</tbody>
</table>

The following table describes the order of precedence from lowest priority to highest priority for MIME message encoding options.

### Order of precedence from lowest priority to highest priority for MIME message encoding options

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Source</th>
<th>Parameter</th>
<th>Values</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Set-RemoteDomain</strong></p></td>
<td><p><em>ContentType</em></p></td>
<td><ul>
<li><p><code>MimeHtmlText</code></p></li>
<li><p><code>MimeText</code></p></li>
<li><p><code>MimeHtml</code></p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook or Outlook Web App settings</p></td>
<td><p>Message format</p></td>
<td><ul>
<li><p>Plain text</p></li>
<li><p>HTML</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outlook settings</p></td>
<td><p>Internet recipient message format</p></td>
<td><p>MIME message format</p>
<ul>
<li><p>Plain text</p></li>
<li><p>Include both plain text and HTML</p></li>
<li><p>HTML</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>UsePreferMessageFormat</em></p></td>
<td><p><code>$true</code></p>
<p><code>$false</code></p>
<p>If the value is <code>$false</code> or if the recipient isn't defined as a mail user or mail contact in the Exchange organization, the mail user or mail contact settings are ignored.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MessageFormat</em></p></td>
<td><ul>
<li><p>Text</p></li>
<li><p>Mime</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MessageBodyFormat</em></p></td>
<td><ul>
<li><p><code>Text</code></p></li>
<li><p><code>Html</code></p></li>
<li><p><code>TextAndHtml</code></p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p><strong>Set-MailUser</strong></p>
<p><strong>Set-MailContact</strong></p></td>
<td><p><em>MacAttachmentFormat</em></p></td>
<td><ul>
<li><p>BinHex</p></li>
<li><p>AppleSingle</p></li>
<li><p>AppleDouble</p></li>
</ul></td>
</tr>
</tbody>
</table>

## For more information

[Message encoding options](message-encoding-options-exchange-2013-help.md)

[TNEF conversion options](tnef-conversion-options-exchange-2013-help.md)

[Remote domains](remote-domains-exchange-2013-help.md)

[Remote domains in Exchange Online](https://docs.microsoft.com/exchange/mail-flow-best-practices/remote-domains/remote-domains)

[Manage mail users](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-users)

[Manage mail contacts](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-contacts)

[Change the message format in Outlook](https://support.microsoft.com/office/338a389d-11da-47fe-b693-cf41f792fefa)
