---
localization_priority: Normal
description: Email has become a reliable and ubiquitous communication medium for information workers in organizations of all sizes. Messaging stores and mailboxes have become repositories of valuable data. It's important for organizations to formulate messaging policies that dictate the fair use of their messaging systems, provide user guidelines for how to act on the policies, and where required, provide details about the types of communication that may not be allowed.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 510524c6-6d3e-416f-9e27-fa25446b099a
ms.reviewer: 
f1.keywords:
- NOCSH
title: Security and compliance for Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Security and compliance for Exchange Online

Email has become a reliable and ubiquitous communication medium for information workers in organizations of all sizes. Messaging stores and mailboxes have become repositories of valuable data. It's important for organizations to formulate messaging policies that dictate the fair use of their messaging systems, provide user guidelines for how to act on the policies, and where required, provide details about the types of communication that may not be allowed.

Organizations must also create policies to manage email lifecycle, retain messages for the length of time based on business, legal, and regulatory requirements, preserve email records for litigation and investigation purposes, and be prepared to search and provide the required email records to fulfill eDiscovery requests.

Leakage of sensitive information such as intellectual property, trade secrets, business plans, and personally identifiable information (PII) collected or handled by your organization must also be protected.

## Security and compliance in Exchange Online

The following table provides an overview of the security and compliance features in Exchange Online and includes links to topics that will help you learn about and manage these features.

|**Feature**|**Description**|
|:-----|:-----|
|[Archive mailboxes in Exchange Online](https://docs.microsoft.com/microsoft-365/compliance/enable-archive-mailboxes)|Archive mailboxes (called In-Place Archiving) let people in your Microsoft 365 or Office 365 organization take control of messaging data by providing additional email storage. People can use Outlook or Outlook on the web (formerly known as Outlook Web App) to view messages in their archive mailbox and move or copy messages between their primary and archive mailboxes.|
|[In-Place Hold and Litigation Hold](in-place-and-litigation-holds.md)|In-Place Hold and Litigation Hold allow you to preserve or archive mailbox content for compliance and eDiscovery.|
|[In-Place eDiscovery](in-place-ediscovery/in-place-ediscovery.md)|In-Place eDiscovery allows authorized compliance officers in your organization to search mailbox data across your Exchange organization, preview search results, copy them to a Discovery mailbox or export them to a .pst file.|
|[Inactive mailboxes in Exchange Online](https://docs.microsoft.com/microsoft-365/compliance/inactive-mailboxes-in-office-365)|You can preserve the contents of deleted mailboxes indefinitely by using inactive mailboxes. You can make an inactive mailbox by placing an In-Place Hold or a Litigation Hold on the mailbox, and then deleting the corresponding user account. In addition to preserving mailbox contents, administrators or compliance officers can use In-Place eDiscovery in Exchange Online or Content Search in the Microsoft 365 Compliance Center to search the contents of an inactive mailbox.|
|[Data loss prevention](data-loss-prevention/data-loss-prevention.md) (DLP)|Data loss prevention (DLP) helps you identify and monitor sensitive information, such as private identification numbers, credit card numbers, or standard forms used in your organization. You can set up DLP policies to notify users that they are sending sensitive information or block the transmission of sensitive information.|
|[Exchange auditing reports](exchange-auditing-reports/exchange-auditing-reports.md)|You can use the auditing functionality in Exchange Online to track changes made to your Exchange Online configuration by Microsoft and by your organization's administrators, and to audit mailbox access by persons other than the mailbox owner. In Exchange Online, audited actions are recorded and available to view in an online report or export to a file.|
|[Messaging records management](messaging-records-management/messaging-records-management.md) (MRM)|Messaging records management (MRM) helps your organization manage email lifecycle to meet business and regulatory requirements and reduce the legal risks associated with email. In Exchange Online, you can use In-Place Hold or Litigation Hold to preserve email and [Retention tags and retention policies](messaging-records-management/retention-tags-and-policies.md) to archive and delete email.|
|[Information Rights Management in Exchange Online](https://docs.microsoft.com/microsoft-365/compliance/information-rights-management-in-exchange-online)| Information Rights Management (IRM) helps you and your users control who can access, forward, print, or copy sensitive data within an email. IRM can use your on-premises Active Directory Rights Management Services (AD RMS) server.|
|[Office 365 Message Encryption](https://docs.microsoft.com/microsoft-365/compliance/ome-faq)| Office 365 Message Encryption allow you to send encrypted messages to people inside or outside your organization, regardless of the destination email service, whether it's Outlook.com, Yahoo, Gmail, or another service. Designated recipients can send encrypted replies. Office 365 Message Encryption combines email encryption and rights management capabilities. Rights management capabilities are powered by Azure Information Protection.|
|[S/MIME for Message Signing and Encryption](https://docs.microsoft.com/microsoft-365/security/office-365-security/s-mime-for-message-signing-and-encryption)|Secure/Multipurpose Internet Mail Extensions (S/MIME) allows email users to help protect sensitive information by sending signed and encrypted email within their organization. As an administrator, you can enable S/MIME-based security for your organization if you have mailboxes in either Exchange Server or Exchange Online.|
|[Journaling in Exchange Online](journaling/journaling.md)|Journaling can help you meet legal, regulatory, and organizational compliance requirements by recording inbound and outbound email communications. In Exchange Online, you can create journal rules to deliver journal reports to your on-premises mailbox or archiving system, or to an external archiving service.|
|[Mail flow rules (transport rules) in Exchange Online](mail-flow-rules/mail-flow-rules.md)|You can use mail flow rules (also known as transport rules) to inspect messages sent or received by your users and take actions such as blocking or bouncing a message, holding it for review by a manager or an administrator or delivering a copy to another recipient if the message matches specified conditions.|
