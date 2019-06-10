---
title: 'Use the Exchange Management Shell to manage queues: Exchange 2013 Help'
TOCTitle: Use the Exchange Management Shell to manage queues
ms:assetid: 5433c1d3-ad2e-4f82-b50d-b67964b32f26
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa998047(v=EXCHG.150)
ms:contentKeyID: 50646233
ms.date: 05/13/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Use the Exchange Management Shell to manage queues

_**Applies to:** Exchange Server 2013_

As in previous versions of Exchange, you can use the Exchange Management Shell in Microsoft Exchange Server 2013 to view information about queues and the messages in those queues, and to perform management actions on queues and messages. In Exchange 2013, queues exist on Mailbox servers and Edge Transport servers. This topic refers to these servers as *transport servers*.

When you use the Shell to view and manage queues and messages in queues on transport servers, it's important to understand how to identify the queues or messages you want to manage. Typically, transport servers contain a large number of queues and messages to be delivered. You use the filtering parameters that are available on the queue and message management cmdlets to identify the queues or messages that you want to view or manage.

Note that you can also use Queue Viewer in the Exchange Toolbox to manage queues and messages in queues. However, the queue and message viewing cmdlets support more filterable properties and filter options than Queue Viewer. For more information about using Queue Viewer, see [Queue Viewer](queue-viewer-exchange-2013-help.md).

## Queue filtering parameters

The following table describes the filtering parameters that are available on the queue management cmdlets.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Cmdlet</th>
<th>Filtering parameters</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Get-Queue</strong></p></td>
<td><p><em>Identity</em></p>
<p><em>Filter</em></p>
<p><em>Include</em></p>
<p><em>Exclude</em></p></td>
<td><p>You can't use the <em>Identity</em> parameter in the same command with the <em>Filter</em> parameters. You can use the <em>Include</em> and <em>Exclude</em> parameters with the <em>Filter</em> parameter in the same command.</p></td>
</tr>
<tr class="even">
<td><p><strong>Resume-Queue</strong></p>
<p><strong>Retry-Queue</strong></p>
<p><strong>Suspend-Queue</strong></p></td>
<td><p><em>Identity</em></p>
<p><em>Filter</em></p></td>
<td><p>You need to use either the <em>Identity</em> parameter or the <em>Filter</em> parameter, but you can't use both in the same command.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Get-QueueDigest</strong></p></td>
<td><p><em>Server</em></p>
<p><em>Dag</em></p>
<p><em>Site</em></p>
<p><em>Forest</em></p>
<p><em>Filter</em></p></td>
<td><p>You need to use the <em>Server</em>, <em>Dag</em>, <em>Site</em>, or <em>Forest</em> parameter, but you can't use any of them together in the same command. You can use the <em>Filter</em> parameter with any of the other filtering parameters.</p></td>
</tr>
</tbody>
</table>

Note that a *Server* parameter is available on all queue management cmdlets. On the **Get-QueueDigest** cmdlet, the *Server* parameter is a scope parameter that specifies the server or servers where you want to view summary information about queues. On all other queue management cmdlets, you use the *Server* parameter to connect to a specific server, and run the queue management commands on that server. You can use the *Server* parameter with or without the *Filter* parameter, but you can't use the *Server* parameter with the *Identity* parameter. You use the transport server's hostname or FQDN with the *Server* parameter.

## Queue identity

The *Identity* parameter on the queue management cmdlets identifies a specific queue. When you use the *Identity* parameter, you can't specify any other queue filtering parameters, because you've already uniquely identified the queue. The *Identity* parameter uses the basic syntax *\<Server\>*\\*\<Queue\>*.

The *\<Server\>* placeholder is the hostname or FQDN of the Exchange server, for example `mailbox01` or `mailbox01.contoso.com`. If you omit the *\<Server\>* qualifier, the local server is implied.

The \<*Queue*\> placeholder accepts one of the following values:

- **Persistent queue name**: Persistent queues have unique, consistent names on all Mailbox or Edge Transport servers. The persistent queue names are:

  - **Submission**: This queue contains messages waiting to be processed by the categorizer.

  - **Unreachable**: This queue contains messages that can't be routed. This queue doesn't exist until messages are placed in it.

  - **Poison**: This queue contains messages that are determined to be harmful to the Exchange server. This queue doesn't exist until messages are placed in it.

- **Delivery queue name**: The name of a delivery queue is the value of the **NextHopDomain** property of the queue. For example, the queue name could be the address space of a Send connector, the name of an Active Directory site, or the name of a DAG. For more information, see the "NextHopSolutionKey" section in the [Queues](queues-exchange-2013-help.md) topic.

- **Queue integer**: Delivery queues and shadow queues are assigned a unique integer value in the queue database. However, you need to run the **Get-Queue** cmdlet to find the integer value for the queue in the **Identity** or **QueueIdentity** properties.

- **Shadow queue name**: A shadow queue uses the syntax `Shadow\`*\<QueueInteger\>*

The following table summarizes the syntax you can use with *Identity* parameter on the queue management cmdlets. In all values, *\<Server\>* is the hostname or FQDN of the server.

### Queue identity formats

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Identity parameter value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>&lt;Server&gt;\&lt;PersistentQueueName&gt;</code> or <code>&lt;PersistentQueueName&gt;</code></p></td>
<td><p>A persistent queue on the specified server or the local server.</p>
<p><em>&lt;PersistentQueueName&gt;</em> is <code>Submission</code>, <code>Unreachable</code>, or <code>Poison</code>.</p></td>
</tr>
<tr class="even">
<td><p><code>&lt;Server&gt;\&lt;NextHopDomain&gt;</code> or <code>&lt;NextHopDomain&gt;</code></p></td>
<td><p>A delivery queue on the specified server or the local server.</p>
<p><em>&lt;NextHopDomain&gt;</em> is a routing destination or delivery group for the messages in the queue. For more information, see the &quot;NextHopSolutionKey&quot; section in the <a href="queues-exchange-2013-help.md">Queues</a> topic.</p></td>
</tr>
<tr class="odd">
<td><p><code>&lt;Server&gt;\&lt;QueueInteger&gt;</code> or <code>&lt;QueueInteger&gt;</code></p></td>
<td><p>A delivery queue on the specified server or the local server.</p>
<p><em>&lt;QueueInteger&gt;</em> is the unique integer value of the queue that's displayed in the <strong>Identity</strong> property of the <strong>Get-Queue</strong> cmdlet.</p></td>
</tr>
<tr class="even">
<td><p><code>&lt;Server&gt;\Shadow\&lt;QueueInteger&gt;</code> or <code>Shadow\&lt;QueueInteger&gt;</code></p></td>
<td><p>A shadow queue on the specified server or the local server.</p></td>
</tr>
<tr class="odd">
<td><p><code>&lt;Server&gt;\*</code> or <code>*</code></p></td>
<td><p>All queues on the specified server or the local server. Note that these values can only be used with the <strong>Get-Queue</strong> cmdlet.</p></td>
</tr>
</tbody>
</table>

## Queue Filter parameter

You can use the *Filter* parameter on the all of the queue management cmdlets to specify the queues you want to view or manage based on the properties of the queues. The *Filter* parameter creates an expression with comparison operators that restricts the queue operation to queues that meet the filter criteria. You can use the `-and` logical operator to specify multiple conditions that the results must match.

For a complete list of queue properties you can use with the *Filter* parameter, see [Queues](queues-exchange-2013-help.md).

For a list of comparison operators you can use with the *Filter* parameter, see the Comparison operators to use when filtering queues or messages section in this topic.

For examples of procedures that use the *Filter* parameter to view and manage queues, see [Manage queues](manage-queues-exchange-2013-help.md).

## Include and Exclude parameters

Exchange 2013 has the *Include* and *Exclude* parameters available on the `Get-Queue` cmdlet. You can use these parameters individually, together, and with the *Filter* parameter to fine-tune your queue results on the local or specified transport server. For example, you can:

- Exclude empty queues from the results.

- Exclude queues to external destinations from the results.

- Include queues that have a specific value of **DeliveryType** in the results.

The *Include* and *Exclude* parameters use the following queue properties to filter queues:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
<th>Shell code example</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>DeliveryType</code></p></td>
<td><p>This value includes or excludes queues based on the <strong>DeliveryType</strong> property. You can specify multiple values separated by commas. The valid values for <strong>DeliveryType</strong> are explained in the &quot;NextHopSolutionKey&quot; section in the topic <a href="queues-exchange-2013-help.md">Queues</a> topic.</p></td>
<td><p>This example returns all delivery queues on the local server where the next hop is a Send connector on the local server that's configured for smart host routing:</p>
<p><code>Get-Queue -Include SmartHostConnectorDelivery</code></p></td>
</tr>
<tr class="even">
<td><p><code>Empty</code></p></td>
<td><p>This value includes or excludes empty queues. Empty queues have the value <code>0</code> in the <strong>MessageCount</strong> property.</p></td>
<td><p>This example returns all queues on the local server that contain messages</p>
<p><code>Get-Queue -Exclude Empty</code></p></td>
</tr>
<tr class="odd">
<td><p><code>External</code></p></td>
<td><p>This value includes or excludes queues that have the value <code>External</code> in the <strong>NextHopCategory</strong> property.</p>
<p>External queues always have one of the following values for <strong>DeliveryType</strong>:</p>
<ul>
<li><p><code>DeliveryAgent</code></p></li>
<li><p><code>DnsConnectorDelivery</code></p></li>
<li><p><code>NonSmtpGatewayDelivery</code></p></li>
<li><p><code>SmartHostConnectorDelivery</code></p></li>
</ul>
<p>For more information, see the &quot;NextHopSolutionKey&quot; section in the <a href="queues-exchange-2013-help.md">Queues</a> topic.</p></td>
<td><p>This example returns all internal queues on the local server</p>
<p><code>Get-Queue -Exclude External</code></p></td>
</tr>
<tr class="even">
<td><p><code>Internal</code></p></td>
<td><p>This value includes or excludes queues that have the value <code>Internal</code> in the <strong>NextHopCategory</strong> property. For more information, see the &quot;NextHopSolutionKey&quot; section in the <a href="queues-exchange-2013-help.md">Queues</a> topic.</p></td>
<td><p>This example returns all internal queues on the local server.</p>
<p><code>Get-Queue -Include Internal</code></p></td>
</tr>
</tbody>
</table>

Note that you can duplicate the functionality of the *Include* and *Exclude* parameters by using the *Filter* parameter. For example, the command `Get-Queue -Exclude Empty` yields the same result as `Get-Queue -Filter {MessageCount -gt 0}`. However, the syntax of the *Include* and *Exclude* parameters is simpler and easier to remember.

## Get-QueueDigest

Exchange 2013 adds a new queue cmdlet named **Get-QueueDigest**. This cmdlet allows you to view information about some or all of the queues in your Exchange organization by using a single command. Specifically, the **Get-QueueDigest** cmdlet allows you to view information about queues based on their location on servers, in DAGs, in Active Directory sites, or in the whole Active Directory forest. Note that queues on a subscribed Edge Transport server in the perimeter network aren't included in the results. Also, **Get-QueueDigest** is available on an Edge Transport server, but the results are restricted to queues on the Edge Transport server.

> [!NOTE]
> By default, the <STRONG>Get-QueueDigest</STRONG> cmdlet displays delivery queues that contain ten or more messages, and the results are between one and two minutes old. For instructions on how to change these default values, see <A href="configure-get-queuedigest-exchange-2013-help.md">Configure Get-QueueDigest</A>.

The filtering and sorting parameters that are available with the **Get-QueueDigest** cmdlet are described in the following table.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Dag</em>, <em>Server</em>, or <em>Site</em></p></td>
<td><p>These parameters are mutually exclusive, and set the scope for the cmdlet. You need to specify one of these parameters or the <em>Forest</em> switch. Typically, you would use the name of the server, DAG or Active Directory site, but you can use any value that uniquely identifies the server, DAG, or site. You can specify multiple servers, DAGs, or sites separated by commas.</p></td>
</tr>
<tr class="even">
<td><p><em>Forest</em></p></td>
<td><p>This switch is required if you aren't using the <em>Dag</em>, <em>Server</em>, or <em>Site</em> parameters. You don't specify a value with this switch. By using this switch, you get queues from all Exchange 2013 Mailbox servers in the Active Directory forest. You can't use the <em>Forest</em> switch to view queues in remote Active Directory forests.</p></td>
</tr>
<tr class="odd">
<td><p><em>DetailsLevel</em></p></td>
<td><p>This parameter accepts the values <code>None</code>, <code>Normal</code>, and <code>Verbose</code>. The default value is <code>Normal</code>. When you use the value <code>None</code>, the queue name is omitted from the <strong>Details</strong> column in the results.</p></td>
</tr>
<tr class="even">
<td><p><em>Filter</em></p></td>
<td><p>This parameter allows you to filter queues based on the queue properties. You can use any of the filterable queue properties as described in the <a href="queue-filters-exchange-2013-help.md">Queue filters</a> topic.</p></td>
</tr>
<tr class="odd">
<td><p><em>GroupBy</em></p></td>
<td><p>This parameter groups the queue results. You can group the results by one of the following properties:</p>
<ul>
<li><p>DeliveryType</p></li>
<li><p>LastError</p></li>
<li><p>NextHopCategory</p></li>
<li><p>NextHopDomain</p></li>
<li><p>NextHopKey</p></li>
<li><p>Status</p></li>
<li><p>ServerName</p></li>
</ul>
<p>By default, the results are grouped by <code>NextHopDomain</code>. For information about these queue properties, see <a href="queue-filters-exchange-2013-help.md">Queue filters</a>.</p></td>
</tr>
<tr class="even">
<td><p><em>ResultSize</em></p></td>
<td><p>This parameter limits the queue results to the value you specify. The queues are sorted in descending order based on the number of messages in the queue, and grouped by the value specified by the <em>GroupBy</em> parameter. The default value is 1000. This means that by default, the command displays the top 1000 queues grouped by <strong>NextHopDomain</strong>, and sorted by the queues containing the most messages to the queues containing the least messages.</p></td>
</tr>
<tr class="odd">
<td><p><em>Timeout</em></p></td>
<td><p>The parameter specifies the number of seconds before the operation times out. The default value is <code>00:00:10</code> or 10 seconds.</p></td>
</tr>
</tbody>
</table>

This example returns all non-empty external queues on the Exchange 2013 Mailbox servers named Mailbox01,Mailbox02, and Mailbox03.

```powershell
Get-QueueDigest -Server Mailbox01,Mailbox02,Mailbox03 -Include External -Exclude Empty
```

## Message filtering parameters

The following table describes the filtering parameters that are available on the message management cmdlets.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Cmdlet</th>
<th>Filtering parameters</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Get-Message</strong></p></td>
<td><p><em>Identity</em></p>
<p><em>Filter</em></p>
<p><em>Queue</em></p></td>
<td><p>All filtering parameters are mutually exclusive, and you can use them together in the same command.</p></td>
</tr>
<tr class="even">
<td><p><strong>Remove-Message</strong></p>
<p><strong>Resume-Message</strong></p>
<p><strong>Suspend-Message</strong></p></td>
<td><p><em>Identity</em></p>
<p><em>Filter</em></p></td>
<td><p>You need to use either the <em>Identity</em> parameter or the <em>Filter</em> parameter, but you can't use both in the same command.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Export-Message</strong></p></td>
<td><p><em>Identity</em></p></td>
<td><p>The <em>Identity</em> parameter is required.</p></td>
</tr>
</tbody>
</table>

Note that a *Server* parameter is available on all message management cmdlets except for the **Export-Message** cmdlet. You use the *Server* parameter to connect to a specific server, and run the message management commands on that server. You can use the *Server* parameter with or without the *Filter* parameter, but you can't use the *Server* parameter with the *Identity* parameter. You use the transport server's hostname or FQDN with the *Server* parameter.

## Message identity

The *Identity* parameter on the message management cmdlets identifies a specific message in one or more queues. When you use the *Identity* parameter, you can't specify any other message filtering parameters, because you've already uniquely identified the message. The *Identity* parameter uses the basic syntax *\<Server\>*\\*\<Queue\>*\\*\<MessageInteger\>*.

The *\<Server\>* placeholder is the hostname or FQDN of the Exchange server, for example `mailbox01` or `mailbox01.contoso.com`. If you omit the *\<Server\>* qualifier, the local server is implied.

The \<*Queue*\> placeholder accepts the identity of the queue as described in the "Queue identity" section in this topic. For example, you can use the persistent queue name, the **NextHopDomain** value, or the unique integer value of the queue in the queue database.

The *\<MessageInteger\>* placeholder represents the unique integer value that's assigned to the message when it first enters the queue database on the server. If the message is sent to multiple recipients that require multiple queues, all copies of the message in all queues in the queue database have the same integer value. However, you need to run the **Get-Message** cmdlet to find the integer value for the message in the **Identity** or **MessageIdentity** properties.

The following table summarizes the syntax you can use with *Identity* parameter on the message management cmdlets. In all values, *\<Server\>* is the hostname or FQDN of the server.

### Message identity formats

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Identity parameter value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>&lt;Server&gt;\&lt;Queue&gt;\&lt;MessageInteger&gt;</code> or <code>&lt;Queue&gt;\&lt;MessageInteger&gt;</code></p></td>
<td><p>A message in a specific queue on the specified server or the local server.</p>
<p><em>&lt;MessageInteger&gt;</em> is the unique integer value of the message that's displayed in the <strong>Identity</strong> property of the <strong>Get-Message</strong> cmdlet.</p>
<p><em>&lt;Queue&gt;</em> represents one of the following values:</p>
<ul>
<li><p><strong>Persistent queue name</strong>   The value <code>Submission</code>, <code>Unreachable</code>, or <code>Poison</code>.</p></li>
<li><p><strong>Delivery queue name</strong>   The value of the <strong>NextHopDomain</strong> property of the queue, which is effectively the name of the queue. This value could be a routing destination or a delivery group. For more information, see the &quot;NextHopSolutionKey&quot; section in the <a href="queues-exchange-2013-help.md">Queues</a> topic.</p></li>
<li><p><strong>Queue integer</strong>   The unique integer value of the delivery queue or shadow queue that's displayed in the <strong>Identity</strong> property of the <strong>Get-Message</strong> or <strong>Get-Queue</strong> cmdlets.</p></li>
<li><p><strong>Shadow queue identity</strong>   The shadow queue identity uses the syntax <code>Shadow\&lt;QueueInteger&gt;</code>.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><code>&lt;Server&gt;\*\&lt;MessageInteger&gt;</code> or <code>*\&lt;MessageInteger&gt;</code> or <code>&lt;MessageInteger&gt;</code></p></td>
<td><p>All copies of the message in all queues in the queue database on the specified server or the local server.</p></td>
</tr>
</tbody>
</table>

## Message Filter parameter

You can use the *Filter* parameter on the **Get-Message**, **Remove-Message**, **Resume-Message**, and **Suspend-Message** cmdlets to specify the messages you want to view or manage based on the properties of the messages. The *Filter* parameter creates an expression with comparison operators that restricts the message operation to messages that meet the filter criteria. You can use the `-and` logical operator to specify multiple conditions that the results must match.

For a complete list of message properties you can use with the *Filter* parameter, see [Queues](queues-exchange-2013-help.md).

For a list of comparison operators you can use with the *Filter* parameter, see the Comparison operators to use when filtering queues or messages section in this topic.

For examples of procedures that use the *Filter* parameter to view and manage messages, see [Manage queues](manage-queues-exchange-2013-help.md).

## Queue parameter

The *Queue* parameter is used only with the **Get-Message** cmdlet. You can use this parameter to get all messages in a specific queue, or all messages from multiple queues by using the wildcard character (\*).When you use the *Queue* parameter, use the queue identity format *\<Server\>*\\*\<Queue\>* as described in the "Queue identity" section in this topic.

## Comparison operators to use when filtering queues or messages

When you create a queue or message filter expression by using the *Filter* parameter, you need to include an comparison operator for the property value to match. The following table shows the comparison operators that you can use in a filter expression and how each operator functions. For all operators, the values compared aren't case sensitive.

### Comparison operators

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Operator</th>
<th>Function</th>
<th>Shell code example</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>-eq</code></p></td>
<td><p>This operator is used to specify that the results must exactly match the property value that's supplied in the expression.</p></td>
<td><p>To display a list of all queues that have a status of Retry:</p>
<p><code>Get-Queue -Filter {Status -eq &quot;Retry&quot;}</code></p>
<p>To display a list of all messages that have a status of Retry:</p>
<p><code>Get-Message -Filter {Status -eq &quot;Retry&quot;}</code></p></td>
</tr>
<tr class="even">
<td><p><code>-ne</code></p></td>
<td><p>This operator is used to specify that the results shouldn't match the property value that's supplied in the expression.</p></td>
<td><p>To display a list of all queues that don't have a status of Active:</p>
<p><code>Get-Queue -Filter {Status -ne &quot;Active&quot;}</code></p>
<p>To display a list of all messages that don't have a status of Active:</p>
<p><code>Get-Message -Filter {Status -ne &quot;Active&quot;}</code></p></td>
</tr>
<tr class="odd">
<td><p><code>-gt</code></p></td>
<td><p>This operator is used with properties where the value is expressed as an integer or date/time. The filter results only include queues or messages where the value of the specified property is greater than the value that's supplied in the expression.</p></td>
<td><p>To display a list of queues that currently contain more than 1,000 messages:</p>
<p><code>Get-Queue -Filter {MessageCount -gt 1000}</code></p>
<p>To display a list of messages that currently have a retry count that's more than 3:</p>
<p><code>Get-Message -Filter {RetryCount -gt 3}</code></p></td>
</tr>
<tr class="even">
<td><p><code>-ge</code></p></td>
<td><p>This operator is used with properties where the value is expressed as an integer or date/time. The filter results only include queues or messages where the value of the specified property is greater than or equal to the value that's supplied in the expression.</p></td>
<td><p>To display a list of queues that currently contain 1,000 or more messages:</p>
<p><code>Get-Queue -Filter {MessageCount -ge 1000}</code></p>
<p>To display a list of messages that currently have a retry count that's 3 or more:</p>
<p><code>Get-Message -Filter {RetryCount -ge 3}</code></p></td>
</tr>
<tr class="odd">
<td><p><code>-lt</code></p></td>
<td><p>This operator is used with properties where the value is expressed as an integer or date/time. The filter results only include queues or messages where the value of the specified property is less than the value that's supplied in the expression.</p></td>
<td><p>To display a list of queues that currently contain less than 1,000 messages:</p>
<p><code>Get-Queue -Filter {MessageCount -lt 1000}</code></p>
<p>To display a list of messages that have an SCL that's less than 6:</p>
<p><code>Get-Message -Filter {SCL -lt 6}</code></p></td>
</tr>
<tr class="even">
<td><p><code>-le</code></p></td>
<td><p>This operator is used with properties where the value is expressed as an integer or date/time. The filter results only include queues or messages where the value of the specified property is less than or equal to the value supplied in the expression.</p></td>
<td><p>To display a list of queues that currently contain 1,000 or fewer messages:</p>
<p><code>Get-Queue -Filter {MessageCount -le 1000}</code></p>
<p>To display a list of messages that have an SCL that's 6 or less:</p>
<p><code>Get-Message -Filter {SCL -le 6}</code></p></td>
</tr>
<tr class="odd">
<td><p><code>-like</code></p></td>
<td><p>This operator is used with properties where the value is expressed as a text string. The filter results only include queues or messages where the value of the specified property contains the text string that's supplied in the expression. You can include the wildcard character (*) in a <strong>-like</strong> expression that's applied to a text string field, but not with a field that has the enumeration type.</p></td>
<td><p>To display a list of delivery queues that have a destination to any SMTP domain that ends in Contoso.com:</p>
<p><code>Get-Queue -Filter {Identity -like &quot;*contoso.com&quot;}</code></p>
<p>To display a list of messages that have a subject that contains the text &quot;payday loan&quot;:</p>
<p><code>Get-Messages -Filter {Subject -like &quot;*payday loan*&quot;}</code></p></td>
</tr>
</tbody>
</table>

You can specify a filter that evaluates multiple expressions by using the **-and** comparison operator. The queues or messages must meet all conditions of the filter to be included in the results.

This example displays a list of queues that have a destination to any SMTP domain name that ends in Contoso.com and that currently contain more than 500 messages.

```powershell
Get-Queue -Filter {Identity -like "*contoso.com*" -and MessageCount -gt 500}
```

This example displays a list of messages that are sent from any email address in the contoso.com domain that have an SCL that's greater than 5.

```powershell
Get-Message -Filter {FromAddress -like "*Contoso.com*" -and SCL -gt 5}
```

## Advanced paging parameters

Depending on current mail flow, queries against queues and messages can return a large set of objects. You can use the advanced paging parameters to control how query results are retrieved and displayed.

When you use the Shell to view queues and the messages in the queues, your query retrieves one page of information at a time. The advanced paging parameters control the size of the result set and can also be used to sort the results. All advanced paging parameters are optional and can be combined with any one of the parameter sets that can be used with the **Get-Queue** and **Get-Message** cmdlets. If you don't specify any advanced paging parameters, the query returns the results in ascending order of identity.

By default, when a sort order is specified, the message identity property is always included and is sorted in an ascending order. This is the default ordering relationship. The message identity property is included because the other properties that can be included in a sort order aren't unique. By explicitly including the message identity property in the sort order, you can specify that the results display the message identity sorted in descending order.

You can use the *BookmarkIndex* and *BookmarkObject* parameters to mark a position in the sorted result set. If the bookmark object no longer exists when the next page of results is retrieved, the default ordering relationship makes sure that the result set starts with the closest object to the bookmark. The closest object depends on the specified sort order.

The following table describes the advanced paging parameters.

### Advanced paging parameters

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>BookmarkIndex</em></p></td>
<td><p>This parameter specifies the position in the result set where the displayed results start. The value of this parameter is a 1-based index in the total result set. If the value is less than or equal to zero, the first complete page of results is returned. If the value is set to <code>Int.MaxValue</code>, the last complete page of results is returned.</p></td>
</tr>
<tr class="even">
<td><p><em>BookmarkObject</em></p></td>
<td><p>This parameter specifies the object in the result set where the displayed results start. If you specify a bookmark object, that object is used as the point to start the search. The rows before or after that object, depending on the value of the <em>SearchForward</em> parameter, are retrieved. You can't combine the <em>BookmarkObject</em> parameter and the <em>BookmarkIndex</em> parameter in a single query.</p></td>
</tr>
<tr class="odd">
<td><p><em>IncludeBookmark</em></p></td>
<td><p>This parameter specifies whether to include the bookmark object in the result set. By default, the value is set to <code>$true</code> and the bookmark object is included. You may run a query for a limited result size, and then specify the last item in that result set as the bookmark for the next query. In this case, you may want to set <em>IncludeBookmark</em> to <code>$false</code> so that the object isn't included in both result sets.</p></td>
</tr>
<tr class="even">
<td><p><em>ResultSize</em></p></td>
<td><p>This parameter specifies the number of results to display per page. If you don't specify a value, the default result size of 1,000 objects is used. Exchange limits the result set to 250,000.</p></td>
</tr>
<tr class="odd">
<td><p><em>ReturnPageInfo</em></p></td>
<td><p>This parameter is a hidden parameter. It returns information about the total number of results and the index of the first object of the current page. The default value is <code>$false</code>.</p></td>
</tr>
<tr class="even">
<td><p><em>SearchForward</em></p></td>
<td><p>This parameter specifies whether to search forward or backward in the result set. This parameter doesn't affect the order in which the result set is returned. It determines the direction of search relative to the bookmark index or object. If no bookmark index or object is specified, the <em>SearchForward</em> parameter determines whether the search starts from the first or last object in the result set.</p>
<p>The default value for this parameter is <code>$true</code>. If the this parameter is set to <code>$true</code> and a bookmark is specified, the query searches forward from that bookmark. If you use this configuration and there are no results beyond the bookmark, the query returns the last full page of results.</p>
<p>If the <em>SearchForward</em> parameter is set to <code>$false</code> and a bookmark is specified, the query searches backward from that bookmark. If you use this configuration and there is less than a full page of results beyond the bookmark, the query returns the first full page of results.</p></td>
</tr>
<tr class="odd">
<td><p><em>SortOrder</em></p></td>
<td><p>This parameter specifies an array of message properties used to control the sort order of the result set. The sort order properties are specified in descending order of precedence. Each property is separated by a comma and appended with a plus sign (+) to sort in ascending order, or a minus sign (-) to sort in descending order.</p>
<p>If an explicit sort order isn't specified by using this parameter, the records that match the query are displayed and sorted by the Identity field for the respective object type. The results are always sorted by identity in ascending order when a sort order isn't explicitly specified.</p></td>
</tr>
</tbody>
</table>

The following code example shows how to use the advanced paging parameters in a query. In this example, the command connects to the specified server and retrieves a result set that contains 500 objects. The results are displayed in a sorted order, first in ascending order by sender address, and then in descending order of message size.

`Get-Message -Server mailbox01.contoso.com -ResultSize 500 -SortOrder +FromAddress,-Size`

If you want to view successive pages, you can set a bookmark for the last object retrieved in a result set and run an additional query. You need to use the scripting capabilities of the Shell to perform this procedure.

The following example uses scripting to retrieve the first page of results, sets the bookmark object, excludes the bookmark object from the result set, and then retrieves the next 500 objects on the specified server.

1. Open the Shell and type the following command to retrieve the first page of results.

    ```powershell
    $Results=Get-message -Server mailbox01.contoso.com -ResultSize 500 -SortOrder +FromAddress,-Size
    ```

2. To set the bookmark object, type the following command to save the last element of the first page to a variable.

    ```powershell
    $temp=$results[$results.length-1]
    ```

3. To retrieve the next 500 objects on the specified server and to exclude the bookmark object, type the following command.

    ```powershell
    Get-message -Server mailbox01.contoso.com -BookmarkObject:$temp -IncludeBookmark $False -ResultSize 500 -SortOrder +FromAddress,-Size
    ```
