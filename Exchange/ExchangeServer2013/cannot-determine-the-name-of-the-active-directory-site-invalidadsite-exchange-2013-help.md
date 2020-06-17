---
title: "Can't determine the name of the Active Directory site"
TOCTitle: Cannot determine the name of the Active Directory site_InvalidADSite
ms:assetid: ef96e077-08a0-4108-9f7d-0d61758abcd4
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.invalidadsite(v=EXCHG.150)
ms:contentKeyID: 46629185
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Cannot determine the name of the Active Directory site\_InvalidADSite

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft® Exchange Server 2007 setup cannot continue because this server does not appear to belong to a valid Active Directory® directory service site.

Microsoft Exchange setup requires that the server that is used for installation of Exchange 2007 belong to a valid Active Directory site.

To resolve this issue, verify that the local server is a member of a valid Active Directory site and rerun Exchange Server 2007 setup.

You can use the **/DsGetSite** option of the Nltest.exe command line tool to verify and display site membership. For more information, see "Nltest.exe: NLTest Overview" in the "Tools and Settings Collection" of [Windows Server 2003 Technical Reference](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc758478(v=ws.10)).

For more information about Active Directory troubleshooting, see "Troubleshooting Active Directory Operations" in *Windows Server 2003: Operations* (<https://go.microsoft.com/fwlink/?linkid=68099>).
