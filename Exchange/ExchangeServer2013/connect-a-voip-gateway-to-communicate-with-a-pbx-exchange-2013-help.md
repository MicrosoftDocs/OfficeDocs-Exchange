---
title: 'Connect a VoIP gateway to communicate with a PBX: Exchange 2013 Help'
TOCTitle: Connect a VoIP gateway to communicate with a PBX
ms:assetid: 76bcdc54-3ec2-408a-bdbe-37826580dd62
ms:mtpsurl: https://technet.microsoft.com/library/Aa998872(v=EXCHG.150)
ms:contentKeyID: 49315440
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Connect a VoIP gateway to communicate with a PBX in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you configure your telephony and data networks for Unified Messaging (UM) in Microsoft Exchange Server 2013, you must configure the VoIP gateways so they communicate with Client Access servers running the Microsoft Exchange Unified Messaging Call Router service and Mailbox servers running the Microsoft Exchange Unified Messaging service. You must also configure the VoIP gateways to communicate with the Private Branch eXchanges (PBXs) in your organization. You can use the information and links in this topic to configure a VoIP gateway to communicate with a PBX.

## Configuring a VoIP gateway

When you configure a VoIP gateway, you must consider whether the VoIP gateway device is analog, digital, or analog and digital. If the VoIP gateway interface that connects to a PBX is analog, you must correctly configure the appropriate settings to enable the VoIP gateway to communicate with a PBX. If the VoIP gateway interface that connects to a PBX is digital, it may require no additional configuration to enable the digital interface to communicate with a PBX.

The following suggested resources in the Exchange TechCenter provide information that can help you correctly configure your VoIP gateways and PBXs:

  - **Supported VoIP gateway, IP PBX, and PBX documentation**: [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013) includes configuration files and setup information that you can use when you configure VoIP gateways and PBXs.

  - **Configuration and technical notes**: [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways) includes configuration files and setup information that you can use when you configure VoIP gateways and PBXs.

  - **Configuring an AudioCodes-based VoIP gateway**  The [Microsoft Exchange Server Resource page](https://www.audiocodes.com/solutions/microsoft/exchange-server) provides the latest support and configuration information to help you configure AudioCodes-based VoIP gateways for use with Unified Messaging.

  - **Configuring a Dialogic-based VoIP gateway**: The [Dialogic website](https://www.dialogic.com/) provides the latest support and configuration information for Dialogic-based VoIP gateways.

We recommend that all customers who plan to deploy Unified Messaging obtain the assistance of a Unified Messaging specialist. An Exchange Unified Messaging specialist will help make sure that there's a smooth upgrade to Unified Messaging from a legacy or third-party voice mail system and help you plan and deploy a new voice mail system with Exchange Unified Messaging. Deploying a new voice mail system or upgrading a legacy voice one requires significant knowledge about VoIP gateways, PBXs, and Unified Messaging. To contact a Unified Messaging specialist, see the [Microsoft solution providers](https://www.microsoft.com/solution-providers/) page.

## For more information

[Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways)

[Connect UM to a supported VoIP gateway](connect-um-to-a-supported-voip-gateway-exchange-2013-help.md)
