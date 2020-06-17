---
localization_priority: Normal
description: Learn how to view, retry, resubmit, suspend, and resume queues in Exchange 2016 and Exchange 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 37f11378-a884-4aff-ab55-689f40a46321
ms.reviewer:
title: Procedures for queues
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Procedures for queues

In Exchange Server, you can use the Queue Viewer in the Exchange Toolbox or the Exchange Management Shell to manage queues. For more information about queues, see [Queues and messages in queues](queues.md).

This topic describes how to perform the following procedures on queues:

- **View queues**

- **Retry queues**: When an Exchange server can't connect to the next hop, the queue is put into a status of Retry, and the server periodically tries to connect and deliver the messages. When you manually retry a queue, you override the scheduled retry time by forcing an immediate connection attempt.

- **Resubmit queues**: Resubmitting a queue is similar to retrying a queue, except the messages are sent back to the Submission queue for the categorizer to process, instead of immediately trying to connect to the next hop. This is useful if changes to your network infrastructure are preventing the messages in the queue from being delivered.

- **Suspend queues**: New messages can enter the queue, and messages that are in the act of being transmitted to the next hop will leave the queue, but otherwise, messages won't leave the queue until the queue is manually resumed.

- **Resume queues**: Restart outgoing message delivery for a queue that has a status of Suspended. When you resume a queue, the status of messages in the queue doesn't change (for example, messages that have a status of Suspended remain suspended and won't leave the queue).

For procedures on messages in queues, see [Procedures for messages in queues](message-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- To find and open the Exchange Toolbox, use one of the following procedures:

  - **Windows 10**: Click **Start** \> **All Apps** \> **Microsoft Exchange Server \<Version\> \>** **Exchange Toolbox**.

  - **Windows Server 2012 R2 or Windows 8.1**: On the Start screen, open the Apps view by clicking the down arrow near the lower-left corner or swiping up from the middle of the screen. The **Exchange Toolbox** shortcut is in a group named **Microsoft Exchange Server \<Version\>**.

  - **Windows Server 2012**: Use any of the following methods:

    - On the Start screen, click an empty area, and type Exchange Toolbox.

    - On the desktop or the Start screen, press Windows key + Q. In the Search charm, type Exchange Toolbox.

    - On the desktop or the Start screen, move your cursor to the upper-right corner, or swipe left from the right edge of the screen to show the charms. Click the Search charm, and type Exchange Toolbox.

    When the shortcut appears in the results, you can select it.

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For more information about using filters and identity values in the Exchange Management Shell, see [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Queues" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## View queues

### Use Queue Viewer to view queues

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, click the **Queues** tab. A list of all queues on the server to which you're connected is displayed.

3. You can use the **Export List** link in the action pane to export the list of queues. For more information, see [How to Export Lists from the Exchange Management Consoles](https://docs.microsoft.com/exchange/export-lists-from-queue-viewer-exchange-2013-help).

### Use the Exchange Management Shell to view queues

To view queues, use the following syntax.

```powershell
Get-Queue [-Filter <Filter> -Server <ServerIdentity> -Include <Internal | External | Empty | DeliveryType> -Exclude <Internal | External | Empty | DeliveryType>]
```

This example displays basic information about all non-empty queues on the server named Mailbox01.

```powershell
Get-Queue -Server Mailbox01 -Exclude Empty
```

This example displays detailed information for all queues on the local Exchange server that contain more than 100 messages.

```powershell
Get-Queue -Filter "MessageCount -gt 100" | Format-List
```

For more information, see [Get-Queue](https://docs.microsoft.com/powershell/module/exchange/get-queue) and [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md).

### Use the Exchange Management Shell to view queue summary information on multiple Exchange servers

The **Get-QueueDigest** cmdlet provides a high-level, aggregate view of the state of queues on all servers within a specific scope (for example, a DAG, an Active Directory site, a list of servers, or the entire Active Directory forest).

By default, the **Get-QueueDigest** cmdlet displays delivery queues that contain ten or more messages, and the results are between one and two minutes old. For instructions on how to change these default values, see [Configure Get-QueueDigest](https://docs.microsoft.com/exchange/configure-get-queuedigest-exchange-2013-help).

 **Notes**:

- Queues on a subscribed Edge Transport server aren't included in the results of **Get-QueueDigest**.

- **Get-QueueDigest** is available on Edge Transport servers, but the results are restricted to local queues on the server.

To view summary information about queues on multiple Exchange servers, run the following command:

```powershell
Get-QueueDigest <-Server <ServerIdentity1,ServerIdentity2...> | -Dag <DagIdentity1,DagIdentity2...> | -Site <ADSiteIdentity1,ADSiteIdentity2...> | -Forest> [-Filter <Filter>]
```

This example displays summary information about the queues on all Exchange 2013 or later Mailbox servers in the Active Directory site named FirstSite where the message count is greater than 100.

```powershell
Get-QueueDigest -Site FirstSite -Filter "MessageCount -gt 100"
```

This example displays summary information about the queues on all Mailbox servers in the database availability group (DAG) named DAG01 where the queue status has the value **Retry**.

```powershell
Get-QueueDigest -Dag DAG01 -Filter "Status -eq 'Retry'"
```

For more information, see [Get-QueueDigest](https://docs.microsoft.com/powershell/module/exchange/get-queuedigest).

## Retry queues

When you retry a delivery queue, you force an immediate connection attempt and override the next scheduled retry time. For more information about the schedule retry time for queues, see [Message retry, resubmit, and expiration intervals](message-intervals.md).

**Notes**:

- The queue must be in a status of Retry for this action to have any effect.

- If the connection isn't successful, the retry interval timer is reset.

### Use Queue Viewer to retry a queue

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, click the **Queues** tab. A list of all queues on the server that you're connected to is displayed.

3. Click **Create Filter**, and enter your filter expression as follows:

   1. Select **Status** from the queue property drop-down list.

   2. Select **Equals** from the comparison operator drop-down list.

   3. Select **Retry** from the value drop-down list.

   4. Click **Apply Filter**. All queues that currently have a **Retry** status are displayed.

   5. Select one or more queues from the list. Right-click, and then select **Retry Queue**. If the connection attempt is successful, the queue status changes to **Active**. If no connection can be made, the queue remains in a status of **Retry** and the next retry time is updated.

### Use the Exchange Management Shell to retry a queue

To retry queues, use the following syntax.

```powershell
Retry-Queue <-Identity QueueIdentity | -Filter QueueFilter [-Server ServerIdentity]>
```

This example retries all queues on the local server with the status of Retry.

```powershell
Retry-Queue -Filter "Status -eq 'Retry'"
```

This example retries the queue named contoso.com on the server named Mailbox01.

```powershell
Retry-Queue -Identity Mailbox01\contoso.com
```

### How do you know this worked?

To verify that you have successfully retried a queue, use either of the following procedures:

- In Queue Viewer, verify the values of the **Status**, **Next Retry Time**, and **Last Error** properties.

- In the Exchange Management Shell, replace _\<QueueIdentity\>_ with the identity of the queue, and use the following syntax to verify the property values:

  ```powershell
  Get-Queue -Identity <QueueIdentity> | Format-Table -Auto Identity,Status,LastRetryTime,NextRetryTime
  ```

## Resubmit queues

Resubmitting a queue sends all messages in the queue back to the Submission queue for the categorizer to process. For more information about the categorizer, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

**Notes**:

- You can't use Queue Viewer to resubmit queues. You can only use the Exchange Management Shell.

- You can resubmit the following queues:

  - A delivery queue that has the status of Retry.

  - The Unreachable queue.

    Any messages in the queue that have the status value of Suspended aren't resubmitted.

- You can't resubmit the poison message queue, but you can resubmit individual messages in the queue. For more information, see the [Resubmit messages in the poison message queue](queue-procedures.md#resubmit-messages-in-the-poison-message-queue) section later in this topic.

- Instead of resubmitting the queue, you can export the messages to .eml files and resubmit them by using the Replay directory on any Exchange server. For more information, see [Export messages from queues](export-messages.md)

### Use the Exchange Management Shell to resubmit queues

To resubmit queues, use the following syntax:

```powershell
Retry-Queue <-Identity QueueIdentity | -Filter "Status -eq 'Retry'" -Server ServerIdentity> -Resubmit $true
```

This example resubmits all messages located in any delivery queues with the status of Retry on the server named Mailbox01.

```powershell
Retry-Queue -Filter "Status -eq 'Retry'" -Server Mailbox01 -Resubmit $true
```

This example resubmits all messages located in the Unreachable queue on the server Mailbox01.

```powershell
Retry-Queue -Identity Mailbox01\Unreachable -Resubmit $true
```

For more information, see [Retry-Queue](https://docs.microsoft.com/powershell/module/exchange/retry-queue).

### How do you know this worked?

To verify that you have successfully resubmitted a queue, use either of the following procedures:

- In Queue Viewer, verify the properties of the queue.

- In the Exchange Management Shell, replace _\<QueueIdentity\>_ with the identity of the queue, and run the following command to verify the property values:

  ```powershell
  Get-Queue -Identity <QueueIdentity>
  ```

### Resubmit messages in the poison message queue

A special case for resubmitting messages is the poison message queue. You can't resubmit the poison message queue like other queues, but you can resubmit individual messages in the poison message queue.

**Notes**:

- Messages in the poison message queue might be genuinely harmful, or they might be valid messages that are the victims of an poorly written transport agent or a software bug. If you're unsure of the safety of the messages in the poison message queue, you should export the messages to files so you can examine them. For more information, see [Export messages from queues](export-messages.md).

- The procedure to resubmit messages from the poison message queue is the same as resuming suspended messages from other queues. You can use Queue Viewer or the Exchange Management Shell. For more information about resuming messages, see [Resume messages in queues](message-procedures.md#Resume).

- The poison message queue is only visible when the queue contains messages.

#### Use Queue Viewer to resubmit messages in the poison message queue

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, click the **Queues** tab. A list of all queues on the server that you're connected to is displayed.

3. Select the poison message queue. In the action pane, select **View Messages**.

4. Select one or more messages from the list, right-click, and select **Resume**.

#### Use the Exchange Management Shell to resubmit messages in the poison message queue

To resubmit a message from the poison message queue, perform the following steps.

1. Find the identity of the message by running the following command on the local server.

   ```powershell
   Get-Message -Queue Poison | Format-Table Identity
   ```

2. Use the identity of the message from the previous step in the following command.

   ```powershell
   Resume-Message <PoisonMessageIdentity>
   ```

   This example resumes a message from the poison message queue that has the message Identity value of 222.

   ```powershell
   Resume-Message 222
   ```

For more information, see [Resume-Message](https://docs.microsoft.com/powershell/module/exchange/resume-message).

### How do you know this worked?

To verify that you have successfully resubmitted a message from the poison message queue, use either of the following procedures to verify that the message is no longer in the queue:

- In Queue Viewer, view the poison message queue where you attempted to resubmit the message.

- In the Exchange Management Shell, run the following command:

  ```powershell
  Get-Message -Queue Poison
  ```

If the message you resubmitted was the only message in the poison message queue, and the queue is no longer visible, that's also an indication of a successful message resubmission.

## Suspend queues

You can suspend a queue to stop mail flow, and then suspend one or more messages in the queue. For more information, see [Suspend messages in queues](message-procedures.md#suspend-messages-in-queues).

**Notes**:

- You can suspend the following queues:

  - A delivery queue that has any status.

  - The Unreachable queue. Until you manually resume this queue, messages are no longer automatically resubmitted to the categorizer when configuration updates are detected.

  - The Submission queue. Until you manually resume this queue, messages aren't picked up by the categorizer.

- Suspending a queue doesn't change the status of the messages in the queue to Suspended.

### Use Queue Viewer to suspend a queue

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, click the **Queues** tab. A list of all queues on the server that you're connected to is displayed. You can create a filter to display only queues that meet specific criteria.

3. Select one or more queues, right-click, and then select **Suspend**.

### Use the Exchange Management Shell to suspend a queue

To suspend a queue, use the following syntax:

```powershell
Suspend-Queue <-Identity QueueIdentity | -Filter "QueueFilter" [-Server ServerIdentity]>
```

This example suspends all queues on the local server that have a message count equal to or greater than 1,000 and that have a status of Retry.

```powershell
Suspend-Queue -Filter "MessageCount -ge 1000 -and Status -eq 'Retry'"
```

This example suspends the queue named contoso.com on the server named Mailbox01.

```powershell
Suspend-Queue -Identity Mailbox01\contoso.com
```

For more information, see [Suspend-Queue](https://docs.microsoft.com/powershell/module/exchange/suspend-queue).

### How do you know this worked?

To verify that you have successfully suspended a queue, use either of the following procedures:

- In Queue Viewer, verify the queue has the **Status** value of Retry.

- In the Exchange Management Shell, replace _\<QueueIdentity\>_ with the identity of the queue, and run the following command to verify the **Status** property value:

  ```powershell
  Get-Queue -Identity <QueueIdentity>
  ```

## Resume queues
<a name="Resume"> </a>

By resuming a queue, you restart outgoing message delivery from a queue that has a status of Suspended.

 **Notes**:

- You can only resume queues that have been suspended.

- Resuming a queue doesn't change the status of messages in the queue. For example, messages that have a status of Suspended remain suspended and don't leave the queue after you resume the queue.

### Use Queue Viewer to resume queues

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, click the **Queues** tab. A list of all queues on the server that you're connected to is displayed.

3. Click **Create Filter**, and enter your filter expression as follows:

    1. Select **Status** from the queue property drop-down list.

    2. Select **Equals** from the comparison operator drop-down list.

    3. Select **Suspended** from the value drop-down list.

4. Click **Apply Filter**. All queues on the server that are currently suspended are displayed.

5. Select one or more queues from the list, right-click, and then select **Resume**.

### Use the Exchange Management Shell to resume queues

To resume queues, use the following syntax:

```powershell
Resume-Queue <-Identity QueueIdentity | -Filter "QueueFilter" [-Server ServerIdentity]>
```

This example resumes all queues on the local server that have a status of Suspended.

```powershell
Resume-Queue -Filter "Status -eq 'Suspended'"
```

This example resumes the suspended delivery queue named contoso.com on the server named Mailbox01.

```powershell
Resume-Queue -Identity Mailbox01\contoso.com
```

For more information, see [Resume-Queue](https://docs.microsoft.com/powershell/module/exchange/resume-queue).

### How do you know this worked?

To verify that you have successfully resumed a queue, use either of the following procedures:

- In Queue Viewer, verify the queue doesn't have the **Status** value Suspended (for example, Active, Connecting, or Ready).

- In the Exchange Management Shell, replace _\<QueueIdentity\>_ with the identity of the queue, and run the following command to verify the **Status** property value:

  ```powershell
  Get-Queue -Identity <QueueIdentity>
  ```
