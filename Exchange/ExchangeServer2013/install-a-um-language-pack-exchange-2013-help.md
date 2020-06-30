---
title: 'Install a UM language pack: Exchange 2013 Help'
TOCTitle: Install a UM language pack
ms:assetid: ed14ffa5-c9b0-4367-b5da-564024b360ff
ms:mtpsurl: https://technet.microsoft.com/library/Dd876951(v=EXCHG.150)
ms:contentKeyID: 49315543
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Install a UM language pack in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

To make a language available in the list of available Unified Messaging languages on a UM dial plan or UM auto attendant, you must first install the appropriate UM language pack. You install the language pack on a Mailbox server running the Microsoft Exchange Unified Messaging service by using the language-specific self-extracting executable file or the **setup.exe /AddUmLanguagePack** command. Before you can install a UM language pack, you must first download it to a local folder on the Mailbox server. You can download UM language packs from [Exchange Server 2013 UM Language Packs](https://www.microsoft.com/download/details.aspx?id=35368). There's a separate executable file for each language.

After you install the appropriate UM language pack, you can view the list of installed UM language packs by viewing the drop-down list on the **Settings** page of a UM dial plan or the **Language for automated voice interface** drop-down list on the **General** page of a UM auto attendant. You can also configure the default language to be a language other than English (en-US) on UM dial plans and auto attendants.

> [!WARNING]
> The UM language packs for Microsoft Exchange Server 2007 or Exchange 2007 Service Pack&nbsp;1&nbsp;(SP1), SP2, or SP3 or Exchange 2010 Service Pack&nbsp;1&nbsp;SP1, SP2, or SP3 can't be used on an Exchange 2013 Mailbox server.

For additional tasks related to UM languages, see [UM languages, prompts, and greetings procedures](um-languages-prompts-and-greetings-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox server (UM service)" entry in [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md).

- Verify that the Mailbox server is installed on a different computer than the Client Access server or that the Client Access and Mailbox servers are on the same hardware.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the UM Language Pack Installation (.exe) file to install a UM language pack

1. From the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=35368), download the language-specific UM language pack (.exe) file into a local folder on the Mailbox server.

2. Double-click the UMLanguagePack.*\<CultureCode\>.exe* file. For example, for the German UM language pack, you would download the file named UMLanguagePack.de-DE.exe.

3. In the Exchange 2013 Setup wizard, on the **License Agreement** page, read the terms of the agreement, select **I accept the terms in the license agreement**, and then click **Next**.

4. On the **Unified Messaging Language Pack** page, verify that the correct language is listed in the **The following Unified Messaging Language Pack(s) will be installed** window, and then click **Install**.

5. Click **Finish** to complete the installation of the UM language pack.

## Use setup.exe to install a UM language pack

This example installs the Japanese (ja-JP) UM language pack that's been downloaded to the D:\\Exchange\\UMLanguagePacks folder on a Mailbox server.

```powershell
setup.exe /AddUmLanguagePack:ja-JP /s:d:\Exchange\UMLanguagePacks /IAcceptExchangeServerLicenseTerms
```

This example installs the Mexican Spanish (es-MX) and German (de-DE) UM language packs that have been downloaded to the D:\\Exchange\\UMLanguagePacks folder on a Mailbox server.

```powershell
setup.exe /AddUmLanguagePack:es-MX,de-DE /s:d:\Exchange\UMLanguagePacks /IAcceptExchangeServerLicenseTerms
```

> [!WARNING]
> If you don't use the /IAcceptExchangeServerLicenseTerms parameter, you'll see the following error: Welcome to Microsoft Exchange Server 2013 Unattended Setup. You need to accept the license terms to install Microsoft Exchange Server 2013. To read the license agreement, visit https://go.microsoft.com/fwlink/p/?LinkId=150127. To accept the license agreement, add the /IAcceptExchangeServerLicenseTerms parameter to the command you're running. For more information, run setup /?.

For more information about available UM languages and the culture codes, see [UM languages, prompts, and greetings](um-languages-prompts-and-greetings-exchange-2013-help.md).
