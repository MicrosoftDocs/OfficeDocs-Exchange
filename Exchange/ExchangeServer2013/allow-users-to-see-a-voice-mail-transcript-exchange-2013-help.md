---
title: 'Allow users to see a voice mail transcript: Exchange 2013 Help'
TOCTitle: Allow users to see a voice mail transcript
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c5192e05-905c-440f-beec-1f697edc15b3
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Allow users to see a voice mail transcript in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Voice Mail Preview is a feature that's available to users who receive their voice mail messages from Unified Messaging (UM). Voice Mail Preview enhances the existing UM voice mail functionality by providing a text version of audio recordings. The voice mail text is displayed in email messages within Microsoft Outlook Web App, Outlook 2010 and later versions, and in other supported email programs. For more information, see [Microsoft Speech Technologies](https://azure.microsoft.com/services/cognitive-services/).

## Do users need to use a specific email program?

No. Voice Mail Preview is included in the message body text of any email program, including mobile programs. Although users can use other email programs to receive voice messages, Outlook and Outlook Web App provide a better experience. For example, in Outlook 2010 and later versions, when a specific word is clicked in the Voice Mail Preview text, the audio playback of the voice message will start to play at that word. This is useful for listening to a specific part of a voice message.

## Can users search for specific voice mail messages?

Yes. Words and phrases in the Voice Mail Preview text are automatically indexed, so voice messages will appear in search results. In Outlook 2010 and later versions or in Outlook Web App, users can also use the **Audio Notes** box to add text about a voice message. These notes are also included in searches, to make it easier to locate a message.

## Why is this feature called "Voice Mail Preview"?

It's important to set users' expectations correctly. Voice Mail Preview doesn't necessarily produce text that's the same as what callers say in their voice messages. In fact, it's usually inaccurate in some way. To call it transcription would suggest a more perfect result than can generally be achieved. Preview suggests that the reader should be able to understand the gist of the voice content, which is closer to the real capability of the feature.

## What makes the Voice Mail Preview text more or less accurate?

The accuracy of the Voice Mail Preview text depends by many factors and sometimes those factors can't be controlled. However, Voice Mail Preview text is likely to be more accurate when:

- The caller leaves a simple voice message that doesn't include slang terms, technical jargon, or unusual words or phrases.

- The caller uses a language that's easily recognized and translated by the voice mail system. Generally, voice messages left by callers who don't speak too quickly or too softly and who don't have strong accents will produce more accurate sentences and phrases.

- The voice message is free of background noise, echo, and the audio doesn't drop out.

## Which languages can be used with Voice Mail Preview?

Voice Mail Preview text is available in the following languages:

- English (US) (en-US)

- English (Canada) (en-CA)

- French (France) (fr-FR)

- Italian (it-IT)

- Polish (pl-PL)

- Portuguese (Portugal) (pt-PT)

- Spanish (Spain) (es-ES)

If you have an on-premises or hybrid deployment of UM, you can download the UM language packs from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=35368).

If you have an on-premises or hybrid deployment, after you install a UM language pack, the dial plans and auto attendants can be configured to use the language you've chosen. For online customers, you don't have to install any UM language packs. Many companies have only one UM dial plan. UM will try to create a voice mail preview in the default dial plan language, but will only be successful if the default language supports Voice Mail Preview. A UM dial plan can only be configured to create voice mail previews in one language at a time.

To configure UM to provide voice mail previews in a language other than en-US, follow these steps:

1. Verify that Voice Mail Preview is supported in the language you want to use.

2. If you have an on-premises or hybrid deployment, download and install the appropriate UM language pack. Downloading and installing the language pack doesn't configure the dial plan default language.

3. Configure the dial plan with the language that will be used for Voice Mail Preview. For more information, see [Set the default language on a dial plan](set-dial-plan-default-language-exchange-2013-help.md).

How Voice Mail Preview displays text in the supported languages depends on the type of voice message that's sent. There are two types:

- **Voice messages that are recorded when a user doesn't answer their phone**

    For these messages, the language used for Voice Mail Preview is determined by the caller's spoken language and whether the language is supported. For example, if a caller leaves a voice message in Italian, the Voice Mail Preview text will appear in Italian if Italian has been configured on the dial plan. However, if a caller leaves a message in Japanese, no Voice Mail Preview text will be included with the message because Japanese isn't available.

- **Voice messages that are sent to by an Outlook Voice Access user**

    For messages sent by an Outlook Voice Access user, the language that's used for Voice Mail Preview is controlled by the voice mail administrator. Thus, the Voice Mail Preview text will be in the same language as the voice mail system. However, if a caller speaking a language that's not supported for Voice Mail Preview uses Outlook Voice Access to leave a message, no Voice Mail Preview text will be included with the message. To learn more about Outlook Voice Access, see [Setting up Outlook Voice Access](set-up-outlook-voice-access-exchange-2013-help.md).

## Does UM know when a voice mail preview is inaccurate?

The confidence level is determined for each voice mail preview included with a voice message. The voice mail system measures how well the sounds in the recording match the words, numbers, and phrases. If matches are found easily, the confidence level is high. A higher level of confidence is generally associated with a higher accuracy.

If the confidence level is determined to be lower than a certain value, the phrase **Voice Mail Preview (confidence is low)** is included above the Voice Mail Preview text. If the confidence level is low, it's likely that the Voice Mail Preview text will be inaccurate.

Unified Messaging uses Automatic Speech Recognition (ASR) to calculate its confidence in the preview, but it has no way to determine which words are wrong and which are correct.

However, UM does try to learn to improve accuracy of its voice mail previews. For example, it tries to match the caller's telephone number (if provided) with the user's personal Contacts and your organization's address book or contacts from social networks. If UM finds a match, it will include the name of the caller, along with its standard lists of names and words, when running ASR on the voice recording.

### Can Voice Mail Preview be used if it isn't completely accurate?

Users may have a better experience with Voice Mail Preview if they don't try to read the preview too carefully, word by word. Instead, they should look for names, phone numbers, and phrases such as "Call me back" or "I need to talk" that may provide clues about the purpose of the call.

Voice Mail Preview isn't expected to dictate messages exactly, but it can help users answer questions such as the following:

- Is this voice message related to my work?

- Is this voice message important to me?

- Did the caller leave a number? Is it different from any numbers that I may have listed for them?

- Does the caller consider this voice message urgent?

- Should I step out of a meeting to call this person back?

- I was expecting a call to confirm my request. Is this the confirmation call?

## Can Voice Mail Preview be turned on or off?

Yes. If you've enabled Voice Mail Preview, users can turn it on or off using Outlook 2010 or a later version or Outlook Web App. However, the dial plan language must support Voice Mail Preview and the UM language pack for that language must be installed.

Although Voice Mail Preview settings are the same whether a user is using Outlook 2010 or a later version or Outlook Web App, they'll access them differently:

 **Outlook Web App**

To access the Voice Mail Preview settings in Outlook Web App, users click **Settings** \> **phone** \> **Voice mail**. On the **Voice mail** page, the settings are available under **voice mail preview**.

By default, both Voice Mail Preview options are available when a user is enabled for Unified Messaging. If the UM dial plan is configured to use a UM language pack that supports Voice Mail Preview, Unified Messaging will create voice mail previews for users when:

- A caller leaves a voice mail message because the user doesn't answer their phone.

- A UM-enabled user signs in to Outlook Voice Access and records a voice message for one or more recipients.

When a caller leaves a voice message, and **Include preview text with voice messages I receive** is selected, Unified Messaging will create a voice mail preview in the email message, attach the audio file, and send it to the recipient's mailbox. You may want to disable this option if the language that's configured on the dial plan doesn't include Voice Mail Preview support and you don't want voice mail previews included in voice mail messages.

When users sign in to Outlook Voice Access and they send a voice message to another user, they may want to clear the **Include preview text with voice messages I send through Outlook Voice Access** check box. For example, they might want to do this if they're sending voice messages in a language that Voice Mail Preview doesn't support or if they don't want to include the voice mail preview with the voice message because it's too long.
