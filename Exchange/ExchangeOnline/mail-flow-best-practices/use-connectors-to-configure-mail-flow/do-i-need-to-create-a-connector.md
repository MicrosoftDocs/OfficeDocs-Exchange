---
localization_priority: Normal
description: Find your mail flow scenario to see if you need to create a connector for your Exchange Online or Exchange Online Protection organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 16731ae9-c909-49dd-bffc-a46e6151fc29
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- ms.exch.eac.ConnectorIsConnectorNeeded
title: Do I need to create a connector in Exchange Online?
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Do I need to create a connector in Exchange Online?

Find your mail flow scenario to see if you need to create a connector for your Exchange Online organization.

|**Scenario**|**Description**|**Connector required?**|**Connector settings**|
|:-----|:-----|:-----|:-----|
|You have a standalone EOP subscription.|You have your own on-premises email servers, and you subscribe to EOP only for email protection services for your on-premises mailboxes (you have no mailboxes in Exchange Online). <br/><br/> For more information, see the topic [Exchange Online Protection overview](https://docs.microsoft.com/office365/securitycompliance/eop/exchange-online-protection-overview) and [How connectors work with my on-premises email servers](use-connectors-to-configure-mail-flow.md#how-connectors-work-with-my-on-premises-email-servers).|Yes|**Connector for incoming email:** <br/>• **From**: Your on-premises email server <br/>• **To**: Office 365<br/><br/> **Connector for outgoing email**: <br/>• **From**: Office 365 <br/>• **To**: Your on-premises mail server|
|Some of your mailboxes are on your on-premises email servers, and some are in Exchange Online.|Before you manually configure connectors, check whether an Exchange hybrid deployment better meets your business needs. <br/> For details, see [I have my own email servers](use-connectors-to-configure-mail-flow.md#i-have-my-own-email-servers) and [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid).|Yes|**Connector for incoming email:** <br/>• **From**: Your on-premises email server <br/>• **To**: Office 365<br/><br/> **Connector for outgoing email:** <br/>• **From**: Office 365 <br/>• **To**: Your on-premises email server|
|All of your mailboxes are in Exchange Online, but you need to send email from sources in your on-premises organization.|You don't have your own email servers, but you need to send email from non-mailboxes: printers, fax machines, apps, or other devices. <br/><br/> For details, see [Option 3: Configure a connector to send mail using Microsoft 365 or Office 365 SMTP relay](../how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md#option-3-configure-a-connector-to-send-mail-using-microsoft-365-or-office-365-smtp-relay)|Optional|**Only one connector for incoming email:** <br/>• **From**: Your organization's email server <br/>• **To**: Office 365|
|You frequently exchange sensitive information with business partners, and you want to apply security restrictions.|You want to use Transport Layer Security (TLS) to encrypt sensitive information or you want to limit the source (IP addresses) for email from the partner domain.<br/><br/> For details, see [Set up connectors for secure mail flow with a partner organization](set-up-connectors-for-secure-mail-flow-with-a-partner.md).|Optional|**Connector for incoming email:** <br/>• **From**: Partner organization <br/>• **To**: Office 365<br/> **Connector for outgoing email:** <br/>• **From**: Office 365 <br/>• To: Partner organization|

> [!NOTE]
> For more information about these scenarios, see [Configure mail flow using connectors in Office 365](use-connectors-to-configure-mail-flow.md).
