---
localization_priority: Normal
description: 'Summary: Learn about the TNEF message conversion and preservation options that are available in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 989a62fc-4bc1-448f-90c8-7c7b56fe1084
ms.reviewer:
title: TNEF conversion options
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# TNEF conversion options

TNEF, also known as the Transport Neutral Encapsulation Format, Outlook Rich Text Format, or Exchange Rich Text Format, is a Microsoft-specific format for encapsulating MAPI message properties. All versions of Outlook fully support TNEF. Outlook on the web (formerly known as Outlook Web App) translates TNEF into MAPI and displays the formatted messages. Other email clients that don't support TNEF typically display TNEF formatted messages as plain text messages with Winmail.dat or Win.dat attachments. For more information about TNEF, see [Exchange and Outlook message formats](content-conversion.md#Exchange).

Administrators can specify whether TNEF should be preserved or removed from messages that leave their Exchange organization. You can specify TNEF conversion options in the following locations:

- Remote domain settings

- Mail contact and mail user settings

- Outlook settings:

  - Message format

  - Internet message format

  - Internet recipient message format (Outlook 2010 or earlier)

Typically, the default TNEF conversion options will work fine (by default, TNEF messages are converted to HTML for external recipients). However, you might need to force plain text conversion for recipients that are using older email clients or messaging systems. They'll likely tell you if TNEF messages from your Exchange environment appear to have formatting issues.

For more information about other content conversion in Exchange, see [Content conversion](content-conversion.md).

## TNEF conversion options for remote domains
<a name="RemoteDomains"> </a>

Remote domains specify settings for messages sent to domains that are external to your Exchange organization. For more information, see [Remote Domains](https://docs.microsoft.com/exchange/remote-domains-exchange-2013-help).

When you configure TNEF conversion options for a remote domain, the settings are applied to all messages sent to recipients in that domain. You can use the Exchange admin center (EAC) or the Exchange Management Shell to configure these options:

- In the EAC, go to **Mail flow** \> **Remote domains** \> **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png), or select an existing remote domain, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Use rich-text format** section.

- In the Exchange Management Shell, use the _TnefEnabled_ parameter on the **Set-RemoteDomain** cmdlet.

The TNEF conversion options for remote domains are described in this table:

|**Setting**|**Value in the EAC**|**Value in Exchange Management Shell**|
|:-----|:-----|:-----|
|Use TNEF for all messages sent to the remote domain.|**Always**| `$true`|
|Never use TNEF for any messages sent to the remote domain.|**Never**| `$false`|
|TNEF messages aren't specifically allowed or prevented for recipients in the remote domain. This is the default value.  <br/> Whether TNEF messages are sent to recipients in the remote domain depends on the specific setting on the mail contact or mail user, or the setting specified by the sender in Outlook.|**Follow user settings**| `$null` (blank)|

## TNEF conversion options for mail contacts and mail users
<a name="MailContacts"> </a>

Mail contacts and mail users represent users in your Exchange organization that have external email addresses For more information, see [Recipients](../../recipients/recipients.md).

When you configure TNEF conversion options for a mail contact or a mail user, those options are applied to all messages sent to that specific recipient. You use the _UseMapiRichTextFormat_ parameter on the **Set-MailUser** and **Set-MailContact** cmdlets in the Exchange Management Shell. Valid values are:

- `Always`: TNEF is used for all messages sent to the recipient.

- `Never`: TNEF is never used for any messages sent to the recipient.

- `UseDefaultSettings`: This is the default value. TNEF messages aren't specifically allowed or prevented for the mail user or mail contact. Whether TNEF messages are sent to the recipient depends on the TNEF conversion setting for the remote domain, or the TNEF conversion setting that's configured by the sender in Outlook.

## TNEF conversion options in Outlook
<a name="Outlook"> </a>

Senders can control the default conversion options for TNEF messages sent to all external recipients. These options are called *Internet message format* options. The options only apply to external recipients, and not to recipients in the Exchange organization.

 **Note**: The following options define how Outlook rich text messages are handled when sent to external recipients. If the messages are HTML or plain text, these settings don't apply.

The following TNEF conversion options are available in Outlook:

- **Convert to HTML format**: This is the default option. TNEF messages sent to external recipients are converted to HTML. Any formatting in the message should closely resemble the original message. MIME-encoded HTML messages are supported by most email clients.

- **Convert to Plain Text format**: Any TNEF messages sent to remote recipients are converted to plain text. Any formatting in the message is lost.

- **Send using Outlook Rich Text Format**: Any TNEF messages sent to remote recipients remain TNEF messages.

Senders in Outlook 2010 or earlier can also control the default TNEF message conversion options for TNEF messages sent to specific external recipients. These options are called *Internet recipient message format* options. The options only apply to external recipients stored in your Contacts folder, and not to recipients in the Exchange organization. The following list describes the TNEF conversion options for an external recipient in your Contacts folder:

- **Let Outlook decide the best sending format**: This is the default setting. This setting forces Outlook to use the TNEF conversion option that's specified by the default Internet format as described in the previous list (**Convert to HTML format**, **Convert to Plain Text format**, or **Send using Outlook Rich Text Format**). Therefore, the TNEF message may be left as TNEF, converted to HTML, or converted to plain text (the default result is converted to HTML). If you want to make sure that the TNEF message remains TNEF for the contact, you should change this setting to **Send using Outlook Rich Text format**.

- **Send Plain Text only**: Any TNEF messages sent to the recipient are converted to plain text. Any formatting in the message is lost.

- **Send using Outlook Rich Text format**: Any TNEF messages sent to remote recipients remain TNEF messages.

To configure the TNEF conversion settings in Outlook, see [Change the message format to HTML, Rich Text Format, or plain text](https://go.microsoft.com/fwlink/p/?linkid=397890).

## Order of precedence for TNEF conversion options
<a name="Order"> </a>

The TNEF conversion options for messages sent to external recipients are described in the following list from highest priority to lowest priority:

1. Remote domain settings

2. Mail user or mail contact settings

3. Outlook settings

The setting at a higher level overrides the setting at a lower level. The TNEF setting on the remote domain overrides the TNEF setting on the mail contact or mail user, or the setting in Outlook. For example, suppose you send a Rich Text message in Outlook, but the recipient is in a domain where the remote domain setting specifically doesn't allow TNEF messages. The message received by the recipient will be plain text or HTML, but not TNEF.

 **Note**: Exchange never sends Summary Transport Neutral Encoding Format (STNEF) messages to external recipients. Only TNEF messages can be sent to recipients outside the Exchange organization.
