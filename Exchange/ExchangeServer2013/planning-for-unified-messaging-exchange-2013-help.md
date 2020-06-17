---
title: 'Planning for Unified Messaging: Exchange 2013 Help'
TOCTitle: Planning for Unified Messaging
ms:assetid: 942788b1-b19d-40b3-a52e-2e1fef8df3f9
ms:mtpsurl: https://technet.microsoft.com/library/JJ674306(v=EXCHG.150)
ms:contentKeyID: 49319925
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Planning for Unified Messaging in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you plan your Unified Messaging (UM) deployment, there are many factors that you must consider to be able to successfully deploy UM. You must understand the different elements of Unified Messaging and each component and feature so that you can plan your Unified Messaging infrastructure and deployment appropriately. Allocating time to plan and work through these issues will help prevent problems when you deploy Unified Messaging in your organization.

You may be deploying Unified Messaging in a new Exchange organization or upgrading from a legacy or third-party voice mail solution. If you're upgrading, you need to decide whether to convert the devices that are accepting telephony circuit-based protocols to a data network using IP or whether to deploy an Enterprise Voice solution like Microsoft Lync Server. This is just the first step in preparing yourself to understand and deploy UM for your organization.

## Planning your voice mail system

UM provides voice mail, fax, and email messaging in one store that can be accessed from a telephone, a user's computer, or a mobile device. Users can access voice messages, email, calendar information, and personal contacts that are located in their Exchange mailbox from email clients such as Outlook and Outlook Web App.

Client Access servers running the Microsoft Exchange Unified Messaging Call Router service and Mailbox servers running the Microsoft Exchange Unified Messaging service are designed to provide voice mail features for users in your organization.

Mailbox servers rely on Client Access servers to forward SIP traffic from incoming calls and then establish a connection with a VoIP gateway, IP PBX, or Session Border Controller (SBC) and accept the RTP/SRTP media traffic. All voice mail and fax messages are submitted from the Microsoft Exchange Unified Messaging service on a Mailbox server to be delivered to the user's mailbox. For a user to use the voice mail features with Unified Messaging, they must have an Exchange mailbox.

## Planning your UM deployment

Generally, the simpler the Unified Messaging topology, the easier UM is to deploy and maintain. Install as few Client Access and Mailbox servers and create as few Unified Messaging components (like UM dial plans, auto attendants, and UM mailbox policies) as you need to support your business and organizational goals. Large enterprises with complex network and telephony environments, multiple business units, or other complexities will require more planning than smaller organizations with relatively straightforward Unified Messaging needs.

The following are some of the areas that you should consider when planning for Unified Messaging in your organization:

- **Organizational requirements**: Evaluate your business needs, the usefulness of deploying a voice mail system, your physical network and business topology, and security requirements for your organization.

- **Telephony requirements**: Review your existing telephony, circuit-switched network, and voice mail system.

- **Network requirements**: Analyze your network topology, your current packet-switched IP network design including your LAN and WAN connectivity points, and devices.

- **Active Directory (directory service)**: Inspect your current implementation and design and think about how to integrate UM.

- **Deployment model**: Decide whether you want to have a hybrid, online-only, or on-premises UM deployment.

- **Exchange requirements**: Determine the following:

  - How many users will be using voice mail.

  - Which UM features and services you want to deploy, such as concurrent calls, internal and external access for users, incoming faxing, Voice Mail Preview, and so on.

  - The number of Client Access and Mailbox servers you'll need to deploy.

  - The storage requirements and quotas for voice mail users.

  - The best design for high availability and site resiliency. This includes UM system requirements, providing a highly available and scalable UM deployment, and system hardware requirements to ensure performance.

- **Integration with telephony components and devices**: Decide whether to use traditional telephony equipment or Microsoft Lync Server. Consider where to place VoIP gateways, telephony equipment, and Client Access and Mailbox servers, and whether you want to enable Enterprise Voice in your organization.

## Connecting your telephony network

Unified Messaging requires that you integrate your Exchange Server deployment with your existing telephony system or integrate it with Microsoft Lync Server. You need to make a careful analysis of your existing telephony infrastructure and Microsoft Lync Server, and then follow the correct planning steps so you can deploy and manage UM voice mail successfully.

**VoIP gateways**: Choosing the correct VoIP gateway, IP PBX, SIP-enabled PBX, or SBC is just the first step in integrating your telephony network with UM. You must configure those devices to work with UM, deploy the required Client Access and Mailbox servers, and create and configure all necessary UM components. These components allow you to make the connection from your circuit-based protocol network to your IP data network and enable voice mail for your users.

**Microsoft Lync Server**: Unified Messaging can use Microsoft Lync Server to combine voice messaging, instant messaging, enhanced presence, audio/video conferencing, and email into a familiar, integrated communications experience. Integrating UM and Microsoft Lync Server has the following benefits:

- Enhanced presence notifications across a variety of applications that keep users informed of the availability of contacts.

- Integration of instant messaging, voice messaging, conferencing, email, and other communication methods, which enables users to select the most appropriate method for the task. Users can also switch from one method to another as needed.

- Availability of communications alternatives from any location where an Internet connection is available.

- A smart client (Microsoft Lync) for telephony, instant messaging, and conferencing.

- Continuity of the user experience across multiple devices.

For more information about Microsoft Lync Server, see [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013).

> [!NOTE]
> Planning and deploying Unified Messaging can pose certain challenges. Depending on your technical experience with Exchange and voice mail systems, you might want to obtain the assistance of a Unified Messaging specialist. An Exchange Unified Messaging specialist will help make sure that there's a smooth transition from a legacy or third-party voice mail system to Exchange Unified Messaging. Performing a new deployment or upgrading a legacy voice mail system requires significant knowledge about VoIP gateways, PBXs, and Unified Messaging. To contact a Unified Messaging specialist, see the [Microsoft solution providers](https://www.microsoft.com/solution-providers/) page.

## Deployment steps

Many deployment options are available for Unified Messaging. Each option has several steps in common that are required to create a scalable and highly-available system to support large numbers of users. These steps are as follows:

1. Deploy and configure your telephony components or Microsoft Lync Server with Unified Messaging.

2. Verify that you've correctly installed the Client Access and Mailbox servers that are required by Unified Messaging.

3. Create and configure the required Unified Messaging components, including UM dial plans, UM IP gateways, UM hunt groups, and UM mailbox policies.

4. Perform post-deployment tasks, including obtaining certificates for mutual TLS, creating UM auto attendants, and configuring faxing.

For details about deploying Unified Messaging, see [Deploy Exchange 2013 UM](deploy-exchange-2013-um-exchange-2013-help.md).

If you're integrating your Unified Messaging environment with Microsoft Lync Server, there are additional planning considerations. For details, see [Deploying Exchange 2013 UM and Lync Server overview](deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help.md).
