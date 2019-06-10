---
title: 'Configure content transfer encoding: Exchange 2013 Help'
TOCTitle: Configure content transfer encoding
ms:assetid: c4922362-18d4-42f7-9189-9076720eb1d8
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Gg144562(v=EXCHG.150)
ms:contentKeyID: 49318587
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure content transfer encoding

_**Applies to:** Exchange Server 2013_

*Content transfer encoding* defines encoding methods for transforming binary email message data into the US-ASCII plain text format. This transformation allows the message to travel through older SMTP messaging servers that only support messages in US-ASCII text. Content transfer encoding is defined in RFC 2045. The transfer encoding method is stored in the **Content-Transfer-Encoding** header field in the message. In Microsoft Exchange Server 2013, the following content transfer encoding methods are available:

- **7-bit**: This value indicates that the message body data is already in the US ASCII plain text format, and no message encoding has been done to the message.

- **Quoted-printable (QP)**: This encoding method uses printable US-ASCII characters to encode the message body data. If the original message text is mostly US-ASCII text, QP encoding gives somewhat readable and compact results. By default, Exchange 2013 uses QP for encoding binary message data.

- **Base64**: This encoding method is based primarily on the privacy-enhanced mail (PEM) standard defined in RFC 1421. Base64 encoding uses the 64-character alphabet encoding method and output padding characters defined by PEM to encode the message body data. Base64 encoding creates a predictable increase in message size and is optimal for binary data and non-US-ASCII text.

You configure the transfer encoding method using the *ByteEncoderTypeFor7BitCharsets* parameter on the **Set-OrganizationConfig** and **Set-RemoteDomain** cmdlets. The content transfer encoding settings you configure with **Set-OrganizationConfig** apply to all messages in the Exchange organization. The content transfer encoding settings you configure with **Set-RemoteDomain** apply only to message sent to external recipients in the remote domain.

The following table lists the values that you can use to set the transfer encoding method.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter in <strong>Set-OrganizationConfig</strong></th>
<th>Parameter in <strong>Set-RemoteDomain</strong></th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>0</p></td>
<td><p><code>Use7Bit</code></p></td>
<td><p>Always use 7-bit encoding for HTML and for plain text. This is the default value.</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p><code>UseQP</code></p></td>
<td><p>Always use QP encoding for HTML and for plain text.</p></td>
</tr>
<tr class="odd">
<td><p>2</p></td>
<td><p><code>UseBase64</code></p></td>
<td><p>Always use Base64 encoding for HTML and for plain text.</p></td>
</tr>
<tr class="even">
<td><p>5</p></td>
<td><p><code>UseQPHtmlDetectTextPlain</code></p></td>
<td><p>Use QP encoding for HTML and for plain text unless line wrapping is enabled in plain text. If line wrapping is enabled, use 7-bit encoding for plain text.</p></td>
</tr>
<tr class="odd">
<td><p>6</p></td>
<td><p><code>UseBase64HtmlDetectTextPlain</code></p></td>
<td><p>Use Base64 encoding for HTML and for plain text, unless line wrapping is enabled in plain text. If line wrapping is enabled in plain text, use Base64 encoding for HTML, and use 7-bit encoding for plain text.</p></td>
</tr>
<tr class="even">
<td><p>13</p></td>
<td><p><code>UseQPHtml7BitTextPlain</code></p></td>
<td><p>Always use QP encoding for HTML. Always use 7-bit encoding for plain text.</p></td>
</tr>
<tr class="odd">
<td><p>14</p></td>
<td><p><code>UseBase64Html7BitTextPlain</code></p></td>
<td><p>Always use Base64 encoding for HTML. Always use 7-bit encoding for plain text.</p></td>
</tr>
</tbody>
</table>

For more details about **Content-Transfer-Encoding** header field, see the "Understanding the structure of email messages" section in [Content conversion](content-conversion-exchange-2013-help.md).

For more information about remote domains, see [Remote domains](remote-domains-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure the content transfer encoding method for the organization

To configure the content transfer encoding method for the organization, run the following command:

```powershell
Set-OrganizationConfig -ByteEncoderTypeFor7BitCharsets <Integer>
```

For example, to set the content transfer encoding method to Base64, run the following command:

```powershell
Set-OrganizationConfig -ByteEncoderTypeFor7BitCharsets 2
```

## Use the Shell to configure the content transfer encoding method for a remote domain

To configure the content transfer encoding method for all the recipients in a remote domain, run the following command:

```powershell
Set-RemoteDomain -ByteEncoderTypeFor7BitCharsets <Value>
```

For example, to set the content transfer encoding method to Base64, run the following command:

```powershell
Set- RemoteDomain -ByteEncoderTypeFor7BitCharsets UseBase64
```

## How do you know this worked?

To verify that you have successfully configured the method for content transfer encoding, do the following:

1. Send a test message that contains a mixture of US-ASCII text and binary data or non-US-ASCII text to an internal or external test account. Use an internal account to test organization settings, and an external account in the remote domain to test remote domain settings.

2. In an email client, view the **Content-Transfer-Encoding** header field in the message, and verify the content transfer encoding method that was used on the message matches the method you configured.
