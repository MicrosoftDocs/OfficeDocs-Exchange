---
localization_priority: Normal
description: 'Summary: Learn about journaling in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6a20f207-4485-44ef-b010-ec760eb5165b
ms.reviewer: 
title: Journaling in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Journaling in Exchange Server

Journaling in Exchange Server can help your organization respond to legal, regulatory, and organizational compliance requirements by recording all or targeted email messages. Journaling in Exchange Server is basically unchanged from Exchange Server 2010.

Exchange provides the following journaling options:

- **Standard journaling**: Journal all messages that are sent to and received by mailboxes on a specific mailbox database. To journal all messages in your organization, you need to configure journaling on all mailbox databases on all Exchange servers.

- **Premium journaling**: Use *journal rules* to journal messages based on recipients (all recipients or specified recipients), and scope (internal messages, external messages, or all messages). Premium journaling requires Exchange Enterprise client access licenses (CALs). For more information about CALs, see [Exchange licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-server-licensing-licensing-overview).

To configure journaling, see [Journaling procedures in Exchange Server](journaling-procedures.md).

When you plan for messaging retention and compliance, it's important to understand journaling, and how journaling fits in your organization's compliance policies.

## Why journaling is important

First, it's important to understand the difference between journaling and archiving when it comes to email messages:

- *Journaling* refers to recording email communications as part of the organization's email retention strategy.

- *Archiving* refers to removing email messages from their native location (for example, a user's mailbox), and storing them elsewhere.

Many organizations need to maintain records of the email communication that occurs as employees perform their daily business tasks. You can use Exchange journaling as a tool in your email retention or archival strategy.

Although a regulation may not specifically require journaling, Exchange journaling can help your organization achieve compliance with the regulation. For example, corporate officers in some financial sectors can be held liable for claims that are made by their employees to customers. Designated compliance managers can use journaling to collect and regularly review the email messages that are sent by employees to customers as part of their greater employee-to-customer communications review. The compliance managers can report their approval to the corporate officer, and the corporate officer can then report compliance to the regulating body.

The following list shows some of the more well-known U.S. and international regulations where Exchange journaling may help form part of your compliance strategies:

- Sarbanes-Oxley Act of 2002 (SOX)

- Security Exchange Commission Rule 17a-4 (SEC Rule 17 A-4)

- National Association of Securities Dealers 3010 & 3110 (NASD 3010 & 3110)

- Gramm-Leach-Bliley Act (Financial Modernization Act)

- Financial Institution Privacy Protection Act of 2001

- Financial Institution Privacy Protection Act of 2003

- Health Insurance Portability and Accountability Act of 1996 (HIPAA)

- Uniting and Strengthening America by Providing Appropriate Tools Required to Intercept and Obstruct Terrorism Act of 2001 (Patriot Act)

- European Union Data Protection Directive (EUDPD)

- Japan's Personal Information Protection Act

## Journaling agent

The *Journaling agent* is the built-in Exchange transport agent that processes messages as they flow through the Transport service on Mailbox servers. The journaling configuration settings are stored in Active Directory, and are read by the Journaling agent. The Journaling agent is registered on the **OnSubmittedMessage** and **OnRoutedMessage** categorizer events in the transport pipeline. For more information about the transport pipeline, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

Note that built-in transport agents like the Journaling agent are invisible and unmanageable by the transport agent management cmdlets (**\*-TransportAgent**).

## Journal reports

A *journal report* is the message that's recorded by journaling. The journal report contains the original message as an unaltered file attachment. The body of the journal report contains summary information from the original message (for example, the sender's email address, message subject, **Message-ID**, and recipient email addresses). This type of journaling is known as *envelope journaling*, and is the only journaling method that's supported by Exchange.

### Journal reports and IRM-protected messages

You need to consider the effects of IRM-protected messages on journal reports. Third-party archiving systems that don't have built-in RMS support can't decrypt the IRM-protected messages in journal reports, which negatively affects the search and discovery of content in journaled messages. In Exchange, you can configure journal report decryption to save a clear-text copy of the message in the journal report. For more information, see [Enable journal report decryption](journaling-procedures.md#enable-journal-report-decryption).

## Journal rules

The basic components of a journal rule are:

- **Journal recipient**: Who you want to journal.

- **Journal rule scope**: What you want to journal.

- **Journaling mailbox**: Where you want to store the journaled messages.

### Journal recipient

The *journal recipient* specifies who you want to journal. Messages that are sent to or received by the journal recipient are journaled (the direction doesn't matter). You can configure a journal rule to journal messages for all senders and recipients in the Exchange organization, or you can limit a journal rule to an Exchange mailbox, group, mail user, or mail contact. If you specify a distribution group, you enable journaling for the *members* of the distribution group (not for the group itself).

By targeting specific recipients or groups of recipients, you can configure a journaling environment that helps you meet your organization's regulatory and legal requirements, while minimizing the storage and other costs that are associated with retaining large amounts of data.

#### Journal recipients that are enabled for Unified Messaging in Exchange 2016

By default, if your Exchange 2016 organization uses Unified Messaging (UM) to consolidate the email, voice mail, and fax infrastructure, Exchange is configured to journal voice mail notification and missed call notification messages. You can disable journaling for these types of messages, but messages that contain UM-generated faxes are always journaled.

To disable journaling for voice mail and missed call notifications, see [Enable or disable journaling for voice mail and missed call notifications](journaling-procedures.md#enable-or-disable-journaling-for-voice-mail-and-missed-call-notifications).

> [!NOTE]
> Unified Messaging is not available in Exchange 2019.

### Journal rule scope

After you define who you want to journal, you need to define the scope of the messages to journal. The available scopes are:

- **Internal messages only**: The source or destination of the message is inside your Exchange organization.

- **External messages only**: The source or destination of the message is outside your Exchange organization.

- **All messages**: The source or destination of the message doesn't matter. Note that a journal rule with this scope could potentially journal messages that were already journaled by other rules with internal only or external only scopes.

### Journaling mailbox

The journaling mailbox is where the journaled messages are delivered. How you configure the journaling mailbox depends on your organization's policies, regulatory requirements, and legal requirements. For example, you may be able to configure one journaling mailbox for all journal rules in your organization, or you may be required to use different journaling mailboxes for different journal rules.

 **Notes**:

- Journaling mailboxes contain sensitive information, so you need to secure access to them. Messages in the journaling mailbox may be part of legal proceedings or subject to regulatory requirements. We recommend that you create and enforce clearly-defined policies that indicate who has access to a journaling mailbox. Speak with your legal representatives to verify that your journaling solution complies with all the laws and regulations that apply to your organization.

- An Office 365 mailbox can't be used as a journaling mailbox. If you're running a hybrid deployment between on-premises Exchange and Office 365, you can designate on-premises journaling mailboxes for your Office 365 and on-premises organizations. You can also deliver journaled messages to an on-premises email archiving system or a third-party email archiving service.

- Journaling mailboxes need to accept messages that are at least as large as the maximum message size that's available in your organization. Be sure to account for any custom maximum message sizes that you've configured on individual mailboxes. For more information, see [Configure message size limits for a mailbox](../../recipients/user-mailboxes/mailbox-message-size-limits.md).

- We recommend that you configure the journaling mailbox to only accept messages from the Microsoft Exchange recipient (the only sender of journal reports). Note that you can only do this in the Exchange Management Shell. For more information, see [Configure message delivery restrictions for a mailbox](../../recipients/user-mailboxes/message-delivery-restrictions.md).

- We recommend that you disable the storage quota limits for the journaling mailbox. For more information, see [Configure storage quotas for a mailbox](../../recipients/user-mailboxes/storage-quotas.md).

#### Alternate journaling mailbox

Like other messages, undeliverable journal reports are queued, and delivery is periodically retried until the message expires (the default value is two days, and is configured by the _MessageExpirationTimeout_ parameter on the **Set-TransportService** cmdlet). Unlike other messages, expired journal reports can't be returned to the sender in a non-delivery report (also known as an NDR or bounce message), because the sender is the Microsoft Exchange recipient. Expired journal reports can't be recovered.

If you don't want undeliverable journal reports to queue and eventually expire, you can specify an *alternate journaling mailbox* that accepts the NDRs for *all* undeliverable journal reports when *any* journaling mailbox is unavailable (one alternate journaling mailbox for all journaling mailboxes in your organization). The original journal report is an attachment in the NDR. When the journaling mailbox becomes available again, you can use the **Resend this message** feature in Outlook on the NDRs in the alternate journaling mailbox to send the unaltered delivery reports to the journaling mailbox.

Before you configure an alternate journaling mailbox, contact your legal representatives. Laws or regulations that apply to your organization may prohibit all journaled messages from being stored in the same mailbox.

When you configure an alternate journaling mailbox, you should use the same criteria that you used when you configured the journaling mailbox.

 **Notes**:

- If the alternate journaling mailbox also becomes unavailable and rejects the NDRs for undeliverable journal reports, the original journal reports are lost and can't be recovered.

- You should treat the alternate journaling mailbox as a special dedicated mailbox. Journal rules, Inbox rules, and mail flow rules (also known as transport rules) that involve the alternate journaling mailbox are ignored.

## Journal rule replication

Because journal rules are stored in Active Directory, they're read and applied by the Transport service on all Mailbox servers in the organization. When you create, modify, or remove a journal rule, the change is replicated between the domain controllers in your organization. This allows Exchange to provide a consistent set of journal rules across the organization.

 **Notes**:

- Replication between domain controllers depends on factors that aren't controlled by Exchange (for example, the number of Active Directory sites, and the speed of network links). Therefore, you need to consider replication delays when you implement journal rules in your organization. For more information about Active Directory replication, see [Introduction to Active Directory Replication and Topology Management Using Windows PowerShell](https://go.microsoft.com/fwlink/p/?LinkId=).

- Each Mailbox server caches expanded distribution groups to avoid repeated Active Directory queries to determine a group's membership. By default, entries in the expanded groups cache expire every four hours. Therefore, changes to the group's membership can't be applied to journal rules until the expanded groups cache is updated. To force an immediate update of the cache on a Mailbox server, restart the Microsoft Exchange Transport service. You need to restart the service on each Mailbox server where you want to forcibly update the cache.

## Troubleshooting

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612). If you're having trouble with the alternate journaling mailbox, see [KB2829319](https://support.microsoft.com/help/2829319).
