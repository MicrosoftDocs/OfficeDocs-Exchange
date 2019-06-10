---
localization_priority: Normal
description: 'Summary: Learn how address rewriting on Edge Transport servers in Exchange Server can modify sender and recipient email addresses on email messages in transit.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 23f1eaf6-247a-4671-ad72-aae19d9b511d
ms.date: 7/3/2018
ms.reviewer: 
title: Address rewriting on Edge Transport servers
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Address rewriting on Edge Transport servers

Address rewriting in Exchange Server modifies the email addresses of senders and recipients in messages that enter or leave your organization through an Edge Transport server. Two transport agents on the Edge Transport server provide the rewriting functionality: the Address Rewriting Inbound Agent and the Address Rewriting Outbound Agent. The primary reason for address rewriting on outbound messages is to present a single, consistent email domain to external recipients. The primary reason for address rewriting on inbound messages is to deliver messages to the correct recipient.

The *address rewrite entry*, which you create, specifies the internal addresses (the email addresses you want to change) and the external addresses (the final email addresses you want). You can specify whether email addresses are rewritten in inbound and outbound messages, or in outbound messages only. You can create address writing entries for a single user (chris@contoso.com to support@contoso.com), a single domain (contoso.com to fabrikam.com), or for multiple subdomains with exceptions (\*.fabrikam.com to contoso.com, except legal.fabrikam.com).

> [!IMPORTANT]
> Regardless of how you plan to use address rewriting, you need to verify that the resulting email addresses are unique in your organization so you don't end up with duplicates. Address rewriting doesn't verify the uniqueness of a rewritten email address.

To configure address rewriting, see [Address rewriting procedures on Edge Transport servers](address-rewriting-procedures.md).

## Scenarios for address rewriting
<a name="Address"> </a>

The following scenarios are examples of how you can use address rewriting:

- **Group consolidation**: Some organizations segment their internal businesses into separate domains that are based on business or technical requirements. This configuration can cause email messages to appear as if they come from separate groups or even separate organizations.

    The following example shows how an organization, Contoso, Ltd., can hide its internal subdomains from external recipients:

  - Outbound messages from the northamerica.contoso.com, europe.contoso.com, and asia.contoso.com domains are rewritten so they appear to originate from a single contoso.com domain. All messages are rewritten as they pass through Edge Transport servers that provide SMTP connectivity between the whole organization and the Internet.

  - Inbound messages to contoso.com recipients are relayed by the Edge Transport server to a Mailbox server. The message is delivered to the correct recipient based on the proxy address that's configured on the recipient's mailbox.

- **Mergers and acquisitions**: An acquired company might continue to run as a separate business, but you can use address rewriting to make the two organizations appear as if they're one integrated organization.

    The following example shows how Contoso, Ltd. can hide the email domain of the newly acquired company, Fourth Coffee:

  - Contoso, Ltd. wants all outbound messages from Fourth Coffee's Exchange organization to appear as if they originate from contoso.com. All messages from both organizations are sent through the Edge Transport servers at Contoso, Ltd., where email messages are rewritten from _user_@fourthcoffee.com to _user_@contoso.com.

  - Inbound messages to _user_@contoso.com are rewritten and routed to _user_@fourthcoffee.com mailboxes. Inbound messages that are sent to _user_@fourthcoffee.com are routed directly to Fourth Coffee's email servers.

- **Partners**: Many organizations use external partners to provide services for their customers, other organizations, or their own organization. To avoid confusion, the organization might replace the email domain of the partner organization with its own email domain.

    The following example shows how Contoso, Ltd. can hide a partner's email domain:

  - Contoso, Ltd. provides support for the larger Wingtip Toys organization. Wingtip Toys wants a unified email experience for its customers, and it requires all messages from support personnel at Contoso, Ltd. to appear as if they were sent from Wingtip Toys. All outbound messages that relate to Wingtip Toys are sent through their Edge Transport servers, and all contoso.com email addresses are rewritten to wingtiptoys.com email addresses.

  - Inbound messages for support@wingtiptoys.com are accepted by Wingtip Toy's Edge Transport servers, rewritten, and then routed to the support@contoso.com email address.

## Message properties modified by address rewriting
<a name="SMTP"> </a>

A standard SMTP email message consists of a *message envelope* and message content. The message envelope contains information that's required for transmitting and delivering the message between SMTP messaging servers. The message content contains message header fields (collectively called the *message header*) and the message body. The message envelope is described in RFC 2821, and the message header is described in RFC 2822.

When a sender composes an email message and submits it for delivery, the message contains the basic information that's required to comply with SMTP standards, such as a sender, a recipient, the date and time that the message was composed, an optional subject line, and an optional message body. This information is contained in the message itself and, by definition, in the message header.

The sender's mail server generates a message envelope for the message by using the sender's and recipient's information found in the message header. It then transmits the message to the Internet for delivery to the recipient's messaging server. Recipients never see the message envelope because it's generated by the message transmission process, and it isn't actually part of the message.

Address rewriting changes an email address by rewriting specific fields in the message header or message envelope. Address rewriting changes several fields in outbound messages, but only one field in inbound email messages. The following table shows which SMTP header fields are rewritten in outbound and inbound messages.

**Message fields rewritten on outbound and inbound messages**

|**Field name**|**Location**|**Outbound messages**|**Inbound messages**|
|:-----|:-----|:-----|:-----|
|**MAIL FROM**|Message envelope|Rewritten|Not rewritten|
|**RCPT TO**|Message envelope|Not rewritten|Rewritten|
|**To**|Message header|Not Rewritten|Rewritten|
|**Cc**|Message header|Not Rewritten|Rewritten|
|**From**|Message header|Rewritten|Not rewritten|
|**Sender**|Message header|Rewritten|Not rewritten|
|**Reply-To**|Message header|Rewritten|Not rewritten|
|**Return-Receipt-To**|Message header|Rewritten|Not rewritten|
|**Disposition-Notification-To**|Message header|Rewritten|Not rewritten|
|**Resent-From**|Message header|Rewritten|Not rewritten|
|**Resent-Sender**|Message header|Rewritten|Not rewritten|

## What address rewriting doesn't change
<a name="What"> </a>

Address rewriting doesn't modify any message header fields that would break SMTP functionality. For example, modifying certain header fields can affect routing loop detection, invalidate the digital signature, or make a rights-protected message unreadable. Therefore, the following header fields aren't modified by address rewriting.

- **Return-Path**

- **Received**

- **Message-ID**

- **X-MS-TNEF-Correlator**

- **Content-Type Boundary=string**

- Header fields located inside MIME body parts

Address rewriting ignores domains that aren't controlled by the Exchange organization. In other words, the domain needs to be configured as an authoritative accepted domain in the Exchange organization. Rewriting non-authoritative domains would cause an uncontrollable form of message relay.

Address rewriting also doesn't modify the header fields of messages that are embedded in another message. Senders and recipients expect embedded messages to remain intact and be delivered without modification, as long as the messages don't trigger mail flow rules (also known as transport rules) that are implemented between the sender and recipient.

## Considerations for outbound-only address rewriting
<a name="Consid1"> </a>

Outbound-only address rewriting on an Edge Transport server modifies the sender's email address as messages leave the Exchange organization. You can configure outbound-only address rewriting for a single user (chris@contoso.com to support@contoso.com), or for a single domain (contoso.com to fabrikam.com). You are required to configure outbound-only address rewriting for multiple subdomains (\*.fabrikam.com to contoso.com).

The rewritten email address needs to be configured as a proxy address on the affected recipients. For example, if laura@sales.contoso.com is rewritten to laura@contoso.com, the proxy address laura@contoso.com needs to be configured on Laura's mailbox. This allows replies and inbound messages to be delivered correctly.

## Considerations for inbound and outbound address rewriting
<a name="Consid2"> </a>

Inbound and outbound, or *bidirectional* address rewriting on an Edge Transport server modifies the sender's email address as messages leave the Exchange organization, and the recipient's email address as messages enter the Exchange organization.

You can configure bidirectional address rewriting for a single user (chris@contoso.com to support@contoso.com), and a single domain (contoso.com to fabrikam.com). You can't configure bidirectional address rewriting for multiple subdomains (\*.fabrikam.com to contoso.com).

## Considerations for rewriting email addresses in multiple domains
<a name="Consid3"> </a>

When you flatten multiple internal domains or subdomains into a single external domain, you need to consider the following factors:

- **Verify unique aliases**: All email aliases (the part to the left of the @ sign) need to be unique across all subdomains. For example, if there is a joe@sales.contoso.com, there can't be a joe@marketing.contoso.com because the rewritten email address for both users would be joe@contoso.com.

- **Add proxy addresses**: The rewritten email address needs to be configured as a proxy address on all affected senders in the affected domains. For example, if joe@sales.contoso.com is rewritten to joe@contoso.com, you need to add the proxy address joe@contoso.com to Joe's mailbox. This allows replies and inbound messages to be delivered correctly.

- **Mail contacts for non-Exchange organizations**: If you're rewriting email addresses from a non-Exchange email system, you need to create mail contacts in Exchange to represent the users in the non-Exchange email system. These email contacts need to contain the original email addresses and the rewritten email addresses. For example, if joe@unix.contoso.com is rewritten to joe@contoso.com, you need to create a mail contact with joe@unix.contoso.com as the external email address and joe@contoso.com as a proxy address.

### Verify unique aliases

When you rewrite email addresses in multiple subdomains, you need to make sure that all email aliases are unique across all your subdomains. For example, consider the following configuration:

The following users are in the subdomains sales.contoso.com, marketing.contoso.com, and research.contoso.com:

- maria@sales.contoso.com

- chris@sales.contoso.com

- david@marketing.contoso.com

- brian@marketing.contoso.com

- chris@research.contoso.com

- adam@research.contoso.com

Suppose you want to rewrite the subdomains sales.contoso.com, marketing.contoso.com, and research.contoso.com into the single domain contoso.com.

When the email addresses in each subdomain are rewritten, a conflict occurs between chris@sales.contoso.com and chris@research.contoso.com, because both email addresses are rewritten to chris@contoso.com. To resolve this issue, you need to change the email address of one of the affected recipients. For example, you can change chris@research.contoso.com to christopher@research.contoso.com so the email address is rewritten to christopher@contoso.com.

## Priority of address rewrite entries
<a name="Prioritization"> </a>

If a user's email address matches multiple address rewrite entries, the email address is only rewritten once based on the closest match. The following list describes the order of precedence of address rewrite entries from highest priority to lowest priority:

1. **Individual email addresses**: An address rewrite entry is configured to rewrite the email address of john@contoso.com to support@contoso.com.

2. **Domain or subdomain mapping**: An address rewrite entry is configured to rewrite all contoso.com email addresses to northwindtraders.com or all sales.contoso.com email addresses to contoso.com.

3. **Domain flattening**: An address rewrite entry is configured to rewrite \*.contoso.com email addresses to contoso.com.

For example, consider an Edge Transport server where the following outbound address rewrite entries are configured:

- \*.contoso.com email addresses are rewritten to contoso.com

- japan.sales.contoso.com email addresses are rewritten to contoso.jp

If masato@japan.sales.contoso.com sends an email message, the address is rewritten to masato@contoso.jp, because that entry most closely matches the sender's email address.

## Digitally signed, encrypted, and rights-protected messages
<a name="Digitally"> </a>

Address rewriting shouldn't affect most signed, encrypted, or rights-protected messages. If address rewriting were to invalidate or otherwise change the security status of these types of messages in any way, address rewriting isn't applied.

The following values can be rewritten because the information isn't part of message signing, encryption, or rights protection:

- Fields in the message envelope.

- Top-level message body headers.

The following values aren't rewritten because the information is part of message signing, encryption, or rights protection:

- Header fields located inside MIME body parts that may be signed.

- The boundary string parameter of the MIME content type.
