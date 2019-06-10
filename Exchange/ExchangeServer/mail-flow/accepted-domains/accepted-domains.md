---
localization_priority: Normal
description: 'Summary: Learn about the three types of accepted domains in Exchange Server 2016 or Exchange Server 2019: authoritative, internal relay, and external relay.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: c1839a5b-49f9-4c53-b247-f4e5d78efc45
ms.date:
ms.reviewer: 
title: Accepted domains in Exchange, Exchange acceepted domains, exchange authoritative domain, Internal relay domain, Exchange external relay, Exchange external relay domain
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Accepted domains in Exchange Server

*Accepted domains* are the SMTP name spaces (also known as address spaces) that you configure in an Exchange organization to receive email messages. For example, if your company registered the domain contoso.com, and you configured a mail exchanger (MX) record in your Internet DNS for contoso.com, you need to configure contoso.com as an accepted domain in your Exchange organization to accept messages that are addressed to @contoso.com recipients.

Accepted domains in Exchange 2016 and Exchange 2019 are basically unchanged from Exchange Server 2010, and consist of the following types:

- **Authoritative domains**: Recipients (in particular, mailboxes) are configured with email addresses in these domains. The Exchange organization accepts messages that are addressed to recipients in these domains, and is responsible for generating non-delivery reports (also known as NDRs or bounce messages) for non-existent recipients.

- **Relay domains**: The Exchange organization accepts messages that are addressed to recipients in relay domains, but isn't responsible for generating NDRs for non-existent recipients. Instead, Exchange (with additional configuration) relays the messages to messaging servers that are external to the Exchange organization. Relay domains can be **internal** (for domains that you control) or **external** (for domains that you don't control).

An accepted domain can be a single domain (contoso.com) or a domain with subdomains (\*.contoso.com). Accepted domains are a global setting for the Exchange organization, and you can have multiple accepted domains of the same or different types.

To configure accepted domains, see [Procedures for accepted domains in Exchange Server](accepted-domain-procedures.md).

> [!NOTE]
> If you have a subscribed Edge Transport server in your perimeter network, you configure accepted domains on a Mailbox server in your Exchange organization. The accepted domains configuration is replicated to the Edge Transport server during EdgeSync synchronization. For more information, see [Edge Subscriptions](../../architecture/edge-transport-servers/edge-subscriptions.md).

## Authoritative domains
<a name="BKMK_AuthoritativeDomains"> </a>

You configure an accepted domain as an authoritative domain when all recipients in that domain exist in your Exchange organization.

By default, when you install the first Exchange Mailbox server, the fully qualified domain name (FQDN) of your forest root domain in Active Directory is configured as an authoritative domain. If you don't want to use this domain for email, you need to add another authoritative domain. For instructions, see [Create accepted domains](accepted-domain-procedures.md#CreateAcceptedDomain).

An organization can be configured with multiple authoritative domains. The set of email domains for an organization are the authoritative domains. You can use authoritative domains in email address policies, and Exchange is responsible for generating NDRs for non-existent recipients in authoritative domains.

## Relay domains
<a name="BKMK_RelayDomains"> </a>

You configure an accepted domain as a relay domain (also known as non-authoritative domain) when some or none of the recipients in that domain exist in your Exchange organization (for example, partners or subsidiaries). Exchange isn't responsible for generating NDRs for non-existent recipients in a relay domain. Instead, you configure a Send connector with the address space of the relay domain, and you configure this Send connector to use smart host routing to relay messages to their destination (directly or to the next hop). For more information about creating Send connectors that use smart host routing, see [Create a Send connector to route outbound mail through a smart host](../../mail-flow/connectors/outbound-smart-host-routing.md).

You configure a relay domain as an internal relay domain or as an external relay domain. The differences are described in the following list:

- **Internal relay domains**

  - Some of the recipients in the internal relay domain don't exist in the Exchange organization. For example:

    - You share the domain between the Exchange organization and a third-party messaging system.

    - You share the domain between Exchange organizations in different Active Directory forests.

  - Recipients in the internal relay domain can be represented as mail contacts or mail users in the Exchange organization (manually created or automatically created by using directory synchronization).

  - The Send connector that you configure for the internal relay domain is sourced on an internal Mailbox server.

    **Note**: By default, you can't configure a Send connector for an internal relay domain on a subscribed Edge Transport server. Messages sent to recipients in the internal relay domain are automatically forwarded to internal Mailbox servers in the subscribed Active Directory site by using the default "EdgeSync - Inbound to *\<Active Directory site name\>*" Send connector. This Send connector is automatically configured to route mail for all authoritative domains and internal relay domains (the address space value is `--`). For more information, see [Send connectors created automatically by the Edge Subscription](../../architecture/edge-transport-servers/edge-subscriptions.md#send-connectors-created-automatically-by-the-edge-subscription).

  - You can use internal relay domains in email address policies.

- **External relay domains**

  - None of the recipients in the external relay domain exist in the Exchange organization (including mail contacts or mail users). For example, your Exchange organization is the central location for accepting Internet email for a group of separate organizations.

  - The Send connector that you configure for the external relay domain is sourced on an Edge Transport server or Internet-facing Mailbox server.

  - You can't use external relay domains in email address policies.

## Accepted domains and email address policies
<a name="BKMK_ADaEPolicies"> </a>

Email address policies assign email addresses to recipients. You need to add an authoritative domain or an internal relay domain before you can use that domain in an email address policy. For more information about email address policies, see [Email address policies in Exchange Server](../../email-addresses-and-address-books/email-address-policies/email-address-policies.md).

## Recipient Lookup in accepted domains
<a name="RecipientLookup"> </a>

Recipient filtering on a subscribed Edge Transport server can block messages that are addressed to non-existent recipients in your Exchange organization. This feature is known as *Recipient Lookup*. For more information about recipient filtering, see [Recipient filtering on Edge Transport servers](../../antispam-and-antimalware/antispam-protection/recipient-filtering.md).

You can enable or disable Recipient Lookup for an accepted domain by using the _AddressBookEnabled_ parameter on the **Set-AcceptedDomain** cmdlet. The default value for each accepted domain type is described in the following table:

****

|**Accepted domain type**|**Default Recipient Lookup (_AddressBookEnabled_ parameter) value**|**Comments**|
|:-----|:-----|:-----|
|Authoritative domain|Enabled (`$true`)|All recipients in an authoritative domain exist in the Exchange organization, so Recipient Lookup for the domain is enabled by default.|
|Internal relay domain|Disabled (`$false`)|If all recipients in the internal relay domain exist in the Exchange organization (including mail contacts and mail users), you can enable Recipient Lookup for the domain. <br/> If some or none of the recipients in the internal relay domain exist in the Exchange organization, you shouldn't enable Recipient Lookup for the domain.|
|External relay domain|Disabled (`$false`)|No recipients in the authoritative domain exist in the Exchange organization, so you shouldn't enable Recipient Lookup for the domain.|

For configuration instructions, see [Modify accepted domains](accepted-domain-procedures.md#ModifyAcceptedDomain).

## Default domain
<a name="DefaultDomain"> </a>

Because the forest root FQDN is automatically configured as the first accepted domain in your organization, that accepted domain is also configured as the *default domain*. However, after you add additional accepted domains, you can configure one of them as the default domain. Here's some information about the default domain:

- You can't delete the default domain. You need to configure another accepted domain as the default domain (one accepted domain is always configured as the default domain).

- The default domain is used in the external postmaster address: `postmaster@<default domain>`.

- The default domain is used in encapsulated non-SMTP email addresses (Internet Mail Connector Encapsulated Address or IMCEA encapsulation).

- The first default domain is used as the primary address for all recipients in the default email address policy. If you configure another accepted domain as the default domain, the default email address policy isn't automatically updated.

- Although you can configure any accepted domain as the default domain, you typically specify an authoritative domain.
