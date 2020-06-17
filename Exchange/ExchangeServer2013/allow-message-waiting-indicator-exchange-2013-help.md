---
title: 'Allow Message Waiting Indicator: Exchange 2013 Help'
TOCTitle: Allow Message Waiting Indicator
ms:assetid: 57fb439e-8208-499f-a20b-814677843a8c
ms:mtpsurl: https://technet.microsoft.com/library/Dd298001(v=EXCHG.150)
ms:contentKeyID: 53908377
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Allow Message Waiting Indicator in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Message Waiting Indicator (MWI) is a feature that's found in most legacy voice mail systems. It lets users know that they have new or unheard voice mail messages. In its most common form, this feature lights a lamp on a user's phone to indicate the presence of a new or unheard voice message.

## Overview

MWI notifications can include any mechanism that indicates the existence of a new or unheard voice message. The message can be in a new email message or one that's marked as unread. The MWI notification might take any of the following forms:

- A new voice message seen from Microsoft Outlook or Outlook Web App.

- A text or Short Messaging Service (SMS) message sent to a mobile phone that's configured to receive text messages.

- An outbound call made from Exchange Unified Messaging (UM).

- A lamp on a phone.

- A special dial tone.

- Icons or buttons on the display screen of a phone.

- A highlighted notification within a software application.

In Unified Messaging, a user's voice mail is stored in their mailbox. It can be accessed from a telephone using Outlook Voice Access, from a desktop or portable computer using Outlook or Outlook Web App, and from mobile phone clients. When a user receives a new voice message, the message appears in their Voice Mail search folder. If the voice message is accessed using Outlook or Outlook Web App, an email message will be included with the voice message.

When previous versions of UM were deployed in an organization, MWI was supported in either a traditional VoIP gateway environment or an IP PBX environment by using a third-party solution or application, or it was included as part of Exchange UM. In Exchange 2010 or later, MWI is also supported when UM is deployed with Microsoft Lync Server. The MWI notification mechanism used by Lync Server depends on the type of VoIP-based phone that's used by the Enterprise Voice and UM-enabled user, and can be located on any of the following:

- A phone

- A phone display

- A dial pad button

By default, MWI is turned on for all users who are enabled for UM. It's controlled through settings on a UM mailbox policy or on the UM IP gateways that have been created and linked to a UM dial plan. MWI also works with protected voice messages.

To implement MWI in a traditional telephony environment with VoIP gateways, IP PBXs, or SIP-enabled PBXs, a Mailbox server running the Microsoft Exchange Unified Messaging service sends an MWI Session Initiation Protocol (SIP) NOTIFY message to a *SIP peer*, which is either an IP PBX, a VoIP gateway used with a legacy PBX, a SIP-enabled PBX, or if you have deployed Lync Server, Lync servers are also considered SIP peers. The IP PBX or PBX lights the lamp on the desktop phone to notify the user of a new or unheard voice message.

There are two ways callers can leave voice messages: using call answering and Outlook Voice Access. With call answering, a Mailbox server answers an incoming call and allows the caller to leave a voice message for a user. With Outlook Voice Access, when a caller calls an Outlook Voice Access number, they can use the menu system to leave a voice message for a UM-enabled user.

## The Mailbox server's role in MWI

When a caller calls a UM-enabled user and the user doesn't answer their phone, the Microsoft Exchange Unified Messaging service on a Mailbox server receives MWI state change information and uses a SIP NOTIFY message to send the request for a change of notification to a VoIP gateway, IP PBX, or SIP-enabled PBX. The change of notification includes the following information:

- Message Waiting Indicator enabled (Yes or No).

- Number of new voice messages or voice messages marked unheard.

- Number of old voice messages or voice messages marked heard.

- Number of new urgent voice messages or urgent voice messages marked unheard.

- Number of old urgent voice messages or urgent voice messages marked heard.

- The primary extension number on the primary UM dial plan.

- The IP address or fully qualified domain name (FQDN) of the SIP peer to be used for SIP NOTIFY messages.

- The security type of the UM dial plan (Unsecured, SIP secured, or Secured). This information will be used to determine whether the connection to the VoIP gateway or IP PBX must be SIP over TCP or SIP over TLS. Transport Layer Security (TLS) is supported for MWI SIP NOTIFY.

A Mailbox server uses the diversion information on the header of the incoming call to determine the extension number or phone number of the UM-enabled user. When the extension or phone number is determined, the Mailbox server sends the request to the SIP peer. The SIP peer then changes the MWI state and turns on the notification on the user's phone.

> [!NOTE]
> Although PBX outages should be rare, UM automatically refreshes the MWI state for every mailbox at least once every 12&nbsp;hours. There is no way to force a refresh, but if the PBX or IP PBX is powered off and all the MWI lamps go off, all lamps should be restored to the correct state within six hours.

## MWI SIP NOTIFY messages

MWI notifications use SIP NOTIFY messages to communicate with SIP peers. MWI state change information is included in the SIP NOTIFY message and indicates whether MWI notifications will be sent to users. Whenever there's a change in the MWI state, the Mailbox Assistant sends this information to the Microsoft Exchange Unified Messaging service running on a Mailbox server. After the Unified Messaging service receives this information, it parses the message to obtain the target SIP peer and the MWI state change information. It then forms a SIP NOTIFY message with the MWI state change information in the message body and sends this information on to the SIP peer.

MWI is based on RFC 3842. RFC 3842 states that SIP event notifications must be used for message-waiting notifications. MWI is based on the SIP model, and is driven by the endpoints found in a unified messaging system. SIP endpoints, also called SIP peers, that obtain MWI information must send a SIP SUBSCRIBE message to the Unified Messaging service. The service replies with a NOTIFY message indicating that the subscription has been accepted. All MWI state change information will be conveyed from the Unified Messaging system to a SIP endpoint using NOTIFY messages that are embedded within the subscription that was previously created. The exact syntax of the SIP NOTIFY message to be sent to the SIP peer. is based on the format described in RFC 3842.

When the Unified Messaging service on a Mailbox server sends an MWI NOTIFY message to the SIP peer, the event header is set to "message-summary", which indicates that this is an MWI-related NOTIFY message. The To header field indicates the SIP endpoint for which the MWI service must be provided. The Subscription-State header must be set to "terminated" instead of "active". The following actions occur in response to the SIP NOTIFY message:

1. The SIP peer conveys this information to the PBX using a circuit-switched protocol such as SMDI.

2. The PBX sends a success message over a circuit-switched protocol.

3. The SIP peer can respond with either of the following messages: 200 OK (Success) or 480 (Temporarily Unavailable). A Mailbox server can handle both these responses and additional failure responses.

## MWI in a traditional telephony environment

In a traditional telephony environment, an incoming call is received by the PBX and then sent on to the VoIP gateway, or is received by the IP PBX or SIP-enabled PBX. The Mailbox server and the Mailbox Assistant are used to determine the MWI state for the UM-enabled user. They're responsible for delivering the SIP notification back to the VoIP gateway and legacy PBX, IP PBX, or SIP-enabled PBX.

In a traditional telephony environment, the call flow and MWI notification are as follows:

1. The incoming call is received by the legacy PBX, forwarded to the VoIP gateway, or to an IP PBX or SIP-enabled PBX, and then forwarded to the extension number of the UM-enabled user. The user doesn't answer and the caller is prompted to leave a voice message.

2. The VoIP gateway or IP PBX submits the call to a Mailbox server running the Microsoft Exchange Unified Messaging service.

3. The Mailbox server performs an LDAP query to locate the UM-enabled user's information, such as the user's extension number and personal greetings. The greetings for the UM-enabled user are played.

4. The voice message that was created by the Unified Messaging service is submitted to the Microsoft Exchange Transport service on the same Mailbox server.

5. The Transport service on the Mailbox server submits the voice message to the Mailbox server that contains the user's mailbox. The Mailbox Assistant receives a MAPI event for a new voice message.

6. The Mailbox Assistant reads the UM dial plan and the UM mailbox policy to determine whether MWI notifications should be sent to the UM-enabled user. The Mailbox Assistant queries for all Mailbox servers that are associated with the UM dial plan of the UM-enabled user. The Mailbox Assistant tries to send the RPC event to the first Mailbox server that's returned. If this fails, it tries the next one. It will keep retrying for five minutes, or until all Mailbox servers have been tried. If all the RPC calls fail, the Mailbox Assistant logs the error in Event Viewer. The Mailbox server queries for all UM IP gateways that are associated with the UM dial plan of the UM-enabled user's mailbox.

7. UM sends a SIP NOTIFY message to the first VoIP gateway or IP PBX that's returned from the query. If this fails, the Mailbox server will choose the next VoIP gateway or IP PBX. The Mailbox server will keep trying for a VoIP gateway or IP PBX for five minutes. If all attempts to find a VoIP gateway or IP PBX fail, the Mailbox server will log an error. If a VoIP gateway or IP PBX is located successfully, the VoIP gateway will send the notification to the PBX, and the PBX in turn will send a notification of the MWI event to the user's phone to light the phone lamp. If an IP PBX is used, the MWI notification is processed by the IP PBX and it will then light the user's phone lamp. Other MWI notification mechanisms are listed in the Overview section. The type of notification depends on the PBX or IP PBX hardware vendor and whether Lync Server is being used. It also depends on the type of phone (analog, digital, or VoIP) that's being used.

## MWI in a Lync Server environment

In telephony environments that include Lync Server, an incoming call can be sent from an external phone to a Mediation Server, sent from a Lync client, or sent from a unified communications (UC) or other VoIP-based phone. After the call is received, it's sent to the Lync Server front-end server pool. The Mailbox server and the Mailbox Assistant are used to determine the MWI state for the UM-enabled user and to deliver this notification to the Lync client, analog or digital or UC or VoIP-based phone.

In a telephony environment with Lync Server, the call flow and MWI notification are as follows:

1. The call is sent from one of the following:

   - A phone external to the organization (sent to a Mediation Server)

   - A Lync-based client

   - A UC or other VoIP-based phone

2. The incoming call is received by the Lync Server front-end server pool and sent on to the phone or SIP endpoint of the UM-enabled user. The user doesn't answer and the caller is prompted to leave a voice message.

3. The Lync Server front-end server pool submits the call to a Mailbox server within the on-premises network and the user's mailbox is found.

4. The Mailbox server performs an LDAP query to locate information for the UM-enabled user, such as their extension number and greetings. The greetings are played, and the caller is prompted to leave a voice message.

5. The voice message that was created by the Unified Messaging service is submitted to the Transport service on the same Mailbox server within the same site.

6. The Transport service submits the voice message to the Mailbox server that contains the user's mailbox. The Mailbox Assistant receives a MAPI event for the new voice message.

7. The Mailbox Assistant reads the UM dial plan and the UM mailbox policy to decide whether MWI notifications should be sent to the user. The Mailbox Assistant queries for all Mailbox servers that are associated with UM dial plan of the user. The Mailbox Assistant tries to send the RPC event to the first Mailbox server that's returned. If this attempt fails, the Mailbox Assistant tries the next one. It will keep trying to find a Mailbox server for five minutes or until all servers have been tried. If all the RPC calls fail, the Mailbox Assistant will log an error in the Event Viewer. The Mailbox server queries Active Directory for all UM IP gateways that are associated with the UM dial plan of the UM-enabled user.

8. UM sends a SIP NOTIFY message to the first VoIP gateway or IP PBX that's returned from the query. If this fails, the Mailbox server will choose the next VoIP gateway or IP PBX. The Mailbox server will keep trying to find a VoIP gateway or IP PBX for five minutes. If all attempts to contact the VoIP gateway or IP PBX fail, the Mailbox server will log an error. If it's successful, the VoIP gateway or IP PBX will send the notification to the Lync Server front-end server pool, which in turn will send a notification of the MWI event to a SIP endpoint used by the user, the user's phone, or a Lync client.

## MWI resilience

When you're deploying Mailbox and Client Access servers, UM dial plans, and UM IP gateways, and you're using MWI for UM-enabled users, it's best to deploy multiple Mailbox and Client Access servers as well as multiple VoIP gateways and IP PBXs to create fault tolerance and resilience. Doing this also creates MWI resilience because it provides multiple ways to send MWI notifications, as described in the preceding section.

To enable fault tolerance for MWI in Unified Messaging, you must create and configure one or more of the following:

- A UM dial plan that's linked with the UM-enabled user who will receive MWI notifications.

- A UM mailbox policy that's linked with the UM-enabled user who will receive MWI notifications.

- A UM IP gateway that's linked with the UM dial plan that's linked with the UM-enabled user who will receive MWI notifications.

- If Lync Server is used, all the Client Access and Mailbox servers must be added to the UM SIP URI dial plan that's linked with the UM-enabled user who will receive MWI notifications.

## MWI administration

MWI can be administered by configuring settings on two UM components: UM mailbox policies and UM IP gateways. For both UM components, you can enable or disable MWI notifications by using the **Set-UMMailboxPolicy** cmdlet or the **Set-UMIPgateway** cmdlet in the Exchange Management Shell. You can also configure the settings by using the Exchange admin center (EAC). You can view the status of MWI notifications by using the **Get-UMMailboxPolicy** cmdlet and the **Get-UMIPgateway** cmdlet in the Shell, or by viewing the settings in the EAC.

## UM mailbox policies and MWI

You can create a UM mailbox policy to apply a common set of UM policy settings to a collection of UM-enabled mailboxes. For example, you can use a UM mailbox policy to apply PIN policy settings, dialing restrictions, and MWI notifications settings. If you enable or disable MWI on a UM mailbox policy, it will be enabled or disabled for all UM-enabled users who are linked with that UM mailbox policy. The MWI setting can also apply to a subset of the users who are linked with a UM dial plan. To learn more about UM mailbox policies, including how to enable or disable MWI for a group of UM-enabled users, see [UM mailbox policy procedures in Exchange Server](um-mailbox-policy-procedures-exchange-2013-help.md).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in the Shell to configure the MWI setting, as shown in the following table.

### Message Waiting Indicator setting on a UM mailbox policy

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Shell parameter</th>
<th>Setting available in the EAC?</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>AllowMessageWaitingIndicator</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>AllowMessageWaitingIndicator</em> parameter specifies whether users who are linked with a UM mailbox policy can receive MWI notifications when they receive a new voice message. The default value is <code>$true</code>.</p>
<p>When this setting is enabled, MWI notifications are sent to users who are linked with a single UM mailbox policy for calls taken by a UM IP gateway. This setting allows the UM IP gateway to receive and send SIP NOTIFY messages to UM-enabled users' phones or SIP endpoints.</p>
<p></p></td>
</tr>
</tbody>
</table>

For more information about how to manage MWI settings on a UM mailbox policy, see the following topics:

- [Manage a UM mailbox policy in Exchange Server](manage-um-mailbox-policy-exchange-2013-help.md)

- [Enable Message Waiting Indicator (MWI) for users in Exchange Server](enable-mwi-for-users-exchange-2013-help.md)[Enable Message Waiting Indicator (MWI) for users]

- [Disable Message Waiting Indicator (MWI) for users in Exchange Server](disable-mwi-for-users-exchange-2013-help.md)

- [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-UMMailboxPolicy)

## UM IP gateways and MWI

If you disable MWI on a UM IP gateway, you'll disable MWI notifications for all users who connect to the VoIP gateway or IP PBX that's represented by the UM IP gateway. Disabling MWI on a single UM IP gateway that's linked to a UM dial plan can disable MWI notifications for all UM-enabled users associated with a single or multiple UM dial plans or a single or multiple UM mailbox policies. To learn more about UM mailbox policies, including how to enable or disable MWI for a group of UM-enabled users, see [Manage a UM mailbox policy in Exchange Server](manage-um-mailbox-policy-exchange-2013-help.md).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in the Shell to configure the MWI setting, as shown in the following table.

### Message Waiting Indicator setting on a UM IP gateway

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Shell parameter</th>
<th>Setting available in the EAC?</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>MessageWaitingIndicatorAllowed</em></p></td>
<td><p>Yes</p></td>
<td><p>The <em>MessageWaitingIndicatorAllowed</em> parameter specifies whether to enable the UM IP gateway to allow SIP NOTIFY messages to be sent to users associated with a UM dial plan. The default value is <code>$true</code>.</p>
<p>When this setting is enabled, voice mail notifications can be sent to users for calls that are received by the UM IP gateway. This setting allows the UM IP gateway to send message-waiting notifications to UM-enabled users.</p></td>
</tr>
</tbody>
</table>

For more information about how to manage MWI settings, see the following topics:

- [Manage a UM IP gateway in Exchange Server](manage-um-ip-gateway-exchange-2013-help.md)

- [Allow Message Waiting Indicator (MWI) on a UM IP gateway in Exchange Server](allow-mwi-on-um-ip-gateway-exchange-2013-help.md)

- [Prevent Message Waiting Indicator (MWI) on a UM IP gateway in Exchange Server](prevent-mwi-on-um-ip-gateway-exchange-2013-help.md)

- [Set-UMIPGateway](https://docs.microsoft.com/powershell/module/exchange/Set-UMIPGateway)

## Text message (SMS) notifications for voice mail messages and missed calls

As mentioned earlier, an MWI notification is any mechanism that indicates the existence of a new voice mail message. In addition to the mechanisms already discussed, users can be notified that they have a voice message waiting via a text message, also called an SMS (Short Message Service) message. This is a different type of MWI notification for new voice messages than the traditional light or other mechanisms.

A text message is sent to a user's mobile phone when a caller leaves a new voice message. Users can also receive a text message that notifies them when they miss a phone call and a voice message isn't left. The missed call notification text message can be sent to the user along with the new voice mail notification.

> [!NOTE]
> The text message that's sent to a user includes voice mail preview.

Text message notifications use different settings than the MWI settings on the UM IP gateway or the UM mailbox policy. Text message notifications for new voice mail and missed calls are configured on UM mailbox policies and UM mailboxes. You can enable or disable text message notifications by using the **Set-UMMailboxPolicy** cmdlet and the **Set-UMMailbox** cmdlet in the Shell. You can view the status of text message notifications by using the **Get-UMMailboxPolicy** cmdlet and the **Get-UMMailbox** cmdlet. It's not possible to configure text message notifications in the EAC.

The following table shows the parameter on a UM mailbox that must be configured for a user to receive text messages for voice mail and missed call notifications:

### Text message notification settings on a user's mailbox

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><em>UMSMSNotificationOption</em></p></td>
<td><p>No</p></td>
<td><p>The <em>UMSMSNotificationOption</em> parameter specifies whether a UM-enabled user can receive text message notifications for voice mail only, for voice mail and missed calls, or isn't allowed to receive notifications. The values for this parameter are: <code>VoiceMail</code>, <code>VoiceMailAndMissedCalls</code>, and <code>None</code>. The default value is <code>None</code>.</p>
<p></p></td>
</tr>
</tbody>
</table>

For more information about how to manage text message notification settings on a user's mailbox, see the following topics:

- [Manage voice mail settings for a user in Exchange Server](manage-voice-mail-settings-exchange-2013-help.md)

- [Set-UMMailbox](https://docs.microsoft.com/powershell/module/exchange/Set-UMMailbox)

The following table shows the parameter on a UM mailbox policy that must be configured for a user to receive text messages for voice mail and missed call notifications:

### Text message and missed call notification settings on a UM mailbox policy

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Shell parameter</th>
<th>Setting available in the EAC?</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>AllowSMSNotification</em></p></td>
<td><p>No</p></td>
<td><p>The <em>AllowSMSNotification</em> parameter specifies whether UM-enabled users whose mailboxes are associated with the UM mailbox policy are allowed to receive text message notifications on their mobile phones. If this parameter is set to <code>$true</code>, you must also use the <strong>Set-UMMailbox</strong> cmdlet and set the <em>UMSMSNotificationOption</em> parameter for the UM-enabled user to either <code>voicemail</code> or <code>VoiceMailAndMissedCalls</code>. The default value is <code>$true</code>.</p>
<p></p></td>
</tr>
</tbody>
</table>

For more information about how to manage text message notification settings, see the following topics:

- [Manage a UM mailbox policy in Exchange Server](manage-um-mailbox-policy-exchange-2013-help.md)

- [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/Set-UMMailboxPolicy)

For text message notifications for voice mail and missed calls to work correctly, you must perform the following tasks:

1. Use either the EAC or the Shell to enable the user for UM and link them to the correct UM mailbox policy.

2. On the UM mailbox policy that's linked to the user, verify that the *AllowSMSNotification* parameter is set to `$true`. To set the parameter to `$true`, run the following command: `Set-UMMailboxPolicy -id MyUMMailboxPolicy - AllowSMSNotification $true`.

3. On the user's mailbox, enable text message notifications by setting the *UMSMSNotificationOption* parameter to `VoiceMailAndMissedCalls` or `VoiceMail`.

4. Because the default setting is `None`, you must run the following command from the Shell and set the text message notification option to either `VoiceMailAndMissedCalls` or `VoiceMail`. For example: `Set-UMMailbox- -id MyUMMailbox -UMSMSNotificationOption VoiceMailAndMissedCalls`.

    > [!IMPORTANT]
    > The <EM>AllowSMSNotification</EM> parameter on the UM mailbox policy and the <EM>UMSMSNotificationOption</EM> parameter on the user's mailbox must both be set to <CODE>$true</CODE> for SMS notifications to work.

In addition to your configuring the UM mailbox policy and the user's mailbox to enable text message notifications for new voice mail and missed calls, the user must enable and configure text message notifications when they sign in to Outlook Web App. To set up and configure text message notifications, the user must:

1. Sign in to Outlook Web App and go to **Options** \> **Phone** \> **Voice mail**.

2. On the **Voice Mail** page, under **Notifications**, click **Set up notifications**.

3. On the **Text messaging** page, click the **Turn on notifications** button.

    > [!WARNING]
    > Don't click <STRONG>Voice mail notifications</STRONG> or it will take you back to the <STRONG>Voice mail</STRONG> page.

4. On the **Text messaging** page, under **Locale**, use the drop-down list to select the locale or location of the text messaging mobile operator.

5. On the **Text messaging** page, under **Mobile operator**, use the drop-down list to select the text messaging mobile operator, and then click **Next**.

6. On the **Text messaging** page, in the **Enter your phone number and click Next** box, enter the mobile phone number that's used for text message notifications, and then click **Next**. A six-digit passcode will be sent to the mobile phone. If you didn't receive a passcode, click **I didn't receive a passcode and need it sent again**.

7. Enter the passcode in the **Passcode** box, and then click **Finish**.

8. After the user enables text message notifications, they can click **Set up voice mail notifications** on the **Text Messaging** page. They'll be taken back to the voice mail page, where they can scroll down to the **Notifications** section and set up text message notification options for missed calls and voice mail.
