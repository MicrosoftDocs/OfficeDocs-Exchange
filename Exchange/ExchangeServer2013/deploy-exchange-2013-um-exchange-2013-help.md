---
title: 'Deploy Exchange 2013 UM: Exchange 2013 Help'
TOCTitle: Deploy Exchange 2013 UM
ms:assetid: d147d4b1-32d7-476b-b76f-ee3c0b35ba49
ms:mtpsurl: https://technet.microsoft.com/library/JJ673564(v=EXCHG.150)
ms:contentKeyID: 49315527
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploy Exchange 2013 UM

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Unified Messaging (UM) requires that you integrate your Exchange Server deployment with the existing telephony system for your organization. A successful deployment requires you to make a careful analysis of your existing telephony infrastructure and perform the correct planning steps to deploy and manage voice mail in Unified Messaging.

## Before you deploy

Before you deploy Unified Messaging, we recommend that you familiarize yourself with the concepts in the following topics:

- [UM dial plans](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-dial-plans)

- [UM IP gateways](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-ip-gateways)

- [UM services](um-services-exchange-2013-help.md)

- [UM hunt groups](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-hunt-groups)

- [Automatically answer and route incoming calls](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/automatically-answer-and-route-calls/automatically-answer-and-route-calls)

- [UM mailbox policies](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policies)

- [Voice mail for users](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/voice-mail-for-users)

## Deploying Unified Messaging

Whether you're deploying UM using IP Private Branch eXchanges (IP PBXs), VoIP gateways, or Microsoft Lync Server, all the deployment options for Unified Messaging have several steps in common. These steps are required to create a scalable and highly available system to support large numbers of Unified Messaging users. These steps are as follows:

1. Deploy and configure your telephony components for Unified Messaging.

2. Verify that you've correctly installed the Client Access server running the Microsoft Exchange Unified Messaging Call Router service and the Mailbox server running the Microsoft Exchange Unified Messaging service.

3. Create and configure the required Unified Messaging components.

4. Perform any post-deployment tasks for Unified Messaging.

## Deploy and configure telephony components

To successfully deploy Unified Messaging in an Exchange organization, the Exchange administrator needs to become knowledgeable about data networking concepts and telephony terminology and concepts and be able to correctly configure the telephony components that UM requires. Performing a new deployment or upgrading a legacy voice mail system requires significant knowledge about telephony networks and Unified Messaging.

Generally, you need to complete three tasks to successfully configure the telephony components that UM requires:

1. **Provision PBX lines**: The first step in deploying a scalable UM solution is to provision PBX lines.

2. **Organize channels**: After you provision PBX-based voice channels, you can organize the channels into hunt groups.

3. **Deploy VoIP gateways**: After you organize your voice channels as hunt groups, you need to end these channels at VoIP gateways. VoIP gateways are used with a legacy PBX to convert the circuit-switched protocols found on a telephony network to IP-based packet-switched protocols.

When you integrate your organization's telephony and data networks during the deployment of Unified Messaging, you need to configure the telephony and data networking components correctly. You also need to configure the following components or interfaces to successfully deploy Unified Messaging:

- **Configure the connection from the PBXs in your organization to communicate with your VoIP gateways.**: For details, see [Connect a VoIP gateway to communicate with a PBX](connect-a-voip-gateway-to-communicate-with-a-pbx-exchange-2013-help.md).

- **Configure the connection from the VoIP gateway interface to the PBX.**: For more information about how to configure your PBX interface to communicate with your supported VoIP gateway, see the product documentation that's specific to your PBX or see [Connect a VoIP gateway to communicate with a PBX](connect-a-voip-gateway-to-communicate-with-a-pbx-exchange-2013-help.md).

- **Configure the connection from the VoIP gateway interface to the Client Access and Mailbox servers.**: For details, see [Connect a VoIP gateway, IP PBX, or session border controller to UM](connect-a-voip-gateway-ip-pbx-or-session-border-controller-to-um-exchange-2013-help.md).

- **Configure the connection from Client Access and Mailbox servers to the VoIP gateway interface.**: For details, see [Connect UM to a supported VoIP gateway](connect-um-to-a-supported-voip-gateway-exchange-2013-help.md).

## Install the Mailbox and Client Access servers

Different deployment paths are available for organizations that plan to deploy Exchange Unified Messaging. Although these paths all lead to the same end (a successful deployment of Unified Messaging) each path is slightly different because each customer's needs and starting points are different. However, generally there are common starting points and paths that cover all supported deployment scenarios, including new installations and upgrades. Follow these steps to deploy your Client Access and Mailbox servers:

1. Verify that your existing infrastructure meets certain prerequisites. For details, see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

2. Deploy your new Exchange 2013 organization. For details, see [Install Exchange 2013 using the Setup wizard](install-exchange-2013-using-the-setup-wizard-exchange-2013-help.md).

   > [!WARNING]
   > You must deploy at least one Exchange 2013 Mailbox server in your organization before you configure the VoIP gateways or IP PBXs to send UM SIP and RTP traffic to the Exchange 2013 Client Access servers.

3. Verify that you've correctly installed the Client Access and Mailbox servers. After you install the servers, we recommend that you verify the installation and review the server setup logs. For details, see [Verify an Exchange 2013 installation](verify-an-exchange-2013-installation-exchange-2013-help.md).

## Add the required UM language packs

UM language packs enable callers and Outlook Voice Access users to interact with the voice mail system in multiple languages. After you install an additional language pack on a Mailbox server, callers and Outlook Voice Access users can hear email messages and interact with the voice mail system in that language.

When you first install Exchange, U.S. English will be the default language, and the only available language option for your dial plan. After you install a UM language pack on a Mailbox server, the language associated with the language pack will be listed as an available option when you configure the default language for the dial plan. By default, because UM auto attendants are associated with a UM dial plan when they're created, they use the default language setting of the associated UM dial plan. However, this setting can be changed after the UM auto attendant is created.

You can add UM language packs by using the Setup.exe command or by running the *\<UMLanguagePack\>*.exe installation program after you've downloaded the UM language pack from [Exchange Server 2013 UM Language Packs](https://www.microsoft.com/download/details.aspx?id=35368). However, you have to use the Setup.exe command to remove a UM language pack. There's no Exchange Management Shell cmdlet that you can use to add or remove languages from a Mailbox server. For more information about how to install a UM language pack, see [Install a UM language pack](install-a-um-language-pack-exchange-2013-help.md).

> [!NOTE]
> By default, when you install a Mailbox server, the U.S. English language (en-US) is installed. It can't be removed unless you remove the Mailbox server from the computer.

## Create and configure UM components

Several UM components are required for the deployment and operation of Unified Messaging. Unified Messaging components connect the telephony infrastructure with the Unified Messaging environment. After you've successfully installed the Client Access and Mailbox servers, follow these steps.

## Step 1: Create and configure UM dial plans

UM dial plans are important to the operation of Unified Messaging and are required to successfully deploy Unified Messaging on your network. After you've successfully installed your Client Access and Mailbox servers, a UM dial plan will be the first component that you'll create.

By default, UM dial plans and Client Access and Mailbox servers that are associated with the dial plan send and receive data without using encryption. In Unsecured mode, the VoIP and SIP traffic won't be encrypted. When you create the dial plan or after you've created the dial plan, you can configure the dial plan to encrypt the VoIP and SIP traffic by using Mutual Transport Layer Security (mutual TLS). If you will be using mutual TLS, you will set the dial plan to SIP secured or Secured, set the UM start mode to TLS or Dual, and create and distribute a trusted certificate be the Exchange servers and the VoIP gateways, IP PBXs or session border controllers (SBCs). After you configure the VoIP security setting, you'll have to configure the startup mode for the Client Access and Mailbox servers. For details, see [Configure the startup mode on a Mailbox server](configure-the-startup-mode-on-a-mailbox-server-exchange-2013-help.md) or [Configure the startup mode on a Client Access server](configure-the-startup-mode-on-a-client-access-server-exchange-2013-help.md).

Perform the following procedure to create a new UM dial plan.

## Create a UM dial plan

1. In the Exchange admin center (EAC), navigate to **Unified Messaging** \> **UM dial plans**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. On the **New UM Dial Plan** page, complete the following boxes:

   - **Name**: Type the name of the dial plan. A UM dial plan name is required and must be unique. The name you type is used only for display purposes in the EAC and the Shell. The maximum length of a UM dial plan name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

     > [!IMPORTANT]
     > Although the box for the name of the dial plan can accept 64&nbsp;characters, the name of the dial plan can't be longer than 49&nbsp;characters. This is because, when you create a dial plan, a default UM mailbox policy is also created that has the name <EM>&lt;DialPlanName&gt;</EM> Default Policy. The <EM>name</EM> parameter for both the UM dial plan and UM mailbox policy can be 64&nbsp;characters long.

   - **Extension length (digits)**: Enter the number of digits for extension numbers in the dial plan. The number of digits for extension numbers is based on the telephony dial plan created on a PBX. For example, if a user associated with a telephony dial plan dials a four-digit extension to call another user in the same telephony dial plan, you select 4 as the number of digits in the extension.

     This is a required box that has a value range from 1 through 20. The typical extension length is from 3 through 7 numbers. If your existing telephony environment includes extension numbers, you must specify a number of digits that matches the number of digits in those extensions.

     When you create a Telephone extension dial plan, you're required to enter an extension number for the user when they're linked to a Telephone extension dial plan. An extension number is also required with Session Initiation Protocol (SIP) dial plans or E.164 dial plans when a UM-enabled user is linked to a SIP URI or E.164 dial plan. This extension number is used by Outlook Voice Access users when they access their Exchange mailbox.

   - **Dial plan type**: A Uniform Resource Identifier (URI) is a string of characters that identifies or names a resource. The main purpose of this identification is to enable VoIP devices to communicate with other devices over a network using specific protocols. URIs are defined in schemes that define a specific syntax and format and the protocols for the call. In simple terms, this format is passed from the IP PBX or PBX. After you create a UM dial plan, you won't be able to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type. You can select one of the following URI types for the dial plan:

     - **Telephone extension**: This is the most common URI type. The calling and called party information from the VoIP gateway or IP Private Branch eXchange (PBX) is listed in one of the following formats: Tel:512345 or 512345@\<*IP address*\>. This is the default URI type for dial plans.

     - **SIP URI**: Use this URI type if you must have a Session Initiation Protocol (SIP) URI dial plan such as an IP PBX that supports SIP routing or if you're integrating Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server and Unified Messaging. The calling and called party information from the VoIP gateway. IP PBX, or Communications Server 2007 R2 or Lync Server is listed as a SIP address in the following format: sip:\<*username*\>@\<*domain* or *IP address*\>:Port.

     - **E.164**: E.164 is an international numbering plan for public telephone systems in which each assigned number contains a country code, a national destination code, and a subscriber number. The calling and called party information sent from the VoIP gateway or IP PBX is listed in the following format: Tel:+14255550123.

       > [!WARNING]
       > After you create a dial plan, you will be unable to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type.

   - **VoIP security mode**: Use this drop-down list to select the VoIP security setting for the UM dial plan. You can select one of the following security settings for the dial plan:

     - **Unsecured**: By default, when you create a UM dial plan, it is set to not encrypt the SIP signaling or RTP traffic. In unsecured mode, the Client Access and Mailbox servers associated the UM dial plan send and receive data from VoIP gateways, IP PBXs, SBCs and other Client Access and Mailbox servers using no encryption. In unsecured mode, neither the Realtime Transport Protocol (RTP) media channel nor the SIP signaling information is encrypted.

     - **SIP secured**  When you select **SIP secured**, only the SIP signaling traffic is encrypted, and the RTP media channels still use TCP, which isn't encrypted. With SIP secured, Mutual Transport Layer Security (TLS) is used to encrypt the SIP signaling traffic and VoIP data.

     - **Secured**: When you select **Secured**, both the SIP signaling traffic and the RTP media channels are encrypted. Both the secure signaling media channel that uses Secure Realtime Transport Protocol (SRTP) and the SIP signaling traffic use mutual TLS to encrypt the VoIP data.

   - **Country/Region code**: Use this box to type the country/region code number to be used for outgoing calls. This number will automatically be prepended to the telephone number that's dialed. This box accepts from 1 through 4 digits. For example, in the United States, the country/region code is 1. In the United Kingdom, it's 44.

3. Click **Save**.

    > [!IMPORTANT]
    > In previous versions of Exchange, the Unified Messaging server had to be added to a UM dial plan. In Exchange 2013, Client Access and Mailbox servers can't be associated with a Telephone extension or E.164 dial plan. Client Access and Mailbox servers will answer all incoming calls for all types of dial plans. However, if you're integrating UM with Microsoft Lync Server, you must add all Client Access and Mailbox servers to all SIP URI dial plans to enable call routing to work correctly with Lync Server.

## Step 2: Create and configure your UM IP gateways

A UM IP gateway represents either a VoIP gateway hardware device or an IP PBX. The combination of the UM IP gateway and a UM hunt group establishes a link between a VoIP gateway or IP PBX and a UM dial plan.

If you've created or enabled VoIP security on a dial plan, the UM IP gateway that you create by using one of the following procedures will be associated with a UM dial plan that uses VoIP security. In that case, you must use a fully qualified domain name (FQDN) to create the UM IP gateway, and not an IP address. You must also configure the UM IP gateway to listen on TCP port 5061. To configure a UM IP gateway to listen on TCP port 5061, run the following command: `Set-UMIPGateway -identity MyUMIPGateway -Port 5061`. You must also verify that any VoIP gateways or IP PBXs have also been configured to listen on port 5061 for mutual TLS.

Perform the following procedure to create a new UM IP gateway.

## Create a UM IP gateway

1. In the EAC, navigate to **Unified Messaging** \> **UM IP Gateways**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. On the **New UM IP gateway** page, enter the following information:

   - **Name**: Use this box to specify a unique name for the UM IP gateway. This is a display name that appears in the EAC. If you have to change the display name of the UM IP gateway after it's been created, you must first delete the existing UM IP gateway, and then create another UM IP gateway that has the appropriate name. The UM IP gateway name is required, but it's used for display purposes only. Because your organization may use multiple UM IP gateways, we recommend that you use meaningful names for your UM IP gateways. The maximum length of a UM IP gateway name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

   - **Address**: You can configure a UM IP gateway with either an IP address or a fully qualified domain name (FQDN). Use this box to specify the IP address configured on the VoIP gateway, SIP-enabled PBX, IP PBX, or SBC, or an FQDN. This box accepts only FQDNs that are valid and formatted correctly.

     You can enter alphabetical and numeric characters in this box. IPv4 addresses, IPv6 addresses, and FQDNs are supported. If you want to use mutual TLS between a UM IP gateway and a dial plan operating in either SIP secured or Secured mode, you must configure the UM IP gateway with an FQDN. You must also configure it to listen on port 5061 and verify that any VoIP gateways or IP PBXs have also been configured to listen for mutual TLS requests on port 5061. To configure a UM IP gateway, run the following command: `Set-UMIPGateway -identity MyUMIPGateway -Port 5061`.

     If you use an FQDN, you must also make sure that you've correctly configured a DNS host record for the VoIP gateway so that the host name will be correctly resolved to an IP address. Also, if you use an FQDN instead of an IP address, and the DNS configuration for the UM IP gateway is changed, you must disable and then enable the UM IP gateway to make sure that configuration information for the UM IP gateway is updated correctly

   - **UM dial plan**: Click **Browse** to select the UM dial plan that you want to associate with the UM IP gateway. When you select a UM dial plan to associate with a UM IP gateway, a default UM hunt group is also created and associated with the UM dial plan that you selected. If you don't select a UM dial plan, you must manually create a UM hunt group and then associate that UM hunt group with the UM IP gateway that you create.

3. Click **Save**.

## Step 3: Create and configure your UM hunt groups (optional)

*Hunt group* is a term that's used to describe a group of PBX or IP PBX resources or extension numbers that are shared by users. Hunt groups are used to efficiently distribute calls into or out of a given business unit.

If you've created a UM IP gateway and associated the UM IP gateway with a UM dial plan, a default UM hunt group has been created. You can associate another UM hunt group with the same or a different UM IP gateway, depending on the number of UM IP gateways that you've created.

When you create a UM hunt group, you enable all Mailbox servers that are specified within the UM dial plan to communicate with a VoIP gateway. For details, see [UM hunt groups](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/um-hunt-groups).

## Create a UM hunt group

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM Dial Plan** page, under **UM Hunt Groups**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **New UM Hunt Group** page, complete the following boxes:

   - **Associated UM IP gateway**: This display-only box shows the name of the UM IP gateway that will be associated with the UM hunt group.

   - **Name**: Use this box to create the display name for the UM hunt group. A UM hunt group name is required and must be unique, but it's used only for display purposes in the EAC and the Shell. If you have to change the display name of the hunt group after it's been created, you must first delete the existing hunt group and then create another hunt group that has the appropriate name.

     If your organization uses multiple hunt groups, we recommend that you use meaningful names for your hunt groups. The maximum length of a UM hunt group name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

   - **Dial plan**: Click **Browse** to select the dial plan that will be associated with the UM hunt group. Associating a hunt group with a dial plan is required. A UM hunt group can be associated with only one UM IP gateway and one UM dial plan.

   - **Pilot identifier**: Use this box to specify a string that uniquely identifies the pilot identifier or pilot ID configured on the PBX or IP PBX.

     An extension number or a Session Initiation Protocol (SIP) Uniform Resource Identifier (URI) can be used in this box. Alphanumeric characters are accepted in this box. For legacy PBXs, a numeric value is used as a pilot identifier. However, some IP PBXs can use SIP URIs.

4. Click **Save**.

## Step 4: Create and configure a UM mailbox policy

UM mailbox policies are required when you enable users for Unified Messaging. The mailbox of each UM-enabled user must be linked to a single UM mailbox policy. After you create a UM mailbox policy, you link one or more UM-enabled mailboxes to the UM mailbox policy. This lets you control PIN security settings such as the minimum number of digits in a PIN or the maximum number of failed sign-in attempts for the UM-enabled users who are associated with the UM mailbox policy.

Every time that you create a UM dial plan, a UM mailbox policy is also created. The UM mailbox policy will be named \<*DialPlanName*\> Default Policy. However, if you have to create a new UM mailbox policy, perform the following procedure.

## Create a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM dial plan** page, under **UM Mailbox Policies**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **New UM Mailbox Policy** page, in the **Name** text box, enter the name of the new UM mailbox policy.

   Use this box to specify a unique name for the UM mailbox policy. This is a display name that appears in the EAC. If you must change the display name of the UM mailbox policy after it's been created, you must first delete the existing UM mailbox policy, and then create another UM mailbox policy that has the appropriate name. You can't delete a UM mailbox policy if any UM-enabled users are associated with it.

   The UM mailbox policy name is required, but it is used for display purposes only. Because your organization may use multiple UM mailbox policies, we recommend that you use meaningful names for your UM mailbox policies. The maximum length of a UM mailbox policy name is 64 characters, and it can include spaces. However, it can't include any of the following characters: " / \\ \[ \] : ; | = , + \* ? \< \>.

4. Click **Save** to save the new UM mailbox policy. When you save the UM mailbox policy, all of the default settings including PIN policies, voice mail features, and Protected Voice Mail settings are enabled. If you want to customize or change any default settings, use the **Set-UMMailbox** cmdlet to change the settings for the UM mailbox policy you just created.

## Step 5: Create and configure UM auto attendants (optional)

Unified Messaging enables you to create one or more UM auto attendants, depending on the needs of your organization. When you create a UM auto attendant, you create a voice menu system for your organization. Callers from outside or inside your organization can then move through the menu system to locate and place or transfer calls to users or departments in your organization.

Callers can move through the menu system by using dual tone multi-frequency (DTMF), also known as touchtone, or voice inputs. For Automatic Speech Recognition (ASR) to work, so users can use voice inputs, you must speech-enable the UM auto attendant.

Creating and using auto attendants is optional in Unified Messaging. However, if you want to create a new UM auto attendant, perform the following procedure.

## Create a UM auto attendant

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, select the UM dial plan for which you want to add an auto attendant, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

2. On the **UM Dial Plan** page, under **UM Auto Attendants**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. On the **New UM auto attendant** page, complete the following boxes:

   - **Name**: Use this box to create the display name for the UM auto attendant. A UM auto attendant name is required and must be unique. However, it's used only for display purposes in the EAC and the Shell.

     If you have to change the display name of the auto attendant after it's created, you must first delete the existing UM auto attendant and then create another auto attendant that has the appropriate name. If your organization uses multiple UM auto attendants, we recommend that you use meaningful names for your UM auto attendants. The maximum length of a UM auto attendant name is 64 characters, and it can include spaces.

     Although you can name a new UM auto attendant to include spaces, if you integrate Unified Messaging with Office Communications Server 2007 R2 or Microsoft Lync Server, the name of the auto attendant can't include spaces. Therefore, if you created an auto attendant that has spaces in the display name and you're integrating with Office Communications Server 2007 R2 or Lync Server, you must first delete that auto attendant and then create another auto attendant that doesn't include spaces in the display name.

   - **Create this auto attendant as enabled**: Select this check box to enable the auto attendant to answer incoming calls when you finish creating the UM auto attendant. By default, a new auto attendant is created as disabled.

     If you decide to create the UM auto attendant as disabled, you can use the EAC or the Shell to enable the auto attendant after you finish creating the auto attendant.

   - **Set the auto attendant to respond to voice commands**: Select this check box to speech-enable the UM auto attendant. If the auto attendant is speech-enabled, callers can respond to the system or custom prompts used by the UM auto attendant using touchtone or voice inputs. By default, the auto attendant won't be speech-enabled when it's created.

        For callers to use a speech-enabled auto attendant in a language other than U.S. English (en-US), you must install the appropriate UM language pack and configure the properties of the auto attendant to use this language. The en-US UM language pack is installed by default when you install a Mailbox server.

   - **Access numbers**: Use this box to enter the extension or telephone numbers that callers will use to reach the auto attendant. Type an extension number or telephone number in the box, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add the number to the list. The number of digits in the extension number or telephone number that you provide doesn't have to match the number of digits for an extension number configured on the associated UM dial plan. This is because direct calls are allowed to UM auto attendants.

     The number of extension numbers or pilot identifiers you can enter is unlimited. However, you may create a new auto attendant without listing an extension number or telephone number. An extension number or telephone number isn't required.

     You can edit or remove an existing extension number or pilot identifier. To edit an existing extension number or telephone number, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"). To remove an existing extension number or telephone number from the list, click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon").

4. Click **Save**.

## Post-deployment tasks for Unified Messaging

After you complete a new installation of the Client Access and Mailbox servers and have successfully deployed Unified Messaging, you should complete the post-deployment tasks. The post-deployment tasks will help you enable users for Unified Messaging, secure your UM deployment, and deploy incoming faxing for UM-enabled users.

## Enable users for voice mail

After you've deployed your VoIP gateways or IP PBXs, installed the Client Access and Mailbox servers, and created the components required for Unified Messaging, you need to enable your users for Unified Messaging. For details, see [Enable a user for voice mail](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail).

## Protect voice mail

Unified Messaging can be configured to use Active Directory Rights Management Services (AD RMS) to protect voice messages for an organization. This feature is known as Protected Voice Mail. When a voice message is protected, the recipient is not only blocked from forwarding the message, but UM also assures that only the intended recipient or recipients of the message can access its content. Protected voice messages can be accessed by using Microsoft Outlook 2010 or later, Outlook Web App, or Outlook Voice Access. For details, see [Protect voice mail](protect-voice-mail-exchange-2013-help.md).

## Mutual TLS for UM

To use mutual TLS to encrypt SIP and Realtime Transport Protocol (RTP) traffic that's sent and received by your Client Access and Mailbox servers, perform the following tasks:

- Run the Exchange Certificate wizard. For details, see [Deploying certificates for UM](deploying-certificates-for-um-exchange-2013-help.md).

- Import the certificate on the Client Access and Mailbox servers.

- Import the required certificates on the VoIP gateways and the IP PBX and Client Access and Mailbox servers in your organization.

- Configure VoIP security on the UM dial plans. For details, see [Configure the VoIP security setting](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/configure-voip-security-setting).

- Configure the startup mode on the Client Access and Mailbox servers. For details, see [Configure the startup mode on a Mailbox server](configure-the-startup-mode-on-a-mailbox-server-exchange-2013-help.md) and [Configure the startup mode on a Client Access server](configure-the-startup-mode-on-a-client-access-server-exchange-2013-help.md).

- Configure the UM IP gateways to listen on port 5061. For details, see [Configure the listening port](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/connect-voice-mail-system/configure-listening-port).

## PIN policies for UM-enabled users

In Unified Messaging, PIN policies are defined and configured on a UM mailbox policy. When you enable a user for Unified Messaging, you associate the user with an existing UM mailbox policy. The UM PIN policies that are configured on the UM mailbox policy should be based on the security requirements of your organization. For more information about how to configure PIN settings for UM-enabled users, see [Set Outlook Voice Access PIN security](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-outlook-voice-access-pin-security/set-outlook-voice-access-pin-security).

## Set up client voice mail features

After you've deployed your servers and the required UM components, there are several optional voice mail-related features that you can configure. For more information, see the following:

- [Setting up Outlook Voice Access](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/set-up-outlook-voice-access)

- [Allow voice mail users to forward calls](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/allow-voice-mail-users-to-forward-calls)

- [Allow users to see a voice mail transcript](https://docs.microsoft.com/exchange/voice-mail-unified-messaging/set-up-client-voice-mail-features/allow-users-to-see-a-voice-mail-transcript)

- [Enable voice mail users to receive faxes](enable-voice-mail-users-to-receive-faxes-exchange-2013-help.md)

> [!IMPORTANT]
> If you're integrating your Unified Messaging environment with Microsoft Lync Server, there are additional planning considerations. For details, see <A href="deploying-exchange-2013-um-and-lync-server-overview-exchange-2013-help.md">Deploying Exchange 2013 UM and Lync Server overview</A>.
