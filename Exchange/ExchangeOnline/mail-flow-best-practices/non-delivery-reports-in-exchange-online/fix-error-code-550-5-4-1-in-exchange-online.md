---
title: "Fix email delivery issues for error code 550 5.4.1 in Exchange Online"
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
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 7dcf7a8b-e00e-49f8-bf8d-74aba79c5a6a
description: "Learn how to fix email issues for error code 5.4.1 in Exchange Online (the destination email server doesn't accept email from the sender's domain)."
---

# Fix email delivery issues for error code 550 5.4.1 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 5.4.1 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

## Why did I get this bounce message?

The email server that's generating the error doesn't accept email from the sender's domain (for example, @fabrikam.com). This error is generally caused by email server or DNS misconfiguration.

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How can I fix this?](#im-an-email-admin-how-can-i-fix-this)|

## I got this bounce message. How do I fix it?

Here are some steps that you can try to fix the problem yourself.

If the steps in this section don't fix the problem for you, contact your email admin and refer them to the information in this topic so they can try to resolve the issue for you.

- **Just wait**: It might seem strange, but this error might go away on its own after a few days. If your email admin made changes to your organization's domain name system (DNS) records, the change can prevent you from sending and receiving email for a brief period, even if they did everything correctly (it can take up to 72 hours for DNS changes to propagate on the internet). If you'd like more details about DNS records, see [DNS basics](https://support.office.com/article/854b6b2b-0255-4089-8019-b765cff70377.aspx).

- **Service outage**: A problem with the whole Office 365 service could be causing the problem. Even your email admins can't do anything about service outages except wait for the problem to be resolved.

## I'm an email admin. How can I fix this?

The most common issues and fixes are described in the following sections.

### Incorrect MX record

If external senders receive this NDR when they send email to recipients in your domain, try the following fixes:

- **Fix your MX record**: For example, it might be pointing to an invalid mail server. Check with your domain registrar or DNS hosting service to verify the MX record for your domain is correct. The MX record for a domain that's enrolled in Exchange Online uses the syntax  _\<domain\>_.mail.protection.outlook.com.

- **Verify only one MX record is configured for your domain**: We don't support using more than one MX record for domains enrolled in Exchange Online.

- **Test your MX record**: Use the **Outbound SMTP EMail** test in the [Microsoft Remote Connectivity Analyzer](https://testconnectivity.microsoft.com/tests/o365).

### Domain configuration issues

1. Open the [Microsoft 365 admin center](https://admin.microsoft.com).

2. Click **Domains** and verify your domain appears in the list as **Active**.

3. Select the domain and click **Troubleshoot**. Follow the troubleshooting wizard steps.

If you control of the DNS records for your Office 365 domain, you can also check the status of the domain in the Exchange admin center (EAC) by following these steps:

1. In the Microsoft 365 admin center, click **Admin** \> **Exchange**.

2. Click **Mail flow** \> **Accepted domains**.

3. Verify that your domain is listed, and verify the **Domain Type** value for the domain. Typically, the value should be **Authoritative**. However, if you have properly configured a shared domain, the value might be **Internal Relay**.

### Updated DNS records haven't propagated

You updated your domain's DNS records correctly for Office 365, but the changes haven't propagated to all DNS servers on the internet. Changes to your domain's DNS records might take up to 72 hours to propagate to all DNS servers on the internet.

### Hybrid deployment configuration issues

If your domain is part of a hybrid deployment between on-premises Exchange and Exchange Online, check the following items:

- Verify the configuration of the Send connectors and Receive connectors in your on-premises Exchange organization that are used for hybrid. These connectors are configured automatically by the Hybrid Configuration Wizard, and the wizard might need to be run again by your Exchange administrator.

For more information, see [this topic](https://docs.microsoft.com/office365/troubleshoot/antispam/relay-access-denied-ndr).

For more information about transport routing in hybrid deployments, see [Transport Routing in Exchange Hybrid Deployments](https://docs.microsoft.com/exchange/transport-routing).

### Service issues in Exchange Online

A service issue in Office 365 might be causing the problem. To check the status of Office 365, do the following steps:

1. Open the [Microsoft 365 admin center](https://admin.microsoft.com).

2. Click **Service health** to see an overview of any issues.

3. Select **View all** to a get more details about all known issues.

## Details about this NDR

The Exchange Online non-delivery report (NDR) notification for this specific error might contain some or all of the following information:

- **User information section**

  - Relay Access Denied

  - The outbound connection attempt was not answered because either the remote system was busy or it was unable to take delivery of the message.

- **Diagnostic information for administrators section**

  - No answer from host.

  - `#550 5.4.1 Relay Access Denied ##`

## Still need help?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
