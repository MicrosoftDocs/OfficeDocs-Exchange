---
title: Time to Upgrade from Exchange 2003
TOCTitle: Time to Upgrade from Exchange 2003
ms:assetid: aabe635d-6dba-431c-a9ba-5aa2a4fd7a2d
ms:mtpsurl: https://technet.microsoft.com/library/Dn201758(v=EXCHG.150)
ms:contentKeyID: 53892776
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Time to Upgrade from Exchange 2003

As many of you may already know, support for Microsoft Exchange Server 2003 will soon be coming to an end. Each product that Microsoft releases has a lifecycle that determines how long we maintain and support the product. Exchange 2003 mainstream support is already over. And, **Exchange Server 2003 extended support ends on April 8, 2014**.

April 2014 will be here before you know it, so now is the time to start planning your upgrade. Back in 2011, we gave you a heads-up about Exchange 2003 support coming to an end in [Time to Move from Exchange 2003](https://docs.microsoft.com/previous-versions/office/exchange-server-2010-technical-article/hh145599(v=exchg.141)). Since then, we've released Exchange Server 2013 and released more new features in Microsoft 365 and Office 365.

To help you plan, this article discusses the recommended steps to either upgrade to Exchange 2013 or move to Microsoft 365 or Office 365. We also include links to the appropriate documentation that provides detailed information about each stage in the process.

> [!NOTE]
> For more information about specific versions of Exchange 2003, see the <A href="https://support.microsoft.com/lifecycle/search/">Microsoft Support Lifecycle Search</A> page, and enter "Exchange Server 2003" in the <STRONG>Product Name</STRONG> field.

## What's new in Exchange 2013

Before we get into the planning steps for your upgrade to Exchange 2013, let's first talk about why you want to upgrade to Exchange 2013. We've made multiple improvements to Exchange 2013, including, but not limited to the following:

- With Exchange 2013, we reduced the number of server roles to two: the Client Access server role and the Mailbox server role. The primary design goal for Exchange 2013 was for simplicity of scale, hardware utilization, and failure isolation.

- *Data loss prevention* (DLP) is a new feature in Exchange 2013. DLP capabilities help you protect your sensitive data and inform users of internal compliance policies.

- *Smart Search* learns from users' communication and collaboration behavior to enhance and prioritize search results in Exchange.

- Outlook Web App emphasizes a streamlined user interface that also supports the use of touch, enhancing the mobile device experience with Exchange.

- The built-in malware filtering capabilities of Exchange 2013 helps protect your network from malicious software transferred through email messages. All messages sent or received by your Exchange server are scanned for malware (viruses and spyware).

- With Exchange 2013, users can merge contacts from multiple sources to provide a single view of a person, by linking contact information pulled from multiple locations.

- Exchange 2013 offers greater integration with Microsoft SharePoint 2013 and Microsoft Lync 2013 through site mailboxes and In-Place eDiscovery. Together, these products offer a suite of features that make scenarios such as enterprise eDiscovery and collaboration using site mailboxes possible.

- Exchange 2013 helps you to find and search data not only in Exchange, but across your organization. With improved search and indexing, you can search across Exchange 2013, Lync 2013, SharePoint 2013, and Windows file servers.

- In Exchange 2013, the *Managed Store* is the name of the newly rewritten Information Store processes, Microsoft.Exchange.Store.Service.exe and Microsoft.Exchange.Store.Worker.exe. The new Managed Store is written in C\# and tightly integrated with the Microsoft Exchange Replication service (MSExchangeRepl.exe) to provide higher availability through improved resiliency.

- Exchange 2013 reduces the number of certificates that an Administrator must manage, minimizes the interaction the Administrator must have with certificates, and allows management of certificates from a central location.

- Exchange 2013 readiness checks make sure that your computer and your organization are ready for Exchange 2013 deployment.

- Public folders now take advantage of the existing high availability and storage technologies of the mailbox store. The public folder architecture uses specially designed mailboxes to store both the hierarchy and the public folder content. This new design also means that there is no longer a public folder database.

- Exchange 2013 introduces the concept of batch moves, which allows the ability to move multiple mailboxes in large batches. The new move architecture is built on top of MRS (Mailbox Replication service) moves with enhanced management capability.

These improvements have pleased our customers because they've helped to make managing an Exchange organization easier than ever. For more information about the new features in Exchange 2013, see [What's New in Exchange 2013](https://docs.microsoft.com/exchange/what-s-new-in-exchange-2013-exchange-2013-help).

## Planning your upgrade

Now that we've talked about the new features in Exchange 2013, it's time to start the planning. To design a successful Exchange 2013 messaging system, you must first understand the capabilities of the software and hardware. Specifically, you need to balance the limitations of your network infrastructure with the capabilities of your messaging system, operating system, and user software.

For detailed information about how to develop a strong planning strategy, see:

- [Planning and deployment](https://docs.microsoft.com/exchange/planning-and-deployment-for-exchange-2013-installation-instructions)

- [Active Directory](https://docs.microsoft.com/Exchange/active-directory-exchange-2013-help)

- [Exchange 2013 Storage Configuration Options](https://docs.microsoft.com/exchange/exchange-2013-storage-configuration-options-exchange-2013-help)

Before you can upgrade to Exchange 2013, your organization must meet certain requirements. Don't worry, we've automated the installation of some of the Windows prerequisites, so all you have to do is select a check box in the Setup wizard and it will be completed for you. For more information about Exchange 2013 requirements, see the following resources:

- [Exchange 2013 System Requirements](https://docs.microsoft.com/exchange/exchange-2013-system-requirements-exchange-2013-help)

- [Exchange 2013 Prerequisites](https://docs.microsoft.com/exchange/exchange-2013-prerequisites-exchange-2013-help)

## Upgrading from Exchange 2003 to Exchange 2013

You've got two options to consider when upgrading your Exchange 2003 organization from Exchange 2003 to Exchange 2013:

- Move your Exchange organization to the cloud with Microsoft 365 or Office 365, or

- Upgrade your current on-premises organization.

To help simplify your deployment, whether you're moving your organization to the cloud or upgrading your on-premises organization, we created the Exchange Server Deployment Assistants (ExDeploy) to help guide you through the upgrade process. For more information about the ExDeploy tools, see [Microsoft Exchange Server Deployment Assistant](https://assistants.microsoft.com/).

## Go to the cloud with Office 365

Have you heard of Microsoft 365 or Office 365 yet? It delivers the power of cloud productivity to businesses of all sizes, helping to save time and money and free up valued resources. It combines the familiar Office desktop suite with online versions of Microsoft next-generation communications and collaboration services: Exchange Online, SharePoint Portal Server Online, and Lync Online. With Microsoft 365 and Office 365, we provide services that are easy to administer and simple to use, always backed by robust security, reliability, and control to run your business.

To use Microsoft 365 or Office 365, you'll need to upgrade your organization to Exchange 2013. You can use Exchange 2013 ExDeploy to migrate from Exchange 2003 to the cloud. To learn more about the ExDeploy tool, see [Exchange Server 2013 Deployment Assistant](https://docs.microsoft.com/exchange/exchange-server-deployment-assistant-exchange-2013-help). To learn more, see [Powerful tools to support your enterprise](https://www.microsoft.com/microsoft-365/business/compare-more-office-365-for-business-plans).

## Upgrade to Exchange 2013 on-premises

To upgrade to Exchange 2013, you can take either of the following approaches:

- Upgrade to Exchange 2007 and then upgrade to Exchange 2013, -or-

- Upgrade to Exchange 2010, and then upgrade to Exchange 2013.

For this two-phase upgrade, you'll need to use both Exchange 2010 ExDeploy and Exchange 2013 ExDeploy. First, use Exchange 2010 ExDeploy to upgrade from Exchange 2003 to either Exchange 2007 or Exchange 2010. Then, use Exchange 2013 ExDeploy to upgrade from either Exchange 2007 or Exchange 2010 to Exchange 2013.

## Time to Upgrade

Well, we hope this article's been helpful for you as you contemplate life after Exchange 2003 and think about an upgrade to Exchange 2013. After deploying your Exchange 2013 organization, you'll be well on your way to successfully upgrading your Exchange 2003 organization. Then, you'll be ready to secure, administer, and verify mail flow in your Exchange 2013 organization.

![24ee4aef-3dc3-4a8b-a737-953b41ec5ff4](images/Dn201758.24ee4aef-3dc3-4a8b-a737-953b41ec5ff4(EXCHG.150).jpg "24ee4aef-3dc3-4a8b-a737-953b41ec5ff4")  [Kweku Ako-Adjei](https://techcommunity.microsoft.com/t5/exchange-team-blog/bg-p/Exchange), Sr. Technical Writer, Exchange Server
