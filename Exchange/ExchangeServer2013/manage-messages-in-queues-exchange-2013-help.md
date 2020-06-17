---
title: 'Manage messages in queues: Exchange 2013 Help'
TOCTitle: Manage messages in queues
ms:assetid: 83358884-6036-4e91-87a8-35200541874d
ms:mtpsurl: https://technet.microsoft.com/library/Bb123535(v=EXCHG.150)
ms:contentKeyID: 50646238
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage messages in queues

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can use the Queue Viewer in the Exchange Toolbox or the Exchange Management Shell to manage messages in queues. For more information about using the message management cmdlets in the Exchange Management Shell, see [Use the Exchange Management Shell to manage queues](use-the-exchange-management-shell-to-manage-queues-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Queues" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Remove messages from queues

A message that's being sent to multiple recipients might be located in more than one queue. To remove a message from more than one queue in a single operation, you need to use a filter. You can select whether to send a non-delivery report (NDR) when you remove messages from a queue. You can't remove a message from the Submission queue.

## Use Queue Viewer in the Exchange Toolbox to remove messages

1. Click **Start** \> **All Programs** \> **Microsoft Exchange 2013** \> **Exchange Toolbox**.

2. In the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

3. In Queue Viewer, click the **Messages** tab. A list of all messages on the server to which you're connected is displayed. To adjust the action to a single queue, click the **Queues** tab, double-click the queue name, and then click the *Server\\Queue* tab that appears.

4. Select one or more messages from the list, right-click, and then select **Remove Messages (with NDR)** or **Remove Messages (without NDR)**. A dialog box appears that confirms the selected action and displays, **Do you want to continue?** Click **Yes**.

5. To remove all messages from a particular queue, click the **Queues** tab. Select a queue, right-click, and then select **Remove Messages (with NDR)** or **Remove Messages (without NDR)**. A dialog box appears that confirms the selected action and displays, **Do you want to continue?** Click **Yes**.

    > [!NOTE]
    > If you're working with a filtered list, the displayed page may not include all items in the filter. In this case, a prompt appears that displays: <STRONG>This action will affect all items on this page. To expand the scope of this action to include all items in this filter, check the following box before you click OK.</STRONG>

## Use the Shell to remove messages

To remove messages from queues, use the following syntax.

```powershell
Remove-Message <-Identity MessageIdentity | -Filter "MessageFilter"> -WithNDR <$true | $false>
```

This example removes messages in the queues that have a subject of "Win Big" without sending an NDR.

```powershell
Remove-Message -Filter "Subject -eq 'Win Big'" -WithNDR $false
```

This example removes the message with the message ID 3 from the unreachable queue on server named Mailbox01 and sends an NDR.

```powershell
Remove-Message -Identity Mailbox01\Unreachable\3 -WithNDR $true
```

## How do you know this worked?

To verify that you have successfully removed messages from queues, do one of the following:

- In Queue Viewer, select the queue or create a filter to verify the messages no longer exist.

- Use the **Get-Message** cmdlet with the *Queue* or *Filter* parameters to verify the messages no longer exist. For more information, see [Get-Message](https://docs.microsoft.com/powershell/module/exchange/Get-Message).

## Resume messages in queues

You can resume a message that currently has a status of Suspended. By resuming a message, you enable delivery of the message. If you resume a message located in the poison message queue, the message will be sent to the categorizer for processing. A message being sent to multiple recipients might be located in multiple queues. To resume a message in more than one queue in a single operation, you must use a filter.

## Use Queue Viewer in the Exchange Toolbox to resume messages

1. Click **Start** \> **All Programs** \> **Microsoft Exchange 2013** \> **Exchange Toolbox**.

2. In the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

3. In Queue Viewer, click the **Messages** tab. A list of all messages on the server to which you're connected is displayed. To adjust the action to focus on a single queue, click the **Queues** tab, double-click the queue name, and then click the *Server\\Queue* tab that appears.

4. Click **Create Filter**, and enter your filter expression as follows:

    1. Select **Status** from the message property drop-down list.

    2. Select **Equals** from the comparison operator drop-down list.

    3. Select **Suspended** from the value drop-down list.

5. Click **Apply Filter**. All messages that have a status of Suspended are displayed.

6. Select one or more messages from the list, right-click, and select **Resume**.

## Use the Shell to resume messages

To resume messages, use the following syntax:

```powershell
Resume-Message <-Identity MessageIdentity | -Filter "MessageFilter">
```

This example resumes all messages being sent from any sender in the Contoso.com domain.

```powershell
Resume-Message -Filter "FromAddress -eq '*contoso.com'"
```

This example resumes the message with the message ID 3 in the unreachable queue on server Hub01.

```powershell
Resume-Message -Identity Hub01\Unreachable\3
```

To resubmit messages from the poison message queue, perform the following steps:

## How do you know this worked?

To verify that you have successfully resume messages in queues, do one of the following:

- In Queue Viewer, select the queue or create a filter to verify the messages are no longer suspended.

- Use the **Get-Message** cmdlet with the *Queue* or *Filter* parameters to verify the messages are no longer suspended. For more information, see [Get-Message](https://docs.microsoft.com/powershell/module/exchange/Get-Message).

Note that if you can't find the message in any queues on the server, that probably indicates the message was successfully delivered to the next hop.

## Suspend messages in queues

When you suspend a message, you prevent delivery of the message. A message that appears in the queue but is already in delivery won't be suspended. Delivery will continue, and the message status will be **PendingSuspend**. If the delivery fails, the message will re-enter the queue, and the message will then be suspended. You can't suspend a message in the Submission queue or in the poison message queue.

A message being sent to multiple recipients might be located in multiple queues. To suspend a message in more than one queue in a single operation, you need to use a filter.

## Use Queue Viewer in the Exchange Toolbox to suspend messages

1. Click **Start** \> **All Programs** \> **Microsoft Exchange 2013** \> **Exchange Toolbox**.

2. In the **Mail flow tools** section, double-click **Queue Viewer** to open the tool in a new window.

3. In Queue Viewer, click the **Messages** tab. A list of all messages on the server to which you're connected is displayed. To limit the view to a single queue, click the **Queues** tab, double-click the queue name, and then click the *Server\\Queue* tab that appears.

4. Select one or more messages, right-click, and then select **Suspend**.

## Use the Shell to suspend messages

To suspend messages, use the following syntax:

```powershell
Suspend-Message <-Identity MessageIdentity | -Filter "MessageFilter">
```

This example suspends all messages in the queues that are from any sender in the domain contoso.com.

```powershell
Suspend-Message -Filter "FromAddress -eq '*contoso.com'"
```

This example suspends the message with the message ID 3 in the unreachable queue on server named Mailbox01:

```powershell
Suspend-Message -Identity Mailbox01\Unreachable\3
```

## How do you know this worked?

To verify that you have successfully suspended messages in queues, do one of the following:

- In Queue Viewer, select the queue or create a filter to verify messages are suspended.

- Use the **Get-Message** cmdlet with the *Queue* or *Filter* parameters to verify the messages are suspended. For more information, see [Get-Message](https://docs.microsoft.com/powershell/module/exchange/Get-Message).
