---
localization_priority: Normal
description: Learn how to use connectors to control mail flow with Exchange Online or Exchange Online Protection.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 854b5a50-4462-4836-a092-37e208d29624
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- ms.exch.eac.ConnectorSelection
title: Configure mail flow using connectors
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure mail flow using connectors

Connectors are a collection of instructions that customize the way your email flows to and from your Microsoft 365 or Office 365 organization. Actually, most Microsoft 365 and Office 365 organizations don't need connectors for regular mail flow. This topic describes the mail flow scenarios that require connectors.

## What do connectors do?

Connectors are used to:

- Enable mail flow between Microsoft 365 or Office 365 and any email server that you have in your on-premises organization (also known as on-premises email servers).

- Apply security restrictions or controls for to email exchanges between your Microsoft 365 or Office 365 organization and a business partner or service provider.

- Enable email notifications from printers, devices, or other non-mailbox entities.

- Avoid graylisting that would otherwise occur because of the large volume of mail that's regularly exchanged between your Microsoft 365 or Office 365 organization and your on-premises email server or partners.

> [!NOTE]
> Graylisting is a delay tactic that's used to protect email systems from spam. In Microsoft 365 and Office 365, graylisting is done by throttling IPs to limit senders from sending suspiciously large amounts of email. Microsoft 365 or Office 365 responds to these abnormal influxes of mail by returning a temporary non-delivery report error (also known as an NDR or bounce message) in the range 451 4.7.500-699 (ASxxx). For more details on these types of delivery issues, see [Fix email delivery issues for error code 451 4.7.500-699 (ASxxx) in Exchange Online](../non-delivery-reports-in-exchange-online/fix-error-code-451-4-7-500-699-asxxx-in-exchange-online.md).

## What happened to inbound and outbound connectors?

Nothing. We just don't call them "inbound" and "outbound" anymore (although the PowerShell cmdlet names still contains these terms). If you previously set up inbound and outbound connectors, they will still function in exactly the same way.

The process for setting up connectors has changed; instead of using the terms "inbound" and "outbound", we ask you to specify the start and end points that you want to use. The way connectors work in the background is the same as before (inbound means into Microsoft 365 or Office 365; outbound means sent from Microsoft 365 or Office 365).

## When do I need a connector?

Exchange Online is ready to send and receive email from the internet right away. You don't need to set up connectors unless you have Exchange Online Protection (EOP) or other specific circumstances that are described in the following table:


|**Scenario**|**Description**|**Connector required?**|**Connector settings**|
|:-----|:-----|:-----|:-----|
|You have a standalone EOP subscription.|You have your own on-premises email servers, and you subscribe to EOP only for email protection services for your on-premises mailboxes (you have no mailboxes in Exchange Online). <br/><br/> For more information, see the topic [Exchange Online Protection overview](https://docs.microsoft.com/office365/securitycompliance/eop/exchange-online-protection-overview) and the section [How connectors work with my on-premises email servers](#how-connectors-work-with-my-on-premises-email-servers) later in this topic.|Yes|**Connector for incoming email:** <br/>• **From**: Your on-premises email server <br/>• **To**: Office 365<br/><br/> **Connector for outgoing email**: <br/>• **From**: Office 365 <br/>• **To**: Your on-premises mail server|
|Some of your mailboxes are on your on-premises email servers, and some are in Exchange Online.|Before you manually configure connectors, check whether an Exchange hybrid deployment better meets your business needs. <br/> For details, see the [I have my own email servers](#i-have-my-own-email-servers) section later in this topic and the [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid) topic.|Yes|**Connector for incoming email:** <br/>• **From**: Your on-premises email server <br/>• **To**: Office 365<br/><br/> **Connector for outgoing email:** <br/>• **From**: Office 365 <br/>• **To**: Your on-premises email server|
|All of your mailboxes are in Exchange Online, but you need to send email from sources in your on-premises organization.|You don't have your own email servers, but you need to send email from non-mailboxes: printers, fax machines, apps, or other devices. <br/><br/> For details, see [Option 3: Configure a connector to send mail using Office 365 SMTP relay](../how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md#option-3-configure-a-connector-to-send-mail-using-microsoft-365-or-office-365-smtp-relay)|Optional|**Only one connector for incoming email:** <br/>• **From**: Your organization's email server <br/>• **To**: Office 365|
|You frequently exchange sensitive information with business partners, and you want to apply security restrictions.|You want to use Transport Layer Security (TLS) to encrypt sensitive information or you want to limit the source (IP addresses) for email from the partner domain.<br/><br/> For details, see [Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md).|Optional|**Connector for incoming email:** <br/>• **From**: Partner organization <br/>• **To**: Office 365<br/> **Connector for outgoing email:** <br/>• **From**: Office 365 <br/>• To: Partner organization|

> [!TIP]
> If you don't have Exchange Online or EOP and are looking for information about Send connectors and Receive connectors in Exchange 2016 or Exchange 2019, see [Connectors](https://docs.microsoft.com/exchange/mail-flow/connectors/connectors).

> [!IMPORTANT]
> You cannot have an "allow" by sender domain connector when there is a restrict by IP/Cert connector. The restrict connector will take precedence, as partner connectors are pulled up by IP or Cert lookup when restriction and mail rejection are applied. You should not have IPs and Cert configured in the same partner connector. Please use separate connectors. Associated Accepted Domains shouldn't be used, unless you are testing the connector for a subset of the accepted domains or recipient domains.

## I have my own email servers

If you have Exchange Online or EOP and your own on-premises email servers, you definitely need connectors. This is more complicated and has more options as described in the following table:

|**You're on-premises email organization is**|**Your service subscription is**|**Have you completed an Exchange hybrid deployment?**|**Do I need to set up connectors manually?**|
|:-----|:-----|:-----|:-----|
|Exchange 2010 or later|Exchange Online Protection|Not available|Yes. Follow the instructions in [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md).|
|Exchange 2010 or later|Exchange Online|No|Consider whether an Exchange hybrid deployment will better meet your organization's needs by reviewing the topic that matches your current situation in [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid). <br/><br/> If a hybrid deployment is the right option for your organization, use the [Hybrid Configuration wizard](https://docs.microsoft.com/exchange/hybrid-configuration-wizard) to integrate Exchange Online with your on-premises Exchange organization. <br/><br/> If you don't want a hybrid deployment and you only want connectors that enable mail routing, follow the instructions in [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md).|
|Exchange 2010 or later|Exchange Online|Yes|No. The Hybrid Configuration wizard creates connectors for you. To view or edit those connectors, go to the **Connectors** page in the Exchange admin center (EAC), or rerun the Hybrid Configuration wizard.|
|Exchange 2007 or earlier|Exchange Online Protection or Exchange Online|Not available|Yes. Follow the instructions in [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md). <br/><br/> In limited circumstances, you might have a hybrid configuration with Exchange Server 2007 and Microsoft 365 or Office 365. Check whether connectors are already set up for your organization by going to the **Connectors** page in the EAC.|
|Non-Microsoft SMTP server|Exchange Online Protection or Exchange Online|Not available|Yes. Follow the instructions in [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md).|

### How connectors work with my on-premises email servers

Connectors enable mail flow in both directions (to and from Microsoft 365 or Office 365). You can enable mail flow with any SMTP server (for example, Microsoft Exchange or a third-party email server).

The diagram below shows how connectors in Exchange Online or EOP work with your own email servers.

![Connectors between Microsoft 365 or Office 365 and your e-mail server](../../media/0df5ec3d-29c1-4add-9e22-5b0c26bec750.png)

In this example, John and Bob are both employees at your company. John has a mailbox on an email server that you manage, and Bob has a mailbox in Exchange Online. John and Bob both exchange mail with Sun, a customer with an internet email account:

- When email is sent between John and Bob, connectors are needed

- When email is sent between John and Sun, connectors are needed. (All internet email is delivered via Microsoft 365 or Office 365).

- When email is sent between Bob and Sun, no connector is needed.

> [!IMPORTANT]
> Always confirm that your internet-facing email servers aren't accidentally configured to allow open relay. An _open relay_ allows mail from any source (spammers) to be transparently re-routed through the open relay server. This behavior masks the original source of the messages, and makes it look like the mail originated from the open relay server.

### What if I've already run the Hybrid Configuration Wizard?

If you've already run the Hybrid Configuration wizard, the required connectors are already configured for you. You can view your hybrid connectors on the **Connectors** page in the EAC. You can view, troubleshoot, and update these connectors using the procedures described in [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md), or you can re-run the Hybrid Configuration wizard to make changes.

## Connectors for mail flow with a partner organization

You can create connectors to add additional security restrictions for email sent between Microsoft 365 or Office 365 and a partner organization. A partner can be an organization you do business with, such as a bank. It can also be a cloud email service provider that provides services such as archiving, antispam, and so on. You can create a partner connector that defines boundaries and restrictions for email sent to or received from your partners, including scoping the connector to receive email from specific IP addresses, or requiring TLS encryption.

### Example use of connectors with a partner organization

The diagram below shows an example where ContosoBank.com is a business partner that you share financial details with via email. Because you are sharing financial information, you want to protect the integrity of the mail flow between your businesses. Connectors with TLS encryption enable a secure and trusted channel for communicating with ContosoBank.com. In this example, two connectors are created in Microsoft 365 or Office 365. TLS is required for mail flow in both directions, so ContosoBank.com must have a valid encryption certificate. A certificate from a commercial certification authority (CA)that's automatically trusted by both parties is recommended.

![Connectors between Microsoft 365 or Office 365 and a partner organization](../../media/0f9319ae-84bb-4b05-a79f-12fb988f1d10.png)

### Additional partner organization connector options: specify a domain or IP address ranges

When you create a connector, you can also specify the domain or IP address ranges that your partner sends mail from. If email messages don't meet the security conditions that you set on the connector, the message will be rejected. For more information about creating connectors to exchange secure email with a partner organization, see [Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md).

## Connectors for mail notifications from printers and devices

This scenario applies only to organizations that have all their mailboxes in Exchange Online (no on-premises email servers) and allows a program or a device, such as a printer, to send email. For example, if you want a printer to send notifications when a print job is ready, or you want your scanner to email documents, you can use this option to send mail through Microsoft 365 or Office 365 (but there are other options that don't require connectors). For details, see [How to set up a multifunction device or application to send email](../how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md).

## How do I set up connectors?

Before you set up a connector, you need to configure the accepted domains for Microsoft 365 or Office 365. For more information, see [Manage accepted domains in Exchange Online](../../mail-flow-best-practices/manage-accepted-domains/manage-accepted-domains.md).

Connector setup topics:

- [Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md)

- [Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md)

## See also

[Set up connectors to route mail between Microsoft 365 or Office 365 and your own email servers](set-up-connectors-to-route-mail.md)

[Mail flow best practices for Exchange Online and Microsoft 365 or Office 365 (overview)](../mail-flow-best-practices.md)

[Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md)

[What happens when I have multiple connectors for the same scenario?](set-up-connectors-to-route-mail.md#what-happens-when-i-have-multiple-connectors-for-the-same-scenario)
