---
title: "Server roles in Exchange 2013/Exchange 2010 hybrid deployments"
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
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: 7d774948-94f9-4405-af2b-0c67c0405c0f
ms.reviewer:
description: "When you configure a hybrid deployment in an Exchange 2010 organization, we strongly recommend at least one Exchange 2013 server with the Client Access and Mailbox server roles in your existing Exchange 2010 organization. The Exchange 2013 Client Access and Mailbox servers coordinate communications between your existing Exchange 2010 on-premises organization and the Exchange Online organization. This communication includes message transport and messaging features between the on-premises and Exchange Online organizations."
---

# Server roles in Exchange 2013/Exchange 2010 hybrid deployments

When you configure a hybrid deployment in an Exchange 2010 organization, we strongly recommend at least one Exchange 2013 server with the Client Access and Mailbox server roles in your existing Exchange 2010 organization. The Exchange 2013 Client Access and Mailbox servers coordinate communications between your existing Exchange 2010 on-premises organization and the Exchange Online organization. This communication includes message transport and messaging features between the on-premises and Exchange Online organizations.

We highly recommend installing more than one Exchange 2013 server in your on-premises organization to help increase reliability and availability of hybrid deployment features.

## Server roles in a hybrid deployment

Here is a quick overview of the Exchange 2013 server roles in a hybrid deployment:

- **Client Access server role**: The Exchange 2013 Client Access server role continues to provide many of the same functions that are typically provided by Exchange 2010 Client Access servers in your organization with some additions required to support a hybrid deployment and coexistence with Exchange 2010. The Client Access server also handles secure mail messages sent from the Exchange Online organization to the on-premises organization, as well as handling transport rules, journaling policies, and message delivery to Mailbox servers in a hybrid deployment. A dedicated Receive connector is configured on the Client Access server to support secure hybrid mail transport. All client connectivity, including Outlook client access, Outlook Web App, and Outlook Anywhere goes through the Client Access server role. Organization relationship features between the on-premises and Exchange Online organizations, such as free/busy sharing, are also handled by the Client Access server role.

    Learn more at [Client Access server](https://docs.microsoft.com/exchange/client-access-server-exchange-2013-help).

- **Mailbox server role**: The Exchange 2013 Mailbox server role handles secure mail messages sent to the Exchange Online organization from the on-premises organization. Although not typical, it also can host on-premises recipient mailboxes and communicate with the Exchange Online organization by proxy via the on-premises Client Access server. A dedicated Send connector is configured by default on the Mailbox server role to support secure hybrid mail transport.

    Learn more at [Mailbox server](https://docs.microsoft.com/exchange/mailbox-server-exchange-2013-help).

Depending on the hybrid deployment configuration that you want, an Exchange 2013 server requires one or both of the server roles to be installed on it:

- **Single Exchange server**: If you choose to install a single Exchange server in your on-premises organization, you'll need to install both the Client Access and Mailbox server roles on the single server.

- **More than one Exchange server**: If you choose to install more than one Exchange server in your on-premises organization, you can install the server roles on separate servers in your on-premises organization. For example, you could install one Exchange 2013 server that has the Mailbox and Client Access roles installed and also install another Exchange server that has only the Client Access server role installed. However, the best practice and recommended server configuration is to install both the Client Access and Mailbox server roles on each Exchange 2013 server deployed in your on-premises organization.

Learn more about Exchange capacity planning at [Understanding Multiple Server Role Configurations in Capacity Planning](https://docs.microsoft.com/exchange/exchange-2013-sizing-and-capacity-planning-exchange-2013-help.md).

## Exchange server functionality in hybrid deployments

Exchange servers provide several important functions for your on-premises organization in a hybrid deployment:

- **Federation**: Exchange 2013 and Exchange 2010 servers enable you to create a federation trust for your on-premises organization with the Microsoft Federation Gateway. The Microsoft Federation Gateway is a free, cloud-based service offered by Microsoft that acts as the trust broker between your on-premises organization and the Microsoft 365 or Office 365 organization. Federation is a requirement for creating an organization relationship between the on-premises and the Exchange Online organizations.

    Learn more at [Understanding Federation](https://docs.microsoft.com/exchange/federation-exchange-2013-help).

- **Organization relationships**: Exchange 2013 servers with the Client Access server role enable the creation of organization relationships between the on-premises and Exchange Online organizations. Organization relationships are required for many other services in a hybrid deployment, including calendar free/busy information sharing, message tracking, and mailbox moves between the on-premises and Exchange Online organizations.

    Learn more at [Sharing](https://docs.microsoft.com/exchange/sharing-exchange-2013-help).

- **Message transport**: Exchange 2013 servers with the Client Access and Mailbox server roles are responsible for message transport in a hybrid deployment. Using Send and Receive connectors, they serve as the connection endpoints for incoming external messages and also provide outbound message delivery to the Internet and the Exchange Online organization.

    Learn more at [Transport options in Exchange 2013/Exchange 2010 hybrid deployments](transport-options.md).

- **Message transport security**: Exchange 2013 servers with the Client Access and Mailbox server roles help to secure message communication between the on-premises and Exchange Online organizations by using the Domain Security functionality in Exchange 2013. Security can be increased by using mutual transport layer security authentication and encryption for message communications.

    Learn more at [Understanding Domain Security](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124392(v=exchg.141)).

- **Outlook Web App**: Exchange 2013 servers with the Client Access server role support configuring a single URL endpoint for external connections to on-premises and Exchange Online mailboxes. For on-premises mailboxes, Client Access servers are configured to service Outlook Web App requests. For Exchange Online organization mailboxes, Client Access servers are configured to automatically display a link to the Outlook Web App endpoint on the Exchange Online organization.

    Learn more at [Outlook Web App](https://docs.microsoft.com/exchange/outlook-web-app-exchange-2013-help).

## Exchange server topology

If you add additional Exchange 2013 servers to support your hybrid deployment, the Exchange server is deployed much like any other Exchange server is deployed to your existing Exchange 2010 organization. Configuring your existing on-premises Exchange 2010 organization for a hybrid deployment doesn't require any special Exchange server topology. However, you must install Exchange 2010 Service Pack 3 (SP3) on your Exchange 2010 servers and also install Exchange 2013 Cumulative Update 1 (CU1) or greater to enable compatibility and full hybrid functionality with Microsoft 365 and Office 365.

The following table describes briefly the changes in services after configuring a hybrid deployment.

|**Service**|**Before hybrid deployment**|**After hybrid deployment**|**Description**|
|:-----|:-----|:-----|:-----|
|Message transport (inbound and outbound)|Exchange 2010 Client Access server|Exchange 2013 Client Access server or Exchange Online Protection (EOP) included with Microsoft 365 and Office 365|The MX (mail exchanger) record for the domain may remain unchanged or be updated to point to EOP.|
|Outlook Web App public URL|Exchange 2010 Client Access server|Exchange 2013 Client Access server|Exchange 2013 Client Access servers proxy Outlook Web App requests for on-premises mailboxes to Exchange 2010 Client Access servers. Outlook Web App requests for mailboxes hosted on Exchange Online are provided with a link to the Exchange Online Outlook Web App URL.|

## Exchange server software

Exchange 2013 CU1 or greater enables hybrid deployment functionality with the Hybrid Configuration wizard. You can use any Exchange 2013 CU1 or greater media when installing additional Exchange 2013 servers.

For information on how to download the latest version of Exchange Server, see [Updates for Exchange Server](https://docs.microsoft.com/Exchange/new-features/updates).

> [!IMPORTANT]
> You need to license your hybrid server when you configure a hybrid deployment. The HCW is now able to detect and license your designated on-premises Exchange 2010, Exchange 2013, or Exchange 2016 hybrid server for free without going to a separate web site or calling Microsoft support. You can access the HCW [here](https://aka.ms/HybridWizard). Note that the free Exchange Server license is not available for Exchange 2019 hybrid servers.
