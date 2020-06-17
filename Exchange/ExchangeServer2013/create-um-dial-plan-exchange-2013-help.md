---
title: 'Create a UM dial plan: Exchange 2013 Help'
TOCTitle: Create a UM dial plan
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.Servers.UnifiedMessaging.CreateUMDialPlanWizardForm.CreateUMDialPlanWizardPage'
ms.assetid: 963ff2e1-515d-439a-953a-664174e5e283
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Create a UM dial plan in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

A Unified Messaging (UM) dial plan contains configuration information related to your telephony network. A UM dial plan establishes a link from the telephone extension number of a user enabled for voice mail to their mailbox. When you create a UM dial plan, you can configure the number of digits in the extension numbers, the Uniform Resource Identifier (URI) type, and the Voice over IP (VoIP) security setting for the dial plan.

Each time you create a UM dial plan, a UM mailbox policy is also created. The UM mailbox policy is named \< _DialPlanName_\> Default Policy.

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Server](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to create a UM dial plan

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**, and then click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

2. On the **New UM dial plan** page, complete the following boxes:

   - **Name**: Type the name of the dial plan. A UM dial plan name is required and must be unique. However, it's used only for display in the EAC and the Shell. If you have to change the display name of the dial plan after it's been created, you must first delete the existing UM dial plan and then create another dial plan that has the appropriate name. If your organization uses multiple UM dial plans, we recommend that you use meaningful names for your UM dial plans. The maximum length of a UM dial plan name is 64 characters, and it can include spaces. However, it can't include any of the following characters: `" / \ [ ] : ; | = , + * ? < >`.

     Although you can include spaces in a UM dial plan name, if you integrate Unified Messaging with Office Communications Server 2007 R2 or Microsoft Lync Server, the dial plan name can't include spaces. Therefore, if you created a dial plan with spaces in the display name, and you're integrating with Office Communications Server 2007 R2 or Lync Server, you must first delete that dial plan and then create another dial plan that doesn't include spaces in the display name.

     > [!IMPORTANT]
     > Although the box for the name of the dial plan can accept 64 characters, the name of the dial plan can't be longer than 49 characters. If you try to create a dial plan name that contains more than 49 characters, you'll receive an error message. The message will say that the UM mailbox policy couldn't be generated because the UM dial plan name is too long. This happens because, as mentioned earlier, when you create a dial plan a default UM mailbox policy named _\<DialPlanName\>_ Default Policy is also created. When the 15 characters in Default Policy are added to the name of the dial plan, the total characters exceed the limit. The _Name_ parameter for both the UM dial plan and UM mailbox policy can be 64 characters. However, if the name of the dial plan is longer than 49 characters, the name of the default UM mailbox policy will be longer than 64 characters, and this isn't allowed by the system.

   - **Extension length (digits)**: Enter the number of digits for the dial plan. The number of digits for extension numbers is based on the telephony dial plan created on a Private Branch eXchange (PBX) or IP PBX. For example, if a user associated with a telephony dial plan dials a four-digit extension to call another user in the same telephony dial plan, you select 4 as the number of digits in the extension.

     This is a required box that has a value range from 1 through 20. The typical extension length is from 3 through 7. If your existing telephony environment includes extension numbers, you must specify a number of digits that matches the number of digits in those extensions.

     When you create a Session Initiation Protocol (SIP) or an E.164 dial plan and associate a UM-enabled user with the dial plan, you must still input an extension number to be used by the user. This number is used by Outlook Voice Access users when they access their mailbox.

   - **Dial plan type**: A Uniform Resource Identifier (URI) is a string of characters that identifies or names a resource. The main purpose of this identification is to enable VoIP devices to communicate with other devices over a network using specific protocols. URIs are defined in schemes that define a specific syntax and format and the protocols for the call. In simple terms, this format is passed from the IP PBX or PBX. After you create a UM dial plan, you won't be able to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type. You can select one of the following URI types for the dial plan:

   - **Telephone extension**: This is the most common URI type. The calling and called party information from the VoIP gateway or IP Private Branch eXchange (PBX) is listed in one of the following formats: Tel:512345 or 512345@\<_IP address_\>. This is the default URI type for dial plans.

   - **SIP URI**: Use this URI type if you must have a Session Initiation Protocol (SIP) URI dial plan such as an IP PBX that supports SIP routing or if you're integrating Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server and Unified Messaging. The calling and called party information from the VoIP gateway. IP PBX, or Communications Server 2007 R2 or Lync Server is listed as a SIP address in the following format: sip:\<_username_\>@\<_domain_ or _IP address_\>:Port.

   - **E.164**: E.164 is an international numbering plan for public telephone systems in which each assigned number contains a country code, a national destination code, and a subscriber number. The calling and called party information sent from the VoIP gateway or IP PBX is listed in the following format: Tel:+14255550123.

     > [!CAUTION]
     > After you create a dial plan, you will be unable to change the URI type without deleting the dial plan, and then re-creating the dial plan to include the correct URI type.

   - **VoIP security mode**: Use this drop-down list to select the VoIP security setting for the UM dial plan. You can select one of the following security settings for the dial plan:

   - **Unsecured**: By default, when you create a UM dial plan, it is set to not encrypt the SIP signaling or RTP traffic. In unsecured mode, the Client Access and Mailbox servers associated the UM dial plan send and receive data from VoIP gateways, IP PBXs, SBCs and other Client Access and Mailbox servers using no encryption. In unsecured mode, neither the Realtime Transport Protocol (RTP) media channel nor the SIP signaling information is encrypted.

   - **SIP secured**: When you select **SIP secured**, only the SIP signaling traffic is encrypted, and the RTP media channels still use TCP, which isn't encrypted. With SIP secured, Mutual Transport Layer Security (TLS) is used to encrypt the SIP signaling traffic and VoIP data.

   - **Secured**: When you select **Secured**, both the SIP signaling traffic and the RTP media channels are encrypted. Both the secure signaling media channel that uses Secure Realtime Transport Protocol (SRTP) and the SIP signaling traffic use mutual TLS to encrypt the VoIP data.

   - **Audio language**: Use this list to specify the default language to be used by Outlook Voice Access users. This setting doesn't apply to the language setting on a UM auto attendant. You can set the language for Outlook Voice Access to be the same as or different from the language that's used on a UM auto attendant. When a user places a call to a user who is linked with a dial plan, the audio language is the default language that the voice-recorded operator uses. The system prompts that callers hear are played in the same language. The language that is chosen on the UM dial plan is used to read email, voice mail, and calendar items; to say the user's name if a personal greeting hasn't been recorded; to transcribe a voice message using the Voice Mail Preview feature; and to enable Automatic Speech Recognition (ASR) to work correctly.

   - **Country/Region code**: Use this box to type the country/region code number to be used for outgoing calls. This number will precede the telephone number that's dialed. This box accepts from 1 through 4 digits. For example, in the United States, the country/region code is 1. In the United Kingdom, it's 44.

3. Click **Save**.

## Use the Shell to create a UM dial plan

This example creates a new UM dial plan named `MyUMDialPlan` that uses four-digit extension numbers.

```powershell
New-UMDialplan -Name MyUMDialPlan -NumberofDigits 4
```

This example creates a new UM dial plan named `MyUMDialPlan` that uses five-digit extension numbers and supports SIP URIs.

```powershell
New-UMDialplan -Name MyUMDialPlan -UriType SIPName -NumberofDigits 5
```
