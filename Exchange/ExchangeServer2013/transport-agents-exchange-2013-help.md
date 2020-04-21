---
title: 'Transport agents: Exchange 2013 Help'
TOCTitle: Transport agents
ms:assetid: e7389d63-3172-40d5-bf53-0d7cd7e78340
ms:mtpsurl: https://technet.microsoft.com/library/Bb125012(v=EXCHG.150)
ms:contentKeyID: 48385663
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Transport agents

_**Applies to:** Exchange Server 2013_

Transport agents let you install custom software that is created by Microsoft, by third-party vendors, or by your organization, on an Exchange server. This software can then process email messages that pass through the transport pipeline. In Microsoft Exchange Server 2013, the transport pipeline is made of the following processes:

- The Front End Transport service on Client Access servers

- The Transport service on Mailbox servers

- The Mailbox Transport service on Mailbox servers

- The Transport service on Edge Transport servers

For more information about the transport pipeline, see [Mail flow](mail-flow-exchange-2013-help.md)

Like the previous version of Exchange, Exchange 2013 transport provides extensibility through the Microsoft Exchange Server 2013 Transport Agents SDK. The Exchange 2013 version of the SDK is based on the Microsoft .NET Framework version 4.0 and allows third parties to implement the following predefined classes:

- **SmtpReceiveAgent**

- **RoutingAgent**

- **DeliveryAgent**

When complied against libraries in the SDK, the resulting assemblies are registered with Exchange 2013, which loads the agents and invokes their event handlers during specific stages of the SMTP sessions or message processing. These stages, or events, are part of the agent definitions. The agent registration information is stored in an XML configuration file.

The following list explains the requirements for using transport agents in Exchange 2013.

- The Transport service on Mailbox servers and Edge Transport servers fully supports all the predefined classes in the SDK, and therefore any third-party transport agents written for the Hub Transport or Edge Transport server roles in Microsoft Exchange Server 2010 should work in the Transport service in Exchange 2013.

- The Front End Transport service only supports the **SmtpReceiveAgent** class in the SDK, and third-party agents can't operate on the **OnEndOfData** SMTP event.

- The Mailbox Transport service doesn't support the SDK at all, so you can't use any third-party agents in the Mailbox Transport service.

Support for legacy transport agents based on versions of the .NET Framework prior to version 4.0 isn't enabled by default, but you can enable it. For instructions, see [Enable support for legacy transport agents](enable-support-for-legacy-transport-agents-exchange-2013-help.md).

## Updates to transport agent management

Due to the updates to the Exchange 2013 transport pipeline, the transport agent cmdlets need to distinguish between the Transport service and the Front End Transport service, especially if the Client Access server and the Mailbox server are installed on the same computer. For more information, see [Manage transport agents](manage-transport-agents-exchange-2013-help.md).

Transport Agent management cmdlets manipulate a configuration file located at `%ExchangeInstallPath%TransportRoles\Shared`. For the Transport service on Mailbox servers and Edge Transport servers, the file is `agents.config`. For the Front End Transport service on Client Access servers, the file is `fetagents.config`. Both files use the same format as in Exchange 2010. For more information about managing transport agents, see [Manage transport agents](manage-transport-agents-exchange-2013-help.md).

## Transport agents and SMTP events

Transport agents use SMTP events. These events are triggered as messages move through the transport pipeline. SMTP events give transport agents access to messages at specific points during the SMTP conversation and during routing of messages through the organization.

Note that there are new SMTP Receive events in Exchange 2013. SMTP Receive exists in the Front End Transport service on Client Access servers, the Transport service on Mailbox servers and Edge Transport servers and the Mailbox Transport Delivery service on Mailbox servers. The categorizer exists only in the Transport service on Mailbox servers and Edge Transport servers. For more information about transport services and the categorizer, see [Mail routing](mail-routing-exchange-2013-help.md).

The following tables list the SMTP events that provide access to messages in the transport pipeline.

### SMTP Receive events

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Sequence</th>
<th>SMTP event</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>1</p></td>
<td><p><strong>OnConnectEvent</strong></p></td>
<td><p>This event is triggered by the initial connection from a remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>2</p></td>
<td><p><strong>OnHeloCommand</strong></p></td>
<td><p>This event is triggered when the <code>HELO</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>3</p></td>
<td><p><strong>OnEhloCommand</strong></p></td>
<td><p>This event is triggered when the <code>EHLO</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>4</p></td>
<td><p><strong>OnStartTlsCommand</strong></p></td>
<td><p>This event is triggered when the <code>STARTTLS</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>5</p></td>
<td><p><strong>OnAuthCommand</strong></p></td>
<td><p>This event is triggered when the <code>AUTH</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>6</p></td>
<td><p><strong>OnProcessAuthentication</strong></p></td>
<td><p>This event is triggered when authentication with the remote SMTP host is being processed.</p></td>
</tr>
<tr class="odd">
<td><p>7</p></td>
<td><p><strong>OnEndOfAuthentication</strong></p></td>
<td><p>This event is triggered when the remote SMTP host has completed authentication.</p></td>
</tr>
<tr class="even">
<td><p>8</p></td>
<td><p><strong>OnXSessionParamsCommand</strong></p></td>
<td><p>This event is triggered when the <code>XSESSIONPARAMS</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>9</p></td>
<td><p><strong>OnMailCommand</strong></p></td>
<td><p>This event is triggered when the <code>MAIL FROM</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>10</p></td>
<td><p><strong>OnRcptToCommand</strong></p></td>
<td><p>This event is triggered when the <code>RCPT TO</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>11</p></td>
<td><p><strong>OnDataCommand</strong></p></td>
<td><p>This event is triggered when the <code>DATA</code> (text) or <code>BDAT</code> (binary data) command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>12</p></td>
<td><p><strong>OnEndOfHeaders</strong></p></td>
<td><p>This event is triggered when the remote SMTP host has completed submitting the email message headers. This is indicated by a blank line (<code>&lt;CRLF&gt;</code>) that separates the message headers and the message body.</p></td>
</tr>
<tr class="odd">
<td><p>13</p></td>
<td><p><strong>OnProxyInboundMessage</strong></p></td>
<td><p>This event is triggered when an inbound SMTP session is relayed or <em>proxied</em> by the Front End Transport service on a Client Access server to the Transport service on a Mailbox server.</p></td>
</tr>
<tr class="even">
<td><p>14</p></td>
<td><p><strong>OnEndOfData</strong></p></td>
<td><p>This event is triggered when the remote SMTP host issues an end of data command. For text sessions started by the <code>DATA</code> command, the end of data indicator is <code>&lt;CRLF&gt;.&lt;CRLF&gt;</code>. For binary sessions started by the <code>BDAT</code> command, the end of data indicator is <code>BDAT LAST</code>.</p></td>
</tr>
<tr class="odd">
<td><p>**</p></td>
<td><p><strong>OnHelpCommand</strong></p></td>
<td><p>This event is triggered if the <code>HELP</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>**</p></td>
<td><p><strong>OnNoopCommand</strong></p></td>
<td><p>This event is triggered if the <code>NOOP</code> command is issued by the remote SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>**</p></td>
<td><p><strong>OnReject</strong></p></td>
<td><p>This event is triggered if the receiving SMTP host issues a temporary or permanent delivery status notification (DSN) code to the sending SMTP host.</p></td>
</tr>
<tr class="even">
<td><p>**</p></td>
<td><p><strong>OnRsetCommand</strong></p></td>
<td><p>This event is triggered if the <code>RSET</code> command is issued by the sending SMTP host.</p></td>
</tr>
<tr class="odd">
<td><p>15</p></td>
<td><p><strong>OnDisconnectEvent</strong></p></td>
<td><p>This event is triggered by the disconnection of the SMTP conversation by either the receiving or sending SMTP host. Typically, this happens when the <code>QUIT</code> command is issued by the remote SMTP host.</p></td>
</tr>
</tbody>
</table>

\*\* These events can occur at any time after **OnConnectEvent** but before **OnDisconnectEvent**.

### Categorizer events

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Sequence</th>
<th>Categorizer event</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>1</p></td>
<td><p><strong>OnSubmittedMessage</strong></p></td>
<td><p>This event is triggered when a message arrives in the Submission queue in the Transport service on the receiving Mailbox server or Edge Transport server.</p></td>
</tr>
<tr class="even">
<td><p>2</p></td>
<td><p><strong>OnResolvedMessage</strong></p></td>
<td><p>This event is triggered after all the recipients have been resolved, but before the next hop has been determined for each recipient. The <strong>OnResolvedMessage</strong> routing event enables subsequent events to override the default routing behavior by using the per-recipient <strong>SetRoutingOverride</strong> method.</p></td>
</tr>
<tr class="odd">
<td><p>3</p></td>
<td><p><strong>OnRoutedMessage</strong></p></td>
<td><p>This event is triggered after messages have been categorized, distribution lists have been expanded, and recipients have been resolved.</p></td>
</tr>
<tr class="even">
<td><p>4</p></td>
<td><p><strong>OnCategorizedMessage</strong></p></td>
<td><p>This event is triggered when the categorizer completes processing the message.</p></td>
</tr>
</tbody>
</table>

## Priority of transport agents

There are two factors that determine the order that transport agents act on messages in the transport pipeline:

1. The SMTP event where the transport agent is registered, and when that SMTP event encounters messages.

2. The priority value that's assigned to the transport agent if there are multiple agents registered to the same SMTP event. The highest priority is 1. A higher integer value indicates a lower agent priority.

For example, suppose you configured the following transport agents:

- Transport Agent A with a priority of 1 and Transport Agent C with a priority of 2 are registered to the **OnEndOfHeaders** SMTP event.

- Transport Agent B with a priority of 4 is registered to the **OnMailCommand** SMTP event.

Transport Agent B is applied to messages first because the **OnMailCommand** event encounters messages before the **OnEndOfHeaders** event. When messages reach the **OnEndOfHeaders** event, Transport Agent A is applied before Transport Agent C because Transport Agent A has a higher priority (lower integer value) than Transport Agent C.

## Built-in transport agents

Exchange 2013 includes many built-in transport agents that provide features such as anti-spam, transport rules and journaling. Most of the built-in transport agents on Exchange 2013 Mailbox servers and Client Access servers are invisible and unmanageable by the transport agent management cmdlets. Virtually all of the built-in transport agents that are visible and manageable are in the Transport service on Mailbox servers and on Edge Transport servers.

The more interesting built-in transport agents on Mailbox servers are described in the following table. Note that this table doesn't include many of the invisible and unmanageable transport agents.

### Interesting built-in transport agents on Mailbox servers

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Agent name</th>
<th>Manageable?</th>
<th>Priority</th>
<th>SMTP or categorizer events</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Transport Rule Agent</p></td>
<td><p>Yes</p></td>
<td><p>1</p></td>
<td><p><strong>OnResolvedMessage</strong></p></td>
</tr>
<tr class="even">
<td><p>Malware Agent</p></td>
<td><p>Yes</p></td>
<td><p>2</p></td>
<td><p><strong>OnSubmittedMessage</strong></p></td>
</tr>
<tr class="odd">
<td><p>Text Messaging Routing Agent</p></td>
<td><p>Yes</p></td>
<td><p>3</p></td>
<td><p><strong>OnSubmittedMessage</strong></p></td>
</tr>
<tr class="even">
<td><p>Text Messaging Delivery Agent</p></td>
<td><p>Yes</p></td>
<td><p>4</p></td>
<td><p>n/a</p></td>
</tr>
<tr class="odd">
<td><p>Journal Agent</p></td>
<td><p>No</p></td>
<td><p>Not configurable</p></td>
<td><p><strong>OnRoutedMessage</strong></p></td>
</tr>
<tr class="even">
<td><p>Journal Report Decryption Agent</p></td>
<td><p>No</p></td>
<td><p>Not configurable</p></td>
<td><p><strong>OnCategorizedMessage</strong></p></td>
</tr>
<tr class="odd">
<td><p>RMS Decryption Agent</p></td>
<td><p>No</p></td>
<td><p>Not configurable</p></td>
<td><p><strong>OnSubmittedMessage</strong></p></td>
</tr>
<tr class="even">
<td><p>RMS Encryption Agent</p></td>
<td><p>No</p></td>
<td><p>Not configurable</p></td>
<td><p><strong>OnSubmittedMessage</strong>, <strong>OnRoutedMessage</strong></p></td>
</tr>
<tr class="odd">
<td><p>RMS Protocol Decryption Agent</p></td>
<td><p>No</p></td>
<td><p>Not configurable</p></td>
<td><p><strong>OnEndOfData</strong></p></td>
</tr>
</tbody>
</table>

On Edge Transport servers, most of the built-in transport agents are visible and manageable by the transport agent management cmdlets or by other feature-specific cmdlets.

The more interesting built-in transport agents on Edge Transport servers are described in the following table. Note that this table doesn't include invisible or unmanageable transport agents.

### Interesting built-in transport agents on Edge Transport servers

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Agent name</th>
<th>Manageable?</th>
<th>Priority</th>
<th>SMTP or categorizer events</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Connection Filtering Agent</p></td>
<td><p>Yes</p></td>
<td><p>1</p></td>
<td><p><strong>OnConnectEvent</strong>, <strong>OnMailCommand</strong>, <strong>OnRcptComand</strong>, <strong>OnEndOfHeaders</strong></p></td>
</tr>
<tr class="even">
<td><p>Address Rewriting Inbound Agent</p></td>
<td><p>Yes</p></td>
<td><p>2</p></td>
<td><p><strong>OnRcptCommand</strong>, <strong>OnEndOfHeaders</strong></p></td>
</tr>
<tr class="odd">
<td><p>Edge Rule Agent</p></td>
<td><p>Yes</p></td>
<td><p>3</p></td>
<td><p><strong>OnEndOfData</strong></p></td>
</tr>
<tr class="even">
<td><p>Content Filter Agent*</p></td>
<td><p>Yes</p></td>
<td><p>4</p></td>
<td><p><strong>OnEndOfData</strong></p></td>
</tr>
<tr class="odd">
<td><p>Sender ID Agent*</p></td>
<td><p>Yes</p></td>
<td><p>5</p></td>
<td><p><strong>OnEndOfHeaders</strong></p></td>
</tr>
<tr class="even">
<td><p>Sender Filter Agent*</p></td>
<td><p>Yes</p></td>
<td><p>6</p></td>
<td><p><strong>OnMailCommand</strong>, <strong>OnEndOfHeaders</strong></p></td>
</tr>
<tr class="odd">
<td><p>Recipient Filter Agent</p></td>
<td><p>Yes</p></td>
<td><p>7</p></td>
<td><p><strong>OnRcptCommand</strong></p></td>
</tr>
<tr class="even">
<td><p>Protocol Analysis Agent*</p></td>
<td><p>Yes</p></td>
<td><p>8</p></td>
<td><p><strong>OnConnectEvent</strong>, <strong>OnEndOfHeaders</strong>, <strong>OnEndOfData</strong>, <strong>OnReject</strong>, <strong>OnRsetCommand</strong>, <strong>OnDisconnectEvent</strong></p></td>
</tr>
<tr class="odd">
<td><p>Attachment Filtering Agent</p></td>
<td><p>Yes</p></td>
<td><p>9</p></td>
<td><p><strong>OnEndOfData</strong></p></td>
</tr>
<tr class="even">
<td><p>Address Rewriting Outbound Agent</p></td>
<td><p>Yes</p></td>
<td><p>10</p></td>
<td><p><strong>OnSubmittedMessage</strong>, <strong>OnRoutedMessage</strong></p></td>
</tr>
</tbody>
</table>

\* You can also install and configure these anti-spam agents on Mailbox servers. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

## Troubleshoot transport agents

To help you troubleshoot issues with transport agents, you can use the following features:

- **Get-TransportPipeline**: This cmdlet shows the SMTP events and the corresponding transport agents that encounter messages on the Exchange server. For more information, see [View transport agents in the transport pipeline](view-transport-agents-in-the-transport-pipeline-exchange-2013-help.md).

- **Pipeline Tracing**: Pipeline tracing creates an exact snapshot of a message before and after it encounters each transport agent. This allows you to find a transport agent that's causing unexpected results. For more information, see [Pipeline tracing](pipeline-tracing-exchange-2013-help.md).
