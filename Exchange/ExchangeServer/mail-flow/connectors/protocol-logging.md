---
localization_priority: Normal
description: 'Summary: Learn how protocol logging records SMTP activity in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 40da446b-bcc3-4a97-ace7-a54f6ddebd79
ms.date: 7/6/2018
ms.reviewer: 
title: Protocol logging
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Protocol logging

Protocol logging records the SMTP conversations that occur between messaging servers and between Exchange services in the transport pipeline as part of message delivery. You can use protocol logging to diagnose mail flow problems. The SMTP conversations that can be recorded by protocol logging occur in the following locations:

- Send connectors and Receive connectors in the Transport service on Mailbox servers.

- Send connectors and Receive connectors in the Transport service on Edge Transport servers.

- Receive connectors in the Front End Transport service on Mailbox servers.

- The implicit and invisible intra-organization Send connector in the Transport service on Mailbox servers.

- The implicit and invisible intra-organization Send connector in the Front End Transport service on Mailbox servers.

- The implicit and invisible intra-organization Send connector in the Mailbox Transport Submission service on Mailbox servers.

- The implicit and invisible Mailbox Delivery Receive connector in the Mailbox Transport Delivery service on Mailbox servers.

By default, protocol logging is enabled on the following connectors:

- The default Receive connector named Default Frontend _\<ServerName\>_ in the Front End Transport service on Mailbox servers.

- The implicit and invisible Send connector in the Front End Transport service on Mailbox servers.

By default, protocol logging is disabled on all other connectors. You need to enable or disable protocol logging on each individual connector. You configure other protocol logging options for all Receive connectors or all Send connectors that exist in each individual transport service on the Exchange server. All Receive connectors in a transport service share the same protocol log files and protocol log options. These files and options are separate from the Send connector protocol log files and protocol log options in the same transport service on the Exchange server.

By default, Exchange uses circular logging to limit the protocol log based on file size and file age to help control the hard disk space that's used by the log files. To configure protocol logging, see [Configure protocol logging](configure-protocol-logging.md).

## Structure of the protocol log files
<a name="Structure"> </a>

By default, the protocol log files exist in the following locations:

- Front End Transport service on Mailbox servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpSend`

- Transport service on Mailbox servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpSend`

- Mailbox Transport Delivery service on Mailbox servers (**Receive connectors**): `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpReceive\Delivery`

- Mailbox Transport Submission service on Mailbox servers (**Send connectors**): `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Submission`

    **Note**: Protocol logging for side effect messages that are submitted after messages are delivered to mailboxes occurs in `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Delivery`. For example, a message that's delivered to a mailbox triggers an Inbox rule that redirects the message to another recipient.

- Transport service on Edge Transport servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpSend`

The naming convention for log files is `SENDyyyymmdd-nnnn.log` for Send connectors and `RECVyyyymmdd-nnnn.log` for Receive connectors. The placeholders represent the following information:

- _yyyymmdd_ is the coordinated universal time (UTC) date when the log file was created. _yyyy_ = year, _mm_ = month, and _dd_ = day.

- _nnnn_ is an instance number that starts at the value 1 every day.

Information is written to the log file until the file reaches its maximum size. Then, a new log file that has an incremented instance number is opened (the first log file is -1, the next is -2, and so on). Circular logging deletes the oldest log files when either of the following conditions is true:

- A log file reaches its maximum age.

- The protocol log folder reaches its maximum size.

The protocol log files are text files that contain data in the comma-separated value file (CSV) format. Each protocol log file has a header that contains the following information:

- **#Software**: The value is `Microsoft Exchange Server`.

- **#Version**: Version number of the Exchange server that created the message tracking log file. The value uses the format `15.01.nnnn.nnn`.

- **#Log-Type**: The value is either `SMTP Receive Protocol Log` or `SMTP Send Protocol Log`.

- **#Date**: UTC date-time when the log file was created. The UTC date-time is represented in the ISO 8601 date-time format: *yyyy-mm-dd*T*hh:mm:ss.fff*Z, where _yyyy_ = year, _mm_ = month, _dd_ = day, T indicates the beginning of the time component, _hh_ = hour, _mm_ = minute, _ss_ = second, _fff_ = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.

- **#Fields**: Comma-delimited field names that are used in the protocol log files.

## Fields in the protocol log
<a name="Info"> </a>

The protocol log stores each SMTP protocol event on a single line in the log. The information stored on each line is organized by fields, and these fields are separated by commas. The fields that are used in the protocol log are described in the following table.

|**Field name**|**Description**|
|:-----|:-----|
|**date-time**|UTC date-time of the protocol event. The UTC date-time is represented in the ISO 8601 date-time format: *yyyy-mm-dd*T*hh:mm:ss.fff*Z, where _yyyy_ = year, _mm_ = month, _dd_ = day, T indicates the beginning of the time component, _hh_ = hour, _mm_ = minute, _ss_ = second, _fff_ = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.|
|**connector-id**|Distinguished name (DN) of the connector that's associated with the SMTP event.|
|**session-id**|GUID value that's unique for each SMTP session, but is the same for every event that's associated with that SMTP session.|
|**sequence-number**|Counter that starts at 0 and is incremented for each event in the same SMTP session.|
|**local-endpoint**|Local endpoint of an SMTP session. This consists of an IP address and TCP port number formatted as _\<IP address\>_: _\<port\>_.|
|**remote-endpoint**|Remote endpoint of an SMTP session. This consists of an IP address and TCP port number formatted as _\<IP address\>_: _\<port\>_.|
|**event**|Single character that represents the protocol event. Valid values are:  <br/> `+`: Connect  <br/> `-`: Disconnect  <br/> `>`: Send  <br/> `<`: Receive  <br/> `*`: Information|
|**data**|Text information associated with the SMTP event.|
|**context**|Additional contextual information that may be associated with the SMTP event.|

One SMTP conversation that represents sending or receiving a single email message generates multiple SMTP events. Each event is recorded on a separate line in the protocol log. An Exchange server has many SMTP conversations going on at any given time. This creates protocol log entries from different SMTP conversations that are mixed together. You can use the **session-id** and **sequence-number** fields to sort the protocol log entries by each individual SMTP conversation.
