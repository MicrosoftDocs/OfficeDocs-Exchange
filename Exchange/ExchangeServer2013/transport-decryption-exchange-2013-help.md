---
title: 'Transport decryption: Exchange 2013 Help'
TOCTitle: Transport decryption
ms:assetid: 4267c46d-f488-404d-a5cb-51f9127461c0
ms:mtpsurl: https://technet.microsoft.com/library/Dd638122(v=EXCHG.150)
ms:contentKeyID: 49319908
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Transport decryption

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, Microsoft Outlook 2010 and later, and Microsoft Office Outlook Web App, users can use Information Rights Management (IRM) to protect their messages. You can create Outlook protection rules to automatically apply IRM protection to messages before they're sent from an Outlook 2010 client. You can also create transport protection rules to apply IRM protection to messages in transit that match the rule conditions. Transport decryption allows access to IRM-protected messaging content to enforce messaging policies.

For management tasks related to managing IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## Limitations of other encryption solutions

If it's critical that your organization protects sensitive information, including high business impact (HBI) information and personally identifiable information (PII), consider encrypting e-mail messages and attachments. E-mail encryption solutions such as S/MIME have been available for a long time. These encryption solutions have seen varying degrees of adoption in organizations of different types. However, such solutions present the following challenges:

  - **Inability to apply messaging policies**: Organizations also face compliance requirements that require inspection of messaging content to make sure it adheres to messaging policies. However, messages encrypted with most client-based encryption solutions, including S/MIME, prevent content inspection on the server. Without content inspection, an organization can't validate that all messages sent or received by its users comply with messaging policies. For example, to comply with a legal regulation, you've configured a transport rule to detect PII, such as a social security number, and automatically apply a disclaimer to the message. If the message is encrypted, the Transport Rules agent on the Transport service can't access message content, and therefore won't apply the disclaimer. This results in a violation of the policy.

  - **Decreased security**: Antivirus software is unable to scan encrypted message content, further exposing an organization to risk from malicious content such as viruses and worms. Encrypted messages are generally considered to be trusted by most users, thereby increasing the likelihood of a virus spreading throughout your organization. For example, you've configured an Outlook protection rule to automatically apply IRM protection to all messages sent to the All Employees distribution list with the Company Confidential rights management service (RMS) template. A user's workstation is infected with a virus that propagates by automatically using Reply All to reply to messages. If the message carrying the virus is encrypted, the antivirus scanner can't scan the message.

  - **Impact to custom transport agents**: Many organizations develop custom transport agents for different purposes, such as meeting additional processing requirements for compliance, security, or custom message routing. Custom transport agents developed by an organization to inspect or modify messages are unable to process encrypted messages. If the custom transport agents developed by your organization can't access message content, message encryption may prevent your organization from meeting the goals for which the custom transport agents are developed.

## Using transport decryption for encrypted content

In Exchange 2013, IRM features address these challenges. If messages are IRM-protected, transport decryption allows you to decrypt them in transit. IRM-protected messages are decrypted by the Decryption agent, a compliance-focused transport agent.

> [!NOTE]
> In Exchange 2013, the Decryption agent is a built-in agent. Built-in agents aren't included in the list of agents returned by the <STRONG>Get-TransportAgent</STRONG> cmdlet. For more details, see <A href="transport-agents-exchange-2013-help.md">Transport agents</A>.

The Decryption agent decrypts the following types of IRM-protected messages:

  - Messages IRM-protected by the user in Outlook Web App.

  - Messages IRM-protected by the user in Outlook 2010.

  - Messages IRM-protected automatically by Outlook protection rules in Exchange 2013 and Outlook 2010.

> [!IMPORTANT]
> Only messages IRM-protected by the AD&nbsp;RMS server in your organization are decrypted by the Decryption agent.

> [!NOTE]
> Messages protected in-transit using transport protection rules aren't required to be decrypted by the Decryption agent. The Decryption agent fires on the <STRONG>OnEndOfData</STRONG> and <STRONG>OnSubmit</STRONG> transport events. Transport protection rules are applied by the Transport Rules agent, which fires on the <STRONG>OnRoutedMessage</STRONG> event, and IRM-protection is applied by the Encryption agent on the <STRONG>OnRoutedMessage</STRONG> event. For more information about transport agents and a list of SMTP events on which they can be registered, see <A href="transport-agents-exchange-2013-help.md">Transport agents</A>.

Transport decryption is performed on the first Exchange 2013 Transport service that handles a message in an Active Directory forest. If a message is transferred to a Transport service in another Active Directory forest, the message is decrypted again. After decryption, unencrypted content is available to other transport agents on that server. For example, the Transport Rules agent on a Transport service can inspect message content and apply transport rules. Any actions specified in the rule, such as applying a disclaimer or modifying the message in any other way, can be taken on the unencrypted message. Third-party transport agents, such as antivirus scanners, can scan the message for viruses and malware. After other transport agents have inspected the message and possibly made modifications to it, it's encrypted again with the same user rights that it had before being decrypted by the Decryption agent. The same message isn't decrypted again by other the Transport service on other Mailbox servers in the organization.

Messages decrypted by the Decryption agent don't leave the Transport service without being encrypted again. If a transient error is returned when decrypting or encrypting the message, the Transport service retries the operation twice. After the third failure, the error is treated as a permanent error. If any permanent errors occur, including when transient errors are treated as permanent errors after retries, the Transport service treats them as follows:

  - If the permanent error occurs during decryption, a non-delivery report (NDR) is sent only if transport decryption is set to `Mandatory`, and the encrypted message is sent with the NDR. For more details about the configuration options available for transport decryption, see Configuring Transport Decryption later in this topic.

  - If the permanent error occurs during re-encryption, an NDR is always sent without the decrypted message.

> [!IMPORTANT]
> Any custom or third-party agents installed on a Transport service have access to the decrypted message. You must consider the behavior of such transport agents. We recommend that you test all custom and third-party transport agents thoroughly before you deploy them in a production environment.<BR>After a message is decrypted by the Decryption agent, if a transport agent creates a new message and embeds (attaches) the original message to the new one, only the new message is protected. The original message, which becomes an attachment to the new message, doesn't get re-encrypted. A recipient receiving such a message can open the attached message and take actions such as forwarding or replying, which would bypass rights enforcement.

## Configuring transport decryption

Transport decryption is configured by using the [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration) cmdlet in the Exchange Management Shell. However, before you configure transport decryption, you must provide Exchange 2013 servers the right to decrypt content protected by your AD RMS server. This is done by adding the Federation mailbox to the super users group configured on the AD RMS cluster in your organization.

> [!IMPORTANT]
> In cross-forest AD&nbsp;RMS deployments where you have an AD&nbsp;RMS cluster deployed in each forest, you must add the Federation mailbox to the super users group on the AD&nbsp;RMS cluster in each forest to allow the Transport service on an Exchange 2013 Mailbox server or an Exchange 2010 Hub Transport server to decrypt the messages protected against each AD&nbsp;RMS cluster.

For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

Exchange 2013 allows two different settings when enabling transport decryption:

  - **Mandatory**: When transport decryption is set to `Mandatory`, the Decryption agent rejects the message and returns an NDR to the sender if a permanent error is returned when decrypting a message. If your organization doesn't want a message to be delivered if it can't be successfully decrypted and actions such as antivirus scanning and transport rules are applied, you must choose this setting.

  - **Optional**: When transport decryption is set to Optional, the Decryption agent uses a best-effort approach. Messages that can be decrypted are decrypted, but messages with a permanent error on decryption are also delivered. If your organization prioritizes message delivery over messaging policy, you must use this setting.

For more information about configuring transport decryption, see [Enable or Disable Transport Decryption](enable-or-disable-transport-decryption-exchange-2013-help.md).
