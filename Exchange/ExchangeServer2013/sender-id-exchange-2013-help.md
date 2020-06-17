---
title: 'Sender ID: Exchange 2013 Help'
TOCTitle: Sender ID
ms:assetid: 0f628f83-df8c-43fb-bf49-7aaa9ec69ab1
ms:mtpsurl: https://technet.microsoft.com/library/Aa996295(v=EXCHG.150)
ms:contentKeyID: 49248676
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Sender ID

_**Applies to:** Exchange Server 2013_

The Sender ID agent is an anti-spam agent that's available in Microsoft Exchange Server 2013. The Sender ID agent relies on the RECEIVED SMTP header and a query to the sending system's DNS service to determine what action, if any, to take on an inbound message.

Sender ID is intended to combat the impersonation of a sender and a domain, a practice that's frequently called *spoofing*. A *spoofed mail* is an email message that has a sending address that was modified to appear as if it originates from a sender other than the actual sender of the message.

Spoofed mails typically contain a From: address that purports to be from a certain organization. In the past, it was relatively easy to spoof the From: address, in both the SMTP session, such as the MAIL FROM: header, and in the RFC 2822 message data, such as From: "Masato Kawai" masato@contoso.com, because the headers weren't validated.

## Using Sender ID to combat spoofing

Sender ID makes spoofing more difficult. When you enable Sender ID, each message contains a Sender ID status in the metadata of the message. When an email message is received, the Exchange server queries the sender's DNS server to verify that the IP address from which the message was received is authorized to send messages for the domain that's specified in the message headers. The IP address of the authorized sending server is referred to as the purported responsible address (PRA).

Domain administrators publish sender policy framework (SPF) records on their DNS servers. SPF records identify authorized outbound email servers. If an SPF record is configured on the sender's DNS server, the Exchange server parses the SPF record and determines whether the IP address from which the message was received is authorized to send email on behalf of the domain that's specified in the message. For more information about what an SPF record contains and how to create an SPF record, see [Sender ID](sender-id-exchange-2013-help.md).

The Exchange server updates the message metadata with the Sender ID status based on the SPF record. After the Exchange server updates the message metadata, message delivery proceeds as it ordinarily would.

## Sender ID status values

The Sender ID evaluation process generates a Sender ID status for the message. The Sender ID status is used to evaluate the spam confidence level (SCL) rating for the message. This status can be set to one of the following values:

- **Pass**: Both the IP address and Purported Responsible Address (PRA) passed the Sender ID verification check.

- **Neutral**: Published Sender ID data is explicitly inconclusive.

- **Soft fail**: The IP address for the PRA may be in the not permitted set.

- **Fail**: The IP Address is not permitted; no PRA is found in the incoming mail or the sending domain does not exist.

- **None**: No published SPF data exists in the sender's DNS.

- **TempError**: A temporary DNS failure occurred, such as an unavailable DNS server.

- **PermError**: The DNS record is invalid, such as an error in the record format.

The Sender ID status is added to the message metadata and is later converted to a MAPI property. The junk email filter in Microsoft Outlook uses the MAPI property during the generation of the SCL value.

Outlook neither displays the Sender ID status nor necessarily flags a message as junk at certain Sender ID values. Outlook uses the Sender ID status value only during the calculation of the SCL value.

Besides the seven scenarios that generate the Sender ID statuses, the Sender ID evaluation process may reveal instances where the From: IP address is missing. If the From: IP address is missing, the Sender ID status can't be set. If the Sender ID status can't be set, Exchange continues to process the message without including a Sender ID status on the message. The message isn't discarded or rejected. In this scenario, Sender ID status isn't set, and an application event is logged.

For more information about how the Sender ID status is displayed in messages, see [Anti-spam stamps](anti-spam-stamps-exchange-2013-help.md).

## Sender ID options for handling spoofed mail and unreachable DNS servers

You can also define how the Exchange server handles messages that are identified as spoofed mail and how the Exchange server handles messages when a DNS server can't be reached. The options for how the Exchange server handles spoofed mail and unreachable DNS servers include the following actions:

- **Stamp the status**: This option is the default action. All inbound messages to your organization have the Sender ID status included in the metadata of the message.

- **Reject**: This option rejects the message and sends an SMTP error response to the sending server. The SMTP error response is a 5*xx* level protocol response with text that corresponds to the Sender ID status.

- **Delete**: This option deletes the message without informing the sending system of the deletion. In fact, the Exchange server sends a fake OK SMTP command to the sending server and then deletes the message. Because the sending server assumes the message was sent, it doesn't retry sending the message in the same session.

For more information about how to configure the Sender ID agent, see [Manage Sender ID](manage-sender-id-exchange-2013-help.md).

## Updating your organization's Internet-facing DNS to support Sender ID

The effectiveness of Sender ID depends on specific DNS data. The more organizations that update their Internet-facing DNS servers by using an SPF record, the more effectively Sender ID identifies spoofed email messages.

To support the Sender ID infrastructure, you must update your Internet-facing DNS data by creating an SPF record and hosting the SPF record on your public DNS servers. For more information about how to create and deploy SPF records, see [Sender ID](sender-id-exchange-2013-help.md).

## Specifying recipients and sender domains to exclude from Sender ID filtering

You may want to exclude specific recipients and sender domains from Sender ID filtering. To do this, you specify the recipients and sender domains using the **Set-SenderIdConfig** cmdlet in the Exchange Management Shell. For more information, see [Set-SenderIdConfig](https://docs.microsoft.com/powershell/module/exchange/Set-SenderIdConfig).
