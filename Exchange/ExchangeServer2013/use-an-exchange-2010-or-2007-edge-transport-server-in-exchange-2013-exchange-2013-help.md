---
title: 'Use an Exchange 2010 or 2007 Edge Transport server in Exchange 2013'
TOCTitle: Use an Exchange 2010 or 2007 Edge Transport server in Exchange 2013
ms:assetid: ce99b4bd-868c-4767-9009-e22c17ac0ac7
ms:mtpsurl: https://technet.microsoft.com/library/JJ150569(v=EXCHG.150)
ms:contentKeyID: 47560104
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Use an Exchange 2010 or 2007 Edge Transport server in Exchange 2013

_**Applies to:** Exchange Server 2013_

The Edge Transport server is available in Microsoft Exchange Server 2013 Service Pack 1 (SP1). However, you can continue to use existing Exchange Server 2007 or Exchange Server 2010 Edge Transport servers that you have deployed in your perimeter network. Or, you can install a new Exchange 2007 or Exchange 2010 Edge Transport server in your perimeter network for a new or upgraded Exchange 2013 organization.

Here are the things you need to know:

- An Exchange 2007 or Exchange 2010 Edge Transport server expects a connection to a Hub Transport server. In Exchange 2013, the Transport service exists on the Mailbox server. Therefore, Internet mail flow occurs between the Transport service on the Mailbox server and the Edge Transport server, which effectively bypasses the Exchange 2013 Client Access server.

- You can subscribe an Exchange 2007 or Exchange 2010 Edge Transport server to an Active Directory site that contains only Exchange 2013 servers. You can import the Edge Subscription file and run EdgeSync on a standalone Exchange 2013 Mailbox server, or on a server where the Mailbox server and the Client Access server are installed on the same computer. You can't import the Edge Subscription file or run EdgeSync on a standalone Exchange 2013 Client Access server.

- The procedures to deploy a new Exchange 2007 or Exchange 2010 Edge Transport server in your Exchange 2013 organization are basically the same as in previous versions of Exchange. However, any procedures that are performed on the Hub Transport server are performed on the Mailbox server in Exchange 2013. The procedures are:

  - [Configure Internet Mail Flow Through a Subscribed Edge Transport Server](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb738158(v=exchg.141))

  - [Configure Mail Flow Between an Edge Transport Server and Hub Transport Servers Without Using EdgeSync](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/bb232082(v=exchg.141))
