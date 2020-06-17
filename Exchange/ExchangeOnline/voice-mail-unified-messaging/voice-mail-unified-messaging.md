---
localization_priority: Normal
description: Unified Messaging (UM) enables users to use voice mail features, including Outlook Voice Access and Call Answering Rules. UM combines voice messaging and email messaging into one mailbox that can be accessed from many different devices. Users can read or listen to their messages from their email Inbox or by using Outlook Voice Access from any telephone. You have control over how users place outgoing calls, and the experience callers have when they call in to your organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7ddf1356-d4c0-41c7-a495-101633ae2f50
ms.reviewer: 
f1.keywords:
- NOCSH
title: Voice mail in Exchange Online Unified Messaging
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Voice mail in Exchange Online: Unified Messaging

> [!NOTE]
> Cloud Voicemail takes the place of Exchange Unified Messaging (UM) in providing voice messaging functionality for Skype for Business 2019 voice users who have mailboxes on Exchange Server 2019 or Exchange Online, and for Skype for Business Online voice users. For more information please check [Plan Cloud Voicemail service](https://docs.microsoft.com/skypeforbusiness/hybrid/plan-cloud-voicemail) and [Retiring Unified Messaging in Exchange Online](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Retiring-Unified-Messaging-in-Exchange-Online/ba-p/608991).

Unified Messaging (UM) enables users to use voice mail features, including Outlook Voice Access and Call Answering Rules. UM combines voice messaging and email messaging into one mailbox that can be accessed from many different devices. Users can read or listen to their messages from their email Inbox or by using Outlook Voice Access from any telephone. You have control over how users place outgoing calls, and the experience callers have when they call in to your organization.

Today, messaging administrators in organizations frequently manage the voice mail and email systems for their organizations as separate systems. Voice mail and email messages are located in separate mailboxes that are hosted on separate servers. Users can access messages through the desktop for email and through the telephone for voice mail.

UM in Office 365 makes it possible for online administrators to combine voice messaging and email messaging into one mailbox so their users can read or listen to their voice mail messages in their Inbox or by using Outlook Voice Access from any telephone. UM uses a user's mailbox to store both email and voice mail messages.

## Unified Messaging features
<a name="umfeatures"> </a>

The voice mail features found in UM offer benefits for both users and administrators in your organization and in Exchange Online.

### Features for users
<a name="enduser"> </a>

When you configure UM for your organization, users can access voice mail, email, personal Contacts and calendar information that's located in their mailbox from an email client, for example, Microsoft Outlook or Outlook on the web (formerly known as Outlook Web App), from a mobile phone with Microsoft Exchange ActiveSync set up, such as a Windows Phone, or from a telephone. Additionally, users can use the following features:

- **Access to their Exchange mailbox**: Users can access a full set of voice mail features from internet-capable mobile phones, Outlook 2007 or later versions, and Outlook on the web. These features include many voice mail configuration options and the ability to play a voice message from either the reading pane, using an integrated Windows Media Player, or the message list, using computer speakers.

- **Play on Phone**: The Play on Phone feature lets users play voice messages over a telephone. If the user works in an office cubicle, is using a public computer or a computer that isn't enabled for multimedia, or is listening to a voice message that's confidential, they might not want to or be able to listen to a voice message through computer speakers. They can play the voice message using any telephone, including a home, office, or mobile telephone.

- **Voice mail form**: The voice mail form resembles the default email form. It gives users an interface for performing actions such as playing, stopping, or pausing voice messages, playing voice messages on a telephone, and adding and editing notes.

    The voice mail form includes the embedded Windows Media Player and an Audio notes field. The embedded Windows Media Player and notes field are displayed either in the reading pane when users preview a voice message or in a separate window when they open the voice message. If users aren't enabled for UM, or if a supported email client hasn't been installed on the client computer, they view voice messages as email attachments, and the voice mail form isn't available.

- **User configuration**: Users can configure several voice mail options for UM using Outlook on the web. For example, the user can record personal greetings, configure missed call and text message notifications and a voice mail Play on Phone number, and reset a voice mail access PIN.

- **Call answering**: Call answering includes answering incoming calls on behalf of users, playing their personal greetings, recording messages, and then sending the voice mail to their Inbox as an email message.

- **Call Answering Rules**: The Call Answering Rules feature lets users who are enabled for voice mail determine how their incoming call answering calls should be handled. The way call answering rules are applied to incoming calls is similar to the way Inbox rules are applied to incoming email messages. By default, no call answering rules are configured. If an incoming call is answered, the caller is prompted to leave a voice message for the person being called. By using call answering rules, a caller can:

  - Leave a voice message for the user.

  - Transfer to an alternate contact of the user.

  - Transfer to the alternate contact's voice mail.

  - Transfer to other phone numbers that the user has configured.

  - Use the Find Me feature or locate the user through a transfer from an operator.

- **Voice Mail Preview**: Unified Messaging uses Automatic Speech Recognition (ASR) on newly created voice mail messages. When users receive voice messages, the messages contain both a recording and text that's been created from the voice recording. Users see the voice message text displayed in an email message from within Outlook on the web or another supported email client.

- **Message Waiting Indicator**: Message Waiting Indicator is a feature found in most legacy voice mail systems and can refer to any mechanism that indicates the existence of a new message. Enabling or disabling Message Waiting Indicator is done on the user's mailbox or on a UM mailbox policy.

- **Missed call and voice mail notifications using SMS**: When users are part of a hybrid or Office 365 deployment, and they configure their voice mail settings with their mobile phone number and configure call forwarding, they can receive notifications about missed calls and new voice messages on their mobile phones in a text message through the Short Messaging Service (SMS). However, to receive these types of notifications, the users must first configure text messaging and also enable notifications on their account.

- **Protected Voice Mail**: Protected Voice Mail is a feature that enables users to send private mail. This voice mail is protected and users are restricted from forwarding, copying, or extracting the voice file from email. Protected Voice Mail increases the confidentiality of voice mail messages, and lets users limit the audience for voice messages.

- **Outlook Voice Access**: There are two UM user interfaces available to users: the telephone user interface (TUI) and the voice user interface (VUI). These two interfaces together are called Outlook Voice Access. Outlook Voice Access users can use Outlook Voice Access when they access the voice mail system from an external or internal telephone. Users who dial in to the voice mail system can access their mailbox using Outlook Voice Access. However, when a user is searching the directory for your organization, they must use the key pad on their phone to search for a user. Using their voice to search the directory isn't available. Using a telephone, a UM-enabled user can:

  - Access voice mail.

  - Listen to, forward, or reply to email messages.

  - Listen to calendar information.

  - Access or dial contacts who are stored in the organization's directory or a single contact or contact group located in their personal Contacts.

  - Accept or cancel meeting requests.

  - Set a voice message to let callers know the called party is away.

  - Set user security preferences and personal options.

  - Search for users in the directory of the organization.

- **Group addressing using Outlook Voice Access**: Users can send a single email message to a single user in their personal Contacts, to multiple recipients from the directory by adding each recipient individually, or by adding the name of a distribution list from the directory for your organization. In UM in Office 365, when a user signs in to their mailbox using Outlook Voice Access, they can also send email and voice messages to users in a group stored in their personal Contacts.

### Administrative features
<a name="admin"> </a>

Currently, most users and IT departments manage their voice mail separately from their email. Voice mail and email exist as separate inboxes hosted on separate servers accessed through the desktop for email and through the telephone for voice mail. UM offers an integrated store for all messages and access to content through the computer and the telephone.

Exchange administrators can manage UM using the same interface they use to manage the rest of Exchange, using the Exchange admin center (EAC) and Exchange Online PowerShell. They can:

- Manage voice mail and email from a single platform.

- Manage UM using scriptable commands.

- Build a highly available and reliable UM infrastructure.

Office 365 UM offers administrators:

- **Consolidation of voice mail systems**: Currently, most voice messaging systems require that all the voice messaging components be installed in every physical office location in an organization. In this kind of arrangement, the voice messaging systems in branch offices are located outside the central office and must be administered onsite. This frequently results in increased administration costs and complexity. UM lets you manage your voice mail system from a central location. To create a centralized management system for UM, you integrate your VoIP gateways, IP PBXs or PBXs, and your phone system and then deploy session border controllers (SBCs) to connect your phone system with your Office 365 deployment. Deploying a centralized voice messaging system this way can result in a significant savings in hardware and administrative costs.

    > [!NOTE]
    > Exchange Online UM support for third-party PBX systems via direct connections from customer operated SBCs will end on December 1, 2019. See the Exchange team blog [New date for discontinuation of support for Session Border Controllers in Exchange Online Unified Messaging](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/New-date-for-discontinuation-of-support-for-Session-Border/ba-p/607853) for more information.

- **Built-in UM administrative roles**: The set of UM-specific administrative roles for managing UM and voice mail features includes the following:

  - UM Mailboxes

  - UM Prompts

  - Unified Messaging

- **Incoming fax support**: UM provides built-in incoming fax support for users who have a UM-enabled mailbox. They can receive fax messages through calls placed to their extension number.

    Customers who require a fax solution will have to deploy a fax partner solution. Fax partner solutions are available from several fax partners. The fax partner solutions are designed to be tightly integrated with Exchange and enable UM-enabled users to receive incoming fax messages. You can find a fax partner solution by visiting [Microsoft solution providers](https://www.microsoft.com/solution-providers/).

- **Support for multiple languages**: All available language packs contain support for the Text-to-Speech (TTS) engine and the prerecorded prompts for a specified language and ASR support. However, only some language packs contain support for Voice Mail Preview.

- **Auto attendant**: An auto attendant is a set of voice prompts that gives external and internal users access to the voice mail system. Users can use the telephone keypad or speech inputs to move through the auto attendant menu, place a call to a user, or locate a user in your organization and then place a call to them. An auto attendant gives the administrator the ability to:

  - Create a customized menu for external users.

  - Define informational greetings, business hours greetings, and non-business hours greetings.

  - Define holiday schedules.

  - Describe how to search the organization's directory.

  - Describe how to connect to a user's extension so that external callers can call users by specifying their extension.

  - Describe how to search the organization's directory so that external callers can search the directory and call a specific user.

  - Enable external users to call the operator.

## Planning and deploying UM
<a name="planningum"> </a>

Unified Messaging requires that you integrate your existing telephony system for your organization within Office 365 by using SBCs. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and to perform the correct planning steps to deploy and manage voice mail in UM.

> [!NOTE]
> Exchange Online UM support for third-party PBX systems via direct connections from customer operated SBCs will end on December 1, 2019. See the Exchange team blog [New date for discontinuation of support for Session Border Controllers in Exchange Online Unified Messaging](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/New-date-for-discontinuation-of-support-for-Session-Border/ba-p/607853) for more information.

When you plan to use UM in Office 365, you need to consider design and other issues that may affect your ability to reach your organizational goals when you configure UM. Generally, the simpler the UM setup is, the easier UM is to configure and maintain. As a general rule, create as few UM components like UM dial plans, auto attendants, and UM mailbox policies as you need to support your business and organizational goals. Large enterprises with complex network and telephony environments, multiple business units, or other complexities will require more planning than smaller organizations with relatively straightforward UM needs.

You need to consider and evaluate many areas to be able to successfully deploy UM. You need to understand the different aspects of UM and each component and feature so that you can plan your UM infrastructure and deployment appropriately. Allocating time to plan and work through these issues will help prevent problems when you deploy UM in your organization. The following are some of the areas that you should consider and evaluate when planning for UM in your organization:

- The needs of your organization.

- The security requirements in your organization.

- Your existing telephony, circuit-switched network, and voice mail system.

- Your current packet-switched IP network design. This includes your local area network (LAN) and WAN connectivity points and devices.

- The number of users that you'll have to support.

- Whether you'll be integrating UM with Lync Server to enable Enterprise Voice in Office 365.

- The placement of VoIP gateways, telephony equipment, and SBCs.

- The storage requirements for voice mail users.

## Managing UM with the EAC and Exchange Online PowerShell
<a name="managing"> </a>

### EAC management

Office 365 provides a single unified management console for your organization that includes all UM components and features. The EAC provides a streamlined, optimized interface for management of Exchange Online deployments. Some of the EAC features include:

- **List view**: The list view in the EAC has been designed to display recipients, mailboxes, and settings for features that you are using within your organization. Paging within the list view allows you to see results per page. You can also configure page size and the number of entries, and export entries to a CSV file.

- **Add/Remove columns in the Recipient list view**: You can choose which columns to view, and you can save your custom list views.

- **Public folder management**: Public folder management is available in the EAC, and you don't need separate tools to manage public folders.

- **Notifications**: The EAC now has a Notification viewer so that you can view the status of long-running processes and, if you choose, receive notification through an email message when the process completes.

- **Role Based Access Control (RBAC) User Editor**: Within Office 365, the RBAC User Editor functionality is in the EAC, and you don't need a separate tool to manage RBAC.

- **UM tools**: In Office 365 you can use the Call Statistics and User Call Logs tools to help provide UM statistics and information about specific calls for a user.

For more information about the EAC, see [Exchange admin center in Exchange Online](../exchange-admin-center.md).

### Exchange Online PowerShellShell management

Exchange Online PowerShell is a powerful command-line interface that enables automation of administrative tasks. Exchange Online PowerShell can perform every task that can be performed by the EAC plus tasks that can't be done in the EAC. In fact, when you do something in the EAC, it's Exchange Online PowerShell that's doing the work behind the scenes.

For more information about Exchange Online PowerShell, see [Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online-powershell).
