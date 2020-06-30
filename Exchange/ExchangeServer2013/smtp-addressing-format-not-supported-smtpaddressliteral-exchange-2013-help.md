---
title: 'SMTP Addressing Format Not Supported_SMTPAddressLiteral: Exchange 2013 Help'
TOCTitle: SMTP Addressing Format Not Supported_SMTPAddressLiteral
ms:assetid: b8b55917-d81f-4c0a-ad65-7bb10ac58df8
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.smtpaddressliteral(v=EXCHG.150)
ms:contentKeyID: 46629094
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# SMTP Addressing Format Not Supported\_SMTPAddressLiteral

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 and Exchange Server 2010 setup cannot continue because the specified recipient policy uses an unsupported Simple Mail Transfer Protocol (SMTP) address format.

Exchange 2007 and Exchange 2010 setup requires that all SMTP addresses used for e-mail address policies not contain IP address literals, for example: *user@\[10.10.1.1\]*.

To resolve this issue, change the value of the SMTP address in the recipient policy so that it does not contain an IP address literal. Replace brackets (\[\]) and numbers (10.10.1.1) of the IP address literal with the Domain Name System (DNS) naming format, for example: *user@contoso.com*, and then rerun Exchange setup.

For more information about managing recipient policies in Exchange Server 2007, see "Managing E-Mail Address Policies" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa998940(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa998940(v=exchg.80))).

For more information about managing recipient policies in Exchange Server 2010, see "Managing E-Mail Address Policies" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa998940(v=exchg.141)](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/aa998940(v=exchg.141))).
