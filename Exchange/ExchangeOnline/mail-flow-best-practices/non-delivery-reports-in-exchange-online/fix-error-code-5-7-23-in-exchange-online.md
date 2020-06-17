---
title: "Fix email delivery issues for error code 5.7.23 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Normal
f1.keywords:
- CSH
ms.custom: 
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 
description: "Learn how to fix email issues for error code 5.7.23 in Exchange Online (The message was rejected because of Sender Policy Framework violation)."
---

# Fix email delivery issues for error code 5.7.23 in Exchange Online

This topic describes what you can do if you see error code 550 5.7.23 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|

## I got this bounce message. How do I fix it?

Only an email admin in your Microsoft 365 or Office 365 organization can fix this issue. Contact your email admin and refer them to this information so they can try to resolve the issue for you.

## I'm an email admin. How do I fix this?

This bounce message most likely indicates a [Sender policy framework (SPF)](https://docs.microsoft.com/office365/securitycompliance/set-up-spf-in-office-365-to-help-prevent-spoofing) configuration issue in your Microsoft 365 or Office 365 organization.

The **Diagnostic information for administrators** section in the bounce message will contain the original error message when Office 365 tried to send the message to the external email server or service.

To fix this issue, do the following steps:

- Verify the SPF DNS record for your domain. To do this, we recommend that you use a publicly available SPF or DNS record checker on the web.

- Provision all of the domains you own. We limit the number of emails with unprovisioned domains that a tenant can send.

- Add your on-premises IPs, if any, to the SPF record of any domains you send for. This would include any unprovisioned domains you might be relaying through Office 365.

- Verify that the outbound message wasn't identified as spam by Office 365 and routed through the [High Risk Delivery Pool](https://docs.microsoft.com/office365/SecurityCompliance/high-risk-delivery-pool-for-outbound-messages). Messages in the High Risk Delivery Pool won't pass SPF checks, and therefore won't be accepted by the destination email organization.

  To receive Bcc copies of outbound messages that are determined to be spam, see [Configure outbound spam policy notifications in Office 365](https://docs.microsoft.com/office365/SecurityCompliance/configure-the-outbound-spam-policy).

  If you determine that the outbound message was incorrectly detected as spam by Office 365, contact support.

## Still need help with error code 550 5.7.23?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
