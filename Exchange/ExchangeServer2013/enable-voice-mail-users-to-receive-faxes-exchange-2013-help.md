---
title: 'Enable voice mail users to receive faxes: Exchange 2013 Help'
TOCTitle: Enable voice mail users to receive faxes
ms:assetid: 451ab0ea-21e1-4c1f-ae62-4ba7cdfd1e4d
ms:mtpsurl: https://technet.microsoft.com/library/Bb232022(v=EXCHG.150)
ms:contentKeyID: 49315404
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable voice mail users to receive faxes in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Microsoft Exchange Unified Messaging (UM) enables voice mail messages to be delivered to a user's mailbox and also lets users receive faxes in their mailbox. In UM, a fax is sent to the user's mailbox as an email message that has an image file with a .tif extension attached. The user can open the attached file by using a software application that can open and view image files that have a .tif extension. This topic discusses faxing and how it works in UM.

> [!NOTE]
> Although Unified Messaging doesn't let users send outgoing faxes, many third-party solutions, such as an Internet fax service, an email faxing service, or a third-party fax server application, can be used to send outgoing faxes.

## Overview of faxing

Fax is an abbreviation for the word facsimile. It's a technology that's used to electronically transfer documents. Generally, faxes are sent and received by fax machines or computer fax modems by using the Public Switched Telephone Network (PSTN), which is a telephony or circuit-based network. However, other options can be used to send and receive faxes.

Most organizations today want their users to be able to send and receive faxes. Organizations use one or more of the methods described in the following list to send or receive faxes over the PSTN or over the Internet. There are advantages and disadvantages to each of these methods.

- Traditional fax machines and computer-based faxing

- Faxing by using fax servers or fax gateways

- Faxing by using a Voice over IP (VoIP) network

- Faxing by using an email client application

To send a fax message, users in an organization may have to do the following:

- Print a hard copy of the document to be faxed and use a physical fax machine to send it.

- Save the document on their computer and use a fax modem to send the fax, but this requires a phone line connected to the computer.

- Use an Internet fax service that lets them fax a document from a vendor-specific software application.

- Send an outgoing fax to a fax server by using a software application that's configured to use the fax server.

To receive a fax, users in an organization may have to do the following:

- Receive a fax on a physical fax machine within the organization.

- Receive a fax by using a fax modem that's installed on their computer.

- Receive a fax from an Internet faxing service.

- Receive a fax from a fax server that's configured on a network.

- Receive a fax from a fax partner's server that uses Unified Messaging to deliver the fax using a VoIP network.

## Faxing methods

There are several options for sending and receiving faxes, including the following:

**Traditional fax machines and computer-based faxing**: A scanner, a fax modem in a computer, a printer with built-in faxing capabilities, or a dedicated fax machine can be used to send and receive faxes. All of these can be used to transmit data in the form of pulses by using a telephone line to another fax device, usually another fax machine or computer that has a fax modem. The pulses are then transformed into images or used to print the image on paper.

The traditional fax method requires at least a single telephone line on the sending and receiving device, and only one fax can be sent or received at a time. A disadvantage of sending and receiving faxes by using a fax modem is that the computer must be turned on and running fax software or a fax service. This kind of computer-based faxing doesn't use the Internet to send or receive faxes.

**Traditional and computer-based faxing**

![Traditional Faxing](images/Bb232022.7bdc1cab-9504-4314-a6e0-eccdfe2a9cd6(EXCHG.150).gif "Traditional Faxing")

**Fax servers or gateways and Internet fax services**: There are several ways to send and receive faxes over the Internet. These include using a software application on a computer or using an email client to receive faxes. In most cases, this kind of faxing involves using a fax server or fax gateway to convert between faxes and email. This method has become increasingly popular because it enables organizations to remove or avoid purchasing additional fax machines. It also eliminates the need to install additional telephone lines. This kind of faxing involves creating a document, including a fax cover page with the correct identifying information, and then sending the document to a traditional fax machine. For example, the user uses a software application such as Microsoft Word or Microsoft Outlook to create and send the fax to the fax server or gateway. The fax server or gateway receives the fax and then sends it by using a traditional telephone line to a fax machine or a fax modem that's installed on a computer.

**Faxing by using fax servers or gateways**

![Faxing with fax servers/gateways](images/Bb232022.d6fa2402-b4ca-4313-956c-62e5fe7731ad(EXCHG.150).gif "Faxing with fax servers/gateways")

Internet fax services let a user send faxes from a computer by using the Internet. A software application such as Word or Outlook can be used to create and send the fax to an Internet fax service. Many companies offer Internet fax services on a subscription basis or by charging for each fax message that's sent. Internet fax services offer the following advantages:

- No fax machine is required.

- No software or hardware must be installed.

- No dedicated telephone lines are required.

- Confidentiality.

- Multiple faxes can be sent at the same time.

- Faxes can be received when the computer is turned off.

The following figure shows how Internet fax services can be used to send and receive faxes.

**Internet fax services**

![Internet Fax Services](images/Bb232022.5d0fb3d6-95f4-4fbf-80e5-64f5de553e65(EXCHG.150).gif "Internet Fax Services")

**Faxing by using an email client application**: Faxes can be sent and received by a fax machine over the Internet and then received by an email client such as Outlook.

The T.37 protocol was designed to enable a fax machine to send fax messages over the Internet to an email client. The faxes are sent over the Internet as email attachments, typically as .tif or .pdf files. In this kind of faxing, a fax machine that supports iFax or T.37 is required, in addition to an email address for the sending and receiving fax machines. To work with traditional fax machines and fax modems, all T.37 fax machines support standard faxing by using a telephone line. However, in some cases, T.37 fax machines can be used when a fax gateway is also being used. The following figure shows how T.37-based fax machines and email clients can be used to send and receive faxes.

**Faxing with email**

![Faxing with email](images/Bb232022.086f086b-dc39-4439-a694-7a98e03e65d1(EXCHG.150).gif "Faxing with email")

**Faxing by using a VoIP network**: VoIP is a technology that provides hardware and software that enables people to use an IP-based network as the transmission medium for telephone calls. On a VoIP network, voice and fax data is sent in packets by using IP instead of traditional circuit transmissions or the circuit-switched telephone lines of the PSTN. A VoIP gateway that you connect to your IP network uses VoIP to send voice data packets between a Client Access server running the Microsoft Exchange Unified Messaging Call Router service and a Mailbox server running the Microsoft Exchange Unified Messaging service and a Private Branch eXchange (PBX) system. You can also use an IP PBX to perform the functions of both a VoIP gateway and a PBX.

There are two basic types of networks: circuit-switched and packet-switched. A circuit-switched network is a network in which there exists a dedicated connection. A dedicated connection is a circuit or channel that's set up between two nodes so that they can communicate. After a call is established between two nodes, the connection can be used only by these two nodes. When the call is ended by one of the nodes, the connection is canceled. In circuit-switched networks, such as the PSTN, multiple calls are transmitted across the same transmission medium. Frequently, the medium that's used in the PSTN is copper. However, fiber optic cable might also be used.

In packet-switched networks such as the Internet or a local area network (LAN), packets are routed to their destination through the most expedient route, but not all packets traveling between two hosts travel the same route, even those from a single message. This almost guarantees that the packets will arrive at different times and out of order. In a packet-switched network, packets (messages or fragments of messages) are individually routed between nodes over data links that may be shared by other nodes. With packet switching, unlike circuit switching, multiple connections to nodes on the network share the available bandwidth. Packet-switched networking has made it possible for the Internet to exist and, at the same time, has made data networks (especially LAN-based IP and VoIP networks) more available and widespread.

## T.38

T.38 is a faxing standard and protocol that enables faxing over an IP-based network. An IP-based network that uses the T.38 protocol uses Simple Mail Transfer Protocol (SMTP) and Multipurpose Internet Mail Extension (MIME) to send a message to a recipient's mailbox. T.38 allows for IP fax transmissions for IP-enabled fax devices and fax gateways. The devices can include IP network-based hosts such as client computers and printers. In Exchange Unified Messaging, the fax images are separate documents encoded as .tif files and attached to an email message. Both the email message and the .tif file attachment are sent to the user's UM-enabled mailbox.

Unified Messaging relies on the gateway's abilities to translate or convert Time Division Multiplex (TDM) or telephony circuit-switched based protocols like Integrated Services Digital Network (ISDN) and QSIG from a PBX to IP-based or VoIP-based protocols like Session Initiation Protocol (SIP), Real-Time Transport Protocol (RTP), or T.38 for receiving fax messages. The VoIP gateway is integral to the functionality and operation of Unified Messaging. The VoIP gateway is responsible for sensing fax tones. Client Access and Mailbox servers rely on the VoIP gateway to send a notification that a fax has been detected. Then the Client Access and Mailbox servers will renegotiate the media session and use the T.38 protocol.

## Overview of faxing with Unified Messaging

In Unified Messaging, the user receives fax images as separate documents encoded as .tif image files that are attached to an email message. Both the email message and the .tif attachment are sent to the user's UM-enabled mailbox.

There are several advantages to sending a fax message to the user's mailbox. These advantages include the following:

- You can reduce the number of physical or traditional fax machines.

- The number of telephone lines used for faxing in an organization can be reduced, because the Mailbox server can queue many faxes and send each fax when one of the telephone lines becomes available.

- Faxes that are received as a .tif image file are better quality than a traditional fax. Incoming faxes can be printed by a local or shared printer.

- Faxes sent to the user's mailbox are more secure because they're less likely than hard-copy faxes to be picked up by someone other than the recipient.

- Users can receive faxes without leaving their desk.

- Fax messages that are received can be monitored to make sure that they comply with an organization's security policies.

A single fax message can be sent only to a single UM-enabled user. Unified Messaging can't forward fax messages to a distribution list. If you need to have this functionality, you must follow these steps:

1. Create a mailbox to answer the fax call. This will be the mailbox for the distribution list.

2. UM-enable the distribution list mailbox.

3. Create a rule for this UM-enabled mailbox. The rule will be configured to forward all messages to the selected distribution list.

## Receiving incoming faxes

Receiving a fax on a VoIP network differs from receiving a fax on a standard fax machine or by using a fax server that's located on an IP-based network. To enable faxes to be sent and received over a VoIP network, you must have a VoIP gateway or an IP PBX that supports the T.38 protocol and a server that also supports T.38. T.38 allows for IP-based fax transmissions for IP network-based hosts, for example, client computers, printers with built-in faxing capabilities, and servers such as a Mailbox server.

> [!IMPORTANT]
> Sending and receiving faxes using T.38 or G.711 isn't supported in an environment where Unified Messaging and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server are integrated.

When a PBX receives a call, it forwards the call to the appropriate extension. If there is no answer at the user's extension number, the PBX forwards the call to a VoIP gateway, and the gateway forwards the call to the appropriate Mailbox server. The Mailbox server determines whether the call is a voice call or a fax call, based on the protocol that's used for the call. When the SIP protocol is used, the Mailbox server processes the call as a voice message. However, if the T.38 protocol is used from the VoIP gateway, the Mailbox server recognizes that the call is for a fax and processes it as described in the following paragraph. A Mailbox server forwards incoming fax calls to a dedicated fax partner server, which then establishes the fax call with the fax sender and receives the fax on behalf of the UM-enabled user. The fax partner server then sends the fax included as a .tif attachment in an SMTP message to the recipient's mailbox.

When a Mailbox server receives an incoming T.38 fax signal from a VoIP gateway, the Mailbox server queries the directory using the Lightweight Directory Access Protocol (LDAP) to determine if the intended recipient of the incoming fax call is allowed to receive incoming fax messages and to determine the SIP address of the fax partner server. After it has checked this information, the Mailbox server sends a fax call referral request to the VoIP gateway or SIP peer, which then forwards the fax request on to the fax partner server. After the fax call has been successfully established, the fax sender sends the fax media and data to the fax partner server. After the fax partner server receives the fax media and data, it uses SMTP to send an email message to a Mailbox server. This message contains a .tif image of the fax message and special X-headers to the intended fax recipient.

After the fax message is authenticated and is sent from a valid fax partner server, the UM mailbox assistant on the Mailbox server issues an RPC call to the Microsoft Exchange Unified Messaging service. This ensures that the fax message properties match those of fax messages that are created by a Mailbox server. Finally, the Mailbox server again submits the final fax message, which includes the email message and .tif attachment for the incoming fax. Then, using MAPI RPC, the completed and formatted version of the fax message is delivered to the Mailbox server that contains the intended recipient's mailbox.

## Fax call referral methods

An incoming fax call can be signaled to a Mailbox server through SIP re-INVITE from the VoIP gateway or SIP peer (media gateway), CNG (calling fax tone) notification by the SIP peer, or CNG detection by the Mailbox server. The following subsections detail the fax call referral in each case.

## Re-INVITE from a SIP peer

An incoming call to a UM pilot number is directed to UM as an INVITE with a voice (RTP/audio) SDP profile

1. UM accepts the invitation and media streams are established. After the call has been established, fax transmission is initiated by the caller.

2. The SIP peer detects the calling fax tone (CNG). The SIP peer issues a re-INVITE to the Mailbox server, this time specifying a fax (T.38 or G.711) profile in the SDP.

3. UM responds to the invitation with a 200 OK that places the SIP peer "on hold".

4. UM issues a REFER, referring the SIP (CNG) peer to a fax partner solution end point, obtained from its configuration data.

5. The SIP peer sends the fax session INVITE to the fax partner solution.

6. The fax partner solution accepts the invitation, and a media session is established with the SIP peer.

## CNG notification by a SIP peer

An incoming call to a UM pilot number is directed to UM as an INVITE with a voice (RTP/audio) SDP profile.

1. UM accepts the invitation and media streams are established. After the call has been established, fax transmission is initiated by the caller.

2. The SIP peer detects the calling fax tone (CNG).The SIP peer notifies the UM server of the fax by sending a CNG notification in the RTP stream, compliant with RFC 4733.

3. UM responds to the notification by immediately issuing a REFER and referring the SIP peer to a fax partner solution end point, obtained from its configuration data.

4. The SIP peer sends the fax session INVITE to the fax partner solution.

5. The fax partner solution accepts the invitation.

6. A media session is established with the SIP peer.

## CNG detection by a Mailbox server

An incoming call to a UM pilot number is directed to UM as an INVITE with a voice (RTP/audio) SDP profile. UM accepts the invitation and media streams are established.

1. After the call has been established, fax transmission is initiated by the caller.

2. The Mailbox server detects the fax tone (CNG) in the RTP audio stream.

3. UM responds to the notification by immediately issuing a REFER and referring the SIP peer to a fax partner solution end point, obtained from its configuration data.

4. The SIP peer sends the fax session INVITE to the fax partner solution.

5. The fax partner solution accepts the invitation.

6. A media session is established with the SIP peer.

## Configuring faxing

By default, when you install the Mailbox server, the server isn't configured to allow incoming fax calls to be processed or delivered to a UM-enabled user. To configure UM with a fax partner server, you must configure the UM mailbox policy and configure authentication between the Mailbox server and the fax partner server. For more information, see [Setting up incoming faxing](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/set-up-incoming-faxing).

## Telephone numbers and faxing

Exchange Unified Messaging offers the following options when you're configuring UM-enabled users to receive fax messages:

- A Direct Inward Dial (DID) telephone number for an individual user that's used for both faxes and voice mail.

- A separate DID telephone number for an individual user that's used for receiving faxes.

- A central fax telephone number for all users that receives all faxes.

## A single DID telephone number

When you enable a user for Unified Messaging by using the **Enable UM mailbox** page or the **Enable-UMMailbox** cmdlet, you must specify at least a single extension number for the user. This extension number is enabled on a per-user basis and must be unique within a given dial plan. The extension is used by Unified Messaging to locate the correct user and to deliver voice and fax messages to the user's mailbox. For more information, see [Enable-UMMailbox](https://docs.microsoft.com/powershell/module/exchange/Enable-UMMailbox).

Using a single DID number, you can configure faxing so that a user uses a single DID number for both voice and fax. This configuration is easy to administer and doesn't waste additional DID numbers. If the user is away or on the phone when a fax call arrives, UM answers the call, detects the fax tone, creates the fax message, and sends it to the user.

If a user whose faxing is configured using a single DID telephone number receives a call from a fax machine when they're not away or on the phone, they can:

- Not answer the telephone when it rings so that the fax call will be forwarded and answered by a Mailbox server and the fax message will be created and forwarded to the user's mailbox.

- Answer the fax call, and then transfer it to himself or herself so that the call will be forwarded and answered by a Mailbox server and the fax message will be created and forwarded to the user's mailbox.

- Wait for the caller to retry sending the fax and let the fax call be transferred to a Mailbox server.

In summary, using a single DID number requires the user to perform additional actions to be able to receive fax messages.

## Multiple DID telephone numbers

When you enable a user for Unified Messaging, you must enter at least a single extension number for that user. You can add multiple extension numbers for a UM-enabled user by using the Exchange admin center (EAC). For more information, see [Add an extension number](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/add-extension-number).

Adding multiple extension numbers is useful when a UM-enabled user:

- Receives many faxes.

- Doesn't want to be bothered with answering the phone to receive a fax.

- Doesn't want to hear a fax tone when they answer their phone.

Adding multiple extensions is more complex than using a single extension and may require additional configuration settings on a PBX or an IP PBX. To configure multiple extension numbers for a UM-enabled user, you must have DID extension numbers available that aren't being used in your organization. It isn't a good idea to use multiple numbers for a UM-enabled user if your organization has a limited number of DID extension numbers available.

The benefit of using multiple DID extension numbers is that a UM-enabled user receives voice calls on one DID extension number and fax calls on another DID extension number. Using separate DID numbers for voice mail and fax calls is easier for the user.

If you configure two DID extension numbers for a specific user, the DID extension numbers can come from separate UM dial plans. To use two DID numbers, you can create a dial plan and use a Mailbox server as a dedicated server that will receive fax calls and forward fax messages to users. For more information, see [Create a UM dial plan](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan).

You have the following options when you're configuring multiple DID extension numbers for UM-enabled users:

- **One extension for fax without Unified Messaging and one for voice**: This type of configuration is enabled on a per-user basis and is used when you have extra or unused DID extension numbers available. One DID extension number is published as the user's voice mail number and the other DID extension number is published as the user's fax number. Voice calls that are answered because the user doesn't answer the phone or a busy signal is encountered are forwarded to a Mailbox server, and a voice message is created and sent to the UM-enabled user's mailbox. The other extension number can be connected to a fax machine or to another computer that has a fax modem. With this configuration, a Mailbox server doesn't process fax calls, and fax messages aren't sent to the UM-enabled user's mailbox.

- **One extension for fax and one for voice**: This type of configuration is enabled on a per-user basis and can be used when your organization has many DID extension numbers available. In this configuration, both DID extension numbers that are answered because the user doesn't answer the phone or a busy signal is encountered are forwarded to a Mailbox server, which creates a voice or fax message depending on the DID extension number that's called. Although the user publishes one number for voice and one for fax, the Mailbox server detects the type of call that's being received on the DID extension number and can create a voice or fax message from calls to either of the DID extension numbers. This is very useful when a user doesn't have a separate fax machine or a dedicated computer that has a fax modem to answer incoming fax calls.

- **One "phantom" extension for fax and one for voice**: This type of configuration is enabled on a per-user basis. It's essentially the same as the configuration that uses two DID numbers (one for fax and one for voice). However, in this configuration, the number that's published for fax calls for the UM-enabled user is configured on the PBX as a "phantom" extension. Incoming calls that are received on this "phantom" DID extension number are always forwarded to a Mailbox server.

    The advantage of this type of configuration is that incoming fax calls are answered by a Mailbox server. When the phone rings but isn't answered, a fax is created and forwarded by the Mailbox server to the UM-enabled user's mailbox without disturbing the user. This happens automatically because no telephone or fax device is positioned close to the user, and the user doesn't hear the ring of the incoming call.

    The disadvantages of this kind of configuration are that you must have additional DID extensions available and you must configure the PBX or IP PBX to forward the call to a Mailbox server.

## Central fax telephone number

When you enable a user for Unified Messaging by using the **Enable UM Mailbox** page or the **Enable-UMMailbox** cmdlet, you must specify at least a single extension number for the user. However, if you need to configure a central fax number for incoming faxes, you must set up a separate UM enabled mailbox that is used to receive all faxes.

In some organizations, especially those that receive many faxes each day, you might want to publish one fax number for the whole organization. This fax number would be used by all callers when they submit faxes to users in the organization.

Publishing one fax number for the whole organization enables your organization to control the types of faxes that are received by users. The advantage of this configuration is that it requires only a single DID extension number or an external telephone number. Also, it doesn't require a separate DID number for faxing for each UM-enabled user. However, it does require a "fax secretary" or other person to distribute the incoming faxes to users within the organization based on information that's included on the fax cover page or in the fax message itself.

> [!NOTE]
> Using a central fax number with optical character recognition (OCR) isn't available in Exchange Unified Messaging. This kind of configuration can use a central fax number. However, instead of having to be routed to the recipient by a person, the faxing software receives the fax, performs OCR, and then tries to locate the recipient based on the information on the cover page or fax message.

Using a single fax number for the whole organization is useful in the following situations:

- A user within the organization receives too many faxes in their mailbox to manage them effectively.

- A user receives too many spam faxes in their mailbox.

- Business needs are too complex to warrant creating a transport rule to accept incoming faxes and route them to the intended mailbox. This might not be practical, for example, if your organization requires that you route certain faxes to one group and other faxes to another group. For more information, see [Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md).

- Filtering fax messages by using Outlook isn't effective.

## Journaling UM fax messages

Many organizations that implement journaling may also use Unified Messaging to consolidate their email, voice mail, and fax infrastructure. However, you may not want the journaling process to generate journal reports for messages that are generated by Unified Messaging. In this case, you can decide whether to journal voice mail messages and missed call notification messages that are handled by a Mailbox server or to skip such messages. If your organization doesn't require journaling of voice mail and missed call notifications, you can reduce the hard disk space that's required to store journal reports by skipping such messages. When you enable or disable the journaling of voice mail messages and missed call notification messages, your change is applied to all Transport services in your organization. For more information, see [Journaling](journaling-exchange-2013-help.md).

> [!NOTE]
> Messages that contain faxes that are generated by a Mailbox server are always journaled, even if you configure a journal rule that specifies not to journal Unified Messaging voice mail and missed call notification messages.
