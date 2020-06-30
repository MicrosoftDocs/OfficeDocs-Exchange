---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: ccb62ce1-a822-4859-a0db-3d546c5c1f50
ms.reviewer: 
ms.collection: 
- exchange-online
- M365-email-calendar
f1.keywords:
- NOCSH
description: Admins can learn about the mail flow troubleshooting options in Microsoft 365 or Office 365.
audience: ITPro
title: Troubleshoot mail flow

---

# Troubleshoot mail flow

Can't send or receive email? Microsoft 365 and Office 365 for business offer admins several ways to troubleshoot. We recommend using the automated solutions because they are typically easier and faster than manual troubleshooting.

For instructions about troubleshooting options, see [Find and fix email delivery issues as a Microsoft 365 or Office 365 for business admin](https://docs.microsoft.com/exchange/troubleshoot/mail-delivery/email-delivery-issues).

## Troubleshoot mail flow caused by connectors

To validate and troubleshoot mail flow from Microsoft 365 or Office 365 to the email servers in your on-premises organization (also called the on-premises server), validate your connectors. You can set up and validate connectors on the **Connectors** page in the Exchange admin center (EAC). The built-in validation tests that your mail flow from Microsoft 365 or Office 365 reaches:

- Your organization's email server

- A partner organization.

For more information, see [Validate connectors](use-connectors-to-configure-mail-flow/validate-connectors.md).

## Troubleshoot mail flow issues caused by incorrect SPF records or MX records

 [How Microsoft 365 uses Sender Policy Framework (SPF) to prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/how-office-365-uses-spf-to-prevent-spoofing#troubleshooting-best-practices-for-spf-in-office-365) gives tips on how to fix several SPF record errors. The beginning of that article also provides an explanation of what SPF records are and how Microsoft 365 and Office 365 use them to prevent spoofing.

Mail flow issues can also happen when your MX record is not setup correctly. To verify your MX record, see [Find and fix issues after adding your domain or DNS records](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/find-and-fix-issues).

## For more information

[Mail flow best practices for Exchange Online and Microsoft 365 or Office 365 (overview)](mail-flow-best-practices.md)

[Mail flow in EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/mail-flow-in-eop)
