---
title: 'Connect a VoIP gateway, IP PBX, or session border controller to UM: Exchange 2013 Help'
TOCTitle: Connect a VoIP gateway, IP PBX, or session border controller to UM
ms:assetid: a7cecf59-b93a-413b-bb88-29f2669ef2cf
ms:mtpsurl: https://technet.microsoft.com/library/Bb124084(v=EXCHG.150)
ms:contentKeyID: 49315480
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Connect a VoIP gateway, IP PBX, or session border controller to UM in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You must configure the Voice over IP (VoIP) gateways and IP Private Branch eXchanges (PBXs) correctly when you deploy Unified Messaging (UM) for your organization. If you're deploying UM in a hybrid environment, you'll also need to correctly configure your session border controllers (SBCs). To do this, you need to configure the interface or interfaces of the VoIP gateways, IP PBXs, and SBCs to communicate with Client Access servers running the Microsoft Exchange Unified Messaging Call Router service and Mailbox servers running the Microsoft Exchange Unified Messaging service.

> [!IMPORTANT]
> When you perform administrative tasks for a VoIP gateway, IP PBX, or SBC using a web browser, the HTTP requests sent over the network aren't encrypted. To increase the level of security for the VoIP gateways, IP PBXs, or SBCs on your network, use Internet Protocol security (IPsec) or Secure Sockets Layer (SSL) to help protect the administrative credentials and data transmitted over the network. We also recommend that you use a strong authentication mechanism and complex administrative passwords to protect the administrative credentials for the device.

## Telephony IP device interfaces

There are several types of ports or interfaces that you must configure to enable communication between a PBX, VoIP gateway, IP PBX, or SBC and the Client Access and Mailbox servers on your network. When you configure a VoIP gateway, IP PBX, or SBC, you must consider whether the device is analog, digital, or analog and digital.

If the VoIP gateway interface that connects to a PBX is analog, you must correctly configure the appropriate settings to enable the VoIP gateway to communicate with your Client Access and Mailbox servers. For both IP PBXs and SBCs, you must also correctly configure the IP interfaces to enable these devices to communicate with Client Access and Mailbox servers. All of these devices have different types of connections or ports that are available depending on the device model and vendor.

To enable communication with the Client Access and Mailbox servers on your network:

  - For VoIP gateways, you must configure the telephony interfaces to communicate with your PBXs, and you must configure the IP interface for the device.

  - For IP PBXs, you must configure the circuit-based and network or IP-based connections.

  - For SBCs, you must configure both IP interfaces, one for your network and the other interface that connects over the Internet or a dedicated WAN connection.

The following is a list of resources found on the Exchange TechCenter that provide information that can help you correctly configure your VoIP gateways, IP PBXs, and SBCs:

  - **Supported IP gateway, IP PBX, and PBX documentation**: [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013) includes configuration files and setup information that you can use when you configure VoIP gateways, IP PBXs, PBXs, and SBCs.

  - **Configuration and technical notes**: [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways) includes configuration files and setup information that you can use when you configure VoIP gateways, IP PBXs, and PBXs.

  - **Configuration notes for Exchange UM online**: [Configuration notes for supported session border controllers](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-session-border-controllers) includes configuration files and setup information that you can use when you configure SBCs.

Unified Messaging specialists are available to assist you in configuring your telephony and IP-based network devices. A Unified Messaging specialist can help make sure that there's a smooth transition to Unified Messaging from a legacy or third-party voice mail system or help you plan and deploy a new voice mail system with Exchange Unified Messaging. Deploying a new voice mail system or upgrading a legacy one requires significant knowledge about VoIP gateways, IP PBXs, PBXs, and Unified Messaging. To contact a Unified Messaging specialist, see the [Microsoft solution providers](https://www.microsoft.com/solution-providers/) page.

After you configure a VoIP gateway, IP PBX, or SBC IP interface, you must create and configure a UM IP gateway to represent each device that you've deployed. For more information about how to create a UM IP gateway, see [Create a UM IP gateway](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-ip-gateway).

After you create a UM IP gateway, the Client Access and Mailbox servers associated with the UM IP gateway send a SIP OPTIONS request to the VoIP gateway, IP PBX, or SBC to ensure that it's responsive. If the VoIP gateway, IP PBX, or SBC doesn't respond, the Mailbox server will log an event with ID 1088 stating that the request failed. To resolve this issue, make sure that the VoIP gateway, IP PBX, or SBC is available and online and the Unified Messaging configuration is correct.

A Client Access server and a Mailbox server will communicate only with a VoIP gateway, IP PBX, or SBC that's listed as a trusted Session Initiation Protocol (SIP) peer. An event with ID 1175 will be logged when multiple DNS hosts share the same IP address. This event may occur if you've configured your DNS zones with FQDNs for the VoIP gateways on your network. Unified Messaging protects against unauthorized requests by retrieving the internal URL of the Unified Messaging Web Services virtual directory that's located on the Mailbox server and then using the URL to build the list of FQDNs for the trusted SIP peers. After two FQDNs are resolved to the same IP address, this event will be logged.

> [!NOTE]
> You must restart the Microsoft&nbsp;Exchange Unified Messaging service if a VoIP gateway, IP PBX, or SBC is configured to have an FQDN and the DNS record of the VoIP gateway, IP PBX, or SBC is changed after the service has been started. If you don't restart the service, the Mailbox server won't be able to locate the VoIP gateway, IP PBX, or SBC. This occurs because a Mailbox server maintains a cache for all VoIP gateways, IP PBXs, or SBCs in memory, and DNS resolution is performed only when the service is restarted or when the configuration of a VoIP gateway, IP PBX, or SBC has changed.
