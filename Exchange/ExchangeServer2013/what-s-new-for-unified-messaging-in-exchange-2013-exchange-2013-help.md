---
title: "What's new for Unified Messaging in Exchange 2013: Exchange 2013 Help"
TOCTitle: What's new for Unified Messaging in Exchange 2013
ms:assetid: a444ef2d-d893-408e-adf9-c9d8a8b07593
ms:mtpsurl: https://technet.microsoft.com/library/JJ150545(v=EXCHG.150)
ms:contentKeyID: 47560068
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# What's new for Unified Messaging in Exchange 2013

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, we're enhancing earlier releases of Exchange by introducing new features and architectural changes. Unified Messaging (UM) in Exchange 2013 includes the same feature set as Exchange 2010 and Exchange 2007, however Unified Messaging is no longer a separate server role. It's now a component of the voice-related features offered in Exchange 2013.

## Changes in the Voice architecture

The architecture of Exchange 2013 is different than it was in Exchange 2010 and Exchange 2007. In previous versions of Exchange UM, all the components for Unified Messaging were included on a server that had the UM server role installed. In Exchange 2013, all the Unified Messaging components are split between a Client Access server running the Microsoft Exchange Unified Messaging Call Router service and a Mailbox server running the Microsoft Exchange Unified Messaging service. All the functionality, including the services and worker processes for Unified Messaging, is located on each Mailbox server, with the exception of the Client Access server running the Microsoft Exchange Unified Messaging Call Router service, which proxies incoming calls to the Mailbox server. For details, see [Voice architecture changes](voice-architecture-changes-exchange-2013-help.md).

## Support for IPv6

Internet Protocol version 6 (IPv6) is the most recent version of the Internet Protocol. IPv6 is intended to correct many of the shortcomings of IPv4, which was the previous version of the IP. As in Exchange 2010, Exchange 2013 Client Access and Mailbox servers fully support IPv6 networks. For details, see [IPv6 support in Unified Messaging](ipv6-support-in-unified-messaging-exchange-2013-help.md).

## Support for UCMA 4.0 API

Since Service Pack 1 for Exchange 2010, the Unified Messaging role has relied on Unified Communications Managed API v2.0 (UCMA) for signaling and media. Therefore, UCMA 2.0 is a prerequisite for Exchange 2010 UM setup. UCMA 2.0 is downloaded separately and deployed manually by administrators on Unified Messaging servers running Exchange 2010 SP1 or a later version. For Exchange 2013, UCMA 4.0 is required. However, given that the UM server is no longer a separate server role in Exchange 2013, now it's the Client Access and Mailbox servers that require UCMA 4.0.

UCMA 4.0 supports new features in Unified Messaging, such as using the same version of the Speech Engine for both Text-to-Speech (TTS) and Automatic Speech Recognition (ASR). The platform that's used for Exchange 2013, .NET 4.0, includes a single installer file and enables backward compatibility with Exchange 2010 and Exchange 2007 UM servers.

In Exchange 2010 SP2 and SP1, UCMA 2.0 installation is required prior to installing the service pack on a Unified Messaging server. However, UCMA 2.0 had several limitations. UCMA 4.0 corrects many of the shortcomings. In Exchange Server 2013, UM continues to use UCMA. However, the move to the newest version of UCMA provides these benefits:

  - The newest build of UCMA incorporates hotfixes and patches.

  - UCMA requires .NET 4.0, which is the platform used by Exchange Server 2013. (UCMA 2.0 doesn't support .NET 4.0.)

  - UCMA 4.0 supports IPv6.

  - Simplified and automated deployment of UCMA 4.0. Exchange 2013 Setup performs a single check for UCMA 4.0.

  - UCMA 4.0 setup includes all prerequisites for Exchange 2013.

> [!NOTE]
> UCMA 4.0 is installed when you're installing Exchange 2013. For details about UCMA 4.0 and setup requirements, see <A href="exchange-2013-prerequisites-exchange-2013-help.md">Exchange 2013 prerequisites</A>. To upgrade to the most recent version of UCMA, you must first uninstall any previous versions of UCMA that are installed using Add/Remove programs.

## Improvements to Voice Mail Preview

Some enhancements to the speech-related services are offered for Exchange Server 2013 UM via the Speech Engine 11.0 and UCMA 4.0. Grammar generation and language improvements are included. In addition, Exchange Server 2013 UM includes several enhancements to the UI and improvements for confidence and accuracy for Voice Mail Preview. For details, see [Voice mail preview enhancements](voice-mail-preview-enhancements-exchange-2013-help.md).

## Enhanced caller ID support

In previous releases of Exchange Unified Messaging, a UM server that took a call used caller ID to try to look up the identity of the calling party. This search extended across Active Directory and the UM user's personal contacts stored in their mailbox.

Exchange users are often annoyed by failures to identify Exchange or personal contacts from their caller ID. Until now, only the default contact folder in Exchange UM was used for this search. But Exchange Server 2013 users are likely to have contacts aggregated from external social networks or contacts in unique folders that the users have created manually. Exchange 2013 supports contact aggregation from external social networks, provides intelligence to link multiple contacts referring to the same person, and uses that data to present person-centric (rather than contact-centric) views. The contacts that are aggregated from external networks are placed in contact folders along with any additional contact folders that users created.The features in Exchange 2013 UM extend the scope of the search to include the user's other Exchange and personal contact folders that were created manually.

Caller ID look-up is integrated with contact aggregation, so that it searches across external contacts. The PersonID property, where present and with a non-null value, improves the user experience for caller ID resolution by suppressing duplicate matches to contacts that are associated with the same person. Because the PersonID property is the same on both results, UM treats this as a match to a single contact.

## Enhancements to speech platform and speech recognition

Exchange Server 2013 UM introduces some enhancements to the speech platform and speech recognition, including the following:

- Enhancements and improved accuracy for Voice Mail Preview.

- Support for the [Microsoft Speech Platform - Runtime (Version 11.0)](https://www.microsoft.com/download/details.aspx?id=27225).

- Speech grammar generation using the system mailbox for an organization.

Exchange Unified Messaging uses static and dynamic speech grammars to recognize commands, names of contacts in the global address list, and names of personal contacts in the user's mailbox. Today, in Exchange Server 2013, every Mailbox server running the Microsoft Exchange Unified Messaging service generates grammars for all UM languages installed on it and stores them in directories. Every Mailbox server stores every possible grammar, which it generates based on the number of dial plans, auto attendants, and the UM languages that are installed.

Grammar files are used as inputs to the speech recognition process and are generated on a periodic basis. The GGG.exe command in Exchange 2007 and Exchange 2010 allowed you to manually update the grammar files without waiting for the scheduled update. In Exchange Server 2013, to address ASR grammar-generation scalability issues for UM, the speech GAL grammar generation no longer happens on the server with the Unified Messaging server role installed. Instead, it happens periodically using the Mailbox Assistant, on the Mailbox server running the Microsoft Exchange Unified Messaging service that hosts the organization's arbitration mailbox. The GAL speech grammar file is stored in the arbitration mailbox for an organization and then later downloaded to all Mailbox servers in that Exchange organization. By default, the Mailbox Assistant runs every 24 hours. You can adjust the frequency by using the **Set-MailboxServer** cmdlet.

## Cmdlet updates

For Exchange 2013, many UM cmdlets have been brought over from Exchange 2010, but there have been changes in some of those cmdlets, and new cmdlets have been added for new functionality. For details, see [Unified Messaging cmdlet updates](unified-messaging-cmdlet-updates-exchange-2013-help.md).
