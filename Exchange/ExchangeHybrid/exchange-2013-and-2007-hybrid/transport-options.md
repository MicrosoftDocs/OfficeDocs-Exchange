---
title: "Transport options in Exchange 2013/Exchange 2007 hybrid deployments"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 92d9e3ca-8d79-4872-9ff7-0067fcdbd434
ms.reviewer: 
description: "In hybrid deployments, you can have mailboxes that reside in your on-premises Exchange organization and also in an Exchange Online organization. A critical component of making these two separate organizations appear as one combined organization to users and messages exchanged between them is hybrid transport. With hybrid transport, messages sent between recipients in either organization are authenticated, encrypted, and transferred using Transport Layer Security (TLS), and appear as internal to Exchange components such as transport rules, journaling, and anti-spam policies. Hybrid transport is automatically configured by the Hybrid Configuration wizard in Exchange 2013"
---

# Transport options in Exchange 2013/Exchange 2007 hybrid deployments

In hybrid deployments, you can have mailboxes that reside in your on-premises Exchange organization and also in an Exchange Online organization. A critical component of making these two separate organizations appear as one combined organization to users and messages exchanged between them is hybrid transport. With hybrid transport, messages sent between recipients in either organization are authenticated, encrypted, and transferred using Transport Layer Security (TLS), and appear as "internal" to Exchange components such as transport rules, journaling, and anti-spam policies. Hybrid transport is automatically configured by the Hybrid Configuration wizard in Exchange 2013

For hybrid transport configuration to work with the Hybrid Configuration wizard, the on-premises SMTP endpoint that accepts connections from Microsoft Exchange Online Protection (EOP), which handles transport for the Exchange Online organization, must be an Exchange 2013 Client Access server, an Exchange 2013 Edge Transport server, or an Exchange Server 2010 Service Pack 3 (SP3) Edge Transport server.

> [!IMPORTANT]
> There can be no other SMTP hosts or services between the on-premises Exchange 2013 Client Access servers or Exchange 2013/Exchange 2010 SP3 Edge Transport servers and EOP. Information added to messages that enables hybrid transport features is removed when they pass through a non-Exchange 2013 server, pre-Exchange 2010 SP3 servers, or an SMTP host. If you have any Exchange 2010 SP2 Edge Transport servers deployed in your organization, and you want to use them for hybrid transport, they must be upgraded to Exchange 2010 SP3.

Inbound messages sent to recipients in both organizations from external Internet senders follow a common inbound route. Outbound messages sent from the organizations to external Internet recipients can either follow a common outbound route or can be sent via independent routes.

You'll need to choose how to route inbound and outbound mail when you plan and configure your hybrid deployment. The route taken by inbound and outbound messages sent to and from recipients in the on-premises and Exchange Online organizations depends on the following:

- Do you want to route inbound Internet mail for both your on-premises and Exchange Online mailboxes through Microsoft 365 or Office 365 and EOP or through your on-premises organization?

    You can choose to route inbound Internet mail for both organizations through your on-premises organization or through EOP and the Exchange Online organization. The route that inbound messages for both organizations take depends on whether you enable centralized mail transport in your hybrid deployment.

- Do you want to route outbound mail to external recipients from your Exchange Online organization through your on-premises organization (centralized mail transport), or do you want to route it directly to the Internet?

    Known as centralized mail transport, you can route all mail from mailboxes in the Exchange Online organization through the on-premises organization before they're delivered to the Internet. This approach is helpful in compliance scenarios where all mail to and from the Internet must be processed by on-premises servers. Alternately, you can configure Exchange Online to deliver messages for external recipients directly to the Internet.

    > [!NOTE]
    > Centralized mail transport is only recommended for organizations with specific compliance-related transport needs. Our recommendation for typical Exchange organizations is not to enable centralized mail transport.

- Do you want to deploy an Edge Transport server in your on-premises organization?

    If you don't want to expose your domain-joined internal Exchange 2013 servers directly to the Internet, you can deploy Exchange 2013 Edge Transport servers or Exchange 2010 SP3 Edge Transport servers in your perimeter network. For more information about adding an Edge Transport server to your hybrid deployment, see [Edge Transport servers in Exchange 2013/Exchange 2007 hybrid deployments](edge-transport-serverrs.md).

Regardless of how you route messages to and from the Internet, all messages sent between the on-premises and Exchange Online organizations are sent using secure transport. For more information, see [Trusted communication](#trusted-communication) later in this topic.

To learn more about how these options affect message routing in your organization, see [Transport routing in Exchange 2013/Exchange 2007 hybrid deployments](transport-routing.md).

## Exchange Online Protection in hybrid deployments

EOP is an online service provided by Microsoft that's used by many companies to protect their on-premises organizations from viruses, spam, phishing scams, and policy violations. In Microsoft 365 and Office 365, EOP is used to protect Exchange Online organizations from the same threats. When you sign up for Microsoft 365 or Office 365, an EOP company is automatically created that's tied to your Exchange Online organization.

An EOP company contains several of the mail transport settings that can be configured for your Exchange Online organization. You can specify which SMTP domains must come from specific IP addresses, require a TLS and a Secure Sockets Layer (SSL) certificate, can bypass compliance policies, and more. EOP is the front door to your Exchange Online organization. All messages, regardless of their origin, must pass through EOP before they reach mailboxes in your Exchange Online organization. And, all messages sent from your Exchange Online organization must go through EOP before they reach the Internet.

When you configure a hybrid deployment with the Hybrid Configuration wizard, all transport settings are automatically configured in your on-premises organization and in the EOP company included in your Exchange Online organization. The Hybrid Configuration wizard configures all inbound and outbound connectors and other settings in this EOP company to secure messages sent between the on-premises and Exchange Online organizations and route messages to the right destination. If you want to configure custom transport settings for your Exchange Online organization, you'll configure them in this EOP company also.

## Trusted communication

To help protect recipients in both the on-premises and Exchange Online organizations, and to help ensure that messages sent between the organizations aren't intercepted and read, transport between the on-premises organization and EOP is configured to use forced TLS. TLS transport uses Secure Sockets Layer (SSL) certificates provided by a trusted third-party certificate authority (CA). Messages between EOP and the Exchange Online organization also use TLS.

When using forced TLS transport, the sending and receiving servers examine the certificate configured on the other server. The subject name, or one of the subject alternative names (SANs), configured on the certificates must match the FQDN that an administrator has explicitly specified on the other server. For example, if EOP is configured to accept and secure messages sent from the mail.contoso.com FQDN, the sending on-premises Client Access or Edge Transport server must have an SSL certificate with mail.contoso.com in either the subject name or SAN. If this requirement isn't met, the connection is refused by EOP.

> [!NOTE]
> The FQDN used doesn't need to match the email domain name of the recipients. The only requirement is that the FQDN in the certificate subject name or SAN must match the FQDN that the receiving or sending servers are configured to accept.

In addition to using TLS, messages between the organizations are treated as "internal." This approach allows messages to bypass anti-spam settings and other services.

Learn more about SSL certificates and domain security at [Certificate requirements for hybrid deployments](../certificate-requirements.md) and [Understanding TLS Certificates](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa998840(v=exchg.141)).
