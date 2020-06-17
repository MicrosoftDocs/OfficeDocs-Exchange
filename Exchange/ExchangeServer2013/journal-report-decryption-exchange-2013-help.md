---
title: 'Journal report decryption: Exchange 2013 Help'
TOCTitle: Journal report decryption
ms:assetid: c063e2bd-2444-480d-8b35-73f31064a31b
ms:mtpsurl: https://technet.microsoft.com/library/Dd876936(v=EXCHG.150)
ms:contentKeyID: 49319930
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Journal report decryption

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, Information Rights Management (IRM) allows Microsoft Outlook 2010 and later and Microsoft Office Outlook Web App users to protect their messages. You can create Outlook protection rules to automatically apply IRM-protection to messages before they're sent from an Outlook 2010 client. You can also create transport protection rules to apply IRM protection to messages in transit that match the rule conditions.

To learn about Outlook protection rules, see [Outlook protection rules](outlook-protection-rules-exchange-2013-help.md).

## Limitations of standard encryption solutions

If your organization encrypts messages by using traditional solutions such as S/MIME, your records managers won't be able to inspect or search the encrypted content. Archiving encrypted messages that contain inaccessible and unsearchable content may not meet business, regulatory, or compliance requirements. When faced with an electronic discovery (eDiscovery) request, an inability to decrypt, search, and present content from encrypted messages can be a challenge, and failure to do so may expose your organization to legal and financial risks.

Also, your organization's messaging policies may require journaled messages to be decrypted so the content can be accessible to eDiscovery tools, automated processes, or records managers who access a journaling mailbox. Journal report decryption in Exchange 2010 can help you meet these requirements.

To learn more about journaling, see [Journaling](journaling-exchange-2013-help.md).

## Journal report decryption

Journal report decryption allows you to save a clear-text copy of IRM-protected messages in journal reports, along with the original, IRM-protected message. If the IRM-protected message contains any supported attachments that were protected by the Active Directory Rights Management Services (AD RMS) cluster in your organization, the attachments are also decrypted.

> [!IMPORTANT]
> To use journal report decryption, you must have an Exchange Enterprise client access license (CAL). Journal report decryption only supports premium journaling.

Decryption is performed by the Journal Report Decryption agent, a compliance-focused transport agent. The Journal Report Decryption agent fires on the **OnCategorizedMessage** event. Messages protected in-transit using transport protection rules are already encrypted by the Encryption agent, which fires on the **OnRoutedMessage** event, before they get to the Journal Report Decryption agent. The Journal Report Decryption agent decrypts these messages.

> [!NOTE]
> In Exchange 2013, the Journal Report Decryption agent is a built-in agent. Built-in agents aren't included in the list of agents returned by the <STRONG>Get-TransportAgent</STRONG> cmdlet. For more details, see <A href="transport-agents-exchange-2013-help.md">Transport agents</A>.

The agent decrypts the following types of IRM-protected messages:

  - Messages that were IRM-protected by the user in Outlook Web App.

  - Messages that were IRM-protected by the user in Outlook 2010.

  - Messages that were IRM-protected automatically in Outlook 2010 by using Outlook protection rules.

  - Messages that were IRM-protected automatically in transit by using transport protection rules.

> [!IMPORTANT]
> Only messages that were IRM-protected by the AD&nbsp;RMS server in your organization are decrypted by the Journal Report Decryption agent. The agent doesn't decrypt an attachment if it isn't protected at the same time as the message (and therefore doesn't have the same use license), or if an IRM-protected file is attached to an unprotected message.

## Configuring journal report decryption

Journal report decryption is configuredb using the [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration) cmdlet in the Exchange Management Shell. However, before you configure journal report decryption, you must assign Exchange 2013 servers the permissions to decrypt content that's IRM-protected by your AD RMS server. To do this, you add the Federation mailbox to the super users group configured on your organization's AD RMS cluster. For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

> [!IMPORTANT]
> In cross-forest AD&nbsp;RMS deployments where you have an AD&nbsp;RMS cluster deployed in each forest, you must add the Federation mailbox to the super users group on the AD&nbsp;RMS cluster in each forest to allow Exchange 2013 Transport service to decrypt the messages protected against each AD&nbsp;RMS cluster.

For details about how to configure journal report decryption, see [Enable or Disable Journal Report Decryption](enable-or-disable-journal-report-decryption-exchange-2013-help.md).

After you enable journal report decryption, the journaling mailbox may contain journal reports with sensitive information in an unencrypted form. As a best practice, we recommend that access to the journaling mailbox be monitored closely and restricted only to authorized individuals. This is a best-practice even if you're not using IRM protection for e-mail.
