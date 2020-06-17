---
title: 'Unified Messaging audio codecs: Exchange 2013 Help'
TOCTitle: Audio codecs
ms:assetid: 6c39d65c-c2d3-4128-aae9-8596602819c3
ms:mtpsurl: https://technet.microsoft.com/library/Aa998670(v=EXCHG.150)
ms:contentKeyID: 49315434
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Unified Messaging audio codecs in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

In Unified Messaging (UM), an audio codec is used to store voice mail messages. Another codec is used between a VoIP gateway or IP Private Branch eXchange (PBX) and the Mailbox server running the Microsoft Exchange Unified Messaging service or the Client Access server running the Microsoft Exchange Unified Messaging Call Router service. Unified Messaging can use any of the following four audio codecs to create and store voice messages:

  - MP3 (default)

  - Windows Media Audio (WMA)

  - Group System Mobile (GSM) 06.10

  - G.711 Pulse Code Modulation (PCM) Linear

> [!WARNING]
> The G.711 (PCMA and PCMU) and the G.723.1 codecs are VoIP codecs used between a VoIP gateway and the Client Access and Mailbox servers.

Part of planning your UM system involves selecting the correct audio codec based on the needs and requirements of your organization. This topic discusses the audio codecs that UM can use, and you can use it to help plan your UM deployment.

## Codecs

The term *codec* is a combination of the words "coding" and "decoding" and is used with digital audio data. A codec is a software program that transforms digital data into an audio file format or audio streaming format. Codecs are used to convert an analog voice signal to a digital version of the voice signal. Codecs can vary in their sound quality, the bandwidth required to use them, and the system requirements needed to do the encoding.

Two types of codecs are used in Unified Messaging:

  - The codec used between a VoIP gateway, IP PBX, or SIP-enabled PBX and the Client Access and Mailbox servers or between a PBX and a VoIP gateway.

  - The codec used to encode and store voice messages for users.

When you use an ordinary telephone over the Public Switched Telephone Network (PSTN), your voice is transported in an analog format over the telephone line. But with Voice over IP (VoIP), your voice must be converted into digital signals. This conversion process is known as encoding. Encoding is performed by a codec. After the digitized voice has reached its destination, it must then be decoded back to its original analog format so the person on the other end of the call can hear and understand the caller.

## VoIP codec

In UM, three types of codecs can be used between VoIP gateways or IP PBXs and the Client Access and Mailbox servers:

  - G.711 µ-law

  - G.711 A-law

  - G.723.1

G.711 is a standard that was developed for use with audio codecs. There are two main algorithms defined in the standard for G.711: the µ-law algorithm that is used in North America and Japan and the A-law algorithm that is used in Europe and other countries. The G.723.1 audio codec is mostly used in VoIP applications and requires a license to be used. G.723.1 is a high-quality, high-compression type of codec.

A Client Access or Mailbox server and a supported VoIP gateway or IP PBX can offer both the G.711 and G.723.1 codecs. By default, the first codec to be used is G.723.1. If you want to use a codec other than G.723.1 between Client Access and Mailbox servers and the VoIP gateway or IP PBX, we recommend that you change the configuration on the VoIP gateway or IP PBX. The following table summarizes some common VoIP codecs.

### VoIP codecs

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>VoIP codec</th>
<th>Bandwidth (Kbps)</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>G.711</p></td>
<td><p>64</p></td>
<td><p>This codec requires very low processing. It needs a minimum of 128 kilobits per second (Kbps) for two-way communication.</p></td>
</tr>
<tr class="even">
<td><p>G.723.1</p></td>
<td><p>5.3/6.3</p></td>
<td><p>This codec offers high compression with high-quality audio. It requires more processing than the G.711 codec. The G.723.1 codec uses reduced bandwidth but offers poorer-quality audio.</p></td>
</tr>
</tbody>
</table>

## UM voice message storage codec

Unified Messaging dial plans are integral to the operation of UM. By default, when you create a UM dial plan, the UM dial plan uses the MP3 audio codec to create and store voice messages. However, after you create the UM dial plan, you can configure it to use WMA, GSM 06.10, or G.711 PCM Linear audio codecs.

Each audio codec has advantages and disadvantages. The MP3 audio codec was selected as the default audio codec because of its sound quality and compression properties. GSM 06.10 and G.711 PCM Linear audio codecs were included as available options because of their ability to support other types of messaging systems.

When you plan for UM, you must balance the size and the relative quality of the audio file that will be created for voice messages. Generally, the higher the bit rate for an audio file, the higher the quality. You must also consider whether the audio file is compressed. The following table shows the sample bit rate (bit/sec) and compression properties for each audio codec used in UM.

### Default UM voice message storage codecs

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Voice message storage codec</th>
<th>Bits</th>
<th>Compressed file?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>MP3</p></td>
<td><p>16 bit</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>WMA</p></td>
<td><p>16 bit</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>G.711 PCM</p></td>
<td><p>16 bit</p></td>
<td><p>No</p></td>
</tr>
<tr class="even">
<td><p>GSM 06.10</p></td>
<td><p>8-bit</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

In UM, the file type created for a voice message depends on the audio codec that's used to create the voice message audio file. The MP3 audio codec creates .mp3 audio files, the WMA audio codec creates .wma audio files, and the GSM 06.10 and G.711 PCM Linear audio codecs create .wav audio files. All these audio files are sent together with an email message to the recipient of the voice message.

Frequently, but not always, coding and decoding the digital data also involves compression or decompression. Audio compression is a form of data compression that reduces the size of audio data files. The audio compression algorithm used by the audio codec compresses the .wma or .wav audio files. In UM, the type of audio compression algorithm that is used is based on the type of audio codec selected in the UM dial plan properties. After the audio file is created and compressed, it's attached to the voice message.

Sometimes information from the digital data is lost during compression and decompression. The higher the compression that is used to compress the audio file, the greater the loss of information during the conversion. However, less disk space is used because the size of the audio file is reduced. Conversely, the lower the compression, the lower the loss of the information. However, more disk space must be used because of the increased size of each audio file.

RTAudio wideband or high fidelity audio for recording voice messages is also available as an audio codec. However, high fidelity audio using RTAudio is available only after you have successfully integrated Unified Messaging with [Microsoft Lync Server](https://docs.microsoft.com/lyncserver/microsoft-lync-server-2013). To enable RTAudio as the wire codec, either narrow or wideband, the UM dial plan must be configured as a Session Initiation Protocol (SIP) URI-type dial plan and you must set the call answering codec on the dial plan to MP3 or WMA to enable wideband audio (16Khz).

> [!IMPORTANT]
> RTAudio is not available in environments where Lync Server is not deployed. This is because, in environments that haven't integrated Lync Server, the dial plan will be set to Telephone Extension or E.164 and not to SIP URI.

There are two media streams for each incoming call: inbound to a Client Access server and outbound from a Mailbox server. When the dial plan type is set to SIP URI and the call-answering codec on the dial plan is set to MP3 or WMA, a Client Access server tries to select the RTAudio VoIP codec for the inbound media stream. If negotiation is successful, the RTAudio codec for the inbound stream will be used for call-answering calls or calls that originate from a Lync client or server.

> [!NOTE]
> Calls placed by using the Play on Phone feature will not use the RTAudio codec. The inbound stream for calls placed by using Play on Phone will use the G.711 or G.723.1 codec.

When the RTAudio codec is used, the voice message that is recorded will be recorded in high fidelity and will be stored as an audio file that has an .mp3 or .wma extension depending on how the dial plan is configured. When the voice message is played back to the user in Outlook or Outlook Web App they will hear the voice message in high fidelity audio. If negotiation is unsuccessful, either the G.711 or G.723.1 codec will be used. Both the G.711 and the G.723.1 codecs are narrowband codecs. When they're used as the VoIP codec, the voice message is recorded and stored as a narrowband audio file that has an .mp3 or .wma extension.

The outbound media stream will always be negotiated by using either the G.711 or G.723.1 codec. This means that callers will always hear narrowband audio over the telephone. This also applies to situations when a call is placed by using Microsoft Lync Server 2010 or later.

The audio format and codec that Mailbox servers use to store the audio in voice messages depends not only on the audio codec that's configured on the dial plan but also on the bit rate of the audio that UM negotiates with a SIP peer. If your environment includes Lync Server or SIP endpoints, a Mailbox server will also negotiate the audio codec to use with a SIP peer. For example, when wideband RTAudio is negotiated as the wire codec, a Mailbox server will then use either the 32 Kbps MP3 or WMA 9.2 format when creating voice messages, depending on the dial plan setting. The following table shows the relationship between the voice message storage audio codec and the VoIP or wire audio codec that's used.

### Relationship between the storage audio codec and the VoIP or wire audio codec

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Audio codec configured on a UM dial plan</th>
<th>VoIP or wire codec (narrowband) - G.723, G.711, or RTAudio (8KHz)</th>
<th>VoIP or wire codec (wideband) - RTAudio (16KHz)</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>G.711</p></td>
<td><p>G.711</p></td>
<td><p>Not applicable. A Client Access or Mailbox server doesn't negotiate wideband audio if the dial plan is set to G.711.</p></td>
</tr>
<tr class="even">
<td><p>WMA</p></td>
<td><p>WMA 9 Voice</p></td>
<td><p>WMA 9.2</p></td>
</tr>
<tr class="odd">
<td><p>GSM</p></td>
<td><p>GSM 6.10</p></td>
<td><p>Not applicable. A Client Access or Mailbox doesn't negotiate wideband audio if the dial plan is set to GSM.</p></td>
</tr>
<tr class="even">
<td><p>MP3</p></td>
<td><p>MP3 (16 Kbps)</p></td>
<td><p>MP3 (32 Kbps)</p></td>
</tr>
</tbody>
</table>

## UM message sizing

You can configure Unified Messaging to use one of the following four audio codecs for creating voice messages: MP3, WMA, GSM 06.10, and G.711 PCM Linear. By default, the MP3 format is selected.

The WMA audio codec is always stored in the Windows Media format, and the attachment is a file that has a .wma file name extension. Audio files encoded using the GSM or G.711 PCM Linear audio codecs are always stored in RIFF/WAV format, and the attachment is a file that has a .wav file name extension.

The size of UM voice messages depends on the size of the attachment that holds the voice data. In turn, the size of the attachment depends on the following factors:

  - The duration of the voice mail recording

  - The audio codec that's used

  - The audio file storage format

The following figure shows how the size of the audio file depends on the duration of the voice mail recording for the three audio codecs that you can use in UM.

> [!NOTE]
> In this figure, the average length of a call-answered voice message is approximately 30&nbsp;seconds.

**Audio file size**

![UM\_Message\_Sizing](images/Aa998670.76ca4891-450f-4ffd-9493-aac8d0d23a5d(EXCHG.150).gif "UM_Message_Sizing")

## MP3

By default, the MP3 format is selected and is the default audio file format for voice mail messages. The MP3 format is a common audio file format that's used to greatly reduce the size of the audio file and is most commonly used by personal audio devices or MP3 players. MP3 is a cross-platform type of audio codec and is used for compatibility with many mobile phones and devices and different computer operating systems.

## WMA

WMA is the most highly compressed audio codec of the three kinds of codecs. The compression is approximately 11,000 bytes for each 10 seconds of audio. However, the .wma file format has a much larger header section than the .wav file format. The .wma file header section is approximately 7 kilobytes (KB), whereas the header section for the .wav file is less than 100 bytes. Although WMA audio recordings are recorded for longer than 15 seconds, they become smaller than GSM audio recordings. Therefore, for the smallest but highest-quality audio files, use the WMA audio codec.

> [!NOTE]
> If you using push notifications from your on-premises deployment for OWA for Devices, you can't use the WMA format. OWA for Devices only supports the MP3 file format.

## G.711 PCM Linear

The G.711 PCM Linear audio codec creates .wav audio files that are not compressed. Therefore, G.711 PCM Linear .wav audio files occupy the most space for any given duration when they're compared to the GSM and WMA audio codecs. G.711 PCM Linear .wav audio files occupy just over 160,000 bytes for each 10 seconds of audio. G.711 PCM Linear .wav audio files have the highest audio quality of the three audio codecs used by UM. However, the quality of comparable audio files created using the WMA and GSM audio codecs is acceptable to most users who listen to voice messages.

## GSM

The GSM audio codec creates .wav audio files that are compressed. GSM .wav audio files are just over 16,000 bytes for each 10 seconds of audio. However, GSM creates an audio file larger than the audio file created by the WMA audio codec. Therefore, when you are balancing the quality of the voice message and the size, this may not be the best choice.
