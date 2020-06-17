---
localization_priority: Normal
description: Admins can learn about MailTips that are presented to users in Outlook and Outlook on the web.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9c989167-cc0c-40a6-82ba-383f573bd2d5
ms.reviewer: 
title: MailTips
ms.collection: 
- exchange-online
- M365-email-calendar
audience: Admin
search.appverid: MET150
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# MailTips

MailTips are informative messages displayed to users while they're composing a message. While a new message is open and being composed, Exchange analyzes the message (including recipients). If a potential problem is detected, the user is notified with a MailTip prior to sending the message. Using the information in the MailTip, the user can adjust the message to avoid undesirable situations or non-delivery reports (also known as NDRs or bounce messages).

## How MailTips work

MailTips are implemented as a web service in Exchange. When a sender is composing a message, the client software makes an Exchange web service call to the Client Access server to get the list of MailTips. The server responds with the list of MailTips that apply to that message, and the client software displays the MailTips to the sender.

The following unproductive messaging scenarios are common in any messaging environment:

- NDRs resulting from messages that violate organization-wide message restrictions (for example, message size restrictions or maximum number of recipients per message).

- NDRs resulting from messages sent to non-existent recipients, restricted recipients, or users with full mailboxes.

- Sending messages to users with Automatic Replies configured.

All of these scenarios involve the user sending a message, expecting it to be delivered, and instead receiving a response stating that the message isn't delivered. Even in the best-case scenario, like the automatic reply, these events result in lost productivity. In the case of an NDR, this scenario could result in a costly call to the help desk.

There are also several scenarios where sending a message won't result in an error, but can have undesirable, even embarrassing consequences:

- Messages sent to extremely large distribution groups.

- Messages sent to inappropriate distribution groups.

- Messages inadvertently sent to recipients outside your organization.

- Selecting **Reply to All** to a message that was received as a Bcc recipient.

All of these problematic scenarios can be mitigated by informing users of the possible outcome of sending the message as they're composing the message. For example, if senders are notified that the size of their message will exceed the maximum allowed value, they won't attempt to send the message. Similarly, if senders are notified that their message will be delivered to people outside the organization, they're more likely to ensure that the content and the tone of the message are appropriate.

The following messaging clients support MailTips:

- Outlook on the web (formerly known as Outlook Web App)

- Microsoft Outlook 2010 or later

## MailTips in Exchange Online

The following table lists the available MailTips in Exchange Online.

|**MailTip**|**Availability**|**Scenario**|
|:-----|:-----|:-----|
|Invalid Internal Recipient|Outlook|The sender adds an internal recipient that doesn't exist. For example: <br/>• The non-existent recipient resolves due to an entry in the sender's Auto-Complete List (also known as the _nickname cache_) or an entry in the sender's Contacts folder. <br/>• The sender types a non-existent internal email address, and the email address is in an accepted domain (an authoritative domain) for the Exchange organization. <br/><br/> The MailTip indicates the invalid recipient and gives the sender the option to remove the recipient from the message.|
|Mailbox Full|Outlook <br/><br/> Outlook on the web|The sender adds an internal recipient whose mailbox exceeds the maximum mailbox size (the ProhibitSendReceive quota on the mailbox or organization). <br/><br/> The MailTip indicates the recipient whose mailbox is full and gives the sender the option to remove the recipient from the message. <br/><br/> The MailTip is accurate at the time of display. If the message isn't immediately sent, the MailTip is updated every two hours. This also applies to messages that were saved in the Drafts folder and reopened after two hours.|
|Automatic Replies|Outlook <br/><br/> Outlook on the web|The sender adds an internal recipient<sup>\*</sup> who has turned on Automatic Replies. <br/><br/> The MailTip indicates the recipient has Automatic Replies turned on and also displays the first 175 characters of the automatic reply text. <br/><br/> The MailTip is accurate at the time of display. If the message isn't immediately sent, the MailTip is updated every two hours. This also applies to messages that were saved in the Drafts folder and reopened after two hours. <br/><br/> <sup>\*</sup>If the recipient is external, but the recipient's domain is configured as a remote domain, the AllowedOOFType and IsInternal settings determine whether the sender receives the internal automatic reply, the external automatic reply, or no automatic reply at all.|
|Custom|Outlook <br/><br/> Outlook on the web|The sender adds an internal recipient that has a custom MailTip configured. <br/><br/> A custom MailTip can be useful for providing specific information about a recipient. For example, you can create a custom MailTip for a distribution group explaining its purpose to reduce its misuse. For more information, see [Configure custom MailTips for recipients](configure-custom-mailtips.md). <br/><br/> By default, custom MailTips aren't displayed if the sender isn't allowed to send messages to the recipient (the Restricted Recipient MailTip is displayed instead). However, you can change this configuration and have the custom MailTip also display.|
|Restricted Recipient|Outlook <br/><br/> Outlook on the web|The sender adds a recipient that they're not allowed to send messages to (delivery restrictions are configured between the sender and the recipient). <br/><br/> The MailTip indicates the prohibited recipient and gives the sender the option to remove the recipient from the message. It also clearly informs the sender that the message can't be delivered to the restricted recipient. <br/><br/>  If the restricted recipient is external or is a distribution group that contains external recipients, this MailTip is also provided to the sender. However, the following MailTips aren't displayed (if applicable): <br/>• Automatic Replies <br/>•  Mailbox Full <br/>• Custom MailTip <br/>• Moderated Recipient <br/>• Oversize Message|
|External Recipients|Outlook <br/><br/> Outlook on the web <br/><br/> Outlook Mobile|The sender adds an external recipient<sup>\*</sup> or a distribution group that contains external recipients. <br/><br/> The MailTip informs the sender that the message will leave the organization, which can help them make the correct decisions about wording, tone, and content. <br/><br/> By default, this MailTip is turned off. You can turn it on using the **Set-OrganizationConfig** cmdlet. For details, see [MailTips over organization relationships](mailtips-over-organization-relationships.md). <br/><br/> <sup>\*</sup>If the recipient is external, but the recipient's domain is configured as a remote domain, the IsInternal setting determines whether the sender receives this MailTip (the External Recipients MailTip doesn't apply to internal recipients). <br/><br/> **Note**: The External Recipients MailTip isn't evaluated for external distribution group recipients where the distribution group is in a remote domain. <br/><br/> **Note 2**: Outlook Mobile only supports the External Recipients MailTip for Office 365 and for on-premises Exchange mailboxes that use Hybrid Modern Authentication (HMA).|
|Large Audience|Outlook <br/><br/> Outlook on the web|The sender adds a distribution group that has more members than the configured large audience size (the default size is more than 25 members). For details, see [Configure the large audience size for your organization](configure-large-audience-size.md). <br/><br/> The number of distribution group members isn't calculated each time. Instead, the distribution group information is read from group metrics data.|
|Moderated Recipient|Outlook <br/><br/> Outlook on the web|The sender adds a moderated recipient (a recipient that requires message approval). <br/><br/> The MailTip identifies the moderated recipient and informs the sender that moderation might result in delayed delivery. <br/><br/> The MailTip is not displayed if: <br/>• The sender is a moderator for the recipient. <br/>• The sender has been explicitly allowed to send messages to the recipient (by adding the sender's name to the Accept Messages Only From list for the recipient). <br/><br/> To configure moderated recipients in Exchange Online, see [Configure a moderated recipient in Exchange Online](../../recipients-in-exchange-online/configure-a-moderated-recipient.md).|
|Reply-All on Bcc|Outlook on the web|A Bcc recipient selects **Reply All** a message. The MailTip appears in the reply message. <br/><br/> Bcc recipients revealing themselves to other recipients is universally bad, and the MailTip explains this.|
|Oversize Message|Outlook|The message is larger than the maximum allowed message size. <br/><br/> The MailTip is displayed if the message size violates one of the following message size restrictions: <br/>• Maximum send size setting on the sender's mailbox. <br/>• Maximum receive size setting on the recipient's mailbox. <br/>• Maximum message size restriction for the organization. <br/><br/> **Note**: Message size limits on connectors aren't evaluated for this MailTip.|

## MailTip restrictions

MailTips are subject to the following restrictions:

- MailTips aren't supported when working in offline mode in Outlook.

- When a message is addressed to a distribution group, the MailTips for individual recipients that are members of that distribution group aren't evaluated. However, if any of the members are external recipients, the External Recipients MailTip is displayed, which shows the sender the number of external recipients in the distribution group.

- If the message is addressed to more than 200 recipients, individual mailbox MailTips aren't evaluated due to performance reasons.

- Custom MailTips are limited to 175 characters.

- While older versions of Exchange Server would populate MailTips in their entirety, Exchange Online will only display up to 1000 characters.

- If the sender starts composing a message and leaves it open for an extended period of time, the Automatic Replies and Mailbox Full MailTips are evaluated every two hours.
