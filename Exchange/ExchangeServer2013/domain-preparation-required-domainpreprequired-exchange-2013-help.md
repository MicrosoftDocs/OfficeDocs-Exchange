---
title: 'Domain preparation required_DomainPrepRequired: Exchange 2013 Help'
TOCTitle: Domain preparation required_DomainPrepRequired
ms:assetid: f6feae6f-7404-4b1f-887f-ed63c26a6bcd
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.domainpreprequired(v=EXCHG.150)
ms:contentKeyID: 46629205
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Domain preparation required\_DomainPrepRequired

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because the attempt to install the server role failed.

Microsoft Exchange setup requires that the local domain be prepared for Exchange Server 2007 before certain server roles can be installed.

Preparation of the domain for Exchange Server 2007 consists of the following tasks:

  - Setting permissions on the Domain container for the Exchange Servers, Exchange Organization Administrators, Authenticated Users, and Exchange Mailbox Administrators.

  - Creating the Microsoft Exchange System Objects container if it does not exist, and setting permissions on this container for the Exchange Servers, Exchange Organization Administrators, and Authenticated Users.

  - Creating a new domain global group in the current domain called Exchange Install Domain Servers.

  - Adding the Exchange Install Domain Servers group to the Exchange Servers USG in the root domain.

To resolve this issue, run **setup /PrepareDomain** to prepare the local domain and retry the server role installation.

For more information about how to perform the PrepareDomain process, see "How to Prepare Active Directory and Domains" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125224(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/bb125224(v=exchg.80))).
