---
localization_priority: Normal
description: Microsoft Exchange Unified Messaging (UM) includes a feature called Voice Mail Preview, which uses automatic speech recognition (ASR) to add a text version of the voice mail audio file to voice mail messages. ASR isn't entirely accurate, especially when it's used to record audio over a phone that contains unknown voices and noises. Some organizations require consistently error-free (or near-error-free) transcripts of voice messages. The Voice Mail Preview Partner program can help such organizations meet those requirements.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 0957dd54-df6d-4b50-9db5-4757f548b899
ms.reviewer: 
f1.keywords:
- NOCSH
title: Voice Mail Preview advisor in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Voice Mail Preview advisor in Exchange Online

Microsoft Exchange Unified Messaging (UM) includes a feature called Voice Mail Preview, which uses automatic speech recognition (ASR) to add a text version of the voice mail audio file to voice mail messages. ASR isn't entirely accurate, especially when it's used to record audio over a phone that contains unknown voices and noises. Some organizations require consistently error-free (or near-error-free) transcripts of voice messages. The Voice Mail Preview Partner program can help such organizations meet those requirements.

Voice Mail Preview uses Microsoft speech technologies to provide a text version of audio recordings. The voice mail text is displayed in email messages within Outlook on the web (formerly known as Outlook Web App), Outlook 2010 or later versions, and other email programs.

By default, when you enable a user for UM in an on-premises or hybrid deployment, voice mail previews will be sent if a supported UM language pack is installed. When you enable a user for UM in Exchange Online, all the UM language packs are installed. However, Voice Mail Preview isn't supported in all languages that are installed.

There are Voice Mail Preview partners that offer enhanced transcription support and services for the Voice Mail Preview feature. These partners employ people to correct voice mail transcriptions that were created using ASR. Each Voice Mail Preview partner must meet a set of requirements to be certified to interoperate with Exchange UM.

If you determine that the voice mail previews sent to your users aren't accurate enough, you can contact one of the certified Voice Mail Preview partners listed at [Microsoft Pinpoint](https://go.microsoft.com/fwlink/p/?LinkId=281966) and sign up with them at an additional cost.

## Overview

When Unified Messaging records the audio for a voice message, it uses ASR to create voice mail preview text from the audio file, and then submits the whole voice message for delivery to the user. For each voice message that's created, Unified Messaging determines a confidence level for the voice mail preview included with the message. It measures how well the sounds in the recording match the words, numbers, and phrases in the message. If the system finds matches easily, the confidence level will be high. A higher level of confidence is generally associated with a higher accuracy.

The accuracy of voice mail preview text depends on many factors, and sometimes those factors can't be controlled. However, the text is likely to be more accurate when:

- A simple voice message is left, and the caller doesn't use slang terms, technical jargon, or unusual words or phrases.

- The caller uses a language that's easily recognized and translated by the voice mail system. Generally, voice messages left by callers who don't speak too quickly or too softly and who don't have strong accents will produce more accurate sentences and phrases.

- The voice message is free of background noise and echoes, and the audio doesn't drop out.

Most customers who use Unified Messaging find that the voice mail previews are accurate enough for their users. However, when ASR is applied to recordings made over the phone by unknown voices and background noises, the voice mail preview text usually isn't completely accurate. If the level of confidence is consistently low or the voice mail previews that are received aren't very accurate, you can increase the accuracy of the voice mail previews that users receive as follows:

- Sign up for a voice transcription service from a Voice Mail Preview partner.

- After you've signed up with a Voice Mail Preview partner, set the partner up to work with UM. For more information about how to configure UM for a Voice Mail Preview partner, see [Configure Voice Mail Preview partner services for users](configure-voice-mail-preview-partner-services.md).

When you've signed up with a Voice Mail Preview partner, the Exchange servers in your organization redirect voice messages with the audio file attached to the Voice Mail Preview partner instead of generating voice mail preview text for voice messages and submitting the voice messages to the user's mailbox. The email message with the voice mail preview text produced by the Voice Mail Preview partner is then submitted to the Exchange servers in your organization for delivery to the recipient's mailbox.

> [!IMPORTANT]
> We recommend that all customers who plan to deploy Unified Messaging obtain the assistance of a UM specialist. A UM specialist helps you ensure that there's a smooth transition to UM from a legacy voice mail system. Performing a new deployment or upgrading a legacy voice mail system requires significant knowledge about VoIP gateways, IP PBXs, PBXs, session border controllers (SBCs), and Unified Messaging. To contact a Unified Messaging specialist, see the [Microsoft solution providers](https://www.microsoft.com/solution-providers/) page.

## Exchange Unified Messaging Voice Mail Partner program

To become certified as a Voice Mail Preview partner that interoperates with Exchange UM, the partner must implement the requirements contained in the Voice Mail Preview Interoperability Specification, and the partner solution must be certified by an independent certification vendor.

## Voice Mail Preview partners certified for Exchange Unified Messaging

If you've already deployed Unified Messaging in your organization and you're looking for a certified Voice Mail Preview partner to provide transcription support services, see [Microsoft PinPoint](https://go.microsoft.com/fwlink/p/?LinkId=281966). These software vendors have been certified as interoperable with Exchange UM.

## Configuring Voice Mail Preview partners

After UM has been configured, it forwards voice messages with the audio to a dedicated Voice Mail Preview partner, which then takes the audio file and creates the voice mail preview text. However, to allow users to receive the voice mail preview with their voice message in their mailbox, you must configure a UM mailbox policy, associate users with the UM mailbox policy, and then have the users verify that they can receive voice mail previews in their voice messages in Outlook 2010 or a later version or Outlook on the web. For more information about how to configure UM for a Voice Mail Preview partner, see [Configure Voice Mail Preview partner services for users](configure-voice-mail-preview-partner-services.md).

## VoIP or media gateways and IP PBX support

Configuring VoIP gateways and IP PBXs for your organization is a difficult deployment task that must be completed correctly to successfully deploy Unified Messaging with a Voice Mail Preview partner. For information that can help you configure your VoIP gateways and IP PBXs, and for the most up-to-date information about how to configure them, see [Telephony advisor for Exchange 2013](../../voice-mail-unified-messaging/telephone-system-integration-with-um/telephony-advisor-for-exchange-2013.md) or [Configuration notes for supported VoIP gateways, IP PBXs, and PBXs](../../voice-mail-unified-messaging/telephone-system-integration-with-um/configuration-notes-for-voip-gateways.md).

Testing interoperability of Exchange UM with VoIP gateways has been integrated with the Microsoft Unified Communications Open Interoperability Program. For more information, see [Microsoft Unified Communications Open Interoperability Program](https://docs.microsoft.com/SkypeForBusiness/lync-cert/qualified-lync-apps).
