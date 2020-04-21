---
title: 'Unified Messaging: Exchange 2013 Help'
TOCTitle: Unified Messaging
ms:assetid: 004b5d1a-cae8-4034-ab65-db41bd2f7b97
ms:mtpsurl: https://technet.microsoft.com/library/JJ150478(v=EXCHG.150)
ms:contentKeyID: 47559933
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Unified Messaging in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Unified Messaging (UM) enables users to use voice mail and other features, including Outlook Voice Access and Call Answering Rules. UM combines voice messaging and email messaging into one mailbox that can be accessed from many different devices. Users can listen to their messages from their email Inbox or by using Outlook Voice Access from any telephone. You have control over how users place outgoing calls from UM, and the experience people have when they call in to your organization.

Today, IT administrators frequently manage the voice mail or telephony networks and the email systems or data networks for their organizations as separate systems. Voice mail and email are located in separate inboxes that are hosted on separate servers accessed through the desktop for email and through the telephone for voice mail.

Unified Messaging makes it possible for Exchange administrators to combine voice messaging and email messaging into one mailbox so their users can listen to their voice mail messages in their Inbox or by using Outlook Voice Access from any telephone. It uses the Exchange store for both email and voice messages.

## New features

Unified Messaging (UM) was first introduced in Microsoft Exchange Server 2007 and was also available in Exchange 2010. The Unified Messaging feature set in Exchange 2013 is similar to previous versions of Exchange. However, new features have been added and there have been architectural changes. Unified Messaging is now considered a component or sub feature of the voice-related features that are offered in Exchange 2013. The term *Unified Messaging* is still widely used in Exchange Management Shell cmdlets and UM-related services, and all Unified Messaging components (including dial plans, auto attendants, UM mailbox policies, and UM IP gateways) along with the ability to manage those UM components, are located within the Unified Messaging node in the navigation pane of the Exchange admin center (EAC).

The following topics are gateways to information about new or enhanced features found in Exchange 2013 Unified Messaging:

- [Voice architecture changes](voice-architecture-changes-exchange-2013-help.md)

- [IPv6 support in Unified Messaging](ipv6-support-in-unified-messaging-exchange-2013-help.md)

- [Voice mail preview enhancements](voice-mail-preview-enhancements-exchange-2013-help.md)

- [Unified Messaging cmdlet updates](unified-messaging-cmdlet-updates-exchange-2013-help.md)

## Unified Messaging features

The voice mail features found in Unified Messaging offer benefits for both end users and IT administrators.

## Features for end users

When you deploy Unified Messaging, users can access voice mail, email, and calendar information that's located in their mailbox from an email client, for example, Outlook or Microsoft Outlook Web App, from a mobile phone with Microsoft Exchange ActiveSync set up, such as a Windows Phone, or from a telephone. Additionally, users can use the following features:

- **Access to Exchange information**: UM-enabled users can access a full set of voice mail features from Internet-capable mobile phones, Microsoft Office Outlook 2007 or later versions, and Outlook Web App. These features include many voice mail configuration options and the ability to play a voice message from either the Reading Pane, using an integrated Windows Media Player, or the message list, using computer speakers.

- **Play on Phone**: The Play on Phone feature lets UM-enabled users play voice messages over a telephone. If the user works in an office cubicle, is using a public computer or a computer that isn't enabled for multimedia, or is listening to a voice message that's confidential, they might not want to or be able to listen to a voice message through computer speakers. They can play the voice message using any telephone, including a home, office, or mobile telephone.

- **Voice mail form**: The voice mail form resembles the default email form. It gives users an interface for performing actions such as playing, stopping, or pausing voice messages, playing voice messages on a telephone, and adding and editing notes.

    The voice mail form includes the embedded Windows Media Player and an Audio notes field. The embedded Windows Media Player and notes field are displayed in either the Reading Pane when users preview a voice message or in a separate window when they open the voice message. If users aren't enabled for Unified Messaging, or if a supported email client hasn't been installed on the client computer, they view voice messages as email attachments, and the voice mail form isn't available.

- **User configuration**: A user who's enabled for Unified Messaging can configure several voice mail options for Unified Messaging using Outlook Web App. For example, the user can configure telephone access numbers and the voice mail Play on Phone number, and can then reset a voice mail access PIN.

- **Call answering**: Call answering includes answering incoming calls on behalf of users, playing their personal greetings, recording messages, and submitting them for delivery to their Inbox as an email message.

- **Call Answering Rules**: Call Answering Rules lets users who are enabled for voice mail determine how their incoming call answering calls should be handled. The way call answering rules are applied to incoming calls is similar to the way Inbox rules are applied to incoming email messages. By default, no call answering rules are configured. If an incoming call is answered by the Mailbox server, the caller is prompted to leave a voice message for the called party. Using call answering rules, a caller can:

  - Leave a voice message for the UM-enabled user.

  - Transfer to an alternate contact of the UM-enabled user.

  - Transfer to the alternate contact's voice mail.

  - Transfer to other phone numbers that the UM-enabled user has configured.

  - Use the Find Me feature or locate the UM-enabled user via a transfer from an operator.

- **Voice Mail Preview**: The Mailbox server uses Automatic Speech Recognition (ASR) on newly created voice mail messages. When users receive voice messages, the messages contain both a recording and text that's been created from the voice recording. Users see the voice message text displayed in an email message from within Outlook Web App or another supported email client.

- **Message Waiting Indicator**: Message Waiting Indicator is a feature found in most legacy voice mail systems and can refer to any mechanism that indicates the existence of a new message. In Exchange 2007, this functionality was provided by a third-party application, which indicated receipt of a new voice message by lighting the lamp on the desk phone. This feature was added to Exchange 2010, and third-party software is no longer needed. Enabling or disabling Message Waiting Indicator is done on the user's mailbox or on a UM mailbox policy.

- **Missed call and voice mail notifications using SMS**: When users are part of a hybrid or Office 365 deployment, and they configure their voice mail settings with their mobile phone number and configure call forwarding, they can receive notifications about missed calls and new voice messages on their cell phones in a text message via the Short Messaging Service (SMS). However, to receive these types of notifications, the users must first configure text messaging and also enable notifications on their account.

- **Protected Voice Mail**: Protected Voice Mail is Unified Messaging functionality that enables users to send private mail. This mail is protected by Active Directory Rights Management Services (AD RMS), and users are restricted from forwarding, copying, or extracting the voice file from email. Protected Voice Mail increases the confidentiality of Unified Messaging, and lets users limit the audience for voice messages. This functionality is similar to the way private email messages were handled in Exchange 2007 but now it also applies to voice mail messages.

- **Outlook Voice Access**: There are two Unified Messaging user interfaces available to UM-enabled users: the telephone user interface (TUI) and the voice user interface (VUI). These two interfaces together are called Outlook Voice Access. Outlook Voice Access users can use Outlook Voice Access when they access the voice mail system from an external or internal telephone. UM-enabled users who dial in to the voice mail system can access their mailbox using Outlook Voice Access. Using a telephone, a UM-enabled user can:

  - Access voice mail

  - Listen to, forward, or reply to email messages

  - Listen to calendar information

  - Access or dial contacts who are stored in the organization's directory or a single contact or contact group located in their personal Contacts.

  - Accept or cancel meeting requests

  - Set a voice message to let callers know the called party is away

  - Set user security preferences and personal options

- **Group addressing using Outlook Voice Access**: In Exchange 2007, users could use either the telephone user interface (TUI) or voice user interface (VUI) in Outlook Voice Access to send email and voice messages when they signed in to their mailbox. However, users could only send a single email message to a single user in their personal Contacts, to multiple recipients from the directory by adding each recipient individually, or by adding the name of a distribution list from the directory for your organization. In Exchange 2013, when a user signs in to their mailbox using Outlook Voice Access, they can also send email and voice messages to users in a group stored in their personal Contacts.

## Administrative features

Currently, most users and IT departments manage their voice mail separately from their email. Voice mail and email exist as separate inboxes hosted on separate servers accessed through the desktop for email and through the telephone for voice mail. Unified Messaging offers an integrated store for all messages and access to content through the computer and the telephone.

Exchange administrators can manage Unified Messaging using the same interface they use to manage the rest of Exchange, using the Exchange admin center (EAC) and the Exchange Management Shell. They can:

- Manage voice mail and email from a single platform

- Manage Unified Messaging using scriptable commands

- Build highly available and reliable Unified Messaging infrastructures

Exchange 2013 Unified Messaging offers administrators:

- **A complete voice mail system**: Unified Messaging offers a complete voice mail solution using a single store, transport, and directory infrastructure. The store is provided by a Mailbox server and forwarding of incoming calls from a VoIP gateway or IP PBX is handled by a Client Access server. All email and voice mail messages can be managed from a single management point, using a single administration interface and tool set.

- **An Exchange security model**: The Microsoft Exchange Unified Messaging service on a Mailbox server and the Microsoft Exchange Unified Messaging Call Router service on a Client Access server run as a single Exchange server account.

- **Consolidation of voice mail systems**: Currently, most voice messaging systems require that all the voice messaging components be installed in every physical office location in an organization. In this kind of arrangement, the voice messaging systems in branch offices are located outside the central office and must be administered onsite. This frequently results in increased administration costs and complexity. Unified Messaging lets you manage your voice mail system from a central location. To create a centralized management system for Unified Messaging, you can place some of your Exchange servers in a datacenter or other location, and the remainder of your Exchange servers on-premises and then deploy VoIP gateways, IP PBXs, or Session Border Controllers (SBCs) in each of your branch offices to replace the voice messaging system for each branch office. Deploying a centralized voice messaging system this way can result in a significant savings in hardware and administrative costs.

- **Built-in Unified Messaging administrative roles**: The set of UM-specific administrative roles for managing Unified Messaging and voice mail features includes the following:

  - UM Mailboxes

  - UM Prompts

  - Unified Messaging

- **Incoming fax support**: Exchange 2013 provides built-in incoming fax support for users who have a UM-enabled mailbox. They can receive fax messages via calls placed to their extension number.

    Customers who require a fax solution will have to deploy a fax partner solution. Fax partner solutions are available from several fax partners. The fax partner solutions are designed to be tightly integrated with Exchange and enable UM-enabled users to receive incoming fax messages.

- **Support for multiple languages**:    All available language packs contain the Text-to-Speech (TTS) engine and the prerecorded prompts for a specified language and ASR support. However, only some language packs contain support for Voice Mail Preview. The US English (en-US) language pack is included on the installation media and additional UM language packs can be downloaded from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkid=266542).

- **Auto attendant**: An auto attendant is a set of voice prompts that gives external and internal users access to the voice mail system. Users can use the telephone keypad or speech inputs to move through the auto attendant menu, place a call to a user, or locate a user in your organization and then place a call to them. An auto attendant gives the administrator the ability to:

  - Create a customized menu for external users.

  - Define informational greetings, business hours greetings, and non-business hours greetings.

  - Define holiday schedules.

  - Describe how to search the organization's directory.

  - Describe how to connect to a user's extension so that external callers can call users by specifying their extension.

  - Describe how to search the organization's directory so that external callers can search the organization's directory and call a specific user.

  - Enable external users to call the operator.

## Planning and deploying UM

Unified Messaging requires that you integrate your Exchange Server deployment with the existing telephony system for your organization. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and to perform the correct planning steps to deploy and manage voice mail in Unified Messaging.

When you plan your Unified Messaging deployment, you must consider design and other issues that may affect your ability to reach your organizational goals when you deploy Unified Messaging. Generally, the simpler the Unified Messaging topology, the easier Unified Messaging is to deploy and maintain. Install as few Client Access and Mailbox servers and create as few Unified Messaging components like UM dial plans, auto attendants and UM mailbox policies as you need to support your business and organizational goals. Large enterprises with complex network and telephony environments, multiple business units, or other complexities will require more planning than smaller organizations with relatively straightforward Unified Messaging needs.

There are many areas that you must consider and evaluate to be able to successfully deploy Unified Messaging. You must understand the different aspects of Unified Messaging and each component and feature so that you can plan your Unified Messaging infrastructure and deployment appropriately. Allocating time to plan and work through these issues will help prevent problems when you deploy Unified Messaging in your organization.The following are some of the areas that you should consider and evaluate when planning for Unified Messaging in your organization:

- The needs of your organization.

- The security requirements in your organization.

- Your existing telephony, circuit-switched network, and your current voice mail system.

- Your current packet-switched IP network design. This includes your local area network (LAN) and WAN connectivity points and devices.

- Your current Active Directory environment.

- The number of users that you'll have to support.

- The number of Client Access and Mailbox servers you'll need.

- Whether you'll be integrating UM with Microsoft Lync Server to enable Enterprise Voice.

- The placement of VoIP gateways, telephony equipment, and Client Access and Mailbox servers.

- The type of UM deployment: on-premises or hybrid.

- The storage requirements for voice mail users.

## Managing UM with the EAC and the Shell

### EAC management

Exchange 2013 provides a single unified management console for your organization that includes all UM components and features. The Exchange admin center (EAC) provides a streamlined, optimized interface for management of on-premises, online, or hybrid deployments. The EAC in Exchange 2013 replaces the Exchange Management Console (EMC) and the Exchange Control Panel (ECP) in Exchange 2010. Some of the EAC features include:

- **List view**: The list view in EAC has been designed to remove limitations that existed in ECP. ECP was limited to displaying up to 500 objects and, if you wanted to view objects that weren't listed in the details pane, you needed to use searching and filtering to find those specific objects. In Exchange 2013, the viewable limit from within the EAC list view is approximately 20,000 objects. In addition, paging has been added so that you can page to the results. You can also configure page size and export to a CSV file.

- **Add/Remove columns to the Recipient list view**: You can choose which columns to view, and you can save your custom list views.

- **Secure the ECP virtual directory**: You can partition access from the Internet and Intranets from within the ECP IIS virtual directory to allow or disallow management features. With this feature, you can permit or deny access to users trying to access the EAC from the Internet outside of your organizational environment, while still allowing access to an end user's Outlook Web App Options.

- **Public Folder management**: In Exchange 2010 and Exchange 2007, public folders were managed through the Public Folder administration console. Public folders are now in the EAC, and you don't need a separate tool to manage them.

- **Notifications**: In Exchange 2013, the EAC now has a Notification viewer so that you can view the status of long-running processes and, if you choose, receive notification via an email message when the process completes.

- **Role Based Access Control (RBAC) user editor**: In Exchange 2010 you could use the RBAC User Editor to add users to management role groups. In Exchange 2013, the RBAC User Editor functionality is now in the EAC, and you don't need a separate tool to manage RBAC.

- **Unified Messaging tools**: In Exchange 2010 you could use the Call Statistics and User Call Logs tools to help provide UM statistics and information about specific calls for a UM-enabled user. In Exchange 2013, the Call Statistics and User Call Logs tools are now in the EAC, and you don't need a separate tool to manage them.

### Shell management

The Exchange Management Shell, built on Windows PowerShell technology, is a powerful command-line interface that enables automation of administrative tasks. With the Shell, you can manage every aspect of Exchange. You can enable new email accounts, create Send and Receive connectors, configure database properties, manage all aspects of Unified Messaging, and more. The Shell can perform every task that can be performed by the EAC plus tasks that can't be done in the EAC. In fact, when you do something in the EAC, it's the Shell that's doing the work behind the scenes.

## Unified Messaging documentation

The following table contains links to topics that will help you learn about and manage Exchange Unified Messaging.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Topic</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><a href="new-voice-mail-features-exchange-2013-help.md">New voice mail features</a></p></td>
<td><p>Learn about new features in Microsoft Exchange 2013.</p></td>
</tr>
<tr class="even">
<td><p><a href="planning-for-unified-messaging-exchange-2013-help.md">Planning for Unified Messaging</a></p></td>
<td><p>Learn about the concepts and information you need to plan a Unified Messaging deployment.</p></td>
</tr>
<tr class="odd">
<td><p><a href="deploying-voice-mail-and-um-exchange-2013-help.md">Deploying voice mail and UM</a></p></td>
<td><p>Learn about the requirements and steps involved in deploying voice mail and UM.</p></td>
</tr>
<tr class="even">
<td><p><a href="um-languages-prompts-and-greetings-exchange-2013-help.md">UM languages, prompts, and greetings</a></p></td>
<td><p>Learn about UM language packs and language settings.</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/telephone-system-integration-with-um/telephone-system-integration-with-um">Telephone system integration with UM</a></p></td>
<td><p>Learn about integrating your telephony network with UM.</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/connect-voice-mail-system">Connect your voice mail system to your telephone network</a></p></td>
<td><p>Learn how to use and configure UM components to connect your telephony network to Exchange UM.</p></td>
</tr>
<tr class="odd">
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/automatically-answer-and-route-calls">Automatically answer and route incoming calls</a></p></td>
<td><p>Learn how to create UM auto attendants and manage settings for navigation menus, greetings, and business and non-business hours.</p></td>
</tr>
<tr class="even">
<td><p><a href="set-up-https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-for-users">Set up voice mail for users</a></p></td>
<td><p>Learn how to create and manage UM mailbox policies and how to enable users for UM.</p></td>
</tr>
<tr class="odd">
<td><p><a href="set-up-client-voice-mail-features-exchange-2013-help.md">Set up client voice mail features</a></p></td>
<td><p>Learn how to set up client features to enable users to access and manage their voice mail messages.</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-outlook-voice-access-pin-security/set-outlook-voice-access-pin-security">Set Outlook Voice Access PIN security</a></p></td>
<td><p>Learn how to set PIN requirements for Outlook Voice Access users.</p></td>
</tr>
<tr class="odd">
<td><p><a href="protect-voice-mail-exchange-2013-help.md">Protect voice mail</a></p></td>
<td><p>Learn how to use UM to protect voice messages.</p></td>
</tr>
<tr class="even">
<td><p><a href="https://docs.microsoft.com/exchange/voice-mail-unified-messaging/run-voice-mail-call-reports/run-voice-mail-call-reports">Run reports for voice mail calls</a></p></td>
<td><p>Learn about UM call reports.</p></td>
</tr>
</tbody>
</table>
