---
localization_priority: Normal
description: 'Summary: Learn about the filterable properties for messages in queues in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8e6187c1-76f0-49da-bc24-2ab57cfb3c2c
ms.reviewer:
title: Properties of messages in queues
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Properties of messages in queues

Filtering messages in queues by one or more message properties in Exchange Server allows you to quickly locate messages and take action on them. When an email message is sent to multiple recipients, the message might be located in multiple queues on the server. When you filter messages in queues by message properties, you can locate messages across all queues. The following scenarios are examples of how you might use message filtering to manage mail flow:

- The Submission queue on the Mailbox server or Edge Transport server that receives email from the Internet has a high volume of messages that are queued for delivery. Many of the messages have the same subject. Therefore, you suspect that spam is being sent to your organization. You can create a filter to view all the messages that meet the subject criteria. If you determine that the messages are spam, you can select them all and delete them from the delivery queue without sending an NDR.

- A user reports that mail flow is slow. You examine the queues and see that many messages with random subjects appear to be coming from a single domain. You can create a filter to view all the queued messages from that domain. If you determine that the messages are spam, you can select them all and delete them from the queues without sending an NDR.

You can create message filters in Queue Viewer in the Exchange Toolbox, or by using the _Filter_ parameter on the message management cmdlets. Note that the message management cmdlets support more filterable properties than Queue Viewer.

For more information about Queue Viewer, see [Queue Viewer](queue-viewer.md). For more information about the message management cmdlets, see [Procedures for messages in queues](message-procedures.md) and [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md).

## Message properties to use as filters

The following table describes the message properties that you can use as filters in Queue Viewer and the Exchange Management Shell.

|**Queue Viewer**|**Exchange Management Shell**|**Comparison operators**|**Description**|
|:-----|:-----|:-----|:-----|
|n/a|`AccountForest`|n/a|his property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations. <br/><br/> In on-premises Exchange, this property is the forest root domain where the mailbox resides (for example, contoso.com).|
|n/a|`ComponentLatency`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|**Date Received**|`DateReceived`|**Greater Than** (`-gt`) <br/><br/> **Greater Than or Equals** (`-ge`) <br/><br/> **Less Than** (`-lt`) <br/><br/> **Less Than or Equals** (`-le`)|The date/time when the message was placed in the queue.|
|n/a|`DeferReason`|Equals (`-eq`) <br/><br/> Does not equal (`-ne`) <br/><br/> Contains (`-like`)|Indicates why the message was deferred. If the message wasn't deferred, this property has the value `None`. A deferred message is returned to the Submission queue because of transient errors that were encountered during recipient resolution. For more information about deferred messages, see [Recipient resolution in Exchange Server](../../mail-flow/mail-routing/recipient-resolution.md). The possible values are: <br/> `AD Transient Failure During Content Conversion`) <br/> `AD Transient Failure During Resolve` <br/> `Agent` <br/> `Ambiguous Recipient` <br/> `Config Update` <br/> `Loop Detected` <br/> `Marked As Retry Delivery If Rejected` <br/> `Recipient does not have a mailbox database` <br/> `Recipient Thread Limit Exceeded` <br/> `Rerouted By Store Driver` <br/> `Storage Transient Failure During Content Conversion` <br/> `Target Site Inbound Mail Disabled` <br/> `Transient Accepted Domains Load Failure` <br/> `Transient Attribution Failure` <br/> `Transient Failure`|
|n/a|`Directionality`|Equals (`-eq`) <br/><br/> Does Not Equal (`-ne`)|Valid values are `Incoming`, `Originating`, and `Undefined`.|
|**Expiration Time**|`ExpirationTime`|**Greater Than** (`-gt`) <br/><br/> **Greater Than or Equals** (`-ge`) <br/><br/> **Less Than** (`-lt`) <br/><br/> **Less Than or Equals** (`-le`)|The date/time when the message will expire and be deleted from the queue if the message can't be delivered.|
|n/a|`ExternalDirectoryOrganizationId`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations. <br/><br/> In on-premises Exchange, the value is `00000000-0000-0000-0000-000000000000`.|
|**From Address**|`FromAddress`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`)|The SMTP address of the sender.|
|n/a|`Identity`|n/a|The identity of the message in the form of _\<Server\>_\ _\<Queue\>_\ _\<MessageInteger\>_. For more information see [Message identity](queues-and-messages-in-powershell.md#message-identity).|
|**Internet Message ID**|`InternetMessageId`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`)|The value of the **Message-Id:** header field in the message header. This value is constant for the lifetime of the message. For messages created in Exchange, the value is in the format `<GUID@ServerFQDN>`, including the angle brackets (\< \>). For example, `<4867a3d78a50438bad95c0f6d072fca5@mailbox01.contoso.com>`.|
|**Last Error**|`LastError`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`) <br/><br/> **Is Present** <br/> **Is Not Present**|The last error that was recorded for a message. For example, `A matching connector cannot be found to route the external recipient`.|
|n/a|`LockReason`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|n/a|`MessageLatency`|Equals (`-eq`) <br/><br/> Does not equal (`-ne`) <br/><br/> Greater than (`-gt`) <br/><br/> Greater than or equal to (`-ge`) <br/> Less than (`-lt`) <br/> Less than or equal to (`-le`|The amount of time that elapsed between when the message first entered the Submission queue on the server, and when the message was placed in the queue. The value uses the syntax _hh:mm:ss.ff_, where _hh_ = hour, _mm_ = minute, _ss_ = second, and _ff_ = fractions of a second.|
|**Message Source Name**|`MessageSourceName`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`)|The name of the transport component that submitted the message to the queue. For example, if the message came in through a Receive connector, the value is: `SMTP:` _\<ConnectorName\>_. If the message is a delivery status notification (DSN), the value is `DSN`.|
|n/a|`OriginalFromAddress`|Equals (`-eq`) <br/><br/> Does not equal (`-ne`) <br/><br/> Contains (`-like`)|The original sender's email address for any new side effect messages that are created during categorization (for example, journal rules, NDRs, or mail flow rules rules, also known as transport rules).|
|n/a|`Priority`|Equals (`-eq`) <br/><br/> Does not equal (`-ne`)|The priority (importance) of the message that's assigned by the user in Microsoft Outlook or Outlook on the web. Valid values are `Low`, `Normal`, and `High`. For more information, see [Priority Queuing](https://docs.microsoft.com/exchange/priority-queuing-exchange-2013-help).|
|**Queue ID**|`Queue`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`)|The queue that holds the message. The queue identity uses the syntax _\<Server\>\\<Queue\>_. For more information, see [Queue identity](queues-and-messages-in-powershell.md#queue-identity).|
|n/a|`Recipients`|Contains (`-like`)|An array that contains details about the recipient and the Send connector that will be used, or any errors that were encountered. For example: <br/><br/> `{chris@contoso.com;2;2;A matching connector cannot be found to route the external recipient;16;<No Matching Connector>;0}`|
|n/a|`RetryCount`|Equals (`-eq`) <br/><br/> Does not equal (`-ne`) <br/><br/> Greater than (`-gt`) <br/><br/> Greater than or equal to (`-ge`) <br/> Less than (`-lt`) <br/> Less than or equal to (`-le`|The number of times that delivery of the message to the destination was tried, either automatically or manually.|
|**SCL**|`SCL`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Greater Than** (`-gt`) <br/><br/> **Greater Than or Equals** (`-ge`) <br/> **Less Than** (`-lt`) <br/> **Less Than or Equals** (`-le`|The spam confidence level (SCL) rating of the message. Valid SCL entries are integers 0 through 9, or -1 for internal (authenticated) messages. For more information, see [Exchange spam confidence level (SCL) thresholds](../../antispam-and-antimalware/antispam-protection/scl.md).|
|**Size (KB)**|`Size`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Greater Than** (`-gt`) <br/><br/> **Greater Than or Equals** (`-ge`) <br/> **Less Than** (`-lt`) <br/> **Less Than or Equals** (`-le`|The size of the message. In Queue Viewer, you need to specify the message size in kilobytes (KB), but in the Exchange Management Shell, you can also specify other sizes, for example, bytes (B) or megabytes (MB).|
|**Source IP**|`SourceIP`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`)|The IPv4 or IPv6 address of the server that submitted the message to the Exchange server that holds the message in the queue. The address could be the IP address of a remote SMTP server, or the IP address of the local Exchange server.|
|**Status**|`Status`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`)|The current message status. Valid values are: <br/><br/> **Active** <br/> **Locked** <br/> **Pending Remove** (`PendingRemove`) <br/><br/> **Pending Suspend** (`PendingSuspend`) <br/><br/> **Ready** <br/> **Retry** <br/> **Suspended** <br/> For more information, see [Message status](queues.md#message-status).|
|**Subject**|`Subject`|**Equals** (`-eq`) <br/><br/> **Does Not Equal** (`-ne`) <br/><br/> **Contains** (`-contains`) <br/><br/> **Is Present** <br/> **Is Not Present**|The subject of the message (from the **Subject:** header field).|
|n/a|`TrafficType`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations. <br/><br/> In on-premises Exchange, this property is blank or has the value `Email`.|
|n/a|`TrafficSubType`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
