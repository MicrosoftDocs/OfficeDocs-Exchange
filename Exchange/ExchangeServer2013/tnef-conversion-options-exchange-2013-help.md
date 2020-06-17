---
title: 'TNEF conversion options: Exchange 2013 Help'
TOCTitle: TNEF conversion options
ms:assetid: 989a62fc-4bc1-448f-90c8-7c7b56fe1084
ms:mtpsurl: https://technet.microsoft.com/library/Bb310786(v=EXCHG.150)
ms:contentKeyID: 50934221
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# TNEF conversion options

_**Applies to:** Exchange Server 2013_

You can specify whether Transport Neutral Encapsulation Format (TNEF) should be preserved or removed from messages that leave the Exchange organization. TNEF, also known as Outlook Rich Text Format or Exchange Rich Text Format, is a Microsoft-specific format for encapsulating MAPI message properties. All versions of Microsoft Outlook fully understand TNEF. Outlook Web App translates TNEF into MAPI and displays the formatted messages. However, other email clients that don't understand TNEF typically display TNEF formatted messages as plain text messages with Winmail.dat or Win.dat attachments.

## TNEF conversion options for remote domains

When you configure TNEF conversion options for a remote domain, those TNEF conversion options are applied to all messages sent to that domain.

- For Exchange Online Dedicated, you use the Exchange admin center (EAC) to set TNEF conversion options for a remote domain at **Mail flow** \> **Remote domains** \> **Edit** (![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon")) \> **Use Exchange rich-text format**.

- For Exchange Online and Exchange 2013, you use the *TnefEnabled* parameter on the **Set-RemoteDomain** cmdlet to set TNEF conversion options for a remote domain.

For remote domains in your organization, you have the following configuration options for TNEF conversion:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Setting</th>
<th>In the EAC</th>
<th>In the Shell</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Use TNEF for all messages sent to the remote domain.</p></td>
<td><p><strong>Always</strong></p></td>
<td><p><code>$true</code></p></td>
</tr>
<tr class="even">
<td><p>Never use TNEF for any messages sent to the remote domain.</p></td>
<td><p><strong>Never</strong></p></td>
<td><p><code>$false</code></p></td>
</tr>
<tr class="odd">
<td><p>TNEF messages aren't specifically allowed or prevented for recipients in the remote domain. Whether TNEF messages are sent to recipients in the remote domain depends on the specific setting on the mail contact or mail user, or the setting specified by the sender in Outlook. This is the default value.</p></td>
<td><p><strong>Follow user settings</strong></p></td>
<td><p><code>$null</code> (blank)</p></td>
</tr>
</tbody>
</table>

For more information about remote domains, see [Remote domains](remote-domains-exchange-2013-help.md) or [Remote domains in Exchange Online](https://docs.microsoft.com/exchange/mail-flow-best-practices/remote-domains/remote-domains).

## TNEF conversion options for mail users and mail contacts

When you configure TNEF conversion options for a mail contact or a mail user, those TNEF conversion options are applied to all messages sent to that specific recipient. You use the *UseMapiRichTextFormat* parameter on the **Set-MailUser** and **Set-MailContact** cmdlets to configure the TNEF conversion options for mail users and mail contacts.

For mail users and mail contacts in your organization, you have the following configuration options for TNEF conversion:

- **Always**: TNEF is used for all messages sent to the recipient. The corresponding value for the *UseMapiRichTextFormat* parameter is `Always`.

- **Never**: TNEF is never used for any messages sent to the recipient. The corresponding value for the *UseMapiRichTextFormat* parameter is `Never`.

- **Use default settings**: TNEF messages aren't specifically allowed or prevented for the mail user or mail contact. Whether TNEF messages are sent to the recipient depends on the specific setting for the corresponding remote domain or the setting specified by the sender in Outlook. The corresponding value for the *UseMapiRichTextFormat* parameter is `UseDefaultSettings`. This is the default setting.

## TNEF conversion options in Outlook

Senders can control the default TNEF message conversion options for TNEF messages sent to all recipients outside the Exchange organization. These options are called *Internet message format* options. The options only apply to remote recipients, and not to recipients in the Exchange organization.

> [!NOTE]
> The following options define how messages containing Outlook rich text are handled when sent to external recipients. If the message format you're using is HTML or plain text, these settings don't apply.

You have the following TNEF conversion options in Outlook:

- **Convert to HTML format**: This is the default option. Any TNEF messages sent to remote recipients are converted to HTML. Any formatting in the message should closely resemble the original message. MIME-encoded HTML messages are supported by many, but not all, email clients.

- **Convert to Plain Text format**: Any TNEF messages sent to remote recipients are converted to plain text. Any formatting in the message is lost.

- **Send using Outlook Rich Text Format**: Any TNEF messages sent to remote recipients remain TNEF messages.

You can configure these options in the following locations in Outlook:

- **Outlook 2010 or Outlook 2013**: **File** \> **Options** \> **Mail** \> **Message format**.

- **Outlook 2007**: **Tools** \> **Options** \> **Mail Format** \> **Internet Format**.

Senders can also control the default TNEF message conversion options for TNEF messages sent to specific recipients outside the Exchange organization. These options are called *Internet recipient message format* options. The options only apply to remote recipients stored in your Contacts folder, and not to recipients in the Exchange organization. You have the following TNEF conversion options for remote recipients in your Contacts folder:

- **Let Outlook decide the best sending format**: This is the default setting. This setting forces Outlook to use the TNEF conversion option that's specified by the default Internet format. The possible values are **Convert to HTML format**, **Convert to Plain Text format**, or **Send using Outlook Rich Text Format**. Therefore, the TNEF message may be left as TNEF, converted to HTML, or converted to plain text. If you want to make sure that the TNEF message remains TNEF for this recipient, you should change this setting from **Let Outlook decide the best sending format** to **Send using Outlook Rich Text format**.

- **Send Plain Text only**: Any TNEF messages sent to the recipient are converted to plain text. Any formatting in the message is lost.

- **Send using Outlook Rich Text format**: Any TNEF messages sent to remote recipients remain TNEF messages.

You can configure these options for a contact in the following locations in Outlook:

- **Outlook 2010 or Outlook 2013**: Open the contact card, double-click the email address, click the **View more options for interacting with this person** icon, and select **Outlook properties**. In the **E-mail Properties** dialog box, select **Internet format**.

- **Outlook 2007**: Open the contact card, double-click the **E-mail** field and select **Internet format**.

## Order of precedence for TNEF conversion options

Exchange uses the order of precedence as described in the following list to determine the TNEF conversion options for outgoing messages sent to recipients outside the Exchange organization:

1. Remote domain settings

2. Mail user or mail contact settings

3. Outlook settings

The list specifies the order of precedence from highest to lowest. The TNEF setting on the remote domain overrides the TNEF settings on the mail user, mail contact or in Outlook. For example, suppose you send a Rich Text message in Outlook, but the recipient is in a domain where the remote domain setting specifically doesn't allow TNEF-formatted messages. The message received by the recipient will be plain text or HTML, but not TNEF.

Also, Exchange never sends Summary Transport Neutral Encoding Format (STNEF) messages to external recipients. Only TNEF messages can be sent to recipients outside the Exchange organization.
