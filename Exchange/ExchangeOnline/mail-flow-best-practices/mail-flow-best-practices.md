---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: 0e6cd9d5-ad3e-418a-8ea9-3bf33332c491
ms.reviewer: 
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
description: Use Microsoft Exchange Online and Microsoft 365 or Office 365 to manage mail flow. Find out how, and get tips and best practices for setting up and managing your email.
audience: ITPro
title: Mail flow best practices for Exchange Online, Microsoft 365, and Office 365 (overview)

---

# Mail flow best practices for Exchange Online, Microsoft 365, and Office 365 (overview)

Use Microsoft Exchange Online and Microsoft 365 or Office 365 to manage mail flow. Find out how, and get tips and best practices for setting up and managing your email.

 **This article is intended for IT Pros. Want something else?**

 **Try [Set up Microsoft 365 for business](https://docs.microsoft.com/microsoft-365/admin/setup/setup) or [Deploy Office 365 Enterprise for your organization](https://docs.microsoft.com/office365/enterprise/setup-overview-for-enterprises)**.

Microsoft 365 and Office 365 give you flexibility in determining the best arrangement for how email is delivered to your organization's mailboxes. The path email takes from the internet to a mailbox and vice versa is called mail flow. Most organizations want Microsoft 365 or Office 365 to manage all their mailboxes and filtering, and some organizations need more complex mail flow setups to make sure that they comply with specific regulatory or business needs. If you're part of a small business or simply an organization that wants Microsoft 365 or Office 365 to manage all your mailboxes and mail flow, we recommend following the steps in [Set up Microsoft 365 for business](https://docs.microsoft.com/microsoft-365/admin/setup/setup). That article provides a complete checklist for setting up Microsoft 365 or Office 365 services and programs, including how to set up your mail flow and email clients.

For information about how your email is protected with EOP, see [Exchange Online Protection Overview](https://docs.microsoft.com/microsoft-365/security/office-365-security/exchange-online-protection-overview).

> [!TIP]
> Are you new to Microsoft 365 or Office 365 mail flow? Check out the [External Domain Name System records for Microsoft 365 or Office 365](https://docs.microsoft.com/office365/enterprise/external-domain-name-system-records) topic. We especially recommend reading the part about SPF records because customers often list the wrong values in their SPF record, which can cause mail flow problems.

Microsoft 365 and Office 365 mail flow covers the following scenarios:

|**Mail flow setup**|**Your organization's scenario**|**Complexity**|
|:-----|:-----|:-----|
|[Manage all mailboxes and mail flow using Microsoft 365 or Office 365](manage-mailboxes-using-microsoft-365-or-office-365.md)| **[Scenario 1](manage-mailboxes-using-microsoft-365-or-office-365.md#hosted-mail-flow-scenarios)** <br/> I'm a new Microsoft 365 or Office 365 customer, and all my users' mailboxes are in Microsoft 365 or Office 365. I want to use all filtering solutions offered by Microsoft 365 and Office 365. <br/> **[Scenario 2](manage-mailboxes-using-microsoft-365-or-office-365.md#hosted-mail-flow-scenarios)** <br/> I'm a new Microsoft 365 or Office 365 customer. I have an existing email service but plan to move all the existing users' mailboxes to the cloud at once. I want to use all filtering solutions offered by Microsoft 365 and Office 365.|Simple|
|[Manage mail flow using a third-party cloud service with Microsoft 365 or Office 365](manage-mail-flow-using-third-party-cloud.md)|**[Scenario 1](manage-mail-flow-using-third-party-cloud.md#scenario-1---mx-record-points-to-third-party-spam-filtering)** <br/> I plan to have Microsoft 365 or Office 365 host all of my organization's mailboxes. My organization uses (or plans to use) a third-party (mail services) cloud solution for filtering spam and malware. All email sent from the internet must be filtered by this third-party cloud service. <br/> **[Scenario 2](manage-mail-flow-using-third-party-cloud.md#scenario-2---mx-record-points-to-third-party-solution-without-spam-filtering)** <br/> I plan to have Microsoft 365 or Office 365 host all my organization's mailboxes. My organization needs to send all email to a third-party service, such as archiving or auditing. However, the third-party service doesn't provide a spam filtering solution.|Complex|
|[Manage mail flow with mailboxes in multiple locations (Microsoft 365 or Office 365 and on-prem)](manage-mail-flow-for-multiple-locations.md) <br/><br/> **Important**: In the near future, Microsoft 365 and Office 365 will reject email from unknown senders that are relayed from on-premises servers. This means that if the sender or recipient domain of a message doesn't belong to your organization, Microsoft 365 or Office 365 will reject the message unless you have created a connector to allow this behavior. This change will help prevent unauthorized parties from using your organization to send spam or malware through Microsoft 365 or Office 365. <br/> This change potentially affects your mail flow if you use any scenario in this section. Each scenario has best practices to ensure that your mail flow continues uninterrupted.|**[Scenario 1](manage-mail-flow-for-multiple-locations.md#scenario-1-mx-record-points-to-microsoft-365-or-office-365-and-microsoft-365-or-office-365-filters-all-messages)** <br/> I'm migrating my mailboxes to Microsoft 365 or Office 365, and I want to keep some mailboxes on my organization's mail server (on-premises server). I want to use Microsoft 365 or Office 365 as my spam filtering solution and would like to send my messages from my on-premises server to the internet via Microsoft 365 or Office 365. Microsoft 365 or Office 365 sends and receives all messages. <br/> **[Scenario 2](manage-mail-flow-for-multiple-locations.md#scenario-2-mx-record-points-to-microsoft-365-or-office-365-and-mail-is-filtered-on-premises)** <br/> I'm migrating my mailboxes to Microsoft 365 or Office 365, and I want to keep some mailboxes on my organization's mail server (on-premises server). I want to use the filtering and compliance solutions that are already in my on-premises environment. And all messages coming from the internet to my cloud mailboxes or messages sent to the internet from my cloud mailboxes need to route through my on-premises servers. <br/> **[Scenario 3](manage-mail-flow-for-multiple-locations.md#scenario-3-mx-record-points-to-my-on-premises-servers)** <br/> I'm migrating my mailboxes to Microsoft 365 or Office 365, and I want to keep some mailboxes on my organization's mail server (on-premises server). I want to use the filtering and compliance solutions that are already in my on-premises email environment. All messages coming from the internet to my cloud mailboxes or messages sent to the internet from cloud mailboxes must route through my on-premises servers. And I need to point my domain's MX record to my on-premises server. <br/> **[Scenario 4](manage-mail-flow-for-multiple-locations.md#scenario-4-mx-record-points-to-my-on-premises-server-which-filters-and-provides-compliance-solutions-for-your-messages-your-on-premises-server-needs-to-relay-messages-to-the-internet-through-microsoft-365-or-office-365)** <br/> I'm migrating my mailboxes to Microsoft 365 or Office 365, and I want to keep some mailboxes on my organization's mail server (on-premises server). I want to use the filtering and compliance solutions that are already in my on-premises email environment. All messages sent from my on-premises servers must relay through Microsoft 365 or Office 365 to the internet. And I need to point my domain's MX record to my on-premises server.|Very complex|
|[Manage mail flow using a third-party cloud service with mailboxes on Microsoft 365 or Office 365 and on-prem](manage-mail-flow-on-office-365-and-on-prem.md)|**Scenario** <br/> I'm migrating my mailboxes to Microsoft 365 or Office 365, and I want to keep some mailboxes on my organization's mail server (on-premises server). I want to use a third-party cloud service to filter spam from the internet. My messages to the internet need to route through Microsoft 365 or Office 365 to protect my on-premises servers' IP addresses from being added to external block lists.|Most complex|
|Send emails from a multifunction printer/scanner/fax/application through Microsoft 365 or Office 365 <br/> For details about this scenario, see [How to set up a multifunction device or application to send email using Microsoft 365 or Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md).|**Scenario** <br/> All my organization's mailboxes are hosted in Microsoft 365 or Office 365, but I have a multifunction printer, scanner, fax machine, or an application that needs to send email.|Complex|

|Using Exchange Online Protection (EOP) standalone <br/> For details about this scenario, see [Mail Flow in EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/mail-flow-in-eop) and [How connectors work with my on-premises email servers](use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow.md#how-connectors-work-with-my-on-premises-email-servers)|**Scenario** <br/> I have my own email servers (on-premises servers), and I subscribe to EOP for email protection services only.|Simple|

For information about migrating your email to Microsoft Exchange Online, see [Ways to migrate multiple email accounts to Microsoft 365 or Office 365](../mailbox-migration/mailbox-migration.md).

## Introduction to the basics of Microsoft 365 and Office 365 mail flow

Microsoft 365 and Office 365 use domains, like contoso.com, to route email messages. When you set up email in Microsoft 365 or Office 365, you typically switch from the default domain that you got when you first signed up for Microsoft 365 or Office 365 (the domain ending with .onmicrosoft.com) to your organization's domain. Domain names, like contoso.com, are managed by using a worldwide system of domain registrars (for example, GoDaddy, HostGator, or Moniker) and databases called the Domain Name System (DNS). DNS provides a mapping between human-readable computer hostnames and the IP addresses used by networking equipment. If you're new to DNS, we recommend that you read [DNS basics](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/dns-basics). The following video provides you with a quick overview of some of the most important concepts about what DNS is and how it works.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/c005f2a4-90ad-46fe-b1ab-90f41f2a9d53?autoplay=false]

### Understanding how DNS records control mail flow

In Microsoft 365 and Office 365 mail flow, there are several components of DNS that are particularly important for email authentication and delivery: MX records, SPF, DKIM, and DMARC.

 **MX (mail exchanger) records** provide an easy way for mail servers to know where to send email. You can think of the MX record as a type of postal address. If you want Microsoft 365 or Office 365 to receive all email addressed to anyone@contoso.com, the MX record for contoso.com should point to Microsoft 365 or Office 365, and it will look like the following example:

```
Hostname: contoso-com.mail.protection.outlook.com
Priority: 0
TTL: 1 hour
```

 **SPF (sender policy framework)** is a specially formatted TXT record in DNS. SPF validates that only the organization that owns a domain is actually sending email from that domain. SPF is a security measure that helps makes sure someone doesn't impersonate another organization. This impersonation is often called spoofing. As a domain owner, you can use SPF to publish a list of IP addresses or subnets that are authorized to send email on your organization's behalf. This can be helpful if you want to send email from multiple servers or services with different IP addresses.

> [!IMPORTANT]
> You can only have one SPF record per domain. Having multiple SPF records will invalidate all SPF records and cause mail flow problems.

Because most modern email servers look up a domain's SPF record before they accept any email from it, it's important to set up a valid SPF record in DNS when you first set up mail flow. For a quick introduction to SPF and to get it configured quickly, see [Set up SPF in Microsoft 365 or Office 365 to help prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-spf-in-office-365-to-help-prevent-spoofing). For a more in-depth understanding of how Microsoft 365 and Office 365 use SPF, or for troubleshooting or non-standard deployments such as hybrid deployments, start with [How Microsoft 365 and Office 365 use Sender Policy Framework (SPF) to prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/how-office-365-uses-spf-to-prevent-spoofing).

**DomainKeys Identified Mail (DKIM).** lets you attach a digital signature to email messages in the message header of emails you send. Email systems that receive email from your domain use this digital signature to determine if incoming email that they receive is legitimate. For information about DKIM and Microsoft 365 or Office 365, see [Use DKIM to validate outbound email sent from your domain in Microsoft 365 or Office 365](https://docs.microsoft.com/microsoft-365/security/office-365-security/use-dkim-to-validate-outbound-email).

**Domain-based Message Authentication, Reporting, and Conformance (DMARC).** helps receiving mail systems determine what to do with messages that fail SPF or DKIM checks and provides another level of trust for your email partners. For information on setting up DMARC, see [Use DMARC to validate email in Microsoft 365 or Office 365](https://docs.microsoft.com/microsoft-365/security/office-365-security/use-dmarc-to-validate-email).

Use SPF, DKIM, and DMARC together for the best experience.

### How MX records affect spam filtering

For the best mail flow experience (especially for spam filtering) we recommend pointing the MX record for your organization's domain to Microsoft 365 or Office 365. Spam scanning is the initial connection point to the Microsoft 365 or Office 365 service. Who is sending the message, the IP address of the server that originally sent the message, and the behavior of the connecting mail server, all help determine whether a message is legitimate or spam. If your domain's MX record doesn't point to Microsoft 365 or Office 365, the spam filters won't be as effective. If your MX record doesn't point to Microsoft 365 or Office 365, there will be some valid messages that the service misclassifies as spam and some spam messages that the service misclassifies as legitimate email.

With that said, there are legitimate business scenarios that require your domain's MX record to point to somewhere other than Microsoft 365 or Office 365. For example, email destined for your organization might need to initially arrive at another destination (such as a third-party archiving solution), then route through Microsoft 365 or Office 365, and then be delivered to mailboxes on your organization's mail server. This setup might provide the best solution to meet your business requirements.

Whatever your needs, this guide will help you understand how your MX records, SPF, and, potentially, connectors need to be set up.

## For more information

The following are additional topics related to mail flow in Exchange Online:

[Test mail flow by validating your Microsoft 365 or Office 365 connectors](test-mail-flow.md)

[Troubleshoot Microsoft 365 or Office 365 mail flow](troubleshoot-mail-flow.md)

[Use Directory Based Edge Blocking to reject messages sent to invalid recipients](use-directory-based-edge-blocking.md)

[Manage accepted domains in Exchange Online](manage-accepted-domains/manage-accepted-domains.md)

[Remote domains in Exchange Online](remote-domains/remote-domains.md)

[Message format and transmission in Exchange Online](message-format-and-transmission.md)

[Configure the external postmaster address in Exchange Online](configure-external-postmaster-address.md)

[How to set up a multifunction device or application to send email using Microsoft 365 or Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md)
