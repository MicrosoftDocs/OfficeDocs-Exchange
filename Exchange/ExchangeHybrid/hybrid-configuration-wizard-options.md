---
title: "Hybrid Configuration wizard options"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: article
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Ent_O365_Hybrid
- Hybrid
- M365-email-calendar
ms.assetid: 2e6ed294-ee74-4038-8b71-b61786372ba4
ms.reviewer: 
description: "Overview of Exchange hybrid configuration wizard options"
---

# Hybrid Configuration Wizard options

Hybrid Configuration Wizard (HCW) has changed a lot since it was released as part of Exchange 2010 Service Pack 2. Currently, it supports a Classic Topology (Minimal, Express, and Full), and a Modern Topology (Minimal and Full). The table below shows the differences between the options available in the Exchange Classic Topology (Minimal, Express, and Full), and Modern Topology (Minimal and Full). 

> [!NOTE]
> This table is an expanded version of the one which was originally published in Chapter 13 in the book, Microsoft Office 365 Administration Inside Out, 2nd Edition. Reference- Fisher, Ed; Guilmette, Aaron; Kegg, Darryl; Mandich, Lou. Microsoft Office 365 Administration Inside Out (Includes Current Book Service), 2nd ed. Pearson Education, Inc., 2017. Print.

|**Exchange Hybrid Configuration Options**|**Classic Minimal**|**Classic Express**|**Classic Full**|**Modern Minimal**|**Modern Full**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|E-mail Address Policy and Domain configuration|Yes|Yes|Yes|Yes|Yes|
|Send and Receive Connector Configuration|No|No|Yes|No|Yes|
|OAuth Configuration   |No|No|Yes for  Exchange 2013/2016/2019 with current CU|No|Yes for Exchange 2013/2016/2019 with current CU|
|Federation Trust and Organization Relationship|No|No|Yes|No   |Yes  |
|MRS Endpoint Configuration|Yes|Yes|Yes|Yes|Yes|
|AAD Connect in Express Configuration|No|Yes|No; set up separately|No; set up separately|No; set up separately|
|Organization Configuration Transfer|Yes|Yes|Yes|Yes|Yes  |
|Hybrid Modern Authentication|No|No|No [1]|No|No|
|Cross-premises multi mailbox search|No|No|Yes|No|No|
|Cross-premises Mail Tips|No|No|Yes|No|Yes|
|Autodiscover external DNS and public certificate required|Yes|Yes|Yes|Yes (see note after the table)|Yes (see note after the table)|
|Transport external DNS and public certificate required|No|No|Yes|No|Yes|
|EWS external DNS and public certificate required|Yes|Yes|Yes|No|No|
|TCP Port 443 Inbound to Exchange On-premises|Yes|Yes|Yes|Yes for Autodiscover if needed, No for Hybrid Agent|Yes for Autodiscover if needed, No for Hybrid Agent|
|TCP Port 443 Outbound to Exchange Online|Yes|Yes|Yes|Yes|Yes|
|TCP Port 25 Inbound to Exchange on-premises|No|No|Yes|No|Yes|
|TCP Port 25 Outbound to Exchange Online Protection|No|No|Yes|No|Yes|
|TCP Port 80 Outbound for Certificate Revocation Check|Yes|Yes|Yes|Yes|Yes|
|Third-Party Certificate Required- Autodiscover/Transport/EWS for Exchange On-Premises, Exchange Server 2013/2016/2019|Yes|Yes|Yes|Yes for Autodiscover if needed, No for Hybrid Agent|Yes for Autodiscover and Transport if needed, No for Hybrid Agent|
|Exchange Server Edge 2013/2016/2019 with Edge Sync and Edge Third-Party Certificate for Transport Option, TCP Port 25 in/out at network egress|No|No|Yes|No|Yes|
|Exchange Server Edge 2013/2016/2019 without Edge Sync and Edge Third-Party Certificate for Transport Option, TCP Port 25 in/out at network egress|No|No|No|No|No|

> [!NOTE]
> Depending on the your topology and configuration, you may still need to publish Autodiscover records in external DNS or open TCP Port 25 inbound and outbound to your Exchange environments for other reasons, such as Exchange Active Sync Clients using the legacy mail client in Android or iOS (although we highly recommend Microsoft Outlook for iOS and Android as the mobile messaging application). Another reason might be using a feature like Exchange Server Hybrid Modern Authentication. Make sure to review the limitations of the hybrid agent and modern hybrid topology covered in the article [Microsoft Hybrid Agent](hybrid-deployment/hybrid-agent.md).

[1] The Hybrid Configuration Wizard will provide the foundational components to prepare the environment for Hybrid Modern Authentication. The additional steps needed to complete the process for Hybrid Modern Authentication are located [here](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication).

## Related topics

- [Exchange deployment assistant](https://assistants.microsoft.com/)

- [Exchange Server hybrid deployments](exchange-hybrid.md)

- [Using hybrid Modern Authentication with Outlook for iOS and Android](https://docs.microsoft.com/Exchange/clients/outlook-for-ios-and-android/use-hybrid-modern-auth?view=exchserver-2019)

- [How to configure Exchange Server on-premises to use Hybrid Modern Authentication](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)

- [Configure OAuth authentication between Exchange and Exchange Online organizations](../ExchangeServer2013/configure-oauth-authentication-between-exchange-and-exchange-online-organizations-exchange-2013-help.md)

- [Microsoft Hybrid Agent](hybrid-deployment/hybrid-agent.md)
