---
title: 'Send connectors: Exchange 2013 Help'
TOCTitle: Send connectors
ms:assetid: 6aa19a12-c7b2-4eac-a8dc-9a4d26919ac5
ms:mtpsurl: https://technet.microsoft.com/library/Aa998662(v=EXCHG.150)
ms:contentKeyID: 49289286
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Send connectors

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, a Send connector controls the flow of outbound messages to the receiving server. They are configured on Mailbox servers running the Transport service. Most commonly, you configure a Send connector to send outbound email messages to a smart host or directly to their recipient, using DNS.

Exchange 2013 Mailbox servers running the Transport service require Send connectors to deliver messages to the next hop on the way to their destination. Send connectors that are created on Mailbox servers are stored in Active Directory and are available to all Mailbox servers running the Transport service in the organization.

> [!IMPORTANT]
> When you deploy Exchange 2013, outbound mail flow cannot occur until you configure a Send connector to route outbound mail to the Internet. For more information, see <A href="create-a-send-connector-for-email-sent-to-the-internet-exchange-2013-help.md">Create a Send connector for email sent to the Internet</A>.

## Selecting the Type for a Send connector

Typically you create a Send connector in the **Mail flow** section of the Exchange admin center (EAC). When you create a new Send connector, you choose an available **Type** appropriate to your connection scenario. The type determines the default permission sets that are assigned on the connector and grants those permissions to trusted security principals. Security principals include users, computers, and security groups.

Procedures that explain specific **Type** selections include [Create a Send connector to route outbound email through a smart host](create-a-send-connector-to-route-outbound-email-through-a-smart-host-exchange-2013-help.md) and [Create a Send connector to send email to a partner, with Transport Layer Security (TLS) applied](create-a-send-connector-to-send-email-to-a-partner-with-transport-layer-security-tls-applied-exchange-2013-help.md).

In you prefer using the Exchange Management Shell to the EAC, you can create a Send connector and specify a type by using the [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/New-SendConnector) cmdlet.

## New Send connector features in Exchange 2013

With the relationship between the Front End Transport service on Client Access servers and the Transport service on Mailbox servers in Exchange 2013 comes new behavior for Send connectors. Firstly, you can set a Send connector in the Transport service of a Mailbox server to route outbound mail through a Front End transport server in the local Active Directory site, by means of the *FrontEndProxyEnabled* parameter of the [Set-SendConnector](https://docs.microsoft.com/powershell/module/exchange/Set-SendConnector) cmdlet, thus consolidating how email is routed from the Transport service. [Mail routing](mail-routing-exchange-2013-help.md) provides more information about transport services, routing behavior, and destinations in Exchange 2013. [Mail flow](mail-flow-exchange-2013-help.md) provides an overview of the transport pipeline and descriptions of each transport service.

The *IsCoexistenceConnector* parameter has been deprecated. In most cases, when you want to configure a hybrid environment, where a portion of your mailboxes are hosted on-premises and a portion are hosted in the cloud, we recommend that you use the Hybrid Configuration Wizard.

*LinkedReceiveConnector* has been deprecated. This parameter was used to create connectors that could route messages to a third-party anti-spam service, for instance. Now, in most cases, you route mail to your anti-spam service by means of your MX record, and the linked-connector behavior is not necessary.

The default maximum message size, specified by the *MaxMessageSize* parameter, has been increased from 10MB to 25MB. [Set-SendConnector](https://docs.microsoft.com/powershell/module/exchange/Set-SendConnector) provides more information on how to set parameters on a Send connector.

*TlsCertificateName* has been added. It is used to authenticate the local certificate to be used for outbound connections and minimize the risk of fraudulent certificates.
