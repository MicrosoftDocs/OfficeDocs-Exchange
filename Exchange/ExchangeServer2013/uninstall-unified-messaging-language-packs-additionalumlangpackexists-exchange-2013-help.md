---
title: 'Uninstall Unified Messaging Language Packs'
TOCTitle: Uninstall Unified Messaging Language Packs_AdditionalUMLangPackExists
ms:assetid: 3a7e2621-0553-44f5-8029-c72fea25af3c
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.additionalumlangpackexists(v=EXCHG.150)
ms:contentKeyID: 46628886
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Uninstall Unified Messaging Language Packs\_AdditionalUMLangPackExists

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because the Unified Messaging server role upgrade failed.

Exchange setup requires that all Unified Messaging server role language packs, except the US English language pack, be uninstalled before the Unified Messaging server role upgrade can continue.

Unified Messaging (UM) language packs that are included with Exchange 2007 contain pre-recorded prompts, Text-to-Speech (TTS) conversion support for a given language.

Exchange 2007 UM language packs enable callers and Outlook Voice Access users to interact with the Unified Messaging system in multiple languages.

The existing non-US English language packs need to be uninstalled so that new language packs can be installed.

To resolve this issue, uninstall all Unified Messaging server role language packs, except the US English language pack, and then rerun Exchange 2007 setup.

For more information about uninstalling Unified Messaging server role language packs, see [How to Remove a Unified Messaging Language Pack from a Unified Messaging Server](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb124004(v=exchg.80)) in the Exchange 2007 product documentation.
