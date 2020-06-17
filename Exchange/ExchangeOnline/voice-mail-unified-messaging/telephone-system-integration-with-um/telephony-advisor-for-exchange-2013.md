---
localization_priority: Normal
description: Unified Messaging (UM) requires that you integrate Microsoft Exchange with the existing telephony system for your organization. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and to perform the correct planning steps to deploy Unified Messaging.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e9f760f2-5901-4ed2-95a5-724555cc700e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Telephony advisor for Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Telephony advisor for Exchange Online

Unified Messaging (UM) requires that you integrate Microsoft Exchange with the existing telephony system for your organization. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and to perform the correct planning steps to deploy Unified Messaging.

The planning phase can be a significant challenge to Exchange administrators who have little or no experience with a telephony network. To help address this challenge, see the following section **Resources to help with your UM deployment**.

The other sections in this topic cover the supported VoIP gateways for Unified Messaging, how to determine whether your PBX is supported using a specific VoIP gateway model or manufacturer, whether your IP PBX is supported using a direct SIP connection, and supported session border controllers (SBCs) for Exchange Online UM.

## Resources to help with your UM deployment

It's challenging to create guidelines for deploying telephony networks. They can be very different from one another because they can include VoIP gateways, IP PBXs, and PBXs with different configuration settings, firmware, and requirements. However, several resources are available to help you successfully deploy Unified Messaging:

- **Unified Messaging specialists**: UM specialists are systems integrators who have received technical training about Exchange Unified Messaging conducted by the Exchange engineering team. To help ensure a smooth transition to Unified Messaging from legacy voice mail systems, Microsoft recommends that all customers engage a UM specialist. To contact a Unified Messaging specialist, see the [Microsoft solution providers](https://www.microsoft.com/solution-providers/) page.

- **Configuration Notes for Supported VoIP Gateways, IP PBXs and PBXs**: These configuration notes contain settings and other information that's very useful when you're configuring VoIP gateways, IP PBXs, and PBXs to communicate with the Unified Messaging servers that are on your network. For more information, see [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](configuration-notes-for-voip-gateways.md).

- **Configuration Notes for Supported Session Border Controllers**: These configuration notes contain settings and other information that's very useful when you're configuring session border controllers (SBCs) to communicate with the Unified Messaging servers in hybrid and Exchange Online UM deployments. For more information, see [Configuration notes for supported session border controllers](configuration-notes-for-session-border-controllers.md).

    > [!NOTE]
    > Exchange Online UM support for third-party PBX systems via direct connections from customer operated SBCs will end in December 2019. See the Exchange team blog [New date for discontinuation of support for Session Border Controllers in Exchange Online Unified Messaging](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/New-date-for-discontinuation-of-support-for-Session-Border/ba-p/607853) for more information.

Before you engage a Unified Messaging specialist, you should be able to answer key questions that they'll ask. Having the answers to the following questions will help make the conversation between you and the UM specialist productive:

- How many existing telephone or voice mail users, or both, are in your organization?

- How many users do you intend to provide with Unified Messaging?

- Which PBX or PBXs do you intend to use for integration with Unified Messaging?

- How many PBXs does your organization have? Specify the vendors, types (circuit- or IP-based), models, and firmware versions.

- Are the PBXs networked, and are they centralized or located in multiple locations?

- What voice mail system or systems does your organization currently use? Specify the vendors, types, models, and firmware versions.

- How are the voice mail systems integrated into your PBXs (Analog, T1/E1, PRI, Digital set emulation, VoIP, other)?

- Are you currently using voice networking?

- What type of fax system or systems does your organization use, and does the fax system or systems support inbound fax routing to Exchange?

- Does your organization use automated attendants?

- Do you need support for phone-only users, that is, users who won't have email access?

## Supported VoIP gateways

Integrating Unified Messaging with PBXs requires you to use one or more VoIP gateways to translate the circuit-switched protocols that are used by TDM-based PBXs to IP-based, packet-switched protocols that are used by Unified Messaging. VoIP gateway vendors with several models of VoIP and media gateways have been tested and are supported for Unified Messaging.

Interoperability testing of Unified Messaging with VoIP gateways, IP PBXs, and SBCs is now integrated with the Microsoft Unified Communications Open Interoperability Program. For more information, see [Microsoft Unified Communications Open Interoperability Program](https://docs.microsoft.com/SkypeForBusiness/lync-cert/qualified-lync-apps).

The [Microsoft Unified Communications Open Interoperability Program](https://docs.microsoft.com/SkypeForBusiness/lync-cert/qualified-lync-apps) qualification program for VoIP gateways, IP PBXs, and advanced VoIP gateways ensures that customers have a seamless setup and support experience when they're using qualified telephony VoIP gateways and IP PBXs with Microsoft Unified Communications software. Only products that meet rigorous and extensive testing requirements and conform to the specifications and test plans receive qualification.

For details about configuring supported VoIP gateways, IP PBXs, PBXs, and SBCs, see one of the following resources:

- [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](configuration-notes-for-voip-gateways.md)

- [Configuration notes for supported session border controllers](configuration-notes-for-session-border-controllers.md)

Interoperability was verified for the following VoIP gateway vendors:

- AudioCodes

- Dialogic

- The following table shows the VoIP gateway vendor, the VoIP gateway model, and the protocols that are supported by each model.

**Supported VoIP gateways for Unified Messaging**

|**Vendor**|**Model**|**Supported protocols**|
|:-----|:-----|:-----|
|AudioCodes|MediaPack 114/8 FXO|Analog with In-Band DTMF <br/><br/> Analog with SMDI|
|AudioCodes|Mediant 1000|Analog with In-Band DTMF <br/><br/> Analog with SMDI <br/><br/> BRI Q.SIG <br/><br/> T1/E1 Q.SIG <br/><br/> IP-to-IP|
|AudioCodes|Mediant 2000|T1/E1 CAS <br/><br/> T1/E1 Q.SIG <br/><br/> IP-to-IP|
|Dialogic|DMG1000PBXDNIW|Digital Set Emulation|
|Dialogic|DMG1000LSW|Analog with In-Band DTMF <br/><br/> Analog with SMDI|
|Dialogic|DMG2000|T1 CAS <br/><br/> T1/E1 Q.SIG|
|Dialogic|DMG3000|BRI Q.SIG|
|NET|VX1200|T1 Q.SIG|
|Sonus|SBC 1000/2000 2.2.1 or later|TDM Signaling (ISDN): AT&T 4ESS/5ESS, Nortel DMS- 100, Euro ISDN (ETSI 300-102), QSIG, NTT InsNet (Japan), ANSI National ISDN-2 (NI-2) <br/><br/> TDM Signaling (CAS): T1 CAS (E&M, Loop start); E1 CAS (R2)|
|Quintum|Tenor DX Series|T1 Q.SIG|

## Supported PBXs when using an AudioCodes VoIP gateway

The following table shows the PBXs that are supported using AudioCodes VoIP gateways, including MediaPack-114 FXO, MediaPack-118 FXO, and Mediant 2000.

**PBXs supported with an AudioCodes VoIP gateway**

|**PBX manufacturer**|**PBX model/type**|**AudioCodes model "x" - replace with 4 or 8 per need "y" - replace with 1, 2, 4, 8 or 16 per need**|
|:-----|:-----|:-----|
|Alcatel|OmniPCX 4400|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant2000/ySpans/SIP|
|Aastra|M1000, M2000|Mediant2000/ySpans/SIP|
|Avaya|Definity G3|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Avaya|Magix/Merlin|MediaPack 11x/FXO/AC/SIP-0|
|Avaya|S8300|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Avaya|S8700|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Avaya|IP Office|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant2000/ySpans/SIP|
|Cisco|CallManager 4.x|Mediant1000/IP-to-IP <br/><br/> Mediant2000/IP-to-IP|
|NEC|Electra Elite|MediaPack 11x/FXO/AC/SIP-0|
|NEC|NEAX2400|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant2000/ySpans/SIP/RS232|
|NeXspan|S|MediaPack 11x/FXO/AC/SIP-0|
|Nortel|Communication Server-1000M, 1000S, 1000E|Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Nortel|Meridian 11c, 51c, 61c, 81c|Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Panasonic|KX-TES824, KX-TEA308|MediaPack 11x/FXO/AC/SIP-0|
|Panasonic|KX-TDA30, KX-TDA100, KX-TDA200, KX-TDA600|MediaPack 11x/FXO/AC/SIP-0|
|Shortel|IP Telephony System|MediaPack 11x/FXO/AC/SIP-0|
|Siemens|HiCom 150E|MediaPack 11x/FXO/AC/SIP-0|
|Siemens|HiPath 3550|MediaPack 11x/FXO/AC/SIP-0|
|Siemens|HiPath 4000|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|
|Tadiran Telecom|Coral Flexicom, Coral IPX|MediaPack 11x/FXO/AC/SIP-0 <br/><br/> Mediant1000/ySpans/SIP <br/><br/> Mediant2000/ySpans/SIP|

## Supported PBXs when using a Dialogic VoIP gateway

Each Dialogic VoIP gateway model supports different PBXs. The following tables show the PBX manufacturer and model and which Dialogic VoIP gateway can be used. Each VoIP gateway uses different signaling methods, densities, and protocols.

### PBXs supported when using a DMG1000 series Media Gateway

The following table shows the PBXs that are supported with the low-density Dialogic Media Gateway (DMG1000). However, when an analog DMG1000 is used, supplemental signaling (RS232 SMDI, MD110, MCI protocols, or Inband DTMF signaling) is required.

**PBXs supported when using a low-density Dialogic DMG1000 series VoIP gateway**

|**PBX manufacturer**|**PBX model/type**|**DMG model and additional signaling**|
|:-----|:-----|:-----|
|Aastra|Aastra MD110 (formerly Ericsson MD110)|DMG1008LSW <br/><br/> Analog connectivity using the MD110 RS232 protocol|
|Alcatel|Omni PCX 4400|DMG1008LSW|
|Avaya|Definity G3 S8100, S8300, S8700, and S8710 (Communications Mgr SW V2.0 or later versions)|DMG1008DNIW|
|Intercom||DMG1008LSW <br/><br/> Analog connectivity using SMDI serial protocol|
|Mitel|SX-200D, SX-200 Light, SX-2000 Light, SX-2000 S, SX-2000 VS, SX-200 ICP|DMG1008MTLDNIW|
|NEC|2000, 2400, 2400 IPX|DMG1008DNIW|
|Nortel|Meridian 1 - Option 11, 21, 21A, 51, 61, 71, and 81 <br/><br/> Meridian SL1 - Generic X11, Release 15 or later versions <br/><br/> Nortel Communication Server - 1000M, 1000S, 1000E with V3.0 or later versions|DMG1008DNIW|
|Nortel|SL 100|DMG1008LSW <br/><br/> Analog connectivity using SMDI serial protocol|
|Siemens|HiCom 300E CS|DMG1008DNIW|
|Siemens|HiCom 300E (European)|DMG1008LSW <br/><br/> Analog connectivity using Inband DTMF signaling|
|Siemens/ROLM|8000 (SW release 80003 or later versions) <br/><br/> 9000 (All versions) <br/><br/> 9751 (All versions of SW release 9005) <br/><br/> 9751 (SW release 9006.4 or later versions)|DMG1008RLMDNIW|
|Siemens|HiPath 4000|DMG1008LSW|
|Toshiba|CTX (SW version AR1ME021.00)|DMG1008LSW|
|Others|Various|DMG1008LSW <br/><br/> Analog connectivity using either Inband DTMF or SMDI|

### PBXs supported when using a DMG 2000 series Media Gateway

The following table shows the PBXs that are supported with the T1/E1 Dialogic Media Gateway (DMG2000). The DMG2000 gateway, which comes in single span (DMG2030DTIQ), dual span (DMG2060DTIQ), or quad span (DMG2120DTIQ) densities, supports the following protocols:

- T1 CAS

- T1 Q.SIG

- E1 Q.SIG

- T1 NI-2

- T1 5ESS

- T1 DMS100

If Channel Associated Signaling (CAS) signaling is used, supplemental signaling (RS232 SMDI, MD110, MCI protocols, or Inband DTMF signaling) is required. If Q.SIG signaling is used, the PBX must support the supplemental services that are associated with calling and called party information and the call transfer capabilities required by Unified Messaging.

**PBXs supported with the DMG2000 Media Gateway**

|**PBX manufacturer**|**PBX model/type**|**Required software version**|**Protocol and additional signaling**|
|:-----|:-----|:-----|:-----|
|Alcatel|Omni PCX 4400|Version 3.2.712.5|T1 Q.SIG <br/><br/> E1 Q.SIG|
|Avaya|Definity G3|Version 3 or later|T1 CAS|
|Avaya|S8500|Manager SW V2.0 or later versions|T1 CAS <br/><br/> T1 Q.SIG <br/><br/> E1 Q.SIG|
|Ericsson|MD110|Release MX1 TSW R2A (BC13)|E1 Q.SIG|
|Intercom|||CAS (w/ SMDI serial protocol)|
|NEC|2400 IMX|Release 5200 Dec. 92 1b or later versions|CAS (w/ MCI serial protocol)|
|NEC|2400 IPX|R17 Release 03.46.001|T1 Q.SIG|
|Nortel|Meridian 1 - Option 11|Release 15 or later versions, and options 19 and 46 are required|T1 Q.SIG <br/><br/> E1 Q.SIG|
|Nortel|Communication Server 1000|Version 2121, Release 4|T1 Q.SIG <br/><br/> E1 Q.SIG|
|Siemens|HiCom 300E CS|Release 9006.4 or later (Note: North American software load only)|T1 CAS|
|Siemens|HiPath 4000|V2 SMR 9 SMPO|T1 Q.SIG <br/><br/> E1 Q.SIG|
|Mitel|SX-2000 S, SX-2000 VS|LW 34|T1 Q.SIG <br/><br/> E1 Q.SIG|
|Mitel|3300|Version 5.1.4.8|T1 Q.SIG <br/><br/> E1 Q.SIG|

### PBXs supported when using a DMG4008BRI series Media Gateway

The DMG4000 series Media Gateway comes with several TDM interface options. The DMG4008BRI supports 4-port/8-channel densities and supports the following protocols:

- ISDN BRI Q.SIG

- ETSI-DSS1 (Euro ISDN)

- NET 3 (Belgium)

- VN3 (France)

- 1TR6 (Germany)

- INS-64 (Japan)

- 5ESS Custom (North America - AT&T)

- National ISDN (NI1 - North America)

The following table shows the PBXs that are supported using a Dialogic 4000 Media Gateway Series (DMG4008).

**PBXs supported using a DMG4008BRI Media Gateway**

|**PBX manufacturer**|**PBX model/type**|**Required software version**|**Protocol and additional signaling**|
|:-----|:-----|:-----|:-----|
|Siemens|HiCom 300|SA300-V3.05|BRI-Q.SIG (ECMAV2)|
|Siemens|HiPath 4000|S.0 B4400|BRI-Q.SIG (ECMAV2)|

## Supported IP PBXs

IP PBXs are also supported by Unified Messaging. The following table shows the IP PBXs that are supported using a direct SIP connection to Unified Messaging.

**IP PBXs supported when using a direct SIP connection**

|**PBX manufacturer**|**PBX model/type**|**Required software version**|
|:-----|:-----|:-----|
|Aastra|MX-ONE|4.0|
|Avaya|Aura|5.2.1 with Service Pack 5 (SP5)|
|Avaya|Communication Server 2100|CS2100 SE13|
|Cisco|Call Manager, Unified Communications Manager|5.1, 6.x, 7.0 and8.0|

## IP PBXs supported when using SIP media gateways

IP PBXs using SIP media gateways are also supported by Unified Messaging. The following table shows the IP PBXs that are supported using IP to IP capabilities of SIP media gateways to connect to Unified Messaging.

**IP PBXs supported when using a SIP media gateway**

|**PBX manufacturer**|**PBX model/type**|**SIP gateway model**|
|:-----|:-----|:-----|
|Cisco|Call Manager 4.x|AudioCodes Mediant 1000/2000 (IP-to-IP enabled)|

## Exchange Unified Messaging, Office Communications Server 2007 R2, and Microsoft Lync Server

For on-premises and hybrid deployments, Exchange Unified Messaging can be deployed together with Microsoft Office Communications Server 2007 R2, Microsoft Lync Server 2010 or Lync Server 2013 to provide voice messaging, Instant Messaging (IM), enhanced user presence, audio-video conferencing, and an integrated email and messaging experience for users in your organization. For more information, see:

- [Integrate Exchange 2013 UM with Lync Server](https://docs.microsoft.com/exchange/deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help)

- [Microsoft Lync Server 2013](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013)

To find out more about the Microsoft Unified Communications Open Interoperability Program for enterprise telephony infrastructure, including finding qualified SIP PSTN gateways and IP PBXs and the process for telephony infrastructure vendors to join and participate in the program, see [Microsoft Unified Communications Open Interoperability Program](https://docs.microsoft.com/SkypeForBusiness/lync-cert/qualified-lync-apps).
