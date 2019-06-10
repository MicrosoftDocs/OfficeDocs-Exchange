---
localization_priority: Normal
description: Learn how shadow redundancy in Exchange 2016 and Exchange 2019 improves high availability for messages in the transport pipeline.
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: a40dbe61-2a18-48a8-b2e0-4e81a6678d11
ms.date: 7/10/2018
ms.reviewer: 
title: Shadow redundancy in Exchange Server
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Shadow redundancy in Exchange Server

Shadow redundancy was introduced in Exchange 2010 to provide redundant copies of messages before they're delivered to mailboxes. In Exchange 2010, shadow redundancy delayed deleting a message from the queue database on a Hub Transport server until the server verified that the next hop in the message delivery path had completed delivery. If the next hop failed before reporting successful delivery back to the Hub Transport server, the server resubmitted the message to that next hop. Exchange 2010 Hub Transport servers used the **XSHADOW** verb to advertise their shadow redundancy support. If a source messaging server didn't support shadow redundancy, Exchange 2010 used delayed acknowledgment based on a configured time interval on the Receive connector to make a redundant copy of the message.

Exchange 2016 and Exchange 2019 have the same improvements that were made to shadow redundancy in Exchange 2013: the Transport service on a Mailbox server now makes a redundant copy of any message it receives before acknowledging the receipt of the message to the sending server. Maintaining redundant copies of messages in transit is more than a best effort that may or may not work, because now shadow redundancy doesn't depend the sending server's supported features (support or lack of support for shadow redundancy doesn't matter). This helps to ensure that all messages in the transport pipeline are made redundant while they're in transit. If Exchange determines the original message was lost in transit, the redundant copy of the message is redelivered.

For more information about transport high availability features in Exchange Server, see [Transport high availability in Exchange Server](transport-high-availability.md). For more information about message redundancy after a message has been successfully delivered, see [Safety Net in Exchange Server](safety-net.md).

## Shadow redundancy components

This table describes the components of shadow redundancy in the Transport service on Mailbox servers. These terms are used throughout the topic.

|**Term**|**Description**|
|:-----|:-----|
|Transport high availability boundary|A database availability group (DAG) in DAG environments, or an Active Directory site in non-DAG environments. For DAGs that span multiple Active Directory sites, the DAG itself is still the boundary (not the site).  <br/>  When a message arrives on a Mailbox server in the transport high availability boundary, Exchange tries to maintain two redundant copies of the message on Mailbox servers within the boundary. When a message leaves the transport high availability boundary, Exchange stops maintaining redundant copies of the message.|
|Primary message|The message submitted into the transport pipeline for delivery.|
|Shadow message|The redundant copy of the message that the shadow server retains until it confirms the primary message was successfully processed by the primary server.|
|Primary server|The Mailbox server that's currently processing the primary message.|
|Shadow server|The Mailbox server that holds the shadow message for the primary server. A Mailbox server may be the primary server for some messages and the shadow server for other messages simultaneously.|
|Shadow queue|The delivery queue where the shadow server stores shadow messages. For messages with multiple recipients, each next hop for the primary message requires separate shadow queues.|
|Discard status|The information that the Mailbox server maintains for shadow messages to indicate the primary message has been successfully processed.|
|Discard notification|The response a shadow server receives from a primary server indicating a shadow message is ready to be discarded.|
|Safety Net|The improved version of the transport dumpster in Exchange 2013 or later. Messages that are successfully processed or delivered to a mailbox recipient by the Transport service on a Mailbox server are moved into Safety Net. For more information, see [Safety Net in Exchange Server](safety-net.md).|
|Shadow Redundancy Manager|The transport component that manages shadow redundancy.|
|Heartbeat|The process that allows primary servers and shadow servers to verify the availability of each other.|

## Requirements for shadow redundancy

Although it may seem obvious, shadow redundancy requires multiple Mailbox servers:

- If the Mailbox server isn't a member of a DAG, the other Mailbox servers must be in the local Active Directory site.

- If the Mailbox server is a member of a DAG, the other Mailbox servers must belong to the same DAG. The other DAG members can be in the local Active Directory site, or in a remote site. By default, if the DAG spans multiple Active Directory sites, shadow redundancy prefers creating a redundant copy of the message in a remote site for site resiliency.

These are the situations where shadow redundancy can't protect messages in transit:

- In single Exchange server environments.

- In under-provisioned DAGs.

- During the simultaneous failure of two or more Mailbox servers involved in the shadow redundancy of a message.

## Shadow redundancy is enabled by default

By default, shadow redundancy is enabled globally in the Transport service on all Mailbox servers. This table describes the parameters that enable shadow redundancy.

|**Parameter**|**Default value**|**Description**|
|:-----|:-----|:-----|
| _ShadowRedundancyEnabled_ on **Set-TransportConfig**| `$true`| `$true`: Shadow redundancy is enabled on all Mailbox servers in the organization.  <br/> `$false`: Shadow redundancy is disabled on all Mailbox servers in the organization.|
| _RejectMessageOnShadowFailure_ on **Set-TransportConfig**| `$false`| `$false`: When a shadow copy of the message can't be created, the primary message is accepted anyway by Mailbox servers in the organization. These messages aren't redundantly persisted while they're in transit.  <br/> `$true`: No message is accepted or acknowledged by any Mailbox server in the organization until a shadow copy of the message is successfully created. If a shadow copy of the message can't be created, the primary message is rejected with a transient error, but the sending server can transmit the message again. The SMTP response code is `451 4.4.0 Message failed to be made redundant`. All messages in the organization are redundantly persisted while they're in transit.  <br/> **Note**: Use `$true` only if you have multiple Mailbox servers in the same DAG or Active Directory site so a shadow copy of the message can be created.  <br/> This parameter is only meaningful when _ShadowRedundancyEnabled_ is `$true`.|

## How shadow messages are created

The main goal of shadow redundancy is to always have two copies of a message within a transport high availability boundary while the message is in transit. Where and when the redundant copy of the message is created depends on where the message came from, and where the message is going. There are three determining factors for creating shadow messages:

- Messages received from outside a transport high availability boundary (the DAG, or an Active Directory site in non-DAG environments).

- Messages sent outside a transport high availability boundary.

- Messages received from the Mailbox Transport Submission service from a Mailbox server within the transport high availability boundary.

Shadow redundancy never tracks shadow messages across a transport high availability boundary. When a message crosses the transport high availability boundary, shadow redundancy begins or restarts. This reduces shadow message maintenance traffic and prevents shadow message resubmissions across the transport high availability boundary. Exchange 2010 Hub Transport servers are a special case, and are discussed later in this topic.

### Messages received from outside a transport high availability boundary

When the Transport service on a Mailbox server receives a message from outside the transport high availability boundary, the Mailbox server isn't concerned about the support or lack of support for shadow redundancy by the sending server. As long as shadow redundancy is enabled, the Mailbox server that receives the message makes a redundant copy of the message on another Mailbox server within the transport high availability boundary before acknowledging receipt of the message back to the sending server. Here's an example of how the process works:

![Shadow message creation](../../media/ITPro_Transport_ShadowMessagingOverview.gif)

1. A messaging server transmits a message to the Transport service on a Mailbox server. The Mailbox server is the primary server, and the message is the primary message.

2. While the original SMTP session with the messaging server is still active, the Transport service on primary server opens a new, simultaneous SMTP session with the Transport service on a different Mailbox server in the organization to create a redundant copy of the message.

  - If the primary server is a member of a DAG, the primary server connects to a different Mailbox server in the same DAG. If the DAG spans multiple Active Directory sites, a Mailbox server in a different Active Directory site is preferred by default (the default value of the _ShadowMessagePreference_ parameter on the **Set-TransportService** cmdlet is `PreferRemote`, but you can change it to `RemoteOnly` or `LocalOnly`).

  - If the primary server isn't a member of a DAG, the primary server connects to a different Mailbox server in the same Active Directory site (regardless of the value of the _ShadowMessagePreference_ parameter).

3. The primary server transmits a copy of the message to the Transport service on another Mailbox server, and Transport service on the other Mailbox server acknowledges that the copy of the message was created successfully. The copy of the message is the shadow message, and the Mailbox server that holds it is the shadow server for the primary server. The message exists in a shadow queue on the shadow server.

4. After the primary server receives acknowledgment from the shadow server, the primary server acknowledges the receipt of the primary message to the original messaging server in the original SMTP session, and the SMTP session is closed.

### Messages sent outside a transport high availability boundary

When a Mailbox server transmits a message outside the transport high availability boundary, and the messaging server on the other side acknowledges successful receipt of the message, and the Mailbox server moves the message into Safety Net. No resubmission of the message from Safety Net can occur after the primary message has been successfully transmitted across the transport high availability boundary. For more information about Safety Net, see [Safety Net in Exchange Server](safety-net.md).

### Messages transmitted within a transport high availability boundary

Message routing is optimized so that when the ultimate destination is in a DAG or Active Directory site, multiple hops between servers within the destination DAG or site aren't typically required. After the message is accepted by the Transport service on a Mailbox server in the destination DAG or Active Directory, the next hop for the message is typically the ultimate destination itself (for example, the Mailbox server that holds the active copy of the destination mailbox). Shadow redundancy's goal of keeping two copies of a message in transit is fulfilled when one shadow copy of the message exists *anywhere* within the DAG or Active Directory site. Typically, only failover scenarios in a DAG that require the **Redirect-Message** cmdlet to drain the active message queues on a Mailbox server would require multiple hops within the same transport high availability boundary.

### Shadow redundancy with Exchange 2010 Hub Transport servers in the same Active Directory site in Exchange 2016 organizations

When an Exchange 2010 Hub Transport server transmits a message to an Exchange 2016 Mailbox server in the same Active Directory site, the Exchange 2010 Hub Transport server advertises support for shadow redundancy using the XSHADOW command, but the Mailbox server doesn't advertise support for shadow redundancy. This prevents the Exchange 2010 Hub Transport server from creating a shadow copy of the message on an Exchange 2016 Mailbox server.

When the Transport service on an Exchange 2016 Mailbox server transmits a message to an Exchange 2010 Hub Transport in the same Active Directory site, the Exchange 2016 Mailbox server shadows the message for the Exchange 2010 Hub Transport server. After the Exchange 2016 Mailbox server receives acknowledgment from the Exchange 2010 Hub Transport server that the message was successfully received, the Exchange 2016 Mailbox server moves the successfully processed message into Safety Net. However, the successfully processed messages stored in Safety Net by Exchange 2016 Mailbox are never resubmitted to the Exchange 2010 Hub Transport servers.

## SMTP timeouts

During the attempt to make a redundant copy of the message, the SMTP connection between servers (the sending server and the primary server, or the primary server and the shadow server) could timeout. Receive connectors and Send connectors both have a _ConnectionInactivityTimeOut_ parameter for when data is actually being transmitted on the connector. Receive connectors also have an absolute _ConnectionTimeOut_ parameter.

If any of the SMTP sessions time out before the shadow copy of the message is successfully created and acknowledged, the result is controlled by the _RejectMessageOnShadowFailure_ parameter on the **Set-TransportConfig** cmdlet. By default, the value of this parameter is `$false`, which means the primary message is accepted without a shadow copy being created. If the value of this parameter is `$true` the primary message is rejected with the transient error `451 4.4.0`.

If the shadow copy of a message is successfully created, but the SMTP session between the sending server and the primary server times out, the primary server accepts and processes the primary message. The sending server will re-deliver the unacknowledged message, but duplicate message detection will prevent Exchange mailbox users from seeing the duplicate messages. When the sending server resubmits the message, the primary server will create another shadow copy of the message. There's no relationship between the shadow messages created during message resubmissions by the sending server.

The following table describes the parameters that control the creation of shadow messages

|**Source**|**Default value**|**Description**|
|:-----|:-----|:-----|
| _ShadowMessagePreferenceSetting_ on **Set-TransportConfig**| `PreferRemote`|This parameter is only used when the primary server that's trying to make a shadow copy of the message is a member of a DAG that spans multiple Active Directory sites.  <br/> `PreferRemote`: Try to make a shadow copy of the message on a DAG member in a different Active Directory site based on the number of attempts specified by the _MaxRetriesForRemoteSiteShadow_ parameter. If the operation fails, try make a shadow copy of the message on a DAG member in the local Active Directory site based on the number of attempts specified by the _MaxRetriesForLocalSiteShadow_ parameter.  <br/> `LocalOnly`: Try to make a shadow copy of the message only on a DAG member in the local Active Directory site based on the number of attempts specified by the _MaxRetriesForLocalSiteShadow_ parameter.  <br/> `RemoteOnly`: Try to make shadow copy of the message only on a DAG member in a different Active Directory site based on the number of attempts specified by the _MaxRetriesForRemoteSiteShadow_ parameter.|
| _MaxRetriesForRemoteSiteShadow_ on **Set-TransportConfig**|4|This parameter specifies the maximum number of attempts to create a shadow copy of the message on another server in the DAG when the value of the _ShadowMessagePreferenceSetting_ parameter is `PreferRemote` (the default value) or `RemoteOnly`.  <br/> This parameter is only used when the Mailbox server is a member of a DAG that spans multiple Active Directory sites.  <br/> If a shadow copy of the message isn't successfully created after the specified number of attempts, the result depends of the value of the _RejectMessageOnShadowFailure_ parameter:  <br/> `$true`: The primary message is rejected with a transient error.  <br/> `$false`: The primary message is accepted anyway, but isn't redundantly persisted.|
| _MaxRetriesForLocalSiteShadow_ on **Set-TransportConfig**|2|This parameter specifies the maximum number of attempts to create a shadow copy of the message on another Mailbox server in the local Active Directory site when:  <br/> • The Mailbox server is a member of a DAG that spans multiple Active Directory sites, and the value of the _ShadowMessagePreferenceSetting_ parameter is `PreferRemote` (the default value) or `LocalOnly`.  <br/> • The Mailbox server is a member of a DAG that's in one Active Directory site.  <br/> • The Mailbox server isn't a member of a DAG.  <br/> If a shadow copy of the message isn't successfully created after the specified number of attempts, the result depends of the value of the _RejectMessageOnShadowFailure_ parameter:  <br/> `$true`: The primary message is rejected with a transient error.  <br/> `$false`: The primary message is accepted anyway, but isn't redundantly persisted.|
| _ConnectionInactivityTimeout_ on **Set-ReceiveConnector**|5 minutes for Receive connectors in the Transport service on Mailbox servers|This parameter specifies the maximum time that an open SMTP connection with the source messaging server can remain idle before the connection is closed. The value of this parameter must be greater than the value of the _ConnectionTimeout_ parameter.|
| _ConnectionTimeout_ on **Set-ReceiveConnector**|10 minutes for Receive connectors in the Transport service on Mailbox servers|This parameter specifies the maximum time that an SMTP connection with the source messaging server can remain open, even if the server is transmitting data. The value of this parameter must be greater than the value of the _ConnectionInactivityTimeout_ parameter.|
| _ConnectionInactivityTimeOut_ on **Set-SendConnector**|10 minutes|This parameter specifies the maximum time that an open SMTP connection with a destination messaging server can remain idle before the connection is closed.|

## How shadow messages are maintained

After a shadow message is successfully created, the work of shadow redundancy has only just begun. The primary server and the shadow server need to stay in contact with each other to track the progress of the message.

When the primary server successfully transmits the message to the next hop, and the next hop acknowledges receipt of the message, the primary server updates the *discard status* of the message as delivery complete. The discard status is basically a message that contains of list of messages that are being monitored. A successfully delivered message doesn't need to be kept in a shadow queue, so once the shadow server knows the primary server has successfully transmitted the message to the next hop, the shadow server moves the shadow message from the shadow queue into Safety Net.

The shadow server determines the discard status of the shadow messages in its shadow queues by querying the primary server. If the shadow server opens an SMTP session with the primary server for any reason (including the transmission of other unrelated messages), the shadow server issues the **XQDISCARD** command to determine the discard status of the primary messages. Otherwise, the shadow server will automatically open an SMTP session with the primary server after a preconfigured time interval (the _ShadowHeartbeatFrequentcy_ parameter on the **Set-TransportConfig** cmdlet; the default value is 2 minutes).

After the shadow server opens an SMTP session with the primary server, the primary server responds with the *discard notifications* for messages that apply to the querying shadow server. Discard notifications are stored on disk (not in memory) so, if the Microsoft Exchange Transport service restarts, the discard notifications persist. After the service starts, the primary server still knows about the messages it successfully processed, and that information is available to the shadow server.

The SMTP communication between the shadow server and the primary server is used as the *heartbeat* that determines the availability of the servers. If the shadow server can't open an SMTP session with the primary server after a preconfigured time interval (the _ShadowResubmitTimeSpan_ parameter on the **Set-TransportConfig** cmdlet; the default value is 3 hours) the shadow server promotes itself as the primary server, promotes the shadow messages as primary messages, and transmits the messages to the next hop. But, whenever the shadow server detects that the queue database ID of the primary server has changed, the shadow server also promotes itself as the primary server, promotes the shadow messages as primary messages, and transmits the messages to the next hop. This could happen well before the _ShadowResubmitTimeSpan_ parameter value has passed.

 *Shadow Redundancy Manager* is the core component on a Mailbox server that's responsible for managing shadow redundancy. Shadow Redundancy Manager is responsible for maintaining the following information for all the primary messages that a server is currently processing:

- The shadow server for each primary message that's being processed.

- The discard status to be sent to shadow servers.

Shadow Redundancy Manager is responsible for the following actions for all the shadow messages that a shadow server has in its shadow queues:

- Maintaining the list of primary servers for each shadow message.

- Comparing the original database ID and the current database ID of the queue database where the primary copy of the message is stored.

- Checking the availability of each primary server for which a shadow message is queued.

- Processing discard notifications from primary servers.

- Removing the shadow messages from the shadow queues after all expected discard notifications are received.

- Deciding when the shadow server should take ownership of shadow messages, becoming a primary server.

- Tracking message bifurcations and other side-effect messages like delivery status notifications (also known as DSNs, non-delivery reports, NDRs, or bounce messages) and journal reports to verify that the redundant copy of the message isn't released until all forks of the message are fully processed.

This table describes the parameters that control how shadow messages are maintained.

|**Parameter**|**Default value**|**Description**|
|:-----|:-----|:-----|
| _ShadowHeartbeatFrequency_ on **Set-TransportConfig**|2 minutes|The maximum amount of time a shadow server waits before opening an SMTP connection to the primary server to check the discard status of messages.|
| _ShadowResubmitTimeSpan_ on **Set-TransportConfig**|3 hours|How long a server waits before deciding that a primary server has failed and assumes ownership of shadow messages in the shadow queue for the primary server that's unreachable.  <br/> Note that a shadow server can also promote itself as the primary server before the value of this parameter when the queue database of the primary server is found to have a different database ID.|
| _ShadowMessageAutoDiscardInterval_ on **Set-TransportConfig**|2 days|How long a server retains discard events for successfully delivered messages. A primary server queues discard events until it's queried by the shadow server. However, if the shadow server doesn't query the primary server for the duration specified in this parameter, the primary server deletes the queued discard events.|
| _SafetyNetHoldTime_ on **Set-TransportConfig**|2 days|How long successfully processed messages are retained in Safety Net. Unacknowledged shadow messages eventually expire from Safety Net after the sum of the _SafetyNetHoldTime_ and _MessageExpirationTimeout_ parameter values on the **Set-TransportService** cmdlet.|
| _MessageExpirationTimeout_ on **Set-TransportService**|2 days|How long a message can remain in a queue before it expires.|

## Message processing after an outage

This table summarizes how shadow redundancy minimizes message loss due to server outages. For clarity, the server that had an outage is named Mailbox01.

|**Recovery scenario**|**Actions taken**|
|:-----|:-----|
|Mailbox01 comes back online with a new queue database before the value of the _ShadowResubmitTimeSpan_ parameter has passed (by default, 3 hours).  <br/> This scenario can occur when the queue database is unrecoverable due to data corruption or hardware failure.|When the new queue database ID is detected on Mailbox01, each server that has shadow messages queued for Mailbox01 will assume ownership of those messages and resubmit them. The messages are then delivered to their destinations.  <br/> The maximum delay for message submission after the new queue database is detected is the value of the _ShadowHeartbeatFrequency_ parameter (by default, 2 minutes).|
|Mailbox01 comes back online with the same database after the value of the _ShadowResubmitTimeSpan_ parameter has passed (by default, 3 hours)..  <br/> This scenario can occur after a network card failure, or time-consuming maintenance on the server.|After Mailbox01 comes back online, it will deliver the messages in its queues, which have already been delivered by the servers that hold shadow copies of messages for Mailbox01. This will result in duplicate delivery of these messages. Exchange mailbox users won't see duplicate messages due to duplicate message detection. However, recipients on other messaging systems might see duplicate copies of messages.  <br/> The maximum delay for message submission is the value of the _ShadowResubmitTimeSpan_ parameter.|
