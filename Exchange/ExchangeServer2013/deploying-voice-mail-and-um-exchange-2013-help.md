---
title: 'Deploying voice mail and UM: Exchange 2013 Help'
TOCTitle: Deploying voice mail and UM
ms:assetid: 3df61b62-a1e4-41fb-969c-319189ae4e42
ms:mtpsurl: https://technet.microsoft.com/library/JJ673519(v=EXCHG.150)
ms:contentKeyID: 49315394
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploying voice mail and UM in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Exchange Unified Messaging (UM) enables you to provide voice mail services to users in your organization. When you deploy Unified Messaging, you must either integrate your Exchange Server deployment with the existing telephony system for your organization or integrate it with Microsoft Lync Server. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and perform the correct planning steps to deploy and manage voice mail in Unified Messaging. If you're integrating Exchange with Lync Server, you must also familiarize yourself with that product.

When you're deploying Unified Messaging, you have multiple options depending on the telephony hardware found in your organization. If you're connecting UM to your telephony network, you may have one of the following telephony configurations in your organization:

  - One or multiple VoIP gateways with one or multiple PBXs

  - One or multiple IP PBXs

  - One or multiple SIP-enabled PBXs

  - Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server 2010 or 2013

> [!WARNING]
> When you're deploying Exchange UM in a hosted or hybrid environment, you must deploy session border controllers (SBCs). SBCs don't enable UM to connect to a telephony network or provide a dial tone for an organization. However, they do connect your on-premises UM deployment to a datacenter using the IP protocol over a public or private WAN.

**Telephony Hardware**: Choosing the correct VoIP gateway, IP PBX, or SBC is only the first step in integrating your telephony network with UM. You must configure those devices to work with UM, deploy the required Client Access and Mailbox servers, and create and configure all needed UM components. These components allow you to connect your circuit-based protocol network to your IP data network and enable voice mail for users in your organization. For details, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

**Microsoft Lync Server**: Unified Messaging can use Microsoft Lync Server to combine voice messaging, instant messaging, enhanced presence, audio/video conferencing, and email into a familiar, integrated communications experience. Integrating UM and Lync Server has the following benefits:

  - Enhanced presence notifications across a variety of applications that keep users informed of the availability of contacts.

  - Integration of instant messaging, voice messaging, conferencing, email, and other communication methods to enable users to select the most appropriate method for the task. Users can also switch from one method to another as needed.

  - Availability of communications alternatives from any location where an Internet connection is available.

  - A smart client (Microsoft Lync) for telephony, instant messaging, and conferencing.

  - Continuity of the user experience across multiple devices.

For more information about Lync Server, see [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).

## Deployment steps

Many deployment options are available for Unified Messaging. Each option has several steps in common that are required to create a scalable and highly available system to support large numbers of users. These steps are as follows:

1. Deploy and configure your telephony components or Microsoft Lync Server with Unified Messaging.

2. Verify that you've correctly installed the Client Access and Mailbox servers that are required by Unified Messaging.

    > [!WARNING]
    > You must deploy at least one Exchange 2013 Mailbox server in your organization before you configure the VoIP gateways or IP PBXs to send UM SIP and RTP traffic to the Exchange 2013 Client Access servers.

3. Create and configure the required Unified Messaging components including UM dial plans, UM IP gateways, UM hunt groups, and UM mailbox policies.

4. Perform post-deployment tasks including deploying certificates for mutual TLS, creating UM auto attendants, and client features.

For details about deploying Unified Messaging, see [Deploy Exchange 2013 UM](deploy-exchange-2013-um-exchange-2013-help.md).

If you're integrating your Unified Messaging environment with Microsoft Lync Server, there are additional planning considerations. For details, see [Deploying Exchange 2013 UM and Lync Server overview](deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help.md).
