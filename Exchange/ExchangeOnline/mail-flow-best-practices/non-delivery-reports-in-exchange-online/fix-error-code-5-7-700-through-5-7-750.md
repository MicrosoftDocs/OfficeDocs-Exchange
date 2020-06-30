---
title: "Fix email delivery issues for error codes 5.7.700 through 5.7.750 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: dansimp
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Priority
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 
description: "Learn how to fix email issues for error code 5.7.700 through 5.7.750 in Exchange Online."
---

# Fix email delivery issues for error codes 5.7.700 through 5.7.750 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error codes 550 5.7.700 through 5.7.750 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

Use the information in the NDR to help you decide how to fix the problem.

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. What can I do to fix this?](#im-an-email-admin-what-can-i-do-to-fix-this)|

## Why did I get this bounce message?

- **5.7.705 Access denied, tenant has exceeded threshold**: This message occurs when too much spam or bulk mail has been sent by your organization and we place a block on outgoing mail.

- **5.7.708 Access denied, traffic not accepted from this IP**: This error occurs when sending email from known, low reputation IP addresses that are typically used by new customers.

- **5.7.750 Client blocked from sending from unregistered domain**: The error occurs when a large volume of messages are sent from domains that aren't provisioned in Office 365 (added as accepted domains and validated).

## I got this bounce message. How do I fix it?

Only an email admin in your organization can fix the issue. Contact your email admin and refer them to this information so they can resolve the issue for you.

## I'm an email admin. What can I do to fix this?

The solutions for specific error codes are described in the following sections.

### 5.7.705 Access denied, tenant has exceeded threshold

Common causes are compromised on-premises servers or compromised admin accounts that have been used to create connectors. Either condition can allow spam to pass through your organization.

To remove this block, you need to understand and explain the cause to a support agent, as well as correct the underlying problem.

In rare cases, this could also happen if you renew your subscription after it has already expired. It takes time for the service to sync the new subscription information (typically, no more than one day), but your organization could be blocked from sending email in the meantime. The best way to prevent this is to make sure your subscription does not expire.

### 5.7.708 Access denied, traffic not accepted from this IP

If you must send email from these low reputation IP addresses before you can purchase licenses, contact support to request an exception until you're able to purchase licenses.

### 5.7.750 Client blocked from sending from unregistered domain

In most cases, the connectors are set up correctly, but email is being sent from unregistered (also known as unprovisioned) domains. Office 365 allows a reasonable amount of email from unregistered domains, but you should configure every domain that you use to send email as an accepted domain.

To fix this error, you can:

- **Most common solution**: Add and validate all domains in Microsoft 365 or Office 365 that you use to send email messages. For more information, see [Add a domain](https://docs.microsoft.com/microsoft-365/admin/setup/add-domain).

- Use a certificate-based outbound connector where the certificate's domain is an accepted and validated domain in Microsoft 365 or Office 365. For more information, see [Configure mail flow using connectors](../use-connectors-to-configure-mail-flow/use-connectors-to-configure-mail-flow.md).

- Look for unusual connectors and compromised accounts. Attackers will often create new inbound connectors in your Microsoft 365 or Office 365 organization to send spam. For more information, see [Validate connectors](../use-connectors-to-configure-mail-flow/validate-connectors.md), and [Responding to a compromised email account](https://docs.microsoft.com/microsoft-365/security/office-365-security/responding-to-a-compromised-email-account).

## Still need help with error codes 5.7.700 through 5.7.750?

[![Get help from the community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
