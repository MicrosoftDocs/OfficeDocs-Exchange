---
localization_priority: Normal
description: 'Summary: Learn how Edge Transport servers provide Internet mail flow, antispam, and mail flow rules for your Exchange Server organization'
ms.topic: reference
author: chrisda
ms.author: chrisda
ms.assetid: cfff9f59-afac-447c-8297-afcebe49a52d
ms.date: 7/3/2018
ms.reviewer: 
title: Edge Transport servers
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Edge Transport servers

Edge Transport servers handle all inbound and outbound Internet mail flow by providing mail relay and smart host services for your Exchange organization. Agents running on the Edge Transport server provide additional layers of message protection and security. These agents provide protection against spam and apply mail flow rules (also known as transport rules) to control mail flow. All of these features work together to help minimize the exposure of your internal Exchange to threats on the Internet.

Because the Edge Transport server is installed in the perimeter network, it's never a member of your organization's internal Active Directory forest and doesn't have access to Active Directory information. However, the Edge Transport server requires data that resides in Active Directory: for example, connector information for mail flow and recipient information for antispam recipient lookup tasks. This data is synchronized to the Edge Transport server by the Microsoft Exchange EdgeSync service (EdgeSync). EdgeSync is a collection of processes run on an Exchange 2016 or Exchange 2019 Mailbox server to establish one-way replication of recipient and configuration information from Active Directory to the Active Directory Lightweight Directory Services (AD LDS) instance on the Edge Transport server. EdgeSync copies only the information that's required for the Edge Transport server to perform antispam configuration tasks and to enable end-to-end mail flow. EdgeSync performs scheduled updates so the information in AD LDS remains current. For more information about Edge Subscriptions and EdgeSync, see [Edge Subscriptions](edge-subscriptions.md).

You can install more than one Edge Transport server in the perimeter network. Deploying more than one Edge Transport server provides redundancy and failover capabilities for your inbound message flow. You can load balance the SMTP traffic to your organization among Edge Transport servers by defining more than one MX record with the same priority value for your mail domain. You can achieve consistency in the configuration among multiple Edge Transport servers by using cloned configuration scripts.

The Edge Transport server role lets you manage the following message-processing scenarios.

## Internet mail flow

Edge Transport servers accept messages coming into the Exchange organization from the Internet. After the messages are processed by the Edge Transport server, mail is routed to an internal Exchange Mailbox server; first to the Front End Transport service, and then to the Transport service.

All messages sent to the Internet from inside the organization are routed to Edge Transport servers after the messages are processed by the Transport service on the Exchange Mailbox server. You can configure the Edge Transport server to use DNS to resolve MX resource records for external SMTP domains, or you can configure the Edge Transport server to forward messages to a smart host for DNS resolution.

## Antispam protection

In Exchange Server, antispam features provide services to block unsolicited commercial email (spam) at the network perimeter.

Spammers use a variety of techniques to send spam into your organization. Edge Transport servers help prevent users from ever receiving spam by providing a collection of agents that work together to provide different layers of spam filtering and protection. Establishing tarpitting intervals on connectors makes email harvesting attempts ineffective.

## Mail flow rules on Edge Transport servers

Mail flow rules on Edge Transport servers are used to control the flow of messages sent to or received from the internet. Mail flow rules are configured on each Edge Transport server to help protect corporate network resources and data by applying an action to messages meeting specified conditions. Mail flow rule conditions are based on data, such as specific words or text patterns in the message subject, body, header, or from address; the spam confidence level (SCL); or the attachment type. Actions determine how the message is processed when a specified condition is true. Possible actions include quarantining a message, dropping or rejecting a message, appending additional recipients, or logging an event. Optional exceptions exempt particular messages from having an action applied.

## Address rewriting

Address rewriting presents a consistent email address appearance to external recipients. You configure address rewriting on Edge Transport servers to modify the SMTP addresses on inbound and outbound messages. Address rewriting is especially useful for newly merged organizations that want to present a consistent email address appearance.
