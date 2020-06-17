---
title: "Fix email delivery issues for error code 5.4.6 or 5.4.14 in Exchange Online"
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
ms.assetid: 81212ae4-4c36-4e8f-9546-e58b70cfd74b
description: "Learn how to fix email issues for error code 5.4.6, 5.4.14, or other error codes related to mail routing loops in Exchange Online."
---

# Fix email delivery issues for error code 5.4.6 or 5.4.14 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if see the error codes 5.4.6, 5.4.14 or other error codes related to mail routing loops in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

## Why did I get this bounce message?

The most likely cause is if the message hop count is exceeded or the route the message is delivered through is broken. Some causes and solutions are provided in this topic.

5.4.6 indicates a mail loop or routing problem in on-premises Exchange Server, which you would likely encounter in a hybrid environment.

5.4.14 indicates a mail loop or routing problem in Exchange Online.

 The information here applies to a range of error codes 5.4.6 through 5.4.20. Use the information in the NDR to help you decide how to fix the problem.

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|

## I got this bounce message. How do I fix it?

Typically, these can only be fixed by an Exchange Online admin and not the average email sender. Contact your email admin and refer them to this information so they can try to resolve the issue for you.

## I'm an email admin. How do I fix this?

The most common issues and fixes are described in the following sections.

### Accepted domain issues

Verify that the recipient's domain is configured as an authoritative accepted domain in Exchange Online. For more information, see [Manage accepted domains in Exchange Online](../manage-accepted-domains/manage-accepted-domains.md).

### Hybrid configuration issues

If your domain is part of a hybrid deployment between Exchange and Exchange Online, check the following items based on your configuration.

#### You route all incoming mail for your hybrid domain through Exchange Online

This error can happen when the MX record for your hybrid domain points to Exchange Online, and the outbound connector that's used to route email from Exchange Online to your on-premises Exchange organization is configured to use DNS routing instead of smart host routing.

To fix the problem, configure a dedicated outbound connector that uses smart host routing and that has your on-premises hybrid server configured as a smart host. The easiest way to fix the problem is to re-run the Hybrid Configuration Wizard in your on-premises Exchange organization. Or, you can verify the configuration of the connector that's used for hybrid by following these steps:

1. Open the [Microsoft 365 admin center](https://admin.microsoft.com), and then click **Admin centers** \> **Exchange** (you might need to click **...show all** first).

2. In the Exchange admin center (EAC), click **Mail Flow** \> **Connectors**. In the **Outbound connectors** section, select the connector that's used for hybrid, and then click **Edit**.

3. On the **Delivery** tab, verify that **Route mail through smart hosts** is selected and that the correct IP address or FQDN is specified for the smart host in your on-premises Exchange organization.

#### You route all outgoing mail from Exchange Online through your on-premises hybrid server

This configuration is controlled by the value of the _RouteAllMessagesViaOnPremises_ parameter on the outbound connector that's used for hybrid. When the value of this parameter is `$true`, you're routing all outgoing mail from Exchange Online through your on-premises hybrid server. You can verify this value by replacing \<Connector Name\> with your value and running the following command in [Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell):

```powershell
Get-OutboundConnector -Identity "<Connector Name>" | Format-List Name,RouteAllMessagesViaOnPremises
```

In this configuration, the error is caused by either of the following issues on the inbound connector from your on-premises Exchange organization to Exchange Online:

- You don't have an inbound connector that has the **Connector Type** value **On-premises**.

- The inbound connector is scoped to one or more accepted domains.

To fix the problem, configure a dedicated inbound connector that has the **Connector Type** value *On-premises** and that's not scoped to any accepted domains. The easiest way to fix the problem is to re-run the Hybrid Configuration Wizard in the on-premises Exchange organization. Or, you can verify the configuration of the Inbound connector that's used for hybrid by following these steps:

1. Open the [Microsoft 365 admin center](https://admin.microsoft.com), and then click **Admin centers** \> **Exchange** (you might need to click **...show all** first).

2. In the EAC, click **Mail Flow** \> **Connectors**. In the **Inbound connectors** section, select the connector that's used for hybrid, and then click **Edit** ![Edit icon](../../media/6f22ff21-4c94-4b91-a490-173a853c06e3.gif). Verify the following information:

   - **General**: Verify that **On-premises** is selected.

   - **Scope**: Verify that **Accepted domains** is empty.

For more information about mail routing in hybrid deployments, see [Transport routing in Exchange hybrid deployments](https://docs.microsoft.com/exchange/transport-routing).

## Causes for NDR 5.4.14 and what does this error mean?

There are two likely possibilities:

- Based on the domain in the recipient's email address, your Exchange Online organization accepted the message, but then couldn't correctly route the message to the recipient. This is likely caused by accepted domain configuration issues.

- In hybrid environments, there are misconfigured connectors in your Exchange Online organization.

### Details about NDRs related to hop count exceeded

Here are some of the error codes that are related to mail routing loops or a bad mail routing configuration:

- `554 5.4.6 Hop count exceeded - possible mail loop` (always generated by on-premises Exchange Servers)

- `5.4.14 Hop count exceeded - possible mail loop ATTR34` (always generated by Exchange Online)

## Still need help?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
