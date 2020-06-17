---
title: 'Remote domains: Exchange 2013 Help'
TOCTitle: Remote domains
ms:assetid: 10fb7d62-4d78-40a3-82db-d62bcd27ba42
ms:mtpsurl: https://technet.microsoft.com/library/Aa996309(v=EXCHG.150)
ms:contentKeyID: 49289173
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remote domains

_**Applies to:** Exchange Server 2013_

You can create remote domain entries to define the settings for message transfer between the Microsoft Exchange Server 2013 organization and domains outside your Exchange organization. When you create a remote domain entry, you control the types of messages that are sent to that domain. You can also apply message format policies and acceptable character sets for messages that are sent from users in your organization to the remote domain. The settings for remote domains are global configuration settings for the Exchange organization.

The remote domain settings are applied to messages during categorization in the Transport service on Mailbox servers. When recipient resolution occurs, the recipient domain is matched against the configured remote domains. If a remote domain configuration blocks a specific message type from being sent to recipients in that domain, the message is deleted. If you specify a particular message format for the remote domain, the message headers and content are modified. The settings apply to all messages that are processed by the Exchange organization.

> [!NOTE]
> If you configure message settings per user, the per-user settings override the organizational configuration.

By default, there's a single remote domain entry. The domain address space is configured as an asterisk (\*). This represents all remote domains. If you don't create additional remote domain entries, all messages that are sent to all recipients in all remote domains have the same settings applied to them.

When you configure remote domains, you can prevent certain types of messages from being sent to that domain. These message types include out-of-office messages, auto-reply messages, non-delivery reports (NDRs), and meeting forward notifications. If you have a multiple forest environment, you may want to allow the sending of those types of messages to those domains. However, if you have identified a domain from which spam originates, you may want to block sending of those types of messages to those remote domains.

## Message format

You can specify the message format and the character set to use for email messages that are sent to remote domains. These settings can be useful to make sure that email sent by senders in your domain to the remote domain is compatible with the receiving email system. For example, if you know that the remote domain's messaging system is Exchange, you can specify to always use Exchange rich text format (RTF). For more information, see [Content conversion](content-conversion-exchange-2013-help.md).

## Automatic replies settings

In Exchange 2013, users can specify different automatic replies for internal and external recipients. Furthermore, the types of automatic replies available in your organization also depend on the Microsoft Outlook version in use.

In Exchange 2013, there are two types of automatic replies:

- **External**: Supported by Exchange 2013 and Exchange 2010. Can only be set by Outlook 2010 or Office Outlook 2007, or using Outlook Web App.

- **Internal**: Supported by Exchange 2013 and Exchange 2010. Can only be set by Outlook 2010 or Outlook 2007, or using Outlook Web App.

The following table describes various client and server combinations and the types of automatic replies that can be used in each scenario.

**Client and server support for automatic replies**

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Client version</th>
<th>Exchange version</th>
<th>Automatic replies supported</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2010 or Outlook 2007</p></td>
<td><p>Exchange 2013 Exchange 2010 Exchange 2007</p></td>
<td><p>Internal, External</p></td>
</tr>
<tr class="even">
<td><p>Outlook Web App</p></td>
<td><p>Exchange 2013 Exchange 2010 Exchange 2007</p></td>
<td><p>Internal, External</p></td>
</tr>
</tbody>
</table>

## Controlling NDR information

As mentioned at the beginning of this topic, you can prevent NDRs from being sent to a remote domain. By blocking NDRs from being sent to a remote domain, you can prevent the information contained within the NDR message from leaving your organization, thereby limiting the knowledge that a malicious user can obtain about your organization. However, this also prevents legitimate senders from receiving NDRs, resulting in confusion and lost productivity.

Exchange 2013 provides you with granular control over the contents of an NDR destined for a remote domain. With Exchange 2013, you can allow NDRs to a remote domain, while stripping any diagnostic information. This way, you can still prevent information about your Exchange deployment from leaving your organization while at the same time providing NDR notifications to external senders.

This feature is controlled with the *NDRDiagnosticInfoEnabled* parameter on the **Set-RemoteDomain** cmdlet. Because this setting is configurable for each remote domain, you can have different settings based on your needs. For example, you can choose to remove the NDR diagnostic information for the default remote domain, but allow full NDR diagnostic information for the remote domains that represent your partners.

For more information about this new setting, see [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/Set-RemoteDomain).
