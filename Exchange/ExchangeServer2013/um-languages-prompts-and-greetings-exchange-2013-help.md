---
title: 'UM languages, prompts, and greetings: Exchange 2013 Help'
TOCTitle: UM languages, prompts, and greetings
ms:assetid: d48df962-9669-420b-838f-44bfe1012e2f
ms:mtpsurl: https://technet.microsoft.com/library/Bb124728(v=EXCHG.150)
ms:contentKeyID: 49315533
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# UM languages, prompts, and greetings in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can install and configure language packs to support multiple languages in Unified Messaging (UM) environments.

UM language packs enable callers and Outlook Voice Access users to interact with the voice mail system in multiple languages. After you install an additional language on a Mailbox server, callers and Outlook Voice Access users can hear email messages and interact with the voice mail system in that language.

There are several key components that rely on UM language packs to enable users and callers to interact effectively with Unified Messaging in multiple languages. Each UM language pack includes a Text-to-Speech (TTS) engine, the pre-recorded prompts and support for Automatic Speech Recognition (ASR), and Voice Mail Preview for a specific language. This topic discusses UM language packs, the UM components that use the UM language packs, and how UM language packs (after they're installed) can be used to configure UM dial plans and UM auto attendants to use other languages.

Exchange Unified Messaging language packs are version-specific and platform-specific. Since Exchange Server 2007, there have been multiple releases for UM language packs, including the RTM version of Exchange 2007, Exchange 2007 SP1, SP2, and SP3, the RTM version of Exchange Server 2010, Exchange 2010 SP1 and SP2, and Exchange 2013. For some of these versions, both 32-bit and 64-bit downloads are available, but for other releases only 64-bit downloads are available.

It's very important that you install the correct version and platform of the UM language packs on a Mailbox server. Don't install UM language packs on a Mailbox server that's running an earlier version of Exchange or that's designed for a 32-bit platform.

## Overview of UM language packs

Unified Messaging language packs allow a Mailbox server to speak additional languages to callers and recognize other languages when callers use ASR or when voice messages are transcribed. UM language packs contain:

- Pre-recorded prompts in the language of the UM language pack. For example, "After the tone, please record your message. When you've finished recording, hang up, or press the \# key for more options."

- Grammar files in the language of the UM language pack that are used by a Mailbox server to look up the names of given users in the directory.

- Text-to-Speech (TTS) translation so that content (email, calendar, contact information, etc.) can be read to callers in the language of the UM language pack.

- Support for Automatic Speech Recognition, which allows callers to interact with UM using the voice user interface (VUI) in the language of the UM language pack.

- Support for Voice Mail Preview, which allows users to read the transcript of voice mail messages in a specific language from within a supported email client such as Outlook or Outlook Web App.

UM language packs include pre-recorded prompts, TTS conversion support for a specific language, and in some cases, support for ASR. In multiple-language environments, you may have to install additional UM language packs because some callers prefer to be prompted in a different language, or because they receive email in more than one language. You must install multiple UM language packs to support the ability of the Mailbox server to read an email message that contains more than one language, because the TTS conversion system must be instructed which language to select based on the text of the message to be read. If the Unified Messaging language pack hasn't been installed, the email message will be illogical and incoherent when it's read back to the user. Installing the appropriate language pack enables the TTS engine to read email and calendar items to the Outlook Voice Access user by using the correct language and also provides language-specific pre-recorded prompts for Unified Messaging. In some cases, they may also provide support for ASR.

> [!NOTE]
> The TTS engine converts text to speech, but it doesn't convert speech to text. UM-enabled users can send an email message that has a voice file attached to another user. However, they can't create and send a text-based email message to another user.

When you install a language pack, the installation program does the following:

1. Copies the language prompts that will be used to configure UM dial plans and auto attendants.

2. Allows the TTS engine to read messages when Outlook Voice Access users access their Inbox.

3. Enables ASR for speech-enabled UM dial plans and auto attendants for the language installed.

4. Enables Voice Mail Preview for clients in other languages.

You can add UM language packs by using the **Setup.exe** command or by running the *\<UMLanguagePack\>*.exe installation program after you've downloaded the UM language pack from [Exchange Server 2013 UM Language Packs](https://go.microsoft.com/fwlink/p/?linkid=266542). However, you have to use the Setup.exe command to remove a UM language pack. There's no Exchange Management Shell cmdlet that you can use to add or remove languages from a Mailbox server. For more information about how to install a UM language pack, see [Install a UM language pack](install-a-um-language-pack-exchange-2013-help.md). For more information about how to remove a UM language pack, see [Remove a UM language pack](remove-a-um-language-pack-exchange-2013-help.md).

> [!NOTE]
> By default, when you install a Mailbox server, the U.S. English (en-US) language pack is installed. It can't be removed unless you remove the Mailbox server from the computer.

The following table lists the Unified Messaging language packs that are currently available. It also lists the installation file name for each UM language pack and the culture ID for the UM language.

### UM language pack installation file names and culture IDs

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Language</th>
<th>Country/Region</th>
<th>Culture ID</th>
<th>Installation file name</th>
<th>Availability</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Catalan</p></td>
<td><p>Spain</p></td>
<td><p>ca-ES</p></td>
<td><p>UMLanguagePack. ca-ES</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Chinese (Hong Kong)</p></td>
<td><p>China</p></td>
<td><p>zh-HK</p></td>
<td><p>UMLanguagePack. zh-HK</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Chinese (Simplified)</p></td>
<td><p>China</p></td>
<td><p>zh-CHS</p></td>
<td><p>UMLanguagePack.zh-CN</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Chinese (Traditional)</p></td>
<td><p>Taiwan</p></td>
<td><p>zh-TW</p></td>
<td><p>UMLanguagePack.zh-TW</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Danish</p></td>
<td><p>Denmark</p></td>
<td><p>da-DK</p></td>
<td><p>UMLanguagePack.da-DK</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Dutch</p></td>
<td><p>Netherlands</p></td>
<td><p>nl-NL</p></td>
<td><p>UMLanguagePack.nl-NL</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>Australia</p></td>
<td><p>en-AU</p></td>
<td><p>UMLanguagePack.en-AU</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>English</p></td>
<td><p>Canada</p></td>
<td><p>en-CA</p></td>
<td><p>UMLanguagePack. en-CA</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>India</p></td>
<td><p>en-IN</p></td>
<td><p>UMLanguagePack. en-IN</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>English</p></td>
<td><p>United Kingdom</p></td>
<td><p>en-GB</p></td>
<td><p>UMLanguagePack.en-GB</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>United States</p></td>
<td><p>en-US</p></td>
<td><p>Included with installation of a Mailbox server</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Finnish</p></td>
<td><p>Finland</p></td>
<td><p>fi-Fl</p></td>
<td><p>UMLanguagePack.fi-Fl</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>French</p></td>
<td><p>Canada</p></td>
<td><p>fr-CA</p></td>
<td><p>UMLanguagePack.fr-CA</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>French</p></td>
<td><p>France</p></td>
<td><p>fr-FR</p></td>
<td><p>UMLanguagePack.fr-FR</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>German</p></td>
<td><p>Germany</p></td>
<td><p>de-DE</p></td>
<td><p>UMLanguagePack.de-DE</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Italian</p></td>
<td><p>Italy</p></td>
<td><p>it-IT</p></td>
<td><p>UMLanguagePack.it-IT</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Japanese</p></td>
<td><p>Japan</p></td>
<td><p>ja-JP</p></td>
<td><p>UMLanguagePack.ja-JP</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Korean</p></td>
<td><p>Korean</p></td>
<td><p>ko-KR</p></td>
<td><p>UMLanguagePack.ko-KR</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Norwegian (Bokmal)</p></td>
<td><p>Norway</p></td>
<td><p>nb-NO</p></td>
<td><p>UMLanguagePack.nb-NO</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Polish</p></td>
<td><p>Poland</p></td>
<td><p>pl-PL</p></td>
<td><p>UMLanguagePack.pl-PL</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Portuguese</p></td>
<td><p>Brazil</p></td>
<td><p>pt-BR</p></td>
<td><p>UMLanguagePack.pt-BR</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Portuguese</p></td>
<td><p>Portugal</p></td>
<td><p>pt-PT</p></td>
<td><p>UMLanguagePack.pt-PT</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Russian</p></td>
<td><p>Russia</p></td>
<td><p>ru-RU</p></td>
<td><p>UMLanguagePack. ru-RU</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Spanish</p></td>
<td><p>Spain</p></td>
<td><p>es-ES</p></td>
<td><p>UMLanguagePack.es-ES</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="odd">
<td><p>Spanish</p></td>
<td><p>Mexico</p></td>
<td><p>es-MX</p></td>
<td><p>UMLanguagePack.es-MX</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
<tr class="even">
<td><p>Swedish</p></td>
<td><p>Sweden</p></td>
<td><p>sv-SE</p></td>
<td><p>UMLanguagePack.sv-SE</p></td>
<td><p><a href="https://go.microsoft.com/fwlink/p/?linkid=266542">Download available</a></p></td>
</tr>
</tbody>
</table>

## UM language components and features

There are several key components and features in Unified Messaging that enable users and callers to interact with a multiple-language voice mail system. For these components and features to work correctly and enable callers to interact with the system in multiple languages, the UM language packs must be installed correctly on a Mailbox server.

## Pre-recorded prompts

The Mailbox server role is installed with a set of default audio prompt files. These audio files contain the recordings for Outlook Voice Access menus, voice mail greetings, and numbers that are used by Exchange Unified Messaging. The audio files are played by a Mailbox server to incoming callers, both internal and external. Many of the audio files are default prompts that provide the users of the Telephone User Interface (TUI) and Outlook Voice Access the information they need to move through the TUI and the Voice User Interface (VUI). The prompts are located in \<*Program Files*\>\\Microsoft\\Exchange Server\\V15\\UnifiedMessaging\\Prompts\\\<language\>. The prompts used by the Mailbox server to help callers move through the menus shouldn't be replaced or changed.

When an additional UM language pack is installed, the pre-recorded prompts for that language will also be installed. After a UM language pack is installed, the pre-recorded prompts for that language can be used by UM dial plans and auto attendants.

## TTS languages

Unified Messaging relies on a Text-to-Speech (TTS) engine. TTS functionality is provided by the Microsoft Speech Server service. The TTS engine reads and converts written text into audible output that can be heard by a caller. The TTS engine reads and converts the following items in a user's mailbox:

- Email and voice mail message bodies, subjects, and names

- Calendar item bodies, subjects, locations, and names

- Personal contact names

- Users' default voice mail greetings

> [!NOTE]
> After a user has recorded personalized voice mail greetings, the TTS version of the voice greetings are no longer used.

## Automatic Speech Recognition

In addition to TTS, ASR support is included in Unified Messaging. ASR functionality is provided by the Microsoft Speech Server service. ASR enables callers to use voice commands to move through menus and interact with items from their individual mailboxes, including messages, personal contacts, and calendar. ASR support is included with each language pack.

## Voice Mail Preview

UM language packs also provide support for Voice Mail Preview, which allows users to quickly triage their voice messages by reading their transcripts from within a supported email client such as Outlook or Outlook Web App.

When a caller leaves a voice message for a UM-enabled user, the voice message file and a transcript of the voice message are placed in the body of the voice message that's sent to the user's mailbox.

All UM language packs are single files that can be downloaded. These language packs include the pre-recorded prompts, grammar files, Text-to-Speech (TTS) translation, and ASR. However, not all the UM language packs contain support for Voice Mail Preview.

The following UM language packs contain support for all the components and features, including Voice Mail Preview:

- English (US) - (en-US)

- English (Canada) (en-CA)

- French (France) - (fr-FR)

- Italian - (it-IT)

- Polish (pl-PL)

- Portuguese (Portugal) (pt-PT)

- Spanish (Spain) (es-ES)

By default, after you install the Mailbox server, the server will send voice mail previews to UM-enabled users if a supported UM language pack is installed.

There are Unified Messaging Voice Mail Preview partners that offer enhanced transcription support for the Voice Mail Preview feature. These partners employ people to correct voice mail transcriptions that were created using ASR. Each Voice Mail Preview partner must meet a set of requirements to be certified to interoperate with Unified Messaging.

If you find that the voice mail previews sent to your users aren't accurate enough, you can contact one of the certified Voice Mail Preview partners listed on the [Microsoft Solution Providers](https://www.microsoft.com/solution-providers) website and sign up with them at an additional cost.

You can download the UM language packs from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkid=266542). For details, see [Install a UM language pack](install-a-um-language-pack-exchange-2013-help.md).

## Unified Messaging languages

To enable callers to use the multiple language features found in Unified Messaging, you must first install a UM language pack. Then you have the option to configure other UM components.

- Install the UM language pack on the Mailbox servers in your organization.

- If required, configure the default language for a UM dial plan. This lets Outlook Voice Access users associated with the UM dial plan use the new language when they access their mailbox. However, users can still configure their language setting in the Outlook Web App Options.

- If you need to enable multiple languages on auto attendants, configure the language setting on a UM auto attendant. By default, a UM auto attendant uses the UM dial plan language. However, you can change this setting and enable unauthenticated callers to connect to your organization and move through the auto attendant menus in the language that you've specified on the UM auto attendant.

## Unified Messaging language packs

You install a UM language pack on a Mailbox server using Setup.exe. After you install the new language pack, the language associated with the language pack will be added to the list of available languages that you can use. You can view the languages that have been installed using the [Get-UMService](https://docs.microsoft.com/powershell/module/exchange/Get-UMService) cmdlet in the Exchange Management Shell.

When you install the UM language pack, the files that are used by the TTS engine and the pre-recorded prompts for the chosen language are copied and made available for users who connect to the voice mail system. You can download the UM language packs from the [Microsoft Download Center](https://go.microsoft.com/fwlink/p/?linkid=266542). For details, see [Install a UM language pack](install-a-um-language-pack-exchange-2013-help.md).

## UM dial plan languages

Each UM dial plan that's created contains a default language setting. The UM dial plan language setting is needed because Unified Messaging may have to use TTS conversion or play a standard audio prompt for Outlook Voice Access users when they access their mailbox. You don't have to select a default dial plan language.

When you first install Exchange, U.S. English will be the default language, and the only available language option for your dial plan. After you install a UM language pack on a Mailbox server, the language associated with the language pack will be listed as an available option when you configure the default language for the dial plan.

The default language is important to callers. When an Outlook Voice Access user calls in to the voice mail system, the language that is used is based on the language setting in Outlook Web App that was set when the user first signed in to their mailbox. Unified Messaging compares the language set in Outlook Web App to the list of available languages on the dial plan with which the user is associated. If there is no suitable match, the default UM dial plan language will be used. For example, if you have a dial plan that contains only users from France, you may want to change the default language setting on the dial plan to French.

## UM auto attendant languages

By default, because UM auto attendants are associated with a UM dial plan when they're created, they use the default language setting of the associated UM dial plan. However, this setting can be changed after the UM auto attendant is created.

The UM auto attendant language setting is needed because Unified Messaging may have to use TTS conversion or play a standard audio prompt to a caller. Unified Messaging doesn't check whether the language of custom prompts for the auto attendant matches the language setting on the auto attendant. However, as a best practice, make sure that the language setting of the auto attendant matches the language of the custom prompts. Otherwise, the caller may hear the system shift from one language to another.

Being able to change the UM auto attendant language setting is also useful if you need several different language-specific auto attendants for callers.

## Client language selection process

UM language packs enable callers and Outlook Voice Access users to interact with the Unified Messaging system in multiple languages. After you install additional language packs on a Mailbox server, callers and Outlook Voice Access users can hear email messages and interact with the voice mail system, and Outlook Web App users and users who are using Outlook 2010 or a later version can view the transcript of a voice message using Voice Mail Preview in a specific language.

To support a specific language, a UM client language pack for that language must be installed on each Mailbox server.

In some cases, if a UM language pack for a specific language hasn't been installed and isn't available, a client fallback language may be used. Fallback UM client languages are available to be used in place of some languages but, for other languages, no fallback language is available. If there isn't a UM language pack installed for a specific language, and no fallback language is available for that language, en-US (US English) will be used.

The following table includes a list of client languages and the fallback languages that are used when a specific UM language pack hasn't been installed on a Mailbox server.

### Client fallback languages for UM

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Language</th>
<th>Country/Region</th>
<th>Culture ID</th>
<th>First language chosen, if installed</th>
<th>Second language chosen, if installed</th>
<th>Third language chosen, if installed</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Catalan</p></td>
<td><p>Spain</p></td>
<td><p>ca-ES</p></td>
<td><p>ca-ES</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Chinese (Hong Kong)</p></td>
<td><p>China</p></td>
<td><p>zh-HK</p></td>
<td><p>zh-HK</p></td>
<td><p>zh-CN</p></td>
<td><p>zh-TW</p></td>
</tr>
<tr class="odd">
<td><p>Chinese (Simplified)</p></td>
<td><p>China</p></td>
<td><p>zh-CN</p></td>
<td><p>zh-CN</p></td>
<td><p>zh-HK</p></td>
<td><p>zh-TW</p></td>
</tr>
<tr class="even">
<td><p>Chinese (Traditional)</p></td>
<td><p>Taiwan</p></td>
<td><p>zh-TW</p></td>
<td><p>zh-TW</p></td>
<td><p>zh-HK</p></td>
<td><p>zh-CN</p></td>
</tr>
<tr class="odd">
<td><p>Danish</p></td>
<td><p>Denmark</p></td>
<td><p>da-DK</p></td>
<td><p>da-DK</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Dutch</p></td>
<td><p>Netherlands</p></td>
<td><p>nl-NL</p></td>
<td><p>nl-NL</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>Australia</p></td>
<td><p>en-AU</p></td>
<td><p>en-AU</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>English</p></td>
<td><p>Canada</p></td>
<td><p>en-CA</p></td>
<td><p>en-CA</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>India</p></td>
<td><p>en-IN</p></td>
<td><p>en-IN</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>English</p></td>
<td><p>United Kingdom</p></td>
<td><p>en-GB</p></td>
<td><p>en-GB</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>English</p></td>
<td><p>United States</p></td>
<td><p>en-US</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Finnish</p></td>
<td><p>Finland</p></td>
<td><p>fi-FL</p></td>
<td><p>fi-FL</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>French</p></td>
<td><p>Canada</p></td>
<td><p>fr-CA</p></td>
<td><p>fr-CA</p></td>
<td><p>fr-FR</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="even">
<td><p>French</p></td>
<td><p>France</p></td>
<td><p>fr-FR</p></td>
<td><p>fr-FR</p></td>
<td><p>fr-CA</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="odd">
<td><p>German</p></td>
<td><p>Germany</p></td>
<td><p>de-DE</p></td>
<td><p>de-DE</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Italian</p></td>
<td><p>Italy</p></td>
<td><p>it-IT</p></td>
<td><p>it-IT</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>Japanese</p></td>
<td><p>Japan</p></td>
<td><p>ja-JP</p></td>
<td><p>ja-JP</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Korean</p></td>
<td><p>Korea</p></td>
<td><p>ko-KR</p></td>
<td><p>ko-KR</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>Norwegian (Bokmal)</p></td>
<td><p>Norway</p></td>
<td><p>nb-NO</p></td>
<td><p>nb-NO</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Polish</p></td>
<td><p>Poland</p></td>
<td><p>pl-PL</p></td>
<td><p>pl-PL</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="odd">
<td><p>Portuguese</p></td>
<td><p>Brazil</p></td>
<td><p>pt-BR</p></td>
<td><p>pt-BR</p></td>
<td><p>pt-PT</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="even">
<td><p>Portuguese</p></td>
<td><p>Portugal</p></td>
<td><p>pt-PT</p></td>
<td><p>pt-PT</p></td>
<td><p>pt-BR</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="odd">
<td><p>Russian</p></td>
<td><p>Russia</p></td>
<td><p>ru-RU</p></td>
<td><p>ru-RU</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>Spanish</p></td>
<td><p>Spain</p></td>
<td><p>es-ES</p></td>
<td><p>es-ES</p></td>
<td><p>es-MX</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="odd">
<td><p>Spanish</p></td>
<td><p>Mexico</p></td>
<td><p>es-MX</p></td>
<td><p>es-MX</p></td>
<td><p>es-ES</p></td>
<td><p>en-US</p></td>
</tr>
<tr class="even">
<td><p>Swedish</p></td>
<td><p>Sweden</p></td>
<td><p>sv-SE</p></td>
<td><p>sv-SE</p></td>
<td><p>en-US</p></td>
<td><p></p></td>
</tr>
</tbody>
</table>
