---
title: 'Manage a UM dial plan: Exchange 2013 Help'
TOCTitle: Manage a UM dial plan
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.DialPlanGeneralPropertyPage'
ms.assetid: a89735e4-36ec-49fb-ad0f-192fad37e801
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Manage a UM dial plan in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

After you create a Unified Messaging (UM) dial plan, you can view and configure a variety of settings. For example, you can configure the level of Voice over IP (VoIP) security, the audio codec, and dialing restrictions. The settings that you configure on the UM dial plan affect all users who are linked with the dial plan through a UM mailbox policy.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Server](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to view or configure UM dial plan settings

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to view or modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Dial Plan** page, click **Configure**. Use the configuration options to view specific dial plan settings and to enable or disable features as described in the following steps.

4. **General**: Use this page to view specific dial plan settings or to enable or disable features for UM-enabled users:

   - **Name**: This is the name of the dial plan that was created. The maximum length of a UM dial plan name is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

   - Although you can include spaces in a UM dial plan name, if you integrate Unified Messaging with Office Communications Server 2007 R2 or Microsoft Lync Server, the dial plan name can't include spaces. Therefore, if you created a dial plan with spaces in the display name, and you're integrating with Office Communications Server 2007 R2 or Lync Server, you must first delete that dial plan and then create another dial plan that doesn't include spaces in the display name.

     > [!IMPORTANT]
     > Although the box for the name of the dial plan can accept 64 characters, the name of the dial plan can't be longer than 49 characters. If you try to create a dial plan name that contains more than 49 characters, you'll receive an error message. The message will say that the UM mailbox policy couldn't be generated because the UM dial plan name is too long. This happens because, as mentioned earlier, when you create a dial plan a default UM mailbox policy named  _\<DialPlanName\>_ Default Policy is also created. When the 15 characters in Default Policy are added to the name of the dial plan, the total characters exceed the limit. The _Name_ parameter for both the UM dial plan and UM mailbox policy can be 64 characters. However, if the name of the dial plan is longer than 49 characters, the name of the default UM mailbox policy will be longer than 64 characters, and this isn't allowed.

   - **Extension length (digits)**: This is the number of digits in the extension numbers for users who are associated with this dial plan. For example, if a user associated with a dial plan dials a 4-digit extension to call another user in the same dial plan, select 4 as the number of digits in the extension.

    The number of digits for extension numbers is based on the telephony dial plan created on an IP PBX or PBX. This is a required field that has a value range from 1 through 20. The typical extension length is from 3 through 7 digits. If your existing telephony environment includes extension numbers, you must specify a number of digits that matches the number of digits in those extensions when you create the UM dial plan.

   - **Dial plan type**: A Uniform Resource Identifier (URI) is a string of characters that identifies or names a resource. The main purpose of this identification is to enable VoIP devices and PBXs to communicate with other devices over a network using specific protocols. URIs are defined in schemes that define a specific syntax and format and the protocols for the call. In simple terms, this format is passed from the IP PBX or PBX and the type of dial plan you create must match that format. After you create a UM dial plan, you won't be able to change the dial plan type without deleting the dial plan, and then re-creating the correct type of dial plan. You can select one of the following dial plan types:

   - **Telephone extension**: This is the most common dial plan type. The calling and called party information from the VoIP gateway or IP Private Branch eXchange (PBX) is listed in one of the following formats: Tel:512345 or 512345@\<_IP address_\>. This is the default type for dial plans.

   - **SIP URI**: Use this dial plan type if you must have a Session Initiation Protocol (SIP) URI dial plan such as an IP PBX that supports SIP routing, a SIP-enabled PBX, or if you're integrating Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server and Unified Messaging. The calling and called party information from the VoIP gateway. IP PBX, SIP-enabled PBX, or Communications Server 2007 R2 or Lync Server is listed as a SIP address in the following format: sip:\<_username_\>@\<_domain_ or _IP address_\>:Port.

   - **E.164**: E.164 is an international numbering plan for public telephone systems in which each assigned number contains a country code, a national destination code, and a subscriber number. The calling and called party information sent from the VoIP gateway and PBX or IP PBX is listed in the following format: Tel:+14255550123.

     > [!NOTE]
     > After you create a dial plan, you won't be able to change the dial plan type without deleting the dial plan, and then re-creating the correct type of dial plan.

   - **VoIP security mode**: Use this drop-down list to select the VoIP security setting for the UM dial plan. You can select one of the following security settings for the dial plan:

   - **Unsecured**: By default, when you create a UM dial plan, it's set to not encrypt the SIP signaling or RTP traffic. In Unsecured mode, the Exchange servers associated with the UM dial plan send and receive data from VoIP gateways, IP PBXs, SBCs, and other Exchange servers using no encryption. In Unsecured mode, neither the Realtime Transport Protocol (RTP) media channel nor the SIP signaling information is encrypted.

   - **SIP secured**: When you select **SIP secured**, only the SIP signaling traffic is encrypted, and the RTP media channels still use TCP, which isn't encrypted. With SIP secured, mutual Transport Layer Security (TLS) is used to encrypt the SIP signaling traffic and VoIP data.

   - **Secured**: When you select **Secured**, both the SIP signaling traffic and the RTP media channels are encrypted. Both the secure signaling media channel that uses Secure Realtime Transport Protocol (SRTP) and the SIP signaling traffic use mutual TLS to encrypt the VoIP data.

5. **Dial codes**: Use this page to configure the dial codes for a UM dial plan. Several dial code settings can be configured on the dial plan. These include incoming and outgoing calling options. You can configure the following:

   - **Dial codes for outgoing calls**: Use these settings to specify the dialing codes for outgoing calls that can be made by UM-enabled users. These outgoing calls are calls that are placed using Outlook Voice Access or from a voice mail message.

   - **Outside line access code**: Use this field to type the number or numbers used to access an outside telephone number for outgoing external calls. This number will precede the telephone number dialed. This is also called a trunk access code. This field accepts from 1 through 16 digits. For many organizations, this number is 9. By default, this field isn't populated.

     Frequently, this setting is used in telephony environments where a PBX or IP PBX is located onsite or maintained in an organization. It may not have to be configured if your organization's telephony environment is maintained by an external business or vendor.

   - **International access code**: Use this field to type the number code used to access international telephone numbers for outgoing calls. This number will precede the telephone number dialed. By default, this field isn't populated. This field accepts from 1 through 4 digits. For example, the international access code for the United States is 011. For Europe, it's 00.

   - **National number prefix**: Use this field to type the number code used to dial telephone numbers that are out of an area code but within the country/region. This number will precede the telephone number dialed. By default, this field isn't populated. This field accepts from 1 through 4 digits. For example, 0 is used in Europe, and 1 is used in North America.

   - **Country/Region code**: Use this field to type the country/region code number used for outgoing calls. This number will precede the telephone number dialed. By default, this field isn't populated. This field accepts from 1 through 4 digits. For example, in the United States, the country/region code is 1. In the United Kingdom, it's 44.

   - **Number formats for dialing between UM dial plans**: Use these settings to configure calls between users in separate dial plans when they place calls between the dial plans.

   - **Country/Region number format**: Use this field to specify how a user's telephone number should be dialed by the Exchange servers when users are in a different dial plan that has the same country code. This is used by auto attendants and when an Outlook Voice Access user searches and tries to call the user in the directory.

     This entry consists of a number prefix and a variable number of characters (for example, 020 _xxxxxxx_).To determine the telephone number, Unified Messaging will append the last _x_ digits from the telephone number specified in the directory to the prefix specified.

   - **International number format**: Use this field to specify how a user's telephone number should be dialed by Unified Messaging when the users are in different dial plans that have different country codes. This is used by an auto attendant and when an Outlook Voice Access user searches and tries to call the user in the directory.

     This entry consists of a number prefix and a variable number of characters (for example, 4420 _xxxxxxx_). To determine the telephone number, Unified Messaging will append the last _x_ digits from the telephone number specified in the directory to the prefix specified.

   - **Number formats for incoming calls within the same dial plan**: Use this field to add or remove a number format for incoming calls that are placed between users in the same dial plan. This field accepts both numbers and the letter "x" as a wild card character. No other letters can be used in this field.

     For incoming calls within the same dial plan add a number format. For example, to add a number format for 5-digit extensions, enter, 142570xxxxx and click ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove a number format, click ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

6. **Outlook Voice Access**: Use this page to configure Outlook Voice Access settings for the UM dial plan. Outlook Voice Access enables users to access their individual mailboxes to retrieve email, voice messages, contacts, and calendaring information using a telephone. You can view or configure the following:

   - **Welcome greeting**: This display-only field shows the name of the sound file that will be used for the welcome greeting.

   - **Default greeting**: The welcome greeting is used when an Outlook Voice Access user or another caller calls the Outlook Voice Access number and does a directory search. This audio file is the default greeting for a UM dial plan. However, you may want to change this welcome greeting and provide another welcome greeting specific to your company, such as, "Welcome to Outlook Voice Access for Contoso, Ltd."

     If you decide to customize this greeting, you must first record the customized greeting, save it as a .wav file, and then configure the dial plan to use this customized greeting. The file name and path must not exceed 255 characters.

     You can add a customized greeting by clicking **Change**, and then clicking **Browse** to select a previously recorded custom greeting and specify the audio file (.wav) to use for the welcome greeting. If you don't specify an audio file, Outlook Voice Access users will hear a default welcome greeting that says, "Welcome, you are connected to Microsoft Exchange."

   - **Informational announcement**: When enabled, this optional recording plays immediately after the business or non-business hours welcome greeting. An informational announcement may state the organization's security polices for accessing the system, for example, "When you gain access to our system using Outlook Voice Access, you have agreed to the terms of our business agreement and all security policies for our organization apply. Access to our system is monitored and gaining illegal access will be prosecuted." An informational announcement can also provide information that's required for compliance with company policy, for example, "Calls may be monitored for training purposes." If it's important that callers hear the whole informational announcement, it can be marked as uninterruptible.

     By default, there's no informational announcement configured on UM dial plans. To enable an informational announcement and use a custom audio file specific to your organization, click **Change** and then click **Browse**.

   - **Allow announcement to be interrupted**: Select this check box to enable the Outlook Voice Access user to interrupt the informational announcement. You should do this if you have long informational announcements. Outlook Voice Access users may become frustrated if the informational announcement is long and they can't interrupt it to access the options provided by the UM dial plan.

   - **Outlook Voice Access numbers**: Use this field to add a telephone or extension number or a SIP URI that an Outlook Voice Access user will call to access the voice mail system using Outlook Voice Access. In most cases, you enter an extension number or an external telephone number. However, because this field accepts all alphanumeric characters, a SIP URI can be used if you're using an IP PBX, Office Communications Server 2007 R2, or Microsoft Lync Server.

     By default, when a dial plan is created, no Outlook Voice Access numbers are defined. To enable Outlook Voice Access users to call into Outlook Voice Access, you must configure at least one telephone number. The number of alphanumeric characters can't exceed 20.

     When you configure this number on the dial plan, this number will be displayed in Microsoft Office Outlook 2007 or later versions and Outlook Web App for voice mail options.

     To add a new Outlook Voice Access number, enter the number in the box and click ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove an Outlook Voice Access number, click ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

7. **Settings**: Use this page to configure dial plan settings for Unified Messaging. When you configure settings on this page, you can control how Outlook Voice Access users and external callers calling into an auto attendant linked to the dial plan locate users in your organization, the audio codec that is used for voice mail messages, the number of sign-in failures, and time-out values. You can configure the following:

   - **Primary way to search for names**: Use this list to select the primary way that callers can locate a user when they dial in to the system.

     By default, **Last First** is selected. This means that when users are searching for a user in the directory, they will enter the user's last name first and then the first name.

     When an Outlook Voice Access user calls in to an Outlook Voice Access number to access their mailbox, a caller calls in to an Outlook Voice Access number to perform a directory search, or a caller calls in to an auto attendant linked to a UM dial plan, they can search for a user in the directory by spelling their name or alias.

     You must select one of the supported methods to be able to use the dial-by-name primary method. The following methods are supported:

     - **Last First (default)**

     - **First Last**

     - **SMTP address**

   - **Secondary way to search for names**: Use this list to select the secondary way that callers can locate a user when they dial in to the system.

     By default, **SMTP address**: is selected. This means that when users search for a user in the directory, they will enter the user's email alias or SMTP address.

     When an Outlook Voice Access user calls in to an Outlook Voice Access number to access their mailbox, a caller calls in to an Outlook Voice Access number to perform a directory search, or a caller calls in to an auto attendant linked to a UM dial plan, they can search for a user in the directory by spelling their name or alias. When you select one of these options, callers can use the primary way to search for names or the secondary way to search for names to locate users in the directory.

     You aren't required to select one of the four methods that are supported. However, if you don't select a secondary way to search for users, callers will be given only one way to search for a user. The following options are available:

     - **Last First**

     - **First Last**

     - **SMTP address** (default)

     - **None**

   - **Audio codec**: Use this list to select the audio codec that will be used by the dial plan. When a caller places a call to a user who is associated with the dial plan and leaves a voice message, Unified Messaging uses the audio codec that you select from this list to record voice messages that will be sent to voice mail-enabled users. The following audio codecs are supported:

     - **MP3** (default)

     - **WMA** (Windows Media Audio)

     - **G711** (Pulse Code Modulation (PCM) Linear)

     - **GSM** (Group System Mobile 06.10)

     By default, the MP3 format is selected. The MP3 format is a common audio file format that's used to greatly reduce the size of the audio file and is most commonly used by personal audio devices or MP3 players. MP3 is a cross-platform type of audio codec and is used for compatibility with many mobile phone and devices and various computer operating systems.

     WMA is used because it's highly compressed and has high-quality format properties. G.711 PCM Linear is a telephone-quality audio codec format that's the least compressed and has the lowest-quality format. GSM 06.10 is an audio codec format that's used by mobile phone vendors and is the standard for digital mobile phone services.

     If you're concerned about users' disk quotas, select WMA as the audio codec. Voice files saved in .wma format are approximately half the size of the same voice recording made using one of the other audio codecs.

   - **Operator extension**: Use this text box to enter the telephone number or an extension number for the dial plan's operator. This is different than an operator extension that is configured on a UM auto attendant. However, you can put in the same phone or extension number for both types of operators.

     You can configure this setting to transfer calls to an auto attendant if one is configured, to a human operator, to external telephone numbers, or to extension numbers.

     When a caller who is using the telephone keypad presses 0, or says "reception" or "operator," or the **Number of input failures before disconnecting** threshold is exceeded, the caller is transferred to the telephone or extension number that you specify in this text box.

     This telephone number can be a number external to the organization or an internal telephone extension number. For example, if the extension number for the receptionist or operator is 81964 and your organization has only one dial plan, enter 81964.

     By default, this setting is blank. If you don't enter a number in this text box, the ability to transfer calls to the operator is disabled and callers are politely disconnected because there's no one to answer the call.

     We recommend that you populate this text box with a telephone number that transfers callers to an operator if they can't locate a specific user in the directory.

   - **Number of sign-in failures before disconnecting**: Use this text box to enter the number of sequential unsuccessful logon attempts allowed before a caller is disconnected.

     The value of this setting can be from 1 through 20. Setting this value too low can frustrate users. For most organizations, this value should be set to the default of three attempts.

   - **Timeouts and retries**: These settings apply to Outlook Voice Access users and external callers that dial into a UM auto attendant.

   - **Maximum call duration (minutes)**: Use this text box to enter the maximum number of minutes that an incoming call can be connected to the system without being transferred to a valid extension number before the call is ended. For most organizations, this value should be set to the default of 30 minutes.

     This setting applies to all kinds of calls. This includes incoming Outlook Voice Access calls, voice calls internal to your organization, and voice and incoming fax calls external to your organization.

     The value of this setting can be from 10 through 120. Setting this value too low can cause incoming calls to be disconnected before they are completed. For example, if your organization receives many large fax messages, you may want to consider increasing this value from the default so that all the pages for fax messages are received.

   - **Maximum recording duration (minutes)**: Use this text box to enter the maximum number of minutes allowed for each voice recording when a caller leaves a voice mail message. For most organizations, this value should be set to the default of 20 minutes.

     The value of this setting can be from 1 through 100. Setting this value too low can cause long voice messages to be disconnected before they are completed. Setting this value too high lets users save lengthy voice messages in their Inboxes.

     This setting is important if you have implemented strict disk quotas for users. This value must be less than the value set for the **Maximum call duration (minutes)** setting.

   - **Recording idle time out (seconds)**: Use this text box to enter the number of seconds of silence that the system allows when a voice message is being recorded before the call is ended. For most organizations, this value should be set to the default of 5 seconds.

     The value of this setting can be from 2 through 10. Setting this value too low can cause the system to disconnect callers before they are finished leaving their voice messages. Setting this value too high allows lengthy silences in voice messages.

   - **Number of input failures before disconnecting**: Use this text box to configure the number of times that callers can enter incorrect menu choices before they are disconnected. For most organizations, this value should be set to the default of three attempts. This is an important setting for speech-enabled UM dial plans.

     Examples of incorrect data include when a caller requests an extension number that isn't found in the system, the system can't locate the user's extension number to transfer the call, or the caller presses a menu option that isn't valid.

    The value of this setting can be from 1 through 20. Setting this value too low may prematurely disconnect the caller.

   - **Audio language**: Use this list to specify the default language to be used by Outlook Voice Access users. This setting doesn't apply to the language setting on a UM auto attendant. You can set the language for Outlook Voice Access to be the same as or different from the language that's used on a UM auto attendant. When a user places a call to a user who is linked with a dial plan, the audio language is the default language that the voice-recorded operator uses. The system prompts that callers hear are played in the same language. The language that is chosen on the UM dial plan is used to read email, voice mail, and calendar items; to say the user's name if a personal greeting hasn't been recorded; to transcribe a voice message using the Voice Mail Preview feature; and to enable Automatic Speech Recognition (ASR) to work correctly.

     For on-premises deployments, adding other languages lets Outlook Voice Access use a language other than U.S. English. For example, if an Outlook Voice Access user calls in using an Outlook Voice Access number from a desk telephone, the user is greeted with a prerecorded operator's voice in English. Even if the same user selects a different language, such as French, in Outlook Web App, the menus are still read in U.S. English. For the user to be able to hear the prerecorded operator menus in French, you must install the appropriate language pack.

8. **Dialing rules**: Use this page to specify dialing rules for in-country/region and international calls placed by UM-enabled users. Each entry defined on the dialing rule determines the types of calls that users within a specific dialing rule group can make. After you use the **Dialing rules** page to configure dialing rules, you must configure the UM dial plan, a UM mailbox policy, or a UM auto attendant to use the appropriate dialing rule. After you configure the UM mailbox policy to use a dialing rule group, the dialing restrictions configured apply to all UM-enabled users who are associated with the UM mailbox policy. For example, you can configure a dialing rule group that doesn't require users who are associated with the dial plan to dial an outside line access code when they place a call to an in-country/region telephone number. You can configure the following:

   - **In-country/region dialing rules**: Use this box to add, remove, or edit in-country/region dialing rule groups used by UM mailbox policies. To create a dialing rule, click ![Add Icon](images/ITPro_EAC_AddIcon.gif). To edit an existing dialing rule, click ![Edit icon](images/ITPro_EAC_EditIcon.gif). To remove a dialing rule, click ![Remove icon](images/ITPro_EAC_RemoveIcon.gif). When you create a dialing rule, add the following information on the **New dialing rule** page:

   - **Dialing rule name**: Use this text box to enter the name for the dialing rule you are creating. You can use the same name to collect several rules in a group and then enable or disable them under **Dialing authorization**. The name can be up to 32 characters long.

   - **Number pattern to transform (number mask)**: Use this text box to enter the number pattern to transform before dialing, for example 91425xxxxxxx. If a user enters a number that matches this pattern, UM will transform the number dialed into a dialed number before placing the call. You can only enter numbers and the wildcard character, "x".

   - **Dialed number**: Use this text box to enter the number you want to dial that matches the number pattern you set in the **Number pattern to transform (number mask)**. The dialed number is used to determine the actual dial string sent to the VoIP gateway or IP PBX. This number can be different from the number obtained by Unified Messaging for the outgoing call. However, your PBX or IP PBX can also be configured to omit the area code for local calls and can be configured for private voice numbering plans. Any wildcard characters ( _x_) in the dial string are replaced with the digits from the original number that were matched by the number mask on the dialing rule. An example of a valid dialed number is 9 _xxxxxxx_. This field can contain only numbers and the character _x_.

   - **Comment**: Use this text box to put in a comment or description for the dialing rule that you're adding or modifying. By default, this text box is blank.

     > [!NOTE]
    > If you are integrating with Office Communications Server 2007 R2 or Microsoft Lync Server, you'll probably find it unnecessary to configure dialing rules or dialing rule groups in Unified Messaging. Office Communications Server 2007 R2 and Lync Server are designed to perform call routing and number translation for users in your organization, and will also do this when the calls are made on behalf of users.

   - **International rules**: Use this text box to add, remove, or edit international dialing rule groups used by UM mailbox policies.

   - **Dialing rule name**: Use this text box to enter the name for the dialing rule you are creating. You can use the same name to collect several rules in a group and then enable or disable them under **Dialing authorization**. The name can be up to 32 characters long.

   - **Number pattern to transform (number mask)**: Use this text box to enter the number pattern to transform before dialing, for example 91425xxxxxxx. If a user enters a number that matches this pattern, UM will transform the number dialed into a dialed number before placing the call. You can only enter numbers and the wildcard character, "x".

   - **Dialed number**: Use this text box to enter the number you want to dial that matches the number pattern you set in **Number pattern to transform (number mask)**. The dialed number is used to determine the actual dial string sent to the VoIP gateway or IP PBX. This number can be different from the number obtained by Unified Messaging for the outgoing call. However, your PBX or IP PBX can also be configured to omit the area code for local calls and can be configured for private voice numbering plans. Any wildcard characters ( _x_) in the dial string are replaced with the digits from the original number that were matched by the number mask on the dialing rule. An example of a valid dialed number is 9 _xxxxxxx_. This field can contain only numbers and the character _x_.

   - **Comment**: Use this text box to put in a comment or description for the dialing rule that you're adding or modifying. By default, this text box is blank.

     > [!NOTE]
     > For on-premises deployments, if you are integrating with Office Communications Server 2007 R2 or Microsoft Lync Server, you'll probably find it unnecessary to configure dialing rules or dialing rule groups in Unified Messaging. Office Communications Server 2007 R2 or Lync Server are designed to perform call routing and number translation for users in your organization, and will also do this when the calls are made on behalf of users.

9. **Dialing authorization**: Use this page to select dialing rules for callers who call in to an Outlook Voice Access number configured on a UM dial plan. You can restrict the type of calls placed by callers when an unauthenticated user or an Outlook Voice Access user calls in to an Outlook Voice Access number configured on a dial plan by configuring dialing rule groups and dialing restrictions. You can configure the following:

   - **Calls in the same UM dial plan**: Select this check box to let users who call in to an Outlook Voice Access number configured on a dial plan place or transfer calls to an extension number associated with a UM-enabled user who is within the same dial plan. By default, this setting is enabled.

     When you disable this setting, users who call in to the Outlook Voice Access number won't be able to place or transfer calls to any users who aren't UM-enabled, to other extension numbers, or to UM-enabled users who are associated with the same dial plan. This is because the **Allow calls to any extension** setting is disabled by default.

   - **Allow calls to any extension**: When this setting is disabled, users who call in to an Outlook Voice Access number on the dial plan can't place calls to users who aren't UM-enabled or to other extension numbers not associated with a UM-enabled user. However, they can place a call or transfer a call to extension numbers associated with UM-enabled users. This is because the **Calls in the same UM dial plan** setting is enabled by default. The **Allow calls to any extension** setting is disabled by default.

     > [!NOTE]
     > To avoid attempted fraud and other potential threats to your UM environment, follow the guidance in the blog post [Is your Exchange Unified Messaging protected against telecommunication fraud?](https://blogs.msdn.microsoft.com/mahmoud_badran/2017/02/15/is-your-exchange-unified-messaging-protected-against-telecommunication-fraud/)

     When this setting is enabled, users who call in to an Outlook Voice Access number configured on the dial plan can place calls to users who aren't UM-enabled, to other extension numbers not associated with a UM-enabled user, and to UM-enabled users. This is because the **Calls in the same UM dial plan** setting is enabled by default.

     You can enable this setting in an environment where not all users have been UM-enabled. This setting is also useful when you want to allow users who call in to a Outlook Voice Access number configured on a dial plan to call extension numbers that aren't associated.

   - **Authorized in-country/region dialing rule groups**: Use this section to add or remove allowed in-country/region dialing rules. By default, there are no in-country/region dialing rules configured on UM dial plans.

     In-country/region dialing rule groups are used to allow or restrict the telephone numbers within a country or region that any user who has dialed in to the subscriber access number can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

     To add in-country/region dialing rules, you must first create the appropriate in-country/region dialing rule on the dial plan, and then add the appropriate dialing rule entries on the dialing rule. After you create the required dialing rules on the dial plan, you must then add the dialing rule to the list of dialing authorizations on the **Dialing authorization** page on the dial plan.

     In-country/region dialing rule groups can be used to allow or restrict access to telephone numbers within a country or region. This is applied to all users who have called in to an Outlook Voice Access number.

   - **Authorized international dialing rule groups**: Use this section to add or remove allowed international dialing rules. By default, there are no international dialing rules configured on UM dial plans.

     International dialing rules are used to allow or restrict the telephone numbers outside a country or region that any user who has dialed in to the Outlook Voice Access number can dial. This helps prevent unnecessary or unauthorized telephone calls and charges.

     To add international dialing rule groups, you must first create the appropriate international dialing rules on the dial plan, and then add the appropriate dialing rule entries. After you create the required dialing rules on the dial plan, you must then add the dialing rule to the list of dialing authorizations on the **Dialing authorization** page on the dial plan.

     International dialing rule groups can be used to allow or restrict access to telephone numbers outside a country or region. This is applied to all users who have called in to an Outlook Voice Access number.

10. **Transfer & search**: Use this page to configure the UM dial plan features. Several features can be configured on the UM dial plan. These include transferring calls, sending voice messages, and searching for users. You can configure the following:

    - **Allow callers to**: Use these settings to determine how users who call in to an Outlook Voice Access number can contact users. You can configure the following:

    - **Transfer to users**: Select this check box to enable Outlook Voice Access users to transfer calls to users. By default, this option is enabled. This lets users associated with the dial plan transfer calls to users in the same UM dial plan. After you select this check box, you can set the group of users callers can search for by selecting the appropriate option under the **Allow callers to search for users by name or alias** section on this page.

      If you disable this option, Outlook Voice Access won't allow callers to be transferred to any users in the dial plan.

    - **Leave voice messages without ringing a user's phone**: Select this check box to enable callers to send voice messages to users. By default, this option is enabled. This lets Outlook Voice Access users who are associated with the dial plan send voice messages to users in the same UM dial plan. After you select this check box, you can set the group of users callers can search for by selecting the appropriate option under the **Allow callers to search for users by name or alias** section on this page.

     If you disable this option, Outlook Voice Access won't invite callers to send a voice message during a system prompt.

    - **Allow callers to search for users by name or alias**: Use these options to determine a grouping of users that can be searched. By default, the **In this dial plan only** option is selected. However, you can change the grouping of users. Choose from the following options:

    - **In this dial plan only**: Use this option to allow callers who connect to Outlook Voice Access to locate and contact users who are within the dial plan that they are a member of.

    - **In the entire organization**: Use this option to allow callers who connect to Outlook Voice Access to locate and contact anyone who is listed in the entire organization. This includes all users who are mailbox-enabled or UM-enabled users in all dial plans.

    - **Only on this auto attendant**: Use this list to allow Outlook Voice Access users to connect to a UM auto attendant and then potentially connect to another auto attendant you have configured. You must create this auto attendant to allow callers to be transferred to another auto attendant that's specified.

    - **Only for this extension**: Use this option to allow Outlook Voice Access users to connect to an extension number that you specify in the field for this option. This field accepts only numeric digits. The number of digits that you define in this field must match the number of digits configured on the dial plan associated with the auto attendant.

    - **Information to include for users with the same name**: Use this field to select how the dial plan differentiates between users who have the same or similar names. When a caller is prompted to enter letters or say the person's name to find a particular user in the organization, sometimes more than one name matches the caller's input. If there are two users with the same name, UM will use one of the following ways to add additional information to the user's name. For example, if you select **Department**, when an Outlook Voice Access user calls in to Outlook Voice Access and searches for a user and there are duplicate or similar names in the directory, the caller will hear the user's name and department, for example:

      1. System: "Welcome to Outlook Voice Access. Please enter your PIN and press the pound key."

      2. Caller inputs their PIN followed by the # key.

      3. System: "Please say voice mail, email, calendar, personal contacts, directory, or personal options."

      4. Caller: "Directory"

      5. System: "Directory search. Please note, for the following tasks the system requires you to use your telephone keypad rather than speaking. Use the keypad to spell the name of the person you're trying to find, last name first, or to spell the first part of their email address, press the pound key twice, if you know the extension, press the pound key."

      6. Caller uses the key pad and inputs "smithtony" and presses the # key.

      7. System: "For Tony Smith, research, press 1. For Tony Smith, administration, press 2. For Tony Smith, technical support, press 3."

      8. Caller presses the appropriate key on the keypad and the call is transferred to the user.

         By default, all UM auto attendants associated with this dial plan inherit this setting. However, you can change this setting on each UM auto attendant you create.

         Select one of the following methods for providing callers with more information to help them locate the correct user in the organization:

         - **None**: No additional information is given when matches are listed. By default, this method is selected.

         - **Title**: The voice mail system includes each user's title when matches are listed.

         - **Department**: The voice mail system includes each user's department when matches are listed.

         - **Location**: The voice mail system includes each user's location when matches are listed.

         - **Prompt for alias**: The voice mail system prompts the caller for the user's alias.

11. After you configure the required settings, click **Save** to save your changes.

## Use the Shell to configure UM dial plan settings

This example configures a UM dial plan named `MyDialPlan` to use 9 for the outside line access code.

```powershell
Set-UMDialplan -Identity MyDialPlan -OutsideLineAccessCode 9
```

This example configures a UM dial plan named `MyDialPlan` to use a welcome greeting.

```powershell
Set-UMDialplan -Identity MyDialPlan -WelcomeGreetingEnabled $true -WelcomeGreetingFilename welcome.wav
```

This example configures a UM dial plan named `MyDialPlan` with dialing rules.

```powershell
$csv=import-csv "C:\MyInCountryGroups.csv"
Set-UMDialPlan -Identity MyDialPlan -ConfiguredInCountryGroups $csv
Set-UMDialPlan -Identity MyDialPlan -AllowedInCountryGroups "local, long distance"
```

## Use the Shell to view UM dial plan settings

This example displays a list of all the UM dial plans.

```powershell
Get-UMDialplan
```

This example displays a formatted list of all of the settings on a UM dial plan named `MyUMDialPlan`.

```powershell
Get-UMDialplan -Identity MyUMDialPlan | Format-List
```
