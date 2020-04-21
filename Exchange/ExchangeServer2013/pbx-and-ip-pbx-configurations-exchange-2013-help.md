---
title: 'PBX and IP PBX configurations: Exchange 2013 Help'
TOCTitle: PBX and IP PBX configurations
ms:assetid: fb086680-6e3e-477a-a5d8-e24ca30196ee
ms:mtpsurl: https://technet.microsoft.com/library/Bb430797(v=EXCHG.150)
ms:contentKeyID: 53908382
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# PBX and IP PBX configurations in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Increasingly, organizations are purchasing, installing, and maintaining the hardware components, for example, Private Branch eXchanges (PBXs) or IP PBXs, that are required to support their own telephony systems. Many organizations are buying their own telephony equipment and training their staff to reduce expenses associated with maintaining their telephony systems and because they want more control over the telephony features they offer.

For an organization to own and maintain their telephony network, they must buy the required telephony hardware components. They must also consider the day-to-day maintenance of the telephony equipment and the training required for their staff to support their telephony system. This topic discusses the different types of telephony business or organizational systems and the telephony hardware components they require. The topic also gives examples of the different types of telephony configurations.

> [!IMPORTANT]
> We recommend that all customers who plan to deploy Microsoft Exchange 2013 Unified Messaging obtain the help of a UM specialist. This will help ensure a smooth upgrade from a legacy voice mail system. Rolling out a new UM deployment or performing an upgrade of an existing voice mail system requires significant knowledge about PBXs, IP PBXs, and Unified Messaging. For more information about who to contact, see the [Microsoft Solution Providers](https://www.microsoft.com/solution-providers) website.

## Overview of telephony systems

In circuit-switched networks, such as the Public Switched Telephone Network (PSTN), multiple calls are transmitted across the same transmission medium. Frequently, the medium that's used in the PSTN is copper. However, fiber optic cable might also be used.

A circuit-switched network is a network in which there exists a dedicated connection. A dedicated connection is a circuit or channel that's set up between two nodes so they can communicate. After a call is established between two nodes, the connection may be used only by these two nodes. When the call is ended by one of the nodes, the connection is canceled.

Different types or categories of telephone systems found in businesses and organizations include a circuit-based network, an IP-based network, or both. Each type of telephone system has distinct advantages and disadvantages you need to consider when planning and implementing a telephony system.

- **Centrex**: Centrex is a type of telephone service that telephone companies lease to businesses and organizations. A traditional Centrex telephone system eliminates the need for a business or organization to purchase the telephony hardware used onsite to support the organization's telephone system. Typically, Centrex systems are used by small offices that rent Centrex services from a telephone company on a line-by-line and month-by-month basis. Centrex telephony systems are sometimes used by larger organizations, but are most frequently found in government, public, and private organizations. Centrex frequently uses analog telephone lines for the connections to a business or organization. But it can also use T1-circuits with a demultiplexer onsite to support analog and digital telephones or ISDN lines.

  In a Centrex-based telephony system, the telephone company's central office acts as the telephone exchange. It's designed specifically to support the needs of a given organization. The central telephone office routes the calls that originate from inside the company to the appropriate internal or external telephone number. Centrex uses the telephone company's central office exchange to route internal calls back to an extension. For example, with Centrex, the telephone exchange or telephone company's central office knows which extensions are internal. So an employee who's located within the organization's telephony network can dial another employee in the same telephony network or dial plan by using a four-digit extension number. When a call is dialed to the internal telephone extension number, it's forwarded to the telephone company's central office and then routed back to the extension number that initiated the call.

A variation of a traditional Centrex telephony system is called *IP Centrex*. In an IP Centrex telephone system, the call is sent through a Voice over IP (VoIP) gateway located at a telephone company's central office or located onsite at a service provider. In this kind of telephone system, the VoIP gateway translates the call into IP-based data packets that can be sent over the Internet or over a VoIP-based network. However, if the call is sent over the Internet, there's typically another VoIP gateway that receives the call and then translates the call back to a traditional circuit-switched call.

Organizations that currently have a traditional Centrex telephone system in place have to install, deploy, and maintain one or more VoIP gateways for Unified Messaging to work correctly. Unified Messaging may require that you install, deploy, and maintain VoIP gateways to work with IP Centrex. Several variables will determine whether you need a VoIP gateway. These variables include the type of telephones used in your organization (analog, digital, or IP) and the protocols supported by the IP Centrex system.

- **Key telephone**: In a Key telephone system, the telephone company's central office is connected to the organization using standard analog or digital telephone lines. A single telephone extension number is connected to multiple telephones so when a call is placed into the organization using this telephone number, all the telephones associated with that line or extension number will ring at the same time.

With Key telephone systems, individual users share lines across telephones. Therefore, callers won't experience frequent busy signals when they try to call into an organization. Key telephone systems are typically used by small offices where internal call volume is high but external call volume is low.

Key telephone systems have become more sophisticated over time and can work with Unified Messaging if a VoIP gateway is added. However, some less sophisticated systems may not work even if a supported VoIP gateway is used.

- **PBX**: A legacy PBX is a telephony device that switches calls in a telephony or circuit-switched network. A legacy PBX is a PBX that doesn't have a network adapter and can't pass IP packets. Because they can't pass IP packets, some businesses and organizations have replaced legacy PBXs with IP PBXs. For a list of PBXs supported by Unified Messaging, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

    PBXs are used by most medium- and larger-sized companies. A PBX enables users or subscribers of the PBX to share a certain number of outside lines for making telephone calls considered external to the PBX. A PBX is a much less expensive solution than giving each user in a business a dedicated external telephone line. Telephones, in addition to fax machines, modems, and many other communication devices, can be connected to a PBX.

    The PBX equipment is typically installed on an organization's premises and connects calls between the telephones located onsite and the telephone company. A limited number of outside lines, also known as trunk lines, are typically available for making and receiving calls external to the business from an external source such as the PSTN.

    To enable a legacy PBX to be used with Unified Messaging, you need to deploy a supported VoIP gateway. For a list of supported VoIP gateways, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

- **IP PBX**: An IP PBX is a PBX that has a network adapter that supports the IP protocol. It's a piece of telephone switching equipment that generally resides in an organization or business instead of being located at a telephone company office. There are two types of IP PBXs: traditional IP PBXs and hybrid IP PBXs. Both traditional IP PBXs and hybrid IP PBXs support the IP protocol for sending voice conversations in packets to VoIP-based telephones. However, hybrid IP PBXs also connect traditional analog and digital telephones.

    IP PBXs are frequently easier to administer than legacy PBXs, because administrators can more easily configure IP PBX services using an Internet browser or another IP-based tool. Also, no additional wiring, cabling, or patch panels have to be installed. With an IP PBX, you can move an IP-based telephone by merely unplugging the telephone and plugging it in at a new location. This lets you avoid the costly service calls required to move a telephone from legacy PBX vendors. Additionally, organizations that own an IP PBX don't have to incur the additional infrastructure costs required to maintain and manage separate circuit-switched and packet-switched networks. For a list of IP PBXs supported for Unified Messaging, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).



## Legacy and traditional PBX configurations

On telephony networks that have legacy or traditional PBXs, a PBX does the following:

- Creates connections or circuits between the telephone sets of two users.

- Maintains the connection as long as the users need the connection.

- Provides information for accounting purposes (for example, meters calls).

In addition to the three functions included in the previous list, PBXs may offer other calling features such as:

- Auto attendants

- Call accounting

- Call pick-up

- Call transfer

- Call waiting

- Conference calling

- Direct Inward Dialing (DID)

- Do Not Disturb (DND)

Although there are several manufacturers of PBXs, they all fit into two basic categories: analog and digital. These types of PBXs are frequently known as *legacy* or *traditional* PBXs.

Typically, PBX systems are connected to the telephone company's central office by using special telephone lines, known as T1- and E1-lines. T1- and E1-lines have multiple channels. These telephone lines are also known as *trunk lines.* They let the central office or the PBX send multiple calls over the same line for better efficiency using a simplified wiring layout. A PBX can also work with analog or ISDN lines.

By correctly configuring your PBX, you can control how many channels or lines you want to configure to receive calls that come from external callers and how many channels or lines to devote to calls that come from callers inside your organization. Configuring the number of channels or lines helps prevent busy signals and lets you configure the number of channels or lines devoted to applications such as call centers. Correctly configuring your PBX is a cost-effective method for managing the channels or lines in your organization because it reduces the number of leased lines required.

A PBX can route a specific dialed telephone number to a specific telephone so users can have their own individual telephone number or extension number. This is known as a Direct Inward Dialing number. When the telephone number is dialed for a user, the telephone company sends the DID number to the PBX by using Dialed Number Identification Service (DNIS). Because the telephone company uses DNIS to send the number, there's no need for operator intervention to route the call. The PBX has the information about the call to correctly route it to the number that was dialed by the caller. For a list of PBXs supported by Unified Messaging, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

## Analog and digital PBXs

Analog PBXs send voice and call signaling information, such as the touch tones of a dialed telephone number, as an analog sound. Therefore, the sound is never digitized. To correctly direct the call, the PBX and the telephone company's central office have to listen for the signaling information.

> [!NOTE]
> Touchtone is more technically known as dual tone multi-frequency. When a caller presses a key on a telephone keypad, the telephone produces two separate tones: a high-frequency tone and a low-frequency tone. When a person speaks into the telephone, only a single tone or frequency is emitted. Sending two tones with different frequencies at the same time reduces the possibility that the signaling tones will be interpreted as a human voice or that a human voice will be interpreted as the signaling tones.

Digital PBXs encode or digitize the analog sound into a digital format. Digital PBXs typically encode the voice sounds using a standard industry audio codec like G.711 or G.729. After the digitized voice is encoded, it's sent over a channel by using circuit switching. Circuit switching sets up an end-to-end open connection. It leaves the channel open for the length of the call and for the caller's exclusive use. However, the signaling method that's used by the PBX depends on the manufacturer. PBX manufacturers may have their own proprietary signaling method for call setup.

> [!NOTE]
> Digital PBXs can support both digital and analog trunk lines.

In larger organizations, PBXs make it possible for employees in separate physical locations to contact one another by dialing an extension number for a user. This can be done by using a single PBX or may involve multiple PBXs networked together. PBXs at different office locations can be connected to a single transparent circuit-switched network by using T1- or E1-lines. When these lines connect PBXs together, they are frequently known as *tie lines*. The PBXs communicate with one another across the tie lines using a PBX-to-PBX protocol, such as QSIG. QSIG lets a set of PBXs act as if they are a single PBX.

This kind of PBX environment can also include advanced features, such as call transferring and telephone conferencing. In addition to allowing for advanced features, having two connected PBXs can also save the organization money because long distance charges between employees in the different locations will be reduced. This is because a call made between two employees remains on a tie line between the PBXs and requires that the user dial only an extension number for the other user instead of placing a long distance call.

In a telephony environment that includes a single or multiple analog or digital PBXs, a VoIP gateway is required between the PBX and the Exchange 2013 Client Access and Mailbox servers to convert the circuit-based protocols found on a telephony network into the IP-based protocols found on a data network. For more information about VoIP gateways, see the following topics:

- [UM IP gateways](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways)

- [Connect a VoIP gateway to communicate with a PBX](connect-a-voip-gateway-to-communicate-with-a-pbx-exchange-2013-help.md)

For a list of VoIP gateways supported for Unified Messaging, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

## IP PBX configurations

An IP PBX is a PBX that supports the IP protocol to connect telephones by using an Ethernet or packet-switched LAN. It sends voice conversations in IP or data packets. An IP PBX may have multiple interfaces. These include interfaces for a data network and other interfaces that allow for a connection to a telephony or circuit-switched network.

The development of real-time Internet protocols has made it possible to successfully send voice and fax messages over a data network. Such real-time Internet protocols include the VoIP protocols used with Unified Messaging: Session Initiation Protocol (SIP) over Transmission Control Protocol (TCP) for voice messaging. These protocols have made it possible to successfully send voice and fax messages over a data network. Real-time VoIP protocols are required to send voice messages over a packet-switched or data network so the delivery order and timing of data packets can be maintained and controlled. If these protocols weren't used to maintain and control the delivery and timing of the data packets, a person's voice would be broken up and sound incoherent or the images might appear garbled. For a list of IP PBXs supported for Unified Messaging, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

> [!NOTE]
> Unified Messaging supports only SIP over TCP.

## Traditional IP PBX configurations

A standard or traditional IP PBX contains at least a single network interface that connects to a data network using VoIP protocols. It may also contain additional network interfaces or other telephony interfaces that enable it to connect to an existing telephony network such as the PSTN. The connection to the data network allows for communication with other VoIP hosts located on the data network by using IP data packets. These VoIP hosts include other IP PBXs, VoIP-based telephones, VoIP gateways, and Client Access and Mailbox servers that are running UM services. A traditional IP PBX doesn't support analog or digital telephones. It supports only VoIP telephones.

Because the IP PBX can already connect to a data network and can convert the circuit-based protocols from the PSTN to packet-switched VoIP protocols, a VoIP gateway may not be required to enable communication with Client Access and Mailbox servers on the data network.

## IP PBX hybrid configurations

Hybrid IP PBXs can provide analog, digital, and VoIP-based capabilities. If the correct interfaces are installed on an IP PBX and the software that supports multiple types of interfaces is installed correctly, the IP PBX is considered a hybrid IP PBX. An IP PBX hybrid makes it possible to use a mixture of analog, digital, and IP-based telephones.

Most modern IP PBXs can support and provide all three types of voice communication or a traditional IP PBX can be upgraded to a hybrid IP PBX by installing the necessary interfaces and software or firmware updates.

The mixture of analog, digital, and IP-based telephones makes it possible for users in your organization to use many new features and also provides great flexibility in your telephony environment. Using an IP PBX hybrid also allows for a more gradual migration to a completely VoIP-based telephony environment and voice messaging system for your organization.

Several factors determine whether a VoIP gateway will be required when you connect with Client Access and Mailbox servers. One of these factors is the compatibility of the VoIP protocols used by the IP PBX or hybrid IP PBX and Unified Messaging. If a VoIP gateway isn't required, it will reduce the complexity of the telephony infrastructure, and the support that you must have for Unified Messaging will be simpler.

## Calling or called party identification

Calling or called party identification is a telephone company service that can tell the person who is receiving the call the telephone number and sometimes the name of the person who is calling and other information about the call. This information is sent over a serial cable by using call signaling. When a call is received by a PBX or IP PBX from a telephone company, the call includes calling identification information such as the following:

- The calling party's number

- The called party's number

- Status codes such as a ring-no-answer, the state or condition of the line, line busy, and call forward always

- The line or port number that's being used for the call

- In telephony, the signaling information is used to exchange information between endpoints on a network to set up, control, and end calls. Several signaling methods used by VoIP gateways and IP PBXs are supported by Unified Messaging. The signaling method that's used depends on the type of device that's being used and the type of signaling method that's used by the telephone company. The most important factor is that the device that's connecting to the telephone company and to the VoIP gateway or IP PBX must support at least one of the signaling methods that enable calling or called party information to be sent and received by callers. For more information about signaling configuration information for a supported VoIP gateway, see [Telephony advisor for Exchange 2013](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013).

Although other signaling methods can be used, the two most popular signaling methods are as follows:

- **Simplified Message Desk Interface (SMDI)**: SMDI is a protocol that's used to provide signaling, call control, and calling identification information from an interface between a telephone system and a voice mail system. It's used to provide the voice mail system with the information it needs to process an incoming call. Every time an incoming call is sent by using SMDI over a serial interface or RS-232 interface, the information that's sent will identify the line or port, the type of call, and the calling or called party numbers. The SMDI cable connects from a device such as a PBX to a serial connection on the VoIP gateway. However, SMDI is also used with IP PBXs. The SMDI protocol allows for a maximum of only 10 digits for each calling and called number. This is a limitation of the protocol and can't be changed.

- **In-band:** In-band signaling allows for the exchange of signaling, call control, and calling identification information from a telephone company. This information is sent over the same channel and in the same band (300 Hz to 3.4 kHz) as the voice and other sounds that are being made during the call. For example, when a user places a call by using DTMF or touchtone dialing and talks to the called party, both the touchtone and the voice conversation use the same channel and band. In-band signaling is less secure because the control signals are exposed to the user and is a less popular signaling method than SMDI. In-band signaling applies only to Channel Associated Signaling (CAS).
