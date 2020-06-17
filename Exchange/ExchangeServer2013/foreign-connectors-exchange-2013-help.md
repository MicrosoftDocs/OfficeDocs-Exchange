---
title: 'Foreign connectors: Exchange 2013 Help'
TOCTitle: Foreign connectors
ms:assetid: 21c6a7a9-f4d2-4359-9ac9-930701b63a4e
ms:mtpsurl: https://technet.microsoft.com/library/Aa996779(v=EXCHG.150)
ms:contentKeyID: 49300447
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Foreign connectors

_**Applies to:** Exchange Server 2013_

A Foreign connector delivers messages to a server or foreign system that does not use SMTP as its primary transport mechanism. An example of this can be a fax-gateway server. A Foreign connector uses a local or shared Drop directory to send outbound messages, by means of a file transfer, to the foreign system. Foreign connectors are created in the Transport service of a Microsoft Exchange Server 2013 Mailbox server.

Foreign gateway servers can send messages into the Exchange 2013 organization by using the Pickup or Replay directories that exist in the Transport service of a Mailbox server. Correctly formatted email message files that you copy to each directory are submitted for delivery to an Exchange mailbox.

> [!TIP]
> In most cases where you must deliver outbound messages to a non-SMTP system, we recommend Delivery Agent connectors, because they allow for queue management of messages, messages do not have to be written to the file system, and other benefits. The <A href="delivery-agents-and-delivery-agent-connectors-exchange-2013-help.md">Delivery agents and Delivery Agent connectors</A> topic provides more details.

## For more information

[Create a Foreign connector to deliver messages to a non-SMTP fax gateway](create-a-foreign-connector-to-deliver-messages-to-a-non-smtp-fax-gateway-exchange-2013-help.md)

[Delivery agents and Delivery Agent connectors](delivery-agents-and-delivery-agent-connectors-exchange-2013-help.md)

[New-ForeignConnector](https://docs.microsoft.com/powershell/module/exchange/New-ForeignConnector)

[Set-ForeignConnector](https://docs.microsoft.com/powershell/module/exchange/Set-ForeignConnector)
