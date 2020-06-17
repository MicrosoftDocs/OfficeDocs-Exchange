---
localization_priority: Normal
description: Some Private Branch eXchange (PBX) and IP PBX telephony systems allow the caller to mark a voice mail message as private, blocking the intended recipient of the message from forwarding it to others. In integrated voice mail systems, a voice message can be accessed in multiple ways, which makes it more of a challenge to prevent voice messages marked private from being exposed to unintended listeners. Unified Messaging (UM) can be configured to protect voice messages for an organization. This feature is known as Protected Voice Mail.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bff15214-3e06-43af-b6f8-3fd341fe2389
ms.reviewer: 
f1.keywords:
- NOCSH
title: Protect voice mail in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Protect voice mail in Exchange Online

Some Private Branch eXchange (PBX) and IP PBX telephony systems allow the caller to mark a voice mail message as private, blocking the intended recipient of the message from forwarding it to others. In integrated voice mail systems, a voice message can be accessed in multiple ways, which makes it more of a challenge to prevent voice messages marked private from being exposed to unintended listeners. Unified Messaging (UM) can be configured to protect voice messages for an organization. This feature is known as Protected Voice Mail.

When a voice message is protected, the recipient is not only blocked from forwarding the message, but UM also ensures that only the intended recipient or recipients of the message can access its content. Protected voice messages can be accessed by using Outlook on the web (formerly known as Outlook Web App), or Outlook Voice Access.

## Overview of Protected Voice Mail
<a name="overview"> </a>

The Protected Voice Mail feature is available with Unified Messaging (UM). It can be configured on a UM mailbox policy, and all Protected Voice Mail settings can be configured by using the Exchange admin center (EAC) or cmdlets in Exchange Online PowerShell in Exchange Server.

Protected Voice Mail is implemented by applying Information Rights Management (IRM) to voice messages. When voice messages are protected by UM:

- Users can reply to protected voice messages.

- Recipients of a voice message can't forward it.

- Users can't save a copy of the voice message.

- Users can't save or copy the attached audio of the voice message.

- A voice message can be opened only by the intended recipient or recipients.

Both call-answering voice messages and interpersonal voice messages (voice messages that are sent to a user using Outlook Voice Access) can be protected by UM. However, protection won't be applied to the following types of messages:

- Fax messages.

- Non-voice messages. For example, email messages or meeting requests, even when they're created using Outlook Voice Access (voice replies).

## Client support and end-user features
<a name="clientsupport"> </a>

The email client software that's used to listen to a Protected Voice Mail message must support IRM and know how to read a UM-protected voice message. Email clients that are supported include Outlook, Outlook on the web, and Outlook Voice Access. The following table contains a list of email clients and whether they're supported.

|**Email client**|**Description**|
|:-----|:-----|
|Outlook|Protected voice messages are supported in Outlook 2010 and later versions.|
|Outlook on the web|Outlook on the web supports Protected Voice Mail messages.|
|Outlook Voice Access|Outlook Voice Access supports Protected Voice Mail.|
|Windows Mobile or Windows Phone|Windows Mobile doesn't support Protected Voice Mail. However, Windows Phone 7 and Windows Phone 8 support Protected Voice Mail.|
|Other third-party email clients|Protected Voice Mail isn't supported.|

## Protected voice message structure
<a name="structure"> </a>

There are actually two messages involved for each Protected Voice Mail message. The first message is the outer message, which isn't encrypted. It contains an attachment named message.rpmsg. The attachment contains the IRM-protected voice message and internal rights management control data. The rights management control data includes a content key and rights information that specifies who can access the voice message and how those users can access it.

Protected voice messages are shown in the user's Inbox in the **Voice Mail** search folder. The user can listen to the voice messages by using the embedded audio player just as they would listen to a regular voice message, except that the Forward button will be disabled and a note will be shown at the top of the message stating that it's protected and that it can't be forwarded.

For email clients that don't support Protected Voice Mail, the body of the outer message will be displayed. Administrators can include text when the client's software doesn't support Protected Voice Mail by using UM mailbox policies. You can customize the default text that's included in the email message by configuring a UM mailbox policy. For example, you could configure the UM mailbox policy with customized text such as, _"You can't open this voice mail message because it's protected. To view or listen to this voice message, sign in to your mailbox at https://mail.contoso.com or call +1 (425) 555-1234 to call in to Outlook Voice Access."_

## Composing a Protected Voice Mail message
<a name="composing"> </a>

There are two situations in which protected voice messages can be created:

- **Call answering**: Call answering occurs when a caller calls a UM-enabled user, but the user isn't available to answer the call or forwards it directly to voice mail. In call-answering scenarios, the voice mail system will play a series of voice prompts after the caller records a voice message.

    The caller can then choose from additional message options, including the option to mark the voice message as private by pressing the pound (#) key. If the caller presses the # key, they can follow the instructions provided by UM to mark the message as private, remove the private marking from the private voice message, or mark the voice message with High importance. The following diagram shows the menu options that are available to callers when they leave a private voice message for a user.

    > [!NOTE]
    > For call-answering calls, UM uses the Protected Voice Mail settings on the UM mailbox policy of the intended recipient of the message, because the caller isn't authenticated.

   **Create a Protected Voice Mail message using Call Answering**

![Create protected voice mail using call answering](../../media/ITPro_CallAnswering_PVM.jpg)

- **Outlook Voice Access**: Outlook Voice Access lets UM-enabled users access their mailbox using analog, digital, or cellular telephones by dialing their Outlook Voice Access number. There are two Unified Messaging user interfaces available to UM-enabled users: the telephone user interface (TUI) and the voice user interface (VUI).

    Outlook Voice Access users can search for contacts in the directory and send them voice messages. If Protected Voice Mail has been enabled for the UM-enabled recipients, callers can mark the messages as private after they're recorded. Alternatively, administrators can configure a UM mailbox policy to ensure that all voice messages sent by authenticated users are protected by UM.

    > [!NOTE]
    > If a caller is authenticated, the Protected Voice Mail settings on the UM mailbox policy that's linked to the caller are applied, regardless of the UM mailbox policy settings for the intended recipient of the voice message.

   **Create a Protected Voice Mail message using the voice user interface**

![Create protected voice mail using voice interface](../../media/ITPro_OVA_PVM.jpg)

   **Create a Protected Voice Mail message using the telephone user interface**

![Create protected voice mail using touchtone input](../../media/ITPro_OVA_PVM_TUI.jpg)

## UM mailbox policies
<a name="policies"> </a>

You can create a Unified Messaging mailbox policy to apply a common set of UM policy settings, such as PIN policy settings, dialing restrictions, and Protected Voice Mail settings, to a collection of UM-enabled mailboxes. To learn more about UM mailbox policies, see [Manage a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/manage-um-mailbox-policy.md) and [Protected Voice Mail procedures](protected-voice-mail-procedures.md).

You can use the EAC or the **Set-UMMailboxPolicy** cmdlet in Exchange Online PowerShell to configure Protected Voice Mail options. The following table lists the settings that can be configured for Protected Voice Mail.

 **Protected Voice Mail settings**

|**Parameter**|**Setting available in EAC?**|**Description**|
|:-----|:-----|:-----|
|_ProtectAuthenticatedVoiceMail_|Yes|The _ProtectAuthenticatedVoiceMail_ parameter specifies whether UM-enabled users can send protected voice messages when they're accessing their mailbox using Outlook Voice Access. The default setting is `None`. This means that no protection is applied when voice messages are composed and that callers won't have the option to mark voice messages as Private. If the value is set to `Private`, only messages marked as Private by the caller are protected. If the value is set to `All`, every voice message is protected, regardless of the option chosen by the caller.|
|_ProtectUnauthenticatedVoiceMail_|Yes|The _ProtectUnauthenticatedVoiceMail_ parameter specifies whether the Mailbox servers that answer calls for UM-enabled users associated with a UM mailbox policy create protected voice messages. This setting also applies when a message is sent from a UM auto attendant to a UM-enabled user. The default setting is `None`. This means that no protection is applied to voice messages and that the caller won't be offered the option to mark the message as Private. If the value is set to `Private`, only messages marked as Private by the caller are protected. If the value is set to `All`, every voice message is protected, regardless of whether if the message has been marked as private by the caller.|
|_ProtectedVoiceMailText_|Yes|The _ProtectedVoiceMailText_ parameter specifies the text to be included in the body of the outer message of a Protected Voice Mail message. This text will be shown in all email client applications that don't support Protected Voice Mail messages. Note that a default message is always provided by UM when this property is set to `Null` or is empty.|
|_RequireProtectedPlayOnPhone_|Yes|The _RequireProtectedPlayOnPhone_ parameter specifies whether users associated with the UM mailbox policy will be forced to listen to the protected voice message over the phone (using Play On Phone). The default value is `$false`. When the value is set to `$true`, the audio media player on Protected Voice Mail forms in Outlook or Outlook on the web will be shown as disabled. Note that the preview text for the voice message can always be accessed. The user can't play the audio file using any media player software or use the embedded media player to listen to the voice message.|
|_AllowVoiceResponseToOtherMessageTypes_|Yes|The _AllowVoiceResponseToOtherMessageTypes_ parameter specifies whether callers who have authenticated to Outlook Voice Access to access their email will be able to compose a voice reply to email messages and meeting requests.|

For more information about how to manage Protected Voice Mail settings, see [Protected Voice Mail procedures](protected-voice-mail-procedures.md) or [Set-UMMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/set-ummailboxpolicy).

## Text message notifications and Protected Voice Mail
<a name="notifications"> </a>

Users who configure their UM account to send text message notifications (also called SMS notifications) to their mobile phone when voice messages are received will also receive audio transcription (Voice Mail Preview) text as part of the body of the text message. However, for protected voice messages, this represents a security issue because the content of the voice messages should always be protected.

When UM creates a text message notification for a voice message that's protected, it checks whether the voice message is marked as Private. If so, it won't add the transcribed audio text to the text message that it sends to the mobile phone. The following text will be included in the text message instead: "Use Outlook Voice Access to access this protected voice mail message."
