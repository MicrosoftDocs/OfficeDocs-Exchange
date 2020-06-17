---
localization_priority: Normal
description: 'Summary: Learn how to export messages from queues in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 688b342c-f380-4fe0-afce-7e38cf490627
ms.reviewer: 
title: Export messages from queues
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Export messages from queues

On Mailbox servers and Edge Transport servers in Exchange Server, you can export the messages in a queue to files. The exported messages aren't removed from the queue. Copies of the messages are made in the specified location as a plain text files. You can view the message files in Notepad or Outlook, and you can resubmit the message files by using the Replay directory on any other Mailbox server or Edge Transport server inside or outside your Exchange organization.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Queues" entry in [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- To export messages from a delivery queue, the Submission queue, or the Unreachable queue, the messages need to be in the Suspended state. For active, healthy queues, you first suspend the queue so you can then suspend the messages. Messages in the poison message queue are already in the Suspended state. For more information, see [Suspend queues](queue-procedures.md#suspend-queues) and [Suspend messages in queues](message-procedures.md#suspend-messages-in-queues).

- You can't use Queue Viewer in the Exchange Toolbox to export messages. However, you can use Queue Viewer to locate, identify, and suspend the messages before you export them using the Exchange Management Shell. For more information about Queue Viewer, see [Queue Viewer](queue-viewer.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- When you export messages from a queue, you don't remove the messages from the queue. If you resubmit the exported messages by using the Replay directory, you should remove the messages from the queue to avoid duplicate message delivery. For more information, see [Remove messages from queues](message-procedures.md#Remove).

- Verify the following information about the target location for the exported message files:

  - The target folder needs to exist before you export any messages, and won't be created for you. If you don't specify the complete path, the files are written to the current Exchange Management Shell working directory.

  - The path can be local to the Exchange server, or it can be a UNC path to a share on a remote server (\\server\share).

  - Your account needs to have the **Write** permission in the target folder.

- We use the message's **InternetMessageID** property value for the exported message file names to help ensure uniqueness. The procedures include steps to remove angled brackets (\> and \<), because they aren't allowed in file names. Also, we use the .eml file name extension so you can easily open the files in Outlook or resubmit the files by using the Replay directory.

- For more information about identity and filters for queues and messages in queues, see the following topics:

  - [Find queues and messages in queues in the Exchange Management Shell](queues-and-messages-in-powershell.md)

  - [Queue properties](queue-properties.md)

  - [Properties of messages in queues](message-properties.md)

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to export a specific message from a queue

To export a specific message from a queue, use the following syntax:

```powershell
Export-Message -Identity <MessageIdentity> | AssembleMessage -Path <FilePath>\<FileName>.eml
```

This example takes the following actions on the server named Mailbox01:

1. Suspends the contoso.com delivery queue.

2. Suspends the message in the queue that has the **InternalMessageID** value 1234.

3. Exports a copy of the message to the file D:\contoso Export\export.eml.

```powershell
Suspend-Queue Mailbox01\contoso.com
```

```powershell
Suspend-Message -Identity Mailbox01\contoso.com\1234
```

```powershell
Export-Message -Identity Mailbox01\contoso.com\1234 | AssembleMessage -Path "D:\Contoso Export\export.eml"
```

## Use the Exchange Management Shell to export all messages from a queue

To export all messages from a queue, and use the **InternetMessageID** value of each message as the file name, use the following syntax:

```powershell
Get-Message -Queue <QueueIdentity> -ResultSize Unlimited | ForEach-Object {$Temp=<Path>+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```

This example takes the following actions on the server named Mailbox01:

1. Suspends the contoso.com delivery queue.

2. Suspends all messages in the queue.

3. Exports copies of the messages to the local folder named D:\Contoso Export.

```powershell
Suspend-Queue Mailbox01\contoso.com
```

```powershell
Get-Queue Mailbox01\contoso.com | Get-Message -ResultSize Unlimited | Suspend-Message
```

```powershell
Get-Message -Queue Mailbox01\Contoso.com -ResultSize Unlimited | ForEach-Object {$Temp="D:\Contoso Export\"+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```

## Use the Exchange Management Shell to export specific messages from all queues on a server

To export specific messages from all queues on a server, and use the **InternetMessageID** value of each message as the file name, use the following syntax:

```powershell
Get-Message -Filter "<MessageFilter>" [-Server <ServerIdentity>] -ResultSize Unlimited | ForEach-Object {$Temp=<Path>+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```

This example takes the following actions on the server named Mailbox01:

1. Suspends all queues on the server.

2. Suspends all messages in all queues on the server from senders in the fabrikam.com domain.

3. Exports copies of the messages to the local folder named D:\Fabrikam Export.

```powershell
Suspend-Queue -Server Mailbox01
```

```powershell
Suspend-Message -Filter "FromAddress -like '*@fabrikam.com'" -Server Mailbox01
```

```powershell
Get-Message -Filter "FromAddress -like '*@fabrikam.com'" -Server Mailbox01 -ResultSize Unlimited | ForEach-Object {$Temp="D:\Fabrikam Export\"+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```

## Use the Exchange Management Shell to export all messages from all queues on a server

To export all messages from all queues on a server, and use the **InternetMessageID** value of each message as the file name, use the following syntax:

```powershell
Get-Message [-Server <ServerIdentity>] -ResultSize Unlimited | ForEach-Object {$Temp=<Path>+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```

This example takes the following actions on the server named Mailbox01:

1. Suspends all queues on the server.

2. Suspends all messages in all queues on the server.

3. Exports copies of the messages to the local folder named D:\Mailbox01 Export.

```powershell
Suspend-Queue -Server Mailbox01
```

```powershell
Get-Queue -Server Mailbox01 | Get-Message -ResultSize Unlimited | Suspend-Message
```

```powershell
Get-Message -Server Mailbox01 -ResultSize Unlimited | ForEach-Object {$Temp="D:\Mailbox01 Export\"+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp}
```
