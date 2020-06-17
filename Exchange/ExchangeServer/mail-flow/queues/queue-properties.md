---
localization_priority: Normal
description: Learn about queue properties to use in filters in Exchange Server.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fbfbdcab-e0d2-4ed9-8f7f-e5fa2c87360d
ms.reviewer:
title: Queue properties in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Queue properties in Exchange Server

Filtering queues by one or more queue properties in Exchange Server allows you to quickly find and take action on those queues. The following scenarios are examples of how you might use queue filtering to manage mail flow:

- You receive a message from System Center Operations Manager that indicates a queue length has exceeded the established threshold. You want to investigate whether a server-wide mail flow problem exists.

    You create a filter to view all the queues on a server whose message count exceeds what you consider to be typical. If a mail flow problem is indicated, you can select all the queues in the results and suspend the queues while you continue to investigate.

- You suspend several queues to investigate the cause of mail flow problems. You determine that the problem was caused by an incorrect connector configuration that is now fixed.

    You can create a filter to view all the queues that have a status of Suspended, and then select all the queues in the filter results and resume the queues.

You can create queue filters in Queue Viewer in the Exchange Toolbox, or by using the _Filter_ parameter on the queue management cmdlets. Note that the queue management cmdlets support more filterable properties than Queue Viewer.

For more information about Queue Viewer, see [Queue Viewer](queue-viewer.md). For more information about the queue management cmdlets, see [Procedures for queues](queue-procedures.md) and [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md).

## Queue properties to use as filters

 The following table describes the queue properties that you can use as filters in Queue Viewer and the Exchange Management Shell.

|**Queue Viewer**|**Exchange Management Shell**|**Comparison operators**|**Description**|
|:-----|:-----|:-----|:-----|
|n/a| `DeferredMessageCount`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|The number of messages returned to the Submission queue because of transient errors that were encountered during recipient resolution. For more information about deferred messages, see [Recipient resolution in Exchange Server](../../mail-flow/mail-routing/recipient-resolution.md).|
|n/a| `DDeferredMessageCountsPerPriority`|Equals (`-eq`)  <br/> Does Not Equal (`-ne`)  <br/> Contains (`-like`)|An array that shows the number of deferred messages in the queue by priority (importance) value. The **MessageCountsPerPriority** property shows what each number means.  <br/> For example, the value `{1, 5, 10, 0}` indicates the queue contains 1 deferred High priority message, 5 deferred Normal priority messages, 10 deferred Low priority messages, and no deferred messages that have the priority value None.|
|**Delivery Type**| `DeliveryType`|**Equals** (`-eq`)  <br/> **Does Not Equal** (`-ne`)|The results of the categorization of the message, and how the Transport service intends to transmit the message to the next hop. For a list of the available **DeliveryType** values, see [NextHopSolutionKey](queues.md#nexthopsolutionkey).|
|n/a| `FirstRetryTime`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|The date/time of the first connection attempt for a queue that has a status of `Retry`. For more information, see [Message retry, resubmit, and expiration intervals](message-intervals.md).|
|n/a| `Identity`|n/a|The identity of the queue in the form of _\<Server\>_\ _\<Queue\>_. For more information see [Queue identity](queues-and-messages-in-powershell.md#queue-identity).|
| n/a| `IncomingRate`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|A calculated number that indicates how quickly messages are entering the queue. For more information, see [IncomingRate, OutgoingRate, and Velocity](queues.md#incomingrate-outgoingrate-and-velocity).|
|**Last Error**| `LastError`|**Equals** (`-eq`)  <br/> **Does Not Equal** (`-ne`)  <br/> **Contains** (`-contains`)  <br/> **Is Present** <br/> **Is Not Present**|The last error that was recorded for the queue. For more information about SMTP error codes, see [DSNs and NDRs in Exchange Server](../../mail-flow/non-delivery-reports-and-bounce-messages/non-delivery-reports-and-bounce-messages.md).|
|**Last Retry Time**| `LastRetryTime`|**Greater Than** (`-gt`)  <br/> **Greater Than or Equals** (`-ge`)  <br/> **Less Than** (`-lt`)  <br/> **Less Than or Equals** (`-le`)  <br/> **Is Present** <br/> **Is Not Present**|The date/time of the last connection attempt for a queue that has a status of `Retry`. For more information, see [Message retry, resubmit, and expiration intervals](message-intervals.md).|
|n/a| `LockedMessageCount`|n/a|This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|**Message Count**| `MessageCount`|**Equals** (`-eq`)  <br/> **Does Not Equal** (`-ne`)  <br/> **Greater Than** (`-gt`)  <br/> **Greater Than or Equals** (`-ge`)  <br/> **Less Than** (`-lt`)  <br/> **Less Than or Equals** (`-le`)|The number of messages in the queue.|
|n/a| `MessageCountsPerPriority`|Equals (`-eq`)  <br/> Does Not Equal (`-ne`)  <br/> Contains (`-like`)|An array that shows the number of messages in the queue by priority (importance) value. The **MessageCountsPerPriority** property shows what each number means.  <br/> For example, the value `{1, 100, 10, 0}` indicates the queue contains 1 High priority message, 100 Normal priority messages, 10 Low priority messages, and no messages that have the priority value None.  <br/> For more information about priority queuing, see [Priority Queuing](https://docs.microsoft.com/exchange/priority-queuing-exchange-2013-help).|
| n/a| `NextHopCategory`|Equals (`-eq`)  <br/> Does Not Equal (`-ne`)|The value `Internal` or `External` for the next hop based on the value of the **DeliveryType** property. For more information, see [NextHopSolutionKey](queues.md#nexthopsolutionkey).|
|n/a| `NextHopConnector`|Equals (`-eq`)  <br/> Does Not Equal (`-ne`)  <br/> Contains (`-like`)|The GUID of the next hop based on the value of the **DeliveryType** property. For more information, see [NextHopSolutionKey](queues.md#nexthopsolutionkey).|
|**Next Hop Domain**| `NextHopDomain`|**Equals** (`-eq`)  <br/> **Does Not Equal** (`-ne`)  <br/> **Contains** (`-like`)|The name of next hop based on the value of the **DeliveryType** property. For more information, see [NextHopSolutionKey](queues.md#nexthopsolutionkey).|
|**Next Retry Time**| `NextRetryTime`|**Greater Than** (`-gt`)  <br/> **Greater Than or Equals** (`-ge`)  <br/> **Less Than** (`-lt`)  <br/> **Less Than or Equals** (`-le`)  <br/> **Is Present** <br/> **Is Not Present**|The date/time of the next connection attempt for a queue that has a status of `Retry`. For more information, see [Message retry, resubmit, and expiration intervals](message-intervals.md).|
|n/a| `OutboundIPPool`|n/a| This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|n/a| `OutgoingRate`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|A calculated number that indicates how quickly messages are leaving the queue. For more information, see [IncomingRate, OutgoingRate, and Velocity](queues.md#incomingrate-outgoingrate-and-velocity).|
|n/a| `OverrideSource`|n/a| This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|n/a| `PriorityDescriptions`|n/a|The value descriptions in the **DeferredMessageCountsPerPriority** and **MessageCountsPerPriority** properties. The value of this property is `{High, Normal, Low, None}`.  <br/> Because the value of this property is always the same, it won't make a good filter.|
|n/a| `RetryCount`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|The number connection attempts for a queue that has a status of `Retry`. For more information, see [Message retry, resubmit, and expiration intervals](message-intervals.md).|
| n/a| `RiskLevel`|n/a| This property is reserved for internal Microsoft use, and isn't used in on-premises Exchange organizations.|
|**Status**| `Status`|**Equals** (`eq`)  <br/> **Does Not Equal** (`-ne`)|The current queue status. A queue can have one of the following status values: Active, Connecting, Suspended, Ready, or Retry. For more information, see [Queue status](queues.md#queue-status).|
|n/a| `TlsDomain`|Equals (`-eq`)  <br/> Does Not Equal (`-ne`)  <br/> Contains (`-like`)|The FQDN of the destination domain if the domain is configured for Domain Security (mutual TLS authentication).|
|n/a| `Velocity`|Equals (`-eq`)  <br/> Does not equal (`-ne`)  <br/> Greater than (`-gt`)  <br/> Greater than or equal to (`-ge`)  <br/> Less than (`-lt`)  <br/> Less than or equal to (`-le`)|A calculated number that indicates how effectively the queue is draining. For more information, see [IncomingRate, OutgoingRate, and Velocity](queues.md#incomingrate-outgoingrate-and-velocity)|
