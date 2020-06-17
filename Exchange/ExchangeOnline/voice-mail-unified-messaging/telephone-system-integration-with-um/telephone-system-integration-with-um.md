---
localization_priority: Normal
description: 'To successfully deploy Unified Messaging (UM), you must have a good understanding of basic telephony concepts and telephony components. After you understand telephony basics, you can integrate UM into an Exchange organization. Basic concepts and components include the following:'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b8790117-b040-4c84-9d34-005c75088e76
ms.reviewer: 
f1.keywords:
- NOCSH
title: Telephone system integration with UM in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Telephone system integration with UM in Exchange Online

To successfully deploy Unified Messaging (UM), you must have a good understanding of basic telephony concepts and telephony components. After you understand telephony basics, you can integrate UM into an Exchange organization. Basic concepts and components include the following:

- Circuit-switched and packet-switched networks

- Private Branch eXchange (PBX)

- IP PBX

- Voice over Internet Protocol (VoIP)

- VoIP gateways

In an on-premises, hybrid, or Office 365 environment, connecting and configuring the required telephony components is the most complex and important step in successfully deploying UM, with or without Skype for Business Server Enterprise Voice. You'll need to connect and configure VoIP gateways, advanced VoIP gateways, PBXs, IP PBXs, and session border controllers (SBCs) for a traditional telephony network and connect to a telephony network if you'll be using Microsoft Skype for Business Server and UM.

Planning and deploying a new deployment of UM or upgrading a legacy voice mail system can pose challenges for organizations. It requires significant knowledge about VoIP gateways, PBXs, IP PBXs, Microsoft Skype for Business Server, and Unified Messaging. Depending on your technical experience with Exchange and voice mail systems, you might want to obtain the assistance of a Unified Messaging specialist. An Exchange Unified Messaging specialist will help make sure that there's a smooth transition from a legacy or third-party voice mail system to Exchange Unified Messaging.

> [!NOTE]
> Exchange Online UM support for third-party PBX systems via direct connections from customer operated SBCs will end on December 1, 2019. See the Exchange team blog [New date for discontinuation of support for Session Border Controllers in Exchange Online Unified Messaging](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/New-date-for-discontinuation-of-support-for-Session-Border/ba-p/607853) for more information.

## Integrating your telephony network

Unified Messaging requires that you integrate your Exchange Server deployment with your existing telephony network or integrate UM with Microsoft Skype for Business Server for your organization. To successfully deploy and manage UM voice mail you need to make a careful analysis of your existing telephony infrastructure or your Microsoft Skype for Business Server Enterprise Voice deployment and complete the necessary planning steps.

### VoIP gateways

When you're deploying UM in an Exchange organization, you must either install, deploy, and configure a single or multiple VoIP gateways to connect to the PBXs in your telephony network, or install, deploy, and configure Session Initiation Protocol (SIP)-enabled PBXs or IP PBXs.

A VoIP gateway is a third-party hardware device that connects a legacy PBX to your LAN. The VoIP gateway lets the PBX system communicate with the Exchange servers in your organization.

UM relies on the VoIP gateway's ability to translate or convert Time Division Multiplexing (TDM) or circuit-switched based protocols like ISDN and QSIG from a PBX to IP-based or VoIP-based protocols like SIP, Realtime Transport Protocol (RTP), or T.38 for Realtime Fax Transport. The VoIP gateway is integral to the functionality and operation of UM. The VoIP gateway can also connect to PBX systems that use VoIP instead of public switched telephone network (PSTN) circuit-switched protocols.

Choosing the correct VoIP gateway, IP PBX, SIP-enabled PBX, or SBC is only the first part of integrating your telephony network with UM. You must configure those devices to work with UM. In both on-premises and hybrid deployments, you would need to deploy the required Client Access and Mailbox servers, and create and configure all necessary UM components. For Office 365 with hosted voice mail, you're not required to install and configure any server. The components allow you to make the connection from your telephony, circuit-switched network to your IP data network and to enable voice mail for the users in your organization. For details and supported telephony devices, see the following resources:

- [Telephony advisor for Exchange 2013](telephony-advisor-for-exchange-2013.md)

- [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](configuration-notes-for-voip-gateways.md)

- [Configuration notes for supported session border controllers](configuration-notes-for-session-border-controllers.md)

### Microsoft Skype for Business Server

Unified Messaging can use Microsoft Skype for Business Server to combine voice messaging, instant messaging, enhanced presence, audio/video conferencing, and email into a familiar, integrated communications experience. Providing Enterprise Voice features to the users in your organization by integrating UM and Microsoft Skype for Business Server has the following benefits:

- Enhanced presence notifications across a variety of applications that keep users informed of the availability of contacts.

- Integration of instant messaging, voice messaging, conferencing, email, and other communication modes, which enables users to select the most appropriate mode for the task. Users can also switch from one mode to another as needed.

- Availability of communications alternatives from any location where an internet connection is available.

- A smart client (Skype for Business) for telephony, instant messaging, and conferencing.

- Continuity of the user experience across multiple devices.

The Exchange UM routing component handles voice mail routing between Skype for Business Server and Exchange servers to integrate Skype for Business Server with Unified Messaging features. The Exchange UM routing component found in Skype for Business Server also handles rerouting of voice mail over the PSTN if Exchange servers aren't available. If you have Enterprise Voice deployed at branch office sites, and those sites don't have a resilient WAN link to a central site, a Survivable Branch Appliance that you deploy at the branch site provides voice mail for branch users if a WAN link goes down. When the WAN link is unavailable, the Survivable Branch Appliance does the following:

- Reroutes unanswered calls over the PSTN to an Exchange server in the central site.

- Provides the ability for a user to retrieve voice messages over the PSTN.

- Queues missed call notifications, and then uploads them to the Exchange server when the WAN link is restored.

For more information about Microsoft Skype for Business Server, see [Microsoft Skype for Business Server](https://docs.microsoft.com/SkypeForBusiness/skype-for-business-server-2019).

> [!CAUTION]
> When you're integrating Unified Messaging and Skype for Business Server in an on-premises or hybrid deployment, missed call notifications aren't available to users who have a mailbox located on Exchange 2007 or Exchange 2010 Mailbox servers. A missed call notification is generated when a user disconnects before the call is sent to a Mailbox server.
