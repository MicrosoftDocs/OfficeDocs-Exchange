---
localization_priority: Normal
description: 'Summary: Learn about connectivity logging and how it records outbound connection activity for transmitting messages in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: c31fd710-4ae4-4d9a-8936-d056e7ca2748
ms.date: 7/6/2018
ms.reviewer: 
title: Connectivity logging in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Connectivity logging in Exchange Server

Connectivity logging records the outbound connection activity that's used to transmit messages on Exchange servers. In Exchange Server, the following services transmit messages, so they have connectivity logs:

- The Transport service on Mailbox servers and Edge Transport servers.

- The Front End Transport service on Mailbox servers.

- The Mailbox Transport Submission service on Mailbox servers.

- The Mailbox Transport Delivery service on Mailbox servers.

For more information about these transport services, and where they can transmit messages, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

 Connectivity logging doesn't track the transmission of individual messages. Instead, it tracks the number and size of messages that were transmitted over a connection, DNS resolution information for the destination, and informational messages that are related to the connection.

By default, connectivity logging is enabled, and Exchange uses circular logging to limit the connectivity log files based on size and age to help control the hard disk space that's used. To configure connectivity logging, see [Configure connectivity logging in Exchange Server](configure-connectivity-logging.md).

 **Note**: If you're interested in a detailed record of the entire SMTP protocol conversation from start to finish, see [Protocol logging](../../mail-flow/connectors/protocol-logging.md).

## Structure of the connectivity log files
<a name="Structure"> </a>

By default, the connectivity log files exist in these locations:

- **Mailbox servers**:

  - **Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\Connectivity`

  - **Front End Transport service**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\Connectivity`

  - **Mailbox Transport Delivery service**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\Connectivity\Delivery`

  - **Mailbox Transport Submission service**: `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\Connectivity\Submission`

- **Edge Transport servers**

The naming convention for the connectivity log files is `CONNECTLOGyyymmdd-nnnn.log`. The placeholders represent the following information:

- _yyyymmdd_ is the Coordinated Universal Time (UTC) when the log file was created. _yyyy_ = year, _mm_ = month, and _dd_ = day.

- _nnnn_ is an instance number that starts at the value of 1 for each day.

Information is written to the log file until the file reaches its maximum size. Then, a new log file that has an incremented instance number is opened (the first log file is -1, the next is -2, and so on). Circular logging deletes the oldest log files when either of the following conditions are true:

- A log file reaches its maximum age.

- The connectivity log folder reaches its maximum size.

The connectivity log files are text files that contain data in the comma-separated value file (CSV) format. Each connectivity log file has a header that contains the following information:

- **#Software**: The value is `Microsoft Exchange Server`.

- **#Version**: The value is `15.0.0.0`.

- **#Log-Type**: The value is `Transport Connectivity Log`.

- **#Date**: The UTC date-time when the log file was created. The UTC date-time is represented in the ISO 8601 date-time format: *yyyy-mm-dd*T*hh:mm:ss.fff*Z, where _yyyy_ = year, _mm_ = month, _dd_ = day, T indicates the beginning of the time component, _hh_ = hour, _mm_ = minute, _ss_ = second, _fff_ = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.

- **#Fields**: Comma delimited field names that are used in the connectivity log files. These values are described in the next section.

## Fields in the connectivity log files
<a name="Info"> </a>

Connectivity logging stores each outbound connection event on a single line in the log. The information on each line is organized by fields, and these fields are separated by commas. The following table describes the fields that are used to classify each outgoing connection event.

|**Field name**|**Description**|
|:-----|:-----|
|**date-time**|UTC date-time of the connection event. The UTC date-time is represented in the ISO 8601 date-time format: *yyyy-mm-dd*T*hh:mm:ss.fff*Z, where _yyyy_ = year, _mm_ = month, _dd_ = day, T indicates the beginning of the time component, _hh_ = hour, _mm_ = minute, _ss_ = second, _fff_ = fractions of a second, and Z signifies Zulu, which is another way to denote UTC.|
|**session**|A GUID value. The value is the same for every event that's associated with the session, but different for each session.|
|**source**|One of these values:  <br/> **SMTP** for SMTP connections.  <br/> **MapiDelivery** for connections from the local mailbox database by the Mailbox Transport Delivery service.  <br/> **MapiSubmission** for connections from the local mailbox database by the Mailbox Transport Submission service.|
|**destination**|These are some examples of values you'll see here:  <br/> **In the Transport service**:  <br/> • The FQDN of the destination messaging server  <br/> • `shadowredundancy` (on Mailbox servers only)  <br/> **In the Front End Transport service**:  <br/> • `internalproxy` <br/> • `client proxy` <br/> **In the Mailbox Transport Delivery service**: The GUID of the destination mailbox database.  <br/> **In the Mailbox Transport Submission service**:  <br/> • The GUID of the destination mailbox database.  <br/> • `mailboxtransportsubmissioninternalproxy`|
|**direction**|Single character that represents the start, middle, or end of the connection. The values you'll see here are:  <br/> `+`: Connect  <br/> `-`: Disconnect  <br/> `>`: Send|
|**description**|Text information that's associated with the connection event. For example:  <br/> Number and size of messages that were transmitted.  <br/> DNS MX resource record resolution information for destination domains.  <br/> DNS resolution information for destination Mailbox servers.  <br/> Connection establishment messages.  <br/> Connection failure messages.|

The transport services connect to and transmit messages to multiple destinations simultaneously. Entries in the log file from different connection events are interlaced (they typically aren't grouped together as one uninterrupted series of connection events). However you can use the fields (in particular, the unique **session** field value for a connection) to organize and arrange the log entries for each separate connection from start to finish.
