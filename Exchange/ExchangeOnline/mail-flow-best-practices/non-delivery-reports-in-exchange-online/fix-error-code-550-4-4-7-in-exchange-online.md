---
title: "Fix email delivery issues for error code 550 4.4.7 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: High
f1.keywords:
- CSH
ms.custom: 
- MiniMaven
- '125'
search.appverid:
- BCS160
- MOE150
- ZOL120
- ZOL140
- ZOL150
- ZOL160
- MET150
ms.assetid: 2fda59a8-84f8-4eb0-96e1-49187fbe7ff9
description: "Learn how to fix email issues for error code 550 4.4.7 in Exchange Online (the destination email server won't or can't accept the message)."
---

# Fix email delivery issues for error code 550 4.4.7 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 4.4.7 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

## Why did I get this bounce message?

For more information, see the [Causes for error code 4.4.7](#causes-for-error-code-447) section later in this topic.

Use the information in the NDR to help you decide how to fix the problem.

|||||||
|:-----|:-----|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|![Help symbol](../../media/5bf13e77-0400-4dda-a569-b99b8a918b48.png)|[Causes for error code 4.4.7](#causes-for-error-code-447)|

## I got this bounce message. How do I fix it?

This section contains the steps that you can try to fix the problem yourself.

If the steps in this section don't fix the problem for you, contact your email admin and refer them to this topic so they can try to resolve the issue for you.

If you get this error only for messages that you sent to a specific domain (for example, only recipients in the @fabrikam.com domain), the problem is likely with that destination domain. For example:

- Temporary network or internet connection issues in the destination domain.

- Aggressive anti-spam settings in the destination domain that block legitimate senders (for example, all senders from any domain in Exchange Online).

If you suspect a problem with the destination domain, notify the recipient (by phone, in person, etc.) with the information in the NDR so they can notify their email admins:

- The name of the email mail server in the destination domain, and the error message that's returned by the email server.

- The number of delivery attempts made that were made by the datacenter server in Exchange online, and how long it tried to reach the remote server.

The email admins in the destination domain will need to investigate the issue. Possible solutions might include:

- Stop blocking messages from Exchange Online or specifically allow messages from senders in your domain.

- Contact the support channels for their email server or service. Microsoft support might also be able to help.

## I'm an email admin. How do I fix this?

If the admins in the destination domain determine the problem isn't on their end, the solution might be related the configuration of your Exchange Online organization (or also your on-premises Exchange organization if you're in a hybrid deployment).

Here are some steps for you to try:

- **Solution 1**: The MX record for your domain might be missing or incorrect. Get more information about how MX records work at [DNS basics](https://docs.microsoft.com/microsoft-365/admin/get-help-with-domains/dns-basics).

- **Solution 2**: Test your MX record and your organization's ability to send mail by using the **Outbound SMTP Email** test in the [Microsoft Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/o365).

- **Solution 3**: The Sender Policy Framework (SPF) record for your domain might be incomplete, and might not include all email sources for your domain. For more information, see [Set up SPF to help prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-spf-in-office-365-to-help-prevent-spoofing).

- **Solution 4**: Your domain might have expired due to non-payment. Verify with your domain registrar that your domain is active and not expired.

- **Solution 5**: If the recipient is in your on-premises Exchange organization in a hybrid deployment, there might be a problem with your hybrid configuration. Give the information in the NDR to your on-premises Exchange administrators. They might need to rerun the Hybrid Configuration Wizard due to changes in their on-premises IP addresses or firewall rules.

For more information about message routing in hybrid deployments, see [Transport routing in Exchange hybrid deployments](https://docs.microsoft.com/exchange/transport-routing).

## Causes for error code 4.4.7

When Exchange Online attempts to deliver a message, the destination email might be unable or unwilling to accept the message. This can result in a temporary 4_.x.x_ error code from the destination email server (instead of a permanent 5._x.x_ error code that indicates the message was rejected). Exchange Online repeatedly tries to deliver the message over 24 hours. Only after two days of unsuccessful delivery attempts does the recipient receive this NDR.

The possible causes of this error are:

- The destination email server is offline or unreachable.

- The server won't accept delivery of the message.

- A network problem is causing message delivery to time-out.

## Details for error code 5.1.0

The NDR from Exchange Online for this specific error might contain some or all of the following information:

- **User information section**

  - The server has tried to deliver this message, without success, and has stopped trying. Please try sending this message again. If the problem continues, contact your help desk.

- **Diagnostic information for administrators section**

  - `#550 4.4.7 QUEUE.Expired; message expired ##`

  - The message was considered too old by the rejecting system, either because it remained on that host too long or because the time-to-live value specified by the sender of the message was exceeded.

  - `450 4.7.0 Proxy session setup failed on Frontend with '451 4.4.0 Primary target IP address responded with ...` Be sure to record the error that follows this string and the last end point attempted.

## Still need help with error code 4.4.7?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
