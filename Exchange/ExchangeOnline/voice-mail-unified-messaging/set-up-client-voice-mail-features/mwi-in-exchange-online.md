---
localization_priority: Normal
description: Message Waiting Indicator (MWI) is a feature that's found in most voice mail systems. It lets users know that they have new or unheard voice mail messages. In its most common form, this feature lights a lamp on a user's phone to indicate the presence of a new or unheard voice message.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 467f344c-64b0-4efb-96eb-8904379cce1e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Message Waiting Indicator in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Message Waiting Indicator in Exchange Online

Message Waiting Indicator (MWI) is a feature that's found in most voice mail systems. It lets users know that they have new or unheard voice mail messages. In its most common form, this feature lights a lamp on a user's phone to indicate the presence of a new or unheard voice message.

## Overview
<a name="overview"> </a>

MWI notifications can include any mechanism that indicates the existence of a new or unheard voice message. The message can be in a new email message or one that's marked as unread. The MWI notification might take any of the following forms:

- A new voice message seen from Microsoft Outlook or Outlook on the web (formerly known as Outlook Web App).

- A lamp on a digital, analog, USB, or VoIP phone.

- A special dial tone.

- Icons or buttons on the display screen of a digital, analog, USB, or VoIP phone.

- A highlighted notification within a software application such as:

  - Lync 2010 and 2013 desktop clients

  - Lync Mobile client app for Windows Phone, Microsoft Surface. and iOS devices

- A text or Short Messaging Service (SMS) message sent to a mobile phone that's configured to receive text messages.

In Exchange Online, a user's voice mail is stored in their mailbox. It can be accessed from a telephone using Outlook Voice Access, from a desktop or portable computer using Outlook or Outlook on the web, and from mobile phone clients. When a user receives a new voice message, the message appears in their Voice Mail search folder. If the voice message is accessed using Outlook or Outlook on the web, an email message will be included with the voice message.

By default, MWI is turned on for all users who are enabled for Unified Messaging (UM). It's controlled through settings on a UM mailbox policy or on the UM IP gateways that have been created and linked to a UM dial plan. MWI also works with protected voice messages.

## MWI administration
<a name="administration"> </a>

MWI can be administered by configuring settings on two UM components: UM mailbox policies and UM IP gateways. For both UM components, you can enable or disable MWI notifications by using the **Set-UMMailboxPolicy** cmdlet or the **Set-UMIPgateway** cmdlet in Exchange Online PowerShell. You can also configure the settings by using the Exchange admin center (EAC). You can view the status of MWI notifications by using the **Get-UMMailboxPolicy** cmdlet and the **Get-UMIPgateway** cmdlet in Exchange Online PowerShell, or by viewing the settings in the EAC.

### UM mailbox policies and MWI

You can create a UM mailbox policy to apply a common set of UM policy settings to a collection of UM-enabled mailboxes. For example, you can use a UM mailbox policy to apply PIN policy settings, dialing restrictions, and MWI notifications settings. If you enable or disable MWI on a UM mailbox policy, it will be enabled or disabled for all UM-enabled users who are linked with that UM mailbox policy. The MWI setting can also apply to a subset of the users who are linked with a UM dial plan. To learn more about UM mailbox policies, including how to enable or disable MWI for a group of UM-enabled users, see [UM mailbox policy procedures](../../voice-mail-unified-messaging/set-up-voice-mail/um-mailbox-policy-procedures.md).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in Exchange Online PowerShell to configure the MWI setting, as shown in the following table.

**Message Waiting Indicator setting on a UM mailbox policy**

|**Parameter**|**Setting available in the EAC?**|**Description**|
|:-----|:-----|:-----|
|_AllowMessageWaitingIndicator_|Yes|The _AllowMessageWaitingIndicator_ parameter specifies whether users who are linked with a UM mailbox policy can receive MWI notifications when they receive a new voice message. The default value is `$true`. <br/> When this setting is enabled, MWI notifications are sent to users who are linked with a single UM mailbox policy for calls taken by a UM IP gateway. This setting allows the UM IP gateway to receive and send SIP NOTIFY messages to UM-enabled users' phones or SIP endpoints.|

For more information about how to manage MWI settings on a UM mailbox policy, see the following topics:

- [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md)

- [Enable Message Waiting Indicator (MWI) for users](enable-mwi-for-users.md)

- [Disable Message Waiting Indicator (MWI) for users](disable-mwi-for-users.md)

- [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-ummailboxpolicy)

### UM IP gateways and MWI

If you disable MWI on a UM IP gateway, you'll disable MWI notifications for all users who connect to the VoIP gateway or IP PBX that's represented by the UM IP gateway. Disabling MWI on a single UM IP gateway that's linked to a UM dial plan can disable MWI notifications for all UM-enabled users associated with a single or multiple UM dial plans or a single or multiple UM mailbox policies. To learn more about UM mailbox policies, including how to enable or disable MWI for a group of UM-enabled users, see [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in Exchange Online PowerShell to configure the MWI setting, as shown in the following table.

**Message Waiting Indicator setting on a UM IP gateway**

|**Parameter**|**Setting available in the EAC?**|**Description**|
|:-----|:-----|:-----|
|_MessageWaitingIndicatorAllowed_|Yes|The _MessageWaitingIndicatorAllowed_ parameter specifies whether to enable the UM IP gateway to allow SIP NOTIFY messages to be sent to users associated with a UM dial plan. The default value is `$true`. <br/> When this setting is enabled, voice mail notifications can be sent to users for calls that are received by the UM IP gateway. This setting allows the UM IP gateway to send message-waiting notifications to UM-enabled users.|

For more information about how to manage MWI settings, see the following topics:

- [Manage a UM IP gateway](../../voice-mail-unified-messaging/connect-voice-mail-system/manage-um-ip-gateway.md)

- [Allow Message Waiting Indicator (MWI) on a UM IP gateway](allow-mwi-on-um-ip-gateway.md)

- [Prevent Message Waiting Indicator (MWI) on a UM IP gateway](prevent-mwi-on-um-ip-gateway.md)

- [Set-UMIPGateway](https://docs.microsoft.com/powershell/module/exchange/set-umipgateway)

## Text message (SMS) notifications for voice mail messages and missed calls
<a name="SMS"> </a>

As mentioned earlier, an MWI notification is any mechanism that indicates the existence of a new voice mail message. In addition to the mechanisms already discussed, users can be notified that they have a voice message waiting via a text message, also called an SMS (Short Message Service) message. This is a different type of MWI notification for new voice messages than the traditional light or other mechanisms.

A text message is sent to a user's mobile phone when a caller leaves a new voice message. Users can also receive a text message that notifies them when they miss a phone call and a voice message isn't left. The missed call notification text message can be sent to the user along with the new voice mail notification.

> [!NOTE]
> The text message that's sent to a user includes voice mail preview.

Text message notifications use different settings than the MWI settings on the UM IP gateway or the UM mailbox policy. Text message notifications for new voice mail and missed calls are configured on UM mailbox policies and UM mailboxes. You can enable or disable text message notifications by using the **Set-UMMailboxPolicy** cmdlet and the **Set-UMMailbox** cmdlet in Exchange Online PowerShell. You can view the status of text message notifications by using the **Get-UMMailboxPolicy** cmdlet and the **Get-UMMailbox** cmdlet. It's not possible to configure text message notifications in the EAC.

The following table shows the parameter on a UM mailbox that must be configured for a user to receive text messages for voice mail and missed call notifications:

**Text message notification settings on a user's mailbox**

|**Parameter**|**Setting available in the EAC?**|**Description**|
|:-----|:-----|:-----|
|_UMSMSNotificationOption_|No|Specifies whether a UM-enabled user can receive text message notifications for voice mail only, for voice mail and missed calls, or isn't allowed to receive notifications. The values for this parameter are: `VoiceMail`, `VoiceMailAndMissedCalls`, and `None`. The default value is `None`.|

For more information about how to manage text message notification settings on a user's mailbox, see the following topics:

- [Manage voice mail settings for a user](../../voice-mail-unified-messaging/set-up-voice-mail/manage-voice-mail-settings.md)

- [Set-UMMailbox](https://docs.microsoft.com/powershell/module/exchange/set-ummailbox)

The following table shows the parameter on a UM mailbox policy that must be configured for a user to receive text messages for voice mail and missed call notifications:

**Text message and missed call notification settings on a UM mailbox policy**

|**Parameter**|**Setting available in the EAC?**|**Description**|
|:-----|:-----|:-----|
|_AllowSMSNotification_|No|Specifies whether UM-enabled users whose mailboxes are associated with the UM mailbox policy are allowed to receive text message notifications on their mobile phones. If this parameter is set to `$true`, you must also use the **Set-UMMailbox** cmdlet and set the _UMSMSNotificationOption_ parameter for the UM-enabled user to either `VoiceMail` or `VoiceMailAndMissedCalls`. The default value is `$true`.|

For more information about how to manage text message notification settings, see the following topics:

- [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md)

- [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-ummailboxpolicy)

For text message notifications for voice mail and missed calls to work correctly, you must perform the following tasks:

1. Use either the EAC or Exchange Online PowerShell to enable the user for UM and link them to the correct UM mailbox policy.

2. On the UM mailbox policy that's linked to the user, verify that the _AllowSMSNotification_ parameter is set to `$true`. To set the parameter to `$true`, run the following command:

    ```PowerShell
    Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -AllowSMSNotification $true
    ```

3. On the user's mailbox, enable text message notifications by setting the _UMSMSNotificationOption_ parameter to `VoiceMailAndMissedCalls` or `VoiceMail`.

4. Because the default setting is `None`, you must run the following command in Exchange Online PowerShell and set the text message notification option to either `VoiceMailAndMissedCalls` or `VoiceMail`. For example:

    ```PowerShell
    Set-UMMailbox -Identity MyUMMailbox -UMSMSNotificationOption VoiceMailAndMissedCalls
    ```

    > [!IMPORTANT]
    > The _AllowSMSNotification_ parameter on the UM mailbox policy and the _UMSMSNotificationOption_ parameter on the user's mailbox must both be set to `$true` for SMS notifications to work.

In addition to your configuring the UM mailbox policy and the user's mailbox to enable text message notifications for new voice mail and missed calls, the user must enable and configure text message notifications when they sign in to Outlook on the web. To set up and configure text message notifications, the user must:

1. Sign in to Outlook on the web and go to **Options** \> **Phone** \> **Voice mail**.

2. On the **Voice Mail** page, under **Notifications**, click **Set up notifications**.

3. On the **Text messaging** page, click the **Turn on notifications** button.

    > [!CAUTION]
    > Don't click **Voice mail notifications** or it will take you back to the **Voice mail** page.

4. On the **Text messaging** page, under **Locale**, use the drop-down list to select the locale or location of the text messaging mobile operator.

5. On the **Text messaging** page, under **Mobile operator**, use the drop-down list to select the text messaging mobile operator, and then click **Next**.

6. On the **Text messaging** page, in the **Enter your phone number and click Next** box, enter the mobile phone number that's used for text message notifications, and then click **Next**. A six-digit passcode will be sent to the mobile phone. If you didn't receive a passcode, click **I didn't receive a passcode and need it sent again**.

7. Enter the passcode in the **Passcode** box, and then click **Finish**.

8. After the user enables text message notifications, they can click **Set up voice mail notifications** on the **Text Messaging** page. They'll be taken back to the voice mail page, where they can scroll down to the **Notifications** section and set up text message notification options for missed calls and voice mail.
