---
title: 'View queued message properties in Queue Viewer: Exchange 2013 Help'
TOCTitle: View queued message properties in Queue Viewer
ms:assetid: 9d15d8b8-e061-4288-9354-df58e282fb6b
ms:mtpsurl: https://technet.microsoft.com/library/Bb123934(v=EXCHG.150)
ms:contentKeyID: 49286850
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.Edge.SystemManager.MessagePropertyPage
---

# View queued message properties in Queue Viewer

_**Applies to:** Exchange Server 2013_

You can use the Queue Viewer in the Exchange Toolbox to view the properties of a message that is queued for delivery.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Queues" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can also use the Get-Message cmdlet in the Exchange Management Shell to view additional message properties that aren't visible in Queue Viewer. For more information, see [Message filters](message-filters-exchange-2013-help.md) and [Use the Exchange Management Shell to manage queues](use-the-exchange-management-shell-to-manage-queues-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## What Do You Want to Do?

## Use Queue Viewer in the Exchange Toolbox to view the properties of a message

1. Click **Start** \> **All Programs** \> **Microsoft Exchange 2013** \> **Exchange Toolbox**.

2. In the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

3. In Queue Viewer, select the **Messages** tab to see the list of messages that are currently queued for delivery in your organization.

4. Right-click the message whose properties you want to view and then select **Properties**.

5. The **General** tab displays the following detailed information about the message:

   - **Identity**: This field shows the integer that represents a particular message. The message identity is assigned by the queuing database when the message is received for processing. You can include an optional server and queue identity to identify a unique instance of the message.

   - **Subject**: This field shows the subject of a message and is expressed as a text string. The value is taken from the `Subject:` header field.

   - **Internet Message ID**: This field shows the value of the `MessageID:` header field. The value of this property is expressed as a GUID followed by the SMTP address of the sending server, as in this example: 67D754D6103DC4FB3BA6BC7205DACABA61231@exchange.contoso.com

   - **From Address**: This field shows the SMTP address of the sender of the message. This value is taken from MAIL FROM: in the message envelope.

   - **Status**: This field shows the current message status. A message can have one of the following status values:

     - **Active**: If the message is in a delivery queue, the message is being delivered to its destination. If the message is in the Submission queue, the message is being processed by the categorizer.

     - **Pending Remove**: The message was deleted by the administrator but was already in delivery. The message will be deleted if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

     - **Pending Suspend**: The message was suspended by the administrator but was already in delivery. The message will be suspended if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

     - **Ready**: The message is waiting in the queue and is ready to be processed.

     - **Retry**: The last connection attempt failed for the queue in which this message is located. The message is waiting for the next queue retry.

     - **Suspended**: The message was suspended by the administrator.

   - **Size (KB)**: This field shows the size of the message rounded up to the nearest kilobyte (KB).

   - **Message Source Name**: This field shows the name of the component that submitted this message to the queue.

   - **Source IP**: This field shows the IP address of the external server that submitted the message to the Exchange organization.

   - **SCL**: This field shows the spam confidence Level (SCL) rating of the message. Valid SCL entries are integers 0 through 9 or -1. An empty SCL entry indicates that the message hasn't been processed by the Content Filter agent.

   - **Date Received**: This field shows the date-time when the message was received by the server that holds the queue in which the message is located.

   - **Expiration Time**: This field shows the date-time when the message will expire and will be deleted from the queue if the message can't be delivered.

   - **Last Error**: This field shows the last error that was recorded for a message.

   - **Queue ID**: This field shows the identity of the queue that holds the message. The queue identity is expressed in the form *Server\\destination*, where *destination* is a delivery group, routing destination, persistent queue name, or the queue database identifier. The queue database identifier is represented as an integer and can be determined by viewing the message properties.

   - **Recipients**: This field shows the list of recipients to which the message is addressed.

   - **Retry Count**: This field shows the number of times that delivery of a message to a destination was tried.

6. The **Recipient Information** tab displays the following information about the message recipients:

   - **Address**: This field shows the SMTP address of the recipient of the message. This value is taken from `RCPT TO:` in the message envelope.

   - **Status**: This field shows the current message status. A message can have one of the following status values:

     - **Active**: If the message is in a delivery queue, the message is being delivered to its destination. If the message is in the Submission queue, the message is being processed by the categorizer.

     - **Pending Remove**: The message has been deleted by the administrator but was already in delivery. The message will be deleted if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

     - **Pending Suspend**: The message has been suspended by the administrator but was already in delivery. The message will be suspended if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

     - **Ready**: The message is waiting in the queue and is ready to be processed.

     - **Retry**: The last connection attempt failed for the queue in which this message is located. The message is waiting for the next queue retry.

     - **Suspended**: The message has been suspended by the administrator.

   - **Last Error**: This field shows the last error that was recorded for a message.

## Use the Shell to view the properties of a message

You use the **Get-Message** cmdlet to view the properties of a message that is currently queued for delivery. The following example tabulates the sender address, recipients, subject, and received date information for all messages that are currently in retry state:

```powershell
Get-Message -IncludeRecipientInfo -Filter "Status -eq 'Retry'" | Format-Table FromAddress,Recipients,Subject,DateReceived
```

For detailed syntax and parameter information, see [Get-Message](https://docs.microsoft.com/powershell/module/exchange/Get-Message).
