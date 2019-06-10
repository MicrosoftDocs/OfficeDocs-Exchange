---
localization_priority: Normal
description: 'Summary: Learn how to use the Queue Viewer in the Exchange Toolbox to view message properties in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: chrisda
f1_keywords:
- Microsoft.Exchange.Management.Edge.SystemManager.MessagePropertyPage
ms.author: chrisda
ms.assetid: 9d15d8b8-e061-4288-9354-df58e282fb6b
ms.date: 7/11/2018
ms.reviewer: 
title: View queued message properties in Queue Viewer
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# View queued message properties in Queue Viewer

You can use the Queue Viewer in the Exchange Toolbox to view queues and the properties of messages in queues. In Exchange Server 2016 and Exchange Server 2019, Queue Viewer is available on Mailbox servers and Edge Transport servers.

For more information about queues, see [Queues and messages in queues](queues.md). For more information about Queue Viewer, see [Queue Viewer](queue-viewer.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Queues" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- To find and open the Exchange Toolbox, use one of the following procedures:

  - **Windows 10**: Click **Start** \> **All Apps** \> **Microsoft Exchange Server \<Version\> \>** **Exchange Toolbox**.

  - **Windows Server 2012 R2 or Windows 8.1**: On the Start screen, open the Apps view by clicking the down arrow near the lower-left corner or swiping up from the middle of the screen. The **Exchange Toolbox** shortcut is in a group named **Microsoft Exchange Server \<Version\>**.

  - **Windows Server 2012**: Use any of the following methods:

    - On the Start screen, click an empty area, and type Exchange Toolbox.

    - On the desktop or the Start screen, press Windows key + Q. In the Search charm, type Exchange Toolbox.

    - On the desktop or the Start screen, move your cursor to the upper-right corner, or swipe left from the right edge of the screen to show the charms. Click the Search charm, and type Exchange Toolbox.

    When the shortcut appears in the results, you can select it.

- You can also use the **Get-Message** cmdlet in the Exchange Management Shell to view additional message properties that aren't visible in Queue Viewer. For more information, see [Properties of messages in queues](message-properties.md) and [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Queue Viewer to view the properties of a message

1. In the **Exchange Toolbox**, in the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

2. In Queue Viewer, select the **Messages** tab to see the list of messages that are currently queued for delivery in your organization. The list of messages displays the following information:

  - **From Address**: The sender's email address.

  - **Status**: A message can have one of the following status values:

  - **Active**: If the message is in a delivery queue, the message is being delivered to its destination. If the message is in the Submission queue, the message is being processed by the categorizer.

  - **Pending Remove**: The message was deleted by the administrator, but was already being delivered. The message will be deleted if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

  - **Pending Suspend**: The message was suspended by the administrator, but was already being delivered. The message will be suspended if the delivery ends in an error that causes the message to re-enter the queue. Otherwise, delivery will continue.

  - **Ready**: The message is waiting in the queue, and is ready to be processed.

  - **Retry**: The queue's last connection attempt failed. The message is waiting for the next queue retry.

  - **Suspended**: The message was suspended by the administrator. For more information, see [Suspend messages in queues](message-procedures.md#Suspend).

  - **Size (KB)**: The size of the message rounded up to the nearest kilobyte (KB).

  - **SCL**: The spam confidence level (SCL) rating of the message. Valid SCL entries are integers 0 through 9, or -1 for internal (authenticated) messages. For more information, see [Exchange spam confidence level (SCL) thresholds](../../antispam-and-antimalware/antispam-protection/scl.md).

  - **Queue ID**: The queue that holds the message. The queue identity uses the syntax _\<Server\>_\ _\<Queue\>_, where _\<Queue\>_ is one of the following values:

  - **Persistent queue name**

  - **Poison**: Isolates messages that contain errors and are determined to be harmful to Exchange after a server or service failure. The messages may be genuinely harmful in their content and format, or the messages might have been the victims of a poorly written transport agent or a software bug that crashed the Exchange server while it was processing the otherwise valid messages.

  - **Submission**: Holds messages that have been accepted by the Transport service, but haven't been processed. Messages in the Submission queue are either waiting to be processed, or are actively being processed.

  - **Unreachable**: Contains messages that can't be routed to their destinations. Typically, an unreachable destination is caused by configuration changes that have modified the routing path for delivery. Regardless of destination, all messages that have unreachable recipients reside in this queue.

  - **Delivery queue name**: The value of the **NextHopDomain** property of the queue, which is effectively the name of the queue. For example, a domain name, Active Directory site name, or database availability group (DAG) name. For more information, see [NextHopSolutionKey](queues.md#nexthopsolutionkey).

  - **Message Source Name**: The Exchange component that submitted the message to the queue. For example, `SMTP:Default <ServerName>`.

  - **Subject**: The subject of the message.

  - **Last Error**: The last error that was encountered for the message.

    For example, if you didn't create a Send connector to deliver Internet mail, messages that are addressed to external recipients will go to the Unreachable queue, and the **Last Error** value for the message will be: `A matching connector cannot be found to route the external recipient`. For more information about creating a Send connector, see [Create a Send connector to send mail to the Internet](../../mail-flow/connectors/internet-mail-send-connectors.md).

    For more information about SMTP error codes, see [DSNs and NDRs in Exchange Server](../../mail-flow/non-delivery-reports-and-bounce-messages/non-delivery-reports-and-bounce-messages.md).

3. When you right-click a message and select **Properties**, additional details are available on the **General** and **Recipient Information** tabs.

  - The **General** tab displays the same **Subject**, **From** **Address**, **Status**, **Size**, **Message Source Name**, **SCL**, and **Last Error** values that are shown in the list of messages. The following additional properties are also displayed on the **General** tab:

  - **Identity**: The identity of the message. The message identity uses the syntax _\<Server\>_\ _\<Queue\>_\ _\<MessageInteger\>_, where _\<Queue\>_ is the identity of the queue as described in the **Queue ID** property, and _\<MessageInteger\>_ is the unique integer value of the message that's displayed in the **Identity** property of the **Get-Message** cmdlet.

  - **Internet Message ID**: The value of the **Message-Id:** header field in the message header. This value is constant for the lifetime of the message. For messages created in Exchange, the value is in the format `<GUID@ServerFQDN>`, including the angle brackets (\< \>). For example, `<4867a3d78a50438bad95c0f6d072fca5@mailbox01.contoso.com>`.

  - **Source IP**: The IPv4 or IPv6 address of the internal Exchange server or external messaging server that submitted the message.

  - **Date Received**: The date-time when the message entered the queue.

  - **Expiration Time**: The date-time when the message will expire and will be deleted from the queue if the message can't be delivered.

  - **Recipients**: The recipients in the message, with any corresponding error messages. To see the status and error messages for each recipient, go to the **Recipient Information** tab.

  - The **Recipient Information** tab displays the **Address**, **Status**, and **Last Error** values for each recipient in the message. The **Status** value for a recipient can be **Complete**, **Ready**, or **Retry**.
