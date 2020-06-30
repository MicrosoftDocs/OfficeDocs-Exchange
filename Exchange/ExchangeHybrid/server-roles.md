---
title: "Server roles in Exchange hybrid deployments"
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
ms.assetid: 7a7eaf17-d2b0-4d62-90a2-45a0d2faca54
ms.reviewer:
description: "You can configure hybrid deployments based on Exchange 2013 and Exchange 2016. The roles that need to be configured to support a hybrid deployment depend on the version of Exchange you're using."
---

# Server roles in Exchange hybrid deployments

You can configure hybrid deployments based on Exchange 2013 and Exchange 2016. The roles that need to be configured to support a hybrid deployment depend on the version of Exchange you're using.

## Exchange 2016 hybrid deployment

When configuring a hybrid deployment in an Exchange 2016 organization, you don't have to install any additional Exchange servers in your existing Exchange organization. Your Mailbox servers coordinate communications between your existing Exchange 2016 organization and the Exchange Online organization. This communication includes message transport and messaging features between the on-premises and Exchange Online organizations. We highly recommend installing more than one Exchange server in your on-premises organization to help increase reliability and availability of hybrid deployment features.

Exchange 2016 only has one required server role, the Mailbox role. In addition to hosting the on-premises recipient mailboxes, the Mailbox role performs all of the functions necessary to support a hybrid deployment with Exchange Online. This includes handling secure mail messages between the on-premises and Exchange Online organizations, as well as handling transport rules, journaling policies, and message delivery to user mailboxes in a hybrid deployment. All client connectivity and organization relationship features, such as free/busy sharing, are also handled by the Mailbox server.

Learn more about Exchange 2016 capacity planning at [Sizing Exchange 2016 Deployments](https://techcommunity.microsoft.com/t5/exchange-team-blog/ask-the-perf-guy-sizing-exchange-2016-deployments/ba-p/603970).

## Exchange 2013 hybrid deployment

When configuring a hybrid deployment in an Exchange 2013 organization, you don't have to install any additional Exchange servers in your existing Exchange organization. Your Client Access and Mailbox servers coordinate communications between your existing Exchange 2013 organization and the Exchange Online organization. This communication includes message transport and messaging features between the on-premises and Exchange Online organizations. We highly recommend installing more than one Exchange server in your on-premises organization to help increase reliability and availability of hybrid deployment features.

Here is a quick overview of the Exchange 2013 server roles in a hybrid deployment:

- **Client Access server role**: The Client Access server role continues to provide essentially the same functionality typically provided by Client Access servers in your Exchange 2013 organization with a few additions required to support a hybrid deployment. The Client Access server also handles all secure mail messages sent between the on-premises and the Exchange Online organizations, as well as handling transport rules, journaling policies, and message delivery to user mailboxes in a hybrid deployment. By default, a dedicated Receive connector is configured on the Client Access server to support secure hybrid mail transport. All client connectivity, including Outlook client access, Outlook Web App, and Outlook Anywhere goes through the Client Access server role. Organization relationship features between the on-premises and Exchange Online organizations, such as free/busy sharing, are also handled by the Client Access server role.

    Learn more at [Client Access server](https://docs.microsoft.com/exchange/client-access-server-exchange-2013-help).

- **Mailbox server role**: The Mailbox server role hosts the on-premises recipient mailboxes and communicates with the Exchange Online organization by proxy via the on-premises Client Access server. By default, a dedicated Send connector is configured on the Mailbox server role to support secure hybrid mail transport.

    Learn more at [Mailbox Server](https://docs.microsoft.com/exchange/mailbox-server-exchange-2013-help).

Depending on the hybrid deployment configuration that you want, an Exchange 2013 server requires one or both of the server roles to be installed on it:

- **Single Exchange server**: If you choose to install a single Exchange server in your on-premises organization, you'll need to install both the Client Access and Mailbox server roles on the single server.

- **More than one Exchange server**: If you choose to install more than one Exchange server in your on-premises organization, you can install the server roles on separate servers in your on-premises organization. For example, you could install one Exchange server that has the Mailbox and Client Access roles installed and also install another Exchange server that has only the Client Access server role installed. However, the best practice and recommended server configuration is to install both the Client Access and Mailbox server roles on each server deployed in your on-premises organization.

Learn more about Exchange 2013 capacity planning at [Understanding Multiple Server Role Configurations in Capacity Planning](https://docs.microsoft.com/exchange/exchange-2013-sizing-and-capacity-planning-exchange-2013-help.md).

## Exchange server functionality in hybrid deployments

Exchange servers provide several important functions for your on-premises organization in a hybrid deployment:

- **Federation**: Exchange servers enable you to create a federation trust for your on-premises organization with the Microsoft Federation Gateway. The Microsoft Federation Gateway is a free, cloud-based service offered by Microsoft that acts as the trust broker between your on-premises organization and the Microsoft 365 or Office 365 organization. Federation is a requirement for creating an organization relationship between the on-premises and the Exchange Online organizations.

    Learn more at [Understanding Federation](https://docs.microsoft.com/exchange/federation-exchange-2013-help).

- **Organization relationships**: Exchange 2013 servers with the Client Access role and Exchange 2016 servers with the Mailbox role enable the creation of organization relationships between the on-premises and Exchange Online organizations. Organization relationships are required for many other services in a hybrid deployment, including calendar free/busy information sharing, message tracking, and mailbox moves between the on-premises and Exchange Online organizations.

    Learn more at [Sharing](https://docs.microsoft.com/exchange/sharing-exchange-2013-help).

- **Message transport**: Exchange servers with the Client Access and Mailbox server roles are responsible for message transport in a hybrid deployment. Using Send and Receive connectors, they serve as the connection endpoints for incoming external messages and also provide outbound message delivery to the Internet and the Exchange Online organization.

    Learn more at [Transport options in Exchange hybrid deployments](transport-options.md).

- **Message transport security**: Exchange servers with the Client Access and Mailbox server roles help to secure message communication between the on-premises and Exchange Online organizations by using the Domain Security functionality in Exchange. Security can be increased by using mutual transport layer security authentication and encryption for message communications.

    Learn more at [Understanding Domain Security](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb124392(v=exchg.141)).

- **Outlook on the web (known as Outlook Web App in Exchange 2013)**: Exchange 2013 servers with the Client Access role and Exchange 2016 servers with the Mailbox role support configuring a single URL endpoint for external connections to on-premises and Exchange Online mailboxes. For on-premises mailboxes, Exchange servers are configured to service Outlook on the web requests. For Exchange Online organization mailboxes, Exchange servers are configured to automatically display a link to the Outlook on the web endpoint on the Exchange Online organization.

    Learn more at [Outlook Web App](https://docs.microsoft.com/exchange/outlook-web-app-exchange-2013-help) and [Outlook on the web in Exchange Server](https://docs.microsoft.com/exchange/clients/outlook-on-the-web/outlook-on-the-web).
