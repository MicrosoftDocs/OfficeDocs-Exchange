---
title: 'Journaling: Exchange 2013 Help'
TOCTitle: Journaling
ms:assetid: 6a20f207-4485-44ef-b010-ec760eb5165b
ms:mtpsurl: https://technet.microsoft.com/library/Aa998649(v=EXCHG.150)
ms:contentKeyID: 49354855
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Journaling

_**Applies to:** Exchange Server 2013_

Journaling can help your organization respond to legal, regulatory, and organizational compliance requirements by recording inbound and outbound email communications. When planning for messaging retention and compliance, it's important to understand journaling, how it fits in your organization's compliance policies, and how Microsoft Exchange Server 2013 helps you secure journaled messages.

## Why journaling is important

First, it's important to understand the difference between journaling and a data archiving strategy:

- *Journaling* is the ability to record all communications, including email communications, in an organization for use in the organization's email retention or archival strategy. To meet an increasing number of regulatory and compliance requirements, many organizations must maintain records of communications that occur when employees perform daily business tasks.

- *Data archiving* refers to backing up the data, removing it from its native environment, and storing it elsewhere, therefore reducing the strain of data storage. You can use Exchange journaling as a tool in your email retention or archival strategy.

Although journaling may not be required by a specific regulation, compliance may be achieved through journaling under certain regulations. For example, corporate officers in some financial sectors may be held liable for the claims made by their employees to their customers. To verify that the claims are accurate, a corporate officer may set up a system where managers review some part of employee-to-client communications regularly. Every quarter, the managers verify compliance and approve their employees' conduct. After all managers report approval to the corporate officer, the corporate officer reports compliance, on behalf of the company, to the regulating body. In this example, email messages might be one type of the employee-to-client communications that managers must review; therefore, journaling can be used to collect all email messages sent by client-facing employees. Other client communication mechanisms may include faxes and telephone conversations, which may also be subject to regulation. The ability to journal all classes of data in an enterprise is a valuable functionality of the IT architecture.

The following list shows some of the more well-known U.S. and international regulations where journaling may help form part of your compliance strategies:

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

In an Exchange 2013 organization, all email traffic is routed by Mailbox servers. All messages traverse at least one server running the Transport service in their lifetime. The *Journaling agent* is a compliance-focused transport agent that processes messages on Mailbox servers. It fires on the **OnSubmittedMessage** and **OnRoutedMessage** transport events.

> [!NOTE]
> In Exchange 2013, the Journaling agent is a built-in agent. Built-in agents aren't included in the list of agents returned by the <STRONG>Get-TransportAgent</STRONG> cmdlet. For more details, see <A href="transport-agents-exchange-2013-help.md">Transport agents</A>.

Exchange 2013 provides the following journaling options:

- **Standard journaling**: Standard journaling is configured on a mailbox database. It enables the Journaling agent to journal all messages sent to and from mailboxes located on a specific mailbox database. To journal all messages to and from all recipients and senders, you must configure journaling on all mailbox databases on all Mailbox servers in the organization.

- **Premium journaling**: Premium journaling enables the Journaling agent to perform more granular journaling by using journal rules. Instead of journaling all mailboxes residing on a mailbox database, you can configure journal rules to match your organization's needs by journaling individual recipients or members of distribution groups. You must have an Exchange Enterprise client access license (CAL) to use premium journaling.

When you enable standard journaling on a mailbox database, this information is saved in Active Directory and is read by the Journaling agent. Similarly, journal rules configured with premium journaling are also saved in Active Directory and applied by the Journaling agent. For more information about how to configure standard and premium journaling, see [Manage journaling](https://docs.microsoft.com/exchange/security-and-compliance/journaling/manage-journaling).

## Journal rules

The following are key aspects of journal rules:

- **Journal rule scope**: Defines which messages are journaled by the Journaling agent.

- **Journal recipient**: Specifies the SMTP address of the recipient you want to journal.

- **Journaling mailbox**: Specifies one or more mailboxes used for collecting journal reports.

## Journal rule scope

You can use a journal rule to journal only internal messages, only external messages, or both. The following list describes these scopes:

- **Internal messages only**: Journal rules with the scope set to journal internal messages sent between the recipients inside your Exchange organization.

- **External messages only**: Journal rules with the scope set to journal external messages sent to recipients or received from senders outside your Exchange organization.

- **All messages**: Journal rules with the scope set to journal all messages that pass through your organization regardless of origin or destination. These include messages that may have already been processed by journal rules in the Internal and External scopes.

## Journal recipient

You can implement targeted journaling rules by specifying the SMTP address of the recipient you want to journal. The recipient can be an Exchange mailbox, distribution group, mail user, or contact. These recipients may be subject to regulatory requirements, or they may be involved in legal proceedings where email messages or other communications are collected as evidence. By targeting specific recipients or groups of recipients, you can easily configure a journaling environment that matches your organization's processes and meets regulatory and legal requirements. Targeting only the specific recipients that need to be journaled also minimizes storage and other costs associated with retention of large amounts of data.

All messages sent to or from the journaling recipients you specify in a journaling rule are journaled. If you specify a distribution group as the journaling recipient, all messages sent to or from members of the distribution group are journaled. If you don't specify a journaling recipient, all messages sent to or from recipients that match the journal rule scope are journaled.

**Unified Messaging-enabled journal recipients**

Many organizations that implement journaling may also use Unified Messaging (UM) to consolidate their email, voice mail, and fax infrastructure. However, you may not want the journaling process to generate journal reports for messages generated by Unified Messaging. In these cases, you can decide whether to journal voice mail messages and missed call notification messages handled by an Exchange server running the Unified Messaging service or to skip such messages. If your organization doesn't require journaling of such messages, you can reduce the amount of hard disk storage space required to store journal reports by skipping these messages.

> [!NOTE]
> Messages that contain faxes generated by a the Unified Messaging service are always journaled, even if you disable the journaling of Unified Messaging voice mail and missed call notification messages.

For more information about how to enable or disable voice mail and missed call notification messages, see [Disable or enable journaling of voice mail and missed call notifications](disable-or-enable-journaling-of-voice-mail-and-missed-call-notifications-exchange-2013-help.md).

## Journaling mailbox

The journaling mailbox is used to collect journal reports. How you configure the journaling mailbox depends on your organization's policies, regulatory requirements, and legal requirements. You can specify one journaling mailbox to collect messages for all the journal rules configured in the organization, or you can use different journaling mailboxes for different journal rules or sets of journal rules.

> [!IMPORTANT]
> You can't designate an Office 365 mailbox as a journaling mailbox. You can deliver journal reports to an on-premises archiving system or a third-party archiving service. If you're running a hybrid deployment with your mailboxes split between on-premises servers and Office 365, you can designate an on-premises mailbox as the journaling mailbox for your Office 365 and on-premises mailboxes.

> [!IMPORTANT]
> Journaling mailboxes contain very sensitive information. You must secure journaling mailboxes because they collect messages that are sent to and from recipients in your organization. These messages may be part of legal proceedings or may be subject to regulatory requirements. Various laws require that messages remain tamper-free before they're submitted to an investigatory authority. We recommend that you create policies that govern who can access the journaling mailboxes in your organization, limiting access to only those individuals who have a direct need to access them. Speak with your legal representatives to make sure that your journaling solution complies with all the laws and regulations that apply to your organization.

> [!IMPORTANT]
> Journaling mailboxes should accept messages up to a size equal to or greater than the maximum message size you have set in your organization. If you have configured exceptions to the MaxSendSize and MaxReceiveSize on an individual user's mailbox that are bigger than the general setting in TransportConfig, you should set the journaling mailbox's MaxSendSize and MaxReceiveSize accordingly to ensure that messages sent to journaling are accepted and not queued or rejected. Messages rejected by the journaling mailbox will be retried periodically, causing unnecessary database growth and waste of resources. In addition, we recommend that you set up an alternate journaling mailbox to prevent other non-deliverable situations from occurring.

## Alternate journaling mailbox

When the journaling mailbox is unavailable, you may not want the undeliverable journal reports to collect in mail queues on Mailbox servers. Instead, you can configure an alternate journaling mailbox to store those journal reports. The alternate journaling mailbox receives the journal reports as attachments in the non-delivery reports (NDRs) generated when the journaling mailbox or the server on which it's located refuses delivery of the journal report or becomes unavailable.

When the journaling mailbox becomes available again, you can use the **Send Again** feature of OfficeOutlook to submit journal reports for delivery to the journaling mailbox.

When you configure an alternate journaling mailbox, all the journal reports that are rejected or can't be delivered across your entire Exchange organization are delivered to the alternate journaling mailbox. Therefore, it's important to make sure that the alternate journaling mailbox and the Mailbox server where it's located can support many journal reports.

> [!WARNING]
> If you configure an alternate journaling mailbox, you must monitor the mailbox to make sure that it doesn't become unavailable at the same time as the journal mailboxes. If the alternate journaling mailbox also becomes unavailable or rejects journal reports at the same time, the rejected journal reports are lost and can't be retrieved.

Because the alternate journaling mailbox collects all the rejected journal reports for the entire Exchange organization, you must make sure that this doesn't violate any laws or regulations that apply to your organization. If laws or regulations prohibit your organization from allowing journal reports sent to different journaling mailboxes from being stored in the same alternate journaling mailbox, you may be unable to configure an alternate journaling mailbox. Discuss this with your legal representatives to determine whether you can use an alternate journaling mailbox.

When you configure an alternate journaling mailbox, you should use the same criteria that you used when you configured the journaling mailbox.

> [!IMPORTANT]
> The alternate journaling mailbox should be treated as a special dedicated mailbox. Any messages addressed directly to the alternate journaling mailbox aren't journaled.

## Journal rule replication

Journal rules are stored in Active Directory and applied by all Mailbox servers in the Exchange 2013 organization. When you create, modify, or remove a journal rule, the change is replicated to all Active Directory servers in the organization. All Mailbox servers in the organization then retrieve the updated journal rule configuration from the Active Directory servers and apply the new or modified journal rules.

By replicating all the journal rules across the organization, Exchange 2013 enables you to provide a consistent set of journal rules across the organization. All messages that pass through your Exchange 2013 organization are subject to the same journal rules.

> [!IMPORTANT]
> Replication of journal rules across an organization is dependant on Active Directory replication. Replication time between Active Directory domain controllers varies depending on the number of sites in the organization and the speed of links and other factors outside the control of Microsoft Exchange. Consider replication delays when you implement journal rules in your organization. For more information about Active Directory replication, see <A href="https://docs.microsoft.com/windows-server/identity/ad-ds/manage/powershell/introduction-to-active-directory-replication-and-topology-management-using-windows-powershell--level-100-">Introduction to Active Directory Replication and Topology Management Using Windows PowerShell</A>.

> [!IMPORTANT]
> Each Mailbox server caches distribution group membership to avoid repeated round trips to Active Directory. The expanded groups cache reduces the number of requests that each Mailbox server must make to an Active Directory domain controller. By default, entries in the expanded groups cache expire in four hours. Therefore, if you specify a distribution group as the journal recipient, changes to distribution group membership may not be applied to journal rules until the expanded groups cache is updated. To force an immediate update of the recipient cache, you must stop and start the Microsoft Exchange Transport service. You must do this for each Mailbox server where you want to forcibly update the recipient cache.

## Journal reports

A *journal report* is the message that the Journaling agent generates when a message matches a journal rule and is to be submitted to the journaling mailbox. The original message that matches the journal rule is included unaltered as an attachment to the journal report. The body of a journal report contains information from the original message such as the sender email address, message subject, message-ID, and recipient email addresses. This is also referred to as envelope journaling, and is the only journaling method supported by Exchange 2013.

## Journal reports and IRM-protected messages

When implementing journaling in an Exchange 2013 environment, you must consider journaling reports and IRM-protected messages. IRM-protected messages will affect the search and discovery capabilities of third-party archiving systems that don't have RMS support built-in. In Exchange 2013, you can configure Journal Report Decryption to save a clear-text copy of the message in a journal report.

## Interoperability with Exchange 2007

There isn't much difference between journaling functionality in Exchange 2013, Exchange 2010, and Exchange 2007. However, in Exchange 2010 and later, Setup creates a separate container in Active Directory to store journal rules. When you set up the first Exchange 2010 or later server in an Exchange 2007 organization, Setup creates a copy of the existing journal rules in Exchange 2007 and stores them in the new container. When Setup completes, the journal rules in both versions of Exchange are consistent.

After Setup, if you change the journal rule configuration on Exchange 2010 (or later), you must make the same change on Exchange 2007 servers to make sure they're consistent. Similarly, changes made to Journal rules on Exchange 2007 must be also be made in Exchange 2010 or later. You can also export journal rules from Exchange 2007 and import them to Exchange 2013.

## Troubleshooting

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612). If you're having trouble with the **JournalingReportDNRTo** mailbox, see [Transport and Mailbox Rules in Exchange Online don't work as expected](https://support.microsoft.com/help/2829319).
