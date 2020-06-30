---
localization_priority: Normal
description: Learn how to use enhanced filtering for connectors (also known as skip listing) in Exchange Online if your organization sends mail to a third-party service or device before Microsoft 365 or Office 365.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enhanced filtering for connectors
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---


# Enhanced Filtering for Connectors in Exchange Online

Properly configured inbound connectors are a trusted source of incoming mail to Microsoft 365 or Office 365. But in complex routing scenarios where email for your Microsoft 365 or Office 365 domain is routed somewhere else first, the source of the inbound connector isn't necessarily the true indicator of where the message came from. These complex routing scenarios include:

- Third-party cloud filtering services

- Managed filtering appliance

- Hybrid environments (e.g., on-premises Exchange)

Enhanced Filtering for Connectors (also known as "skip listing") allows you to filter email based on the actual source of messages that arrive over the inbound connector. For more information about connectors in Exchange Online, see [Configure mail flow using connectors](use-connectors-to-configure-mail-flow.md).

## Scenarios

The most common scenarios that Enhanced Filtering is designed for are Hybrid environments; however, the mail destined for on-premise mailboxes will still not be filtered by Exchange Online Protection. The only way to get full Exchange Online Protection scanning on all mailboxes is to [move your MX record to Microsoft 365 or Office 365](https://docs.microsoft.com/Office365/SecurityCompliance/eop/set-up-your-eop-service#step-6-use-the-microsoft-365-admin-center-to-point-your-mx-record-to-eop).

Enhanced Filtering for Connectors is meant to show the value of Exchange Online Protection (EOP) and Advanced Threat Protection (ATP). If you have enabled Enhanced Filtering, we recommend that you move the MX record to Microsoft 365 or Office 365 after testing is done. Although it is possible to keep Enhanced Filtering enabled as a permanent solution, it is not recommended. Enhanced Filtering adds complexity to your environment, but sending the mail directly to Microsoft 365 or Office 365 reduces complexity. For example, some hosts might invalidate DKIM signatures, causing false positives. When two systems are responsible for protection, determining which one acted on the message to correct the problem is more complicated.

## About complex routing

Although we recommend that you point your MX record to Microsoft 365 or Office 365, we realize that there are legitimate business reasons to route your email elsewhere, first. A common scenario resembles the following diagram:

![Mail flow diagram showing inbound email from the internet to a third-party filtering service to Microsoft 365 or Office 365 and from outbound mail from Microsoft 365 or Office 365 to the internet.](../../media/a8ee0cd5-6a4c-4e57-9030-0f233def25f3v2.png)

In this example, the source of the incoming messages is somewhere on the internet. Since the MX record points to a third-party filter, the message goes there first, is processed, and is then sent to the Exchange Online mailbox.

When the message comes into Microsoft 365 or Office 365, Exchange Online Protection (EOP) believes that the third-party filter is the source of the message. This isn't a limitation of Microsoft 365 or Office 365; it's simply how SMTP works.

> [!IMPORTANT]
> Do not put another scanning service or host _after_ Exchange Online Protection (EOP). Once EOP scans a message, be careful not to break the chain of trust by routing mail through any non-Exchange server that is not part of your cloud or on-premises organization; when the message eventually arrives at the destination mailbox, the headers from the first scanning verdict might no longer be accurate. [Centralized Mail Transport](https://docs.microsoft.com/exchange/transport-options) should not be used to introduce non-Exchange servers into the mail flow path.  

## What happens when you enable Enhanced Filtering for Connectors?

In complex routing scenarios where you must point your MX record to something other than Microsoft 365 or Office 365, Enhanced Filtering for Connectors allows EOP to overlook, or skip, your internal (trusted) IP addresses to find the last known external (untrusted) IP address of the message. This previous IP should be the actual source IP address of the message. This feature is known as _skip listing_.

Using the previous example, you would configure the IP address of the third-party filter in Enhanced Filtering for Connectors, and Microsoft 365 or Office 365 will take care of the rest. The following table describes what connections look like before and after your enable Enhanced Filtering for Connector:

||||
|---|---|---|
||**Before Enhanced Filtering is enabled**|**After Enhanced Filtering is enabled**|
|**Email domain authentication**|[Implicit](https://docs.microsoft.com/office365/securitycompliance/anti-spoofing-protection#stopping-spoofing-with-implicit-email-authentication) using anti-spoof protection technology.|Explicit, based on the source domain's SPF, DKIM, and DMARC records in DNS.|
|**X-MS-Exchange-ExternalOriginalInternetSender**|Not available|This is stamped if skip listing was successful, enabled on the connector, and recipient match happens. The value of this field contains information about the true source address.|
|**X-MS-Exchange-SkipListedInternetSender**|Not available|This is stamped if skip listing was successful and enabled on the connector. The value of this field contains information about the true source address. This header is used primarily for reporting purposes and to help understand WhatIf scenarios.|
|

## Procedures for Enhanced Filtering for Connectors

### What do you need to know before you begin?

- You apply Enhanced Filtering for Connectors individually on each inbound connector.

- You need to include all of the trusted IP addresses that are associated with the on-premises hosts or the third-party filters that send email into your Microsoft 365 or Office 365 organization, including any intermediate hops with public IP addresses. To get these IP addresses, consult the documentation or support that's provided with the service.

- To open the Microsoft 365 compliance center, see [Microsoft 365 compliance center](https://docs.microsoft.com/office365/securitycompliance/go-to-the-securitycompliance-center).

- The cmdlets to manage inbound connectors are available in Exchange Online PowerShell. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- The account you use for the procedures needs to be a Microsoft 365 or Office 365 Exchange administrator. For more information about permissions in the Security & Compliance Center, see [Permissions in the Security & Compliance Center](https://docs.microsoft.com/microsoft-365/security/office-365-security/permissions-in-the-security-and-compliance-center)

### Use the Security & Compliance Center to configure Enhanced Filtering for Connectors on an inbound connector

1. Open the Security and Compliance Center, expand **Threat Management**, choose **Policy**, and then choose **Enhanced Filtering**.

   > [!TIP]
   > To go directly to the **Enhanced Filtering** page in the Security & Compliance Center, use this URL: [https://protection.office.com/skiplisting](https://protection.office.com/skiplisting)

2. Select the inbound connector that you want to configure.

3. In the flyout pane, configure the following settings:

   - **IP addresses to skip**: Choose one of the following values:

     - **Automatically detect and skip the last IP address**: We recommend this option if you have only one message source to skip.

     - **Skip these IP addresses that are associated with the connector**: Select this option to configure a list of IP addresses to skip.

     - **Disable Enhanced Filtering for Connectors**: Turn off Enhanced Filtering for Connectors on the connector.

   - **Apply to these users**: Choose one of the following values:

     - **Apply to a small set of users**: We recommend this option as an initial test of the feature.

       **Notes**:

       - This option is only affective on messages where **all** recipients are specified here. If a message contains **any** recipients that aren't specified here, normal filtering is applied to **all** recipients of the message.

       - This option is only affective on the actual email address that you specify. For example, if a user has five email addresses associated with their mailbox (also known as _proxy addresses_), you'll need to specify all five of their email addresses here. Otherwise, messages that are sent to the four other email addresses will go through normal filtering.

        - For a hybrid environment where the inbound mail flow is through on-premises Exchange, you must specify the *targetAddress* of the *MailUser* object. For example, *michelle@contoso.mail.onmicrosoft.com*.

   - **Apply to entire organization**: We recommend this option after you've tested the feature on a small number of recipients first.

4. When you're finished, click **Save**.

5. If you have an mail flow rule (also known as a transport rule) rule that force messages to skip filtering once it comes into EOP (i.e., marking the SCL of the message to -1 to prevent double filtering), you will need to disable that mail flow rule.

### Use Exchange Online PowerShell to configure Enhanced Filtering for Connectors on an inbound connector

To configure Enhanced Filtering for Connectors on an inbound connector, use the following syntax:

```powershell
Set-InboundConnector -Identity <ConnectorIdentity> [-EFSkipLastIP <$true | $false>] [-EFSkipIPs <IPAddresses>] [-EFUsers "emailaddress1","emailaddress2",..."emailaddressN"]
```

- _EFSkipLastIP_: Valid values are:

  - `$true`: Only the last message source is skipped.

  - `$false`: Skip the IP addresses specified by the _EFSkipIPs_ parameter. If no IP addresses are specified there, Enhanced Filtering for Connectors is disabled on the inbound connector. The default value is `$false`.

- _EFSkipIPs_: The specific IP addresses to skip when the _EFSkipLastIP_ parameter value is `$false`. Valid values are:

  - **A single IP address**: For example, `40.108.128.1`.

  - **An IP address range**: For example, `40.108.128.0-40.108.128.31`.

  - **Classless Inter-Domain Routing (CIDR) IP**: For example, `40.108.128.1/27`.

- _EFUsers_: The comma-separated email address of the recipients that you want to apply Enhanced Filtering for Connectors to. The default value is blank (`$null`), which means Enhanced Filtering for Connectors is applied to all recipients.

This example configures Enhanced Filtering for Connectors on the inbound connector named From Anti-spam Service with the following settings:

- _EFSkipLastIP_: `$true`

- _EFUsers_: michelle@contoso.com, laura@contoso.com, julia@contoso.com.

```powershell
Set-InboundConnector -Identity "From Anti-spam Service" -EFSkipLastIP $true -EFUsers "michelle@contoso.com","laura@contoso.com","julia@contoso.com"
```

For detailed syntax and parameter information, see [Set-InboundConnector](https://docs.microsoft.com/powershell/module/exchange/set-inboundconnector).

> [!IMPORTANT]
> - Entering the IP addresses of Microsoft 365 or Office 365 is not supported. Do not use this feature to compensate for issues introduced by unsupported email routing paths. Use caution and limit the IP ranges to only the mail systems that will handle your own organization's messages prior to Microsoft 365 or Office 365. >
>
> - Entering any private IP address defined by RFC 1918 (10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16) is not supported. Enhanced Filtering automatically detects and skips private IP addresses.

## How to measure success

Enhanced Filtering demonstrates that changing the MX record to EOP is not only possible, but is also better. It simplifies the mail flow in your environment and demonstrates the value of EOP, which you already purchased as part of Microsoft 365 or Office 365.  This can easily be measured using variety of reports available in the [Reports dashboard](https://protection.office.com/insightdashboard).

You can [view these email security reports in the Security & Compliance Center](https://docs.microsoft.com/office365/securitycompliance/view-email-security-reports):

- Threat Protection Status report

- Spam Detections report

## See also

[Mail flow best practices for Exchange Online, Microsoft 365, and Office 365 (overview)](../mail-flow-best-practices.md)

[Configure mail flow using connectors](use-connectors-to-configure-mail-flow.md)
