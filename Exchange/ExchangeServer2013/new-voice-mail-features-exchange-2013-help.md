---
title: 'New voice mail features: Exchange 2013 Help'
TOCTitle: New voice mail features
ms:assetid: 89faaa97-3485-4704-a56c-d13632f01e2a
ms:mtpsurl: https://technet.microsoft.com/library/JJ649002(v=EXCHG.150)
ms:contentKeyID: 49317361
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# New voice mail features in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

Unified Messaging (UM) in Microsoft Exchange Server 2013 includes the same feature set as Exchange 2010 and Exchange 2007, with some enhancements and architectural changes. However, Unified Messaging is no longer a separate server role. It's now a component of the voice-related features offered in Exchange 2013.

## Changes in the Voice architecture

The Voice architecture in Exchange 2013 is different than it was in Exchange 2010 and Exchange 2007. In previous versions of Exchange UM, all the components for Unified Messaging were included on a server that had the UM server role installed. In Exchange 2013, the Unified Messaging components are split between a Client Access server running the Microsoft Exchange Unified Messaging Call Router service and a Mailbox server running the Microsoft Exchange Unified Messaging service. Most of the functionality, including the services and worker processes for Unified Messaging, is located on each Mailbox server. The Client Access server, running the Microsoft Exchange Unified Messaging Call Router service, proxies incoming calls to the Mailbox server. For details, see [Voice architecture changes](voice-architecture-changes-exchange-2013-help.md).

## Support for IPv6

Internet Protocol version 6 (IPv6) is the most recent version of the Internet Protocol (IP). IPv6 is intended to correct many of the shortcomings of IPv4, which was the previous version of the IP. Just as Exchange 2010 did, Exchange 2013 Client Access and Mailbox servers fully support IPv6 networks. For details, see [IPv6 support in Unified Messaging](ipv6-support-in-unified-messaging-exchange-2013-help.md).

## Support for UCMA 4.0 API

Since Exchange 2010 Service Pack 1 (SP1), the Unified Messaging role has relied on Unified Communications Managed API v2.0 (UCMA) for signaling and media. Therefore, UCMA 2.0 was a prerequisite for Exchange 2010 UM setup. UCMA 2.0 is downloaded separately and deployed manually by administrators on UM servers running Exchange 2010 SP1 or a later version.

However, UCMA 2.0 has several limitations. Many of these shortcomings are corrected by UCMA 4.0, which is required for Exchange 2013. Now that the UM server is no longer a separate server role, it's the Client Access and Mailbox servers that require UCMA 4.0.

UCMA 4.0 supports new features in Unified Messaging, such as using the same version of the Speech Engine for both Text-to-Speech (TTS) and Automatic Speech Recognition (ASR). The platform that's used for Exchange 2013, .NET 4.0, includes a single installer file and enables backward compatibility with Exchange 2010 and Exchange 2007 UM servers.

In Exchange 2010 SP2 and SP1, UCMA 2.0 installation is required prior to installing the service pack on a Unified Messaging server.

Using UCMA 4.0 offers multiple benefits:

  - It incorporates hotfixes and patches.

  - It supports IPv6.

  - Deployment of UCMA 4.0 has been automated and simplified.

  - UCMA 4.0 setup includes all prerequisites for Exchange 2013.

  - UCMA 4.0 provides more accurate speech engine translations and more scalable voice platform support across multiple products.

> [!NOTE]
> UCMA 4.0 is installed when you're installing Exchange 2013. For details about UCMA 4.0 and setup requirements, see <A href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</A>. To upgrade to the most recent version of UCMA, you must first uninstall any previous versions of UCMA that are installed using Add/Remove programs.

## Improvements to Voice Mail Preview

Some enhancements to speech-related services are included in Exchange Server 2013 UM via the Speech Engine 11.0 and UCMA 4.0. There have been improvements in grammar generation, core voice services, and support for multiple languages. Exchange Server 2013 UM also includes several enhancements for transcription services that are delivered to end users and increased confidence and accuracy for Voice Mail Preview. For details, see [Voice mail preview enhancements](voice-mail-preview-enhancements-exchange-2013-help.md).

## Enhanced caller ID support

In previous releases of Exchange Unified Messaging, a UM server that received a call used caller ID to look up the possible identity of the calling party. This search extended across Active Directory and the UM user's personal contacts stored in their mailbox.

In the past, users sometimes were frustrated by the voice mail system's failure to identify Exchange or personal contacts from their caller ID. Until now, only the default contact folder in a user's Exchange mailbox has been used for this search. But Exchange Server 2013 users are likely to have contacts aggregated from external social networks or contacts they added to unique folders when organizing their contacts. In Exchange 2013, UM extends the scope of the search to include the user's other Exchange and personal contact folders that were created manually. Exchange 2013 also supports contact aggregation from external social networks, provides intelligence to link multiple contacts that refer to the same person, and uses that data to present person-centric (rather than contact-centric) views. This means that contacts that are aggregated from external social networks can be placed in the contact folder stored in the user's mailbox in Microsoft Outlook Web App and Outlook. These contacts can now also be added to any additional contact folders that users create.

Caller ID look-up is integrated with contact aggregation, so that it searches across external contacts. The **PersonID** property, where present and set to a value other than Null, improves the user experience for caller ID resolution by suppressing duplicate matches to contacts that are associated with the same person. Because the PersonID property is the same on both results, UM treats this as a match to a single contact.

## Enhancements to speech platform and speech recognition

Exchange Server 2013 UM introduces some enhancements to the speech platform and speech recognition, including the following:

- Enhancements and improved accuracy for Voice Mail Preview.

- Support for the [Microsoft Speech Platform - Runtime (Version 11.0)](https://www.microsoft.com/download/details.aspx?id=27225).

- Speech grammar generation using the system mailbox for an organization.

Exchange Unified Messaging uses static and dynamic speech grammars to recognize commands, names of contacts in the global address list (GAL), and names of personal contacts in the user's mailbox. Today, in Exchange Server 2013, every Mailbox server running the Microsoft Exchange Unified Messaging service generates grammars for all UM languages installed on it and stores them in directories. Every Mailbox server stores every possible grammar, which it generates based on the number of dial plans, auto attendants, and the UM languages that are installed.

Grammar files are used by UM to allow callers to use speech to locate users in your organization. The files are updated each 24 hours by the Mailbox Assistant. The GGG.exe command in Exchange 2007 and Exchange 2010 made it possible to manually update the grammar files without waiting for the scheduled update. In Exchange Server 2013, to address ASR grammar generation scalability issues for UM, the speech GAL grammar generation no longer happens on the server with the Unified Messaging server role installed. Instead, it happens periodically using the Mailbox Assistant, on the Mailbox server running the Microsoft Exchange Unified Messaging service that hosts the organization's arbitration mailbox. The GAL speech grammar file is stored in the arbitration mailbox for an organization and later downloaded to all Mailbox servers in the Exchange organization. By default, the Mailbox Assistant runs every 24 hours. You can adjust the frequency by using the **Set-MailboxServer** cmdlet.

## Cmdlet updates

For Exchange 2013, many UM cmdlets have been brought over from Exchange 2010. However, there have been changes in some of those cmdlets, and new cmdlets have been added for new functionality. For details, see [Unified Messaging cmdlet updates](unified-messaging-cmdlet-updates-exchange-2013-help.md).
