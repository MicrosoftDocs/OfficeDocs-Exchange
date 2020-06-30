---
title: 'This server is the source for a send connector'
TOCTitle: This Server is the Source for a Send Connector_ServerIsSourceForSendConnector
ms:assetid: 151c0014-c90c-4c52-8e74-4b3f1bc7aaf1
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.serverissourceforsendconnector(v=EXCHG.150)
ms:contentKeyID: 46628815
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# This Server is the Source for a Send Connector\_ServerIsSourceForSendConnector

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 setup cannot continue because its attempt to remove the Hub Transport server role failed. The local computer is the source for one or more Send connectors in the Exchange organization.

A Send connector represents a logical gateway through which outbound messages are sent.

Exchange 2007 setup requires that all Send connectors for an Exchange organization be moved or deleted from the Hub Transport server computer before the server role can be deleted.

To resolve this issue, move or delete all Send connectors from the local computer and then rerun setup.

For more information about modifying or removing Send connectors, see the following topics in the Exchange Server 2007 product documentation:

  - "How to Remove a Send Connector" ([https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa998836(v=exchg.80)](https://docs.microsoft.com/previous-versions/office/exchange-server-2007/aa998836(v=exchg.80))).

  - "How to Modify the Configuration of a Send Connector" ([https://go.microsoft.com/fwlink/?LinkId=86656](https://go.microsoft.com/fwlink/?linkid=86656)).
