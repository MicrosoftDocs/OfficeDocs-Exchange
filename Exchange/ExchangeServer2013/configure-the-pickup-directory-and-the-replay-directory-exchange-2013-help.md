---
title: 'Configure the Pickup directory and the Replay directory: Exchange 2013 Help'
TOCTitle: Configure the Pickup directory and the Replay directory
ms:assetid: c9ca7358-9a08-4f57-89d0-910e4438df8a
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb124549(v=EXCHG.150)
ms:contentKeyID: 49382862
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure the Pickup directory and the Replay directory

_**Applies to:** Exchange Server 2013_

The Pickup and replay directories are used by the Transport service on Mailbox servers and Edge Transport servers to insert message files directly into the transport pipeline. Correctly formatted email message files that you copy to the Pickup or Replay directories are submitted for delivery. The Pickup directory is used by administrators for mail flow testing, or by applications that must create and submit their own messages. The Replay directory receives messages from non-SMTP foreign gateway servers and resubmits messages that you exported from the queues of Microsoft Exchange servers.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- The value of the *PickupDirectoryMaxMessagesPerMinute* parameter on the **Set-TransportService** cmdlet is used by the Pickup and Replay directories.

- Changing the location of the Pickup or Replay directories doesn't copy any existing message files from the old location to the new location. The new Pickup or Replay directory location is active almost immediately after the configuration change, but any existing message files are left in the old location.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## What Do You Want to Do?

## Use the Shell to configure the Pickup directory

To configure the Pickup directory, use the following syntax.

```powershell
Set-TransportService <ServerIdentity> -PickupDirectoryPath <LocalFilePath> -PickupDirectoryMaxHeaderSize <Size> -PickupDirectoryMaxRecipientsPerMessage <Integer> -PickupDirectoryMaxMessagesPerMinute <Integer>
```

This example makes the following changes to the Pickup directory on the Mailbox server named Exchange01:

- The Pickup directory location is set to D:\\Pickup Directory.

- The maximum size allowed for message headers in a message file is increased to 96 KB.

- The maximum number of recipients allowed in a message file is increased to 250.

- The maximum rate of message processing for the Pickup and Replay directories is increased to 200 messages per minute.

```powershell
Set-TransportService Exchange01 -PickupDirectoryPath "D:\Pickup Directory" -PickupDirectoryMaxHeaderSize 96KB -PickupDirectoryMaxRecipientsPerMessage 250 -PickupDirectoryMaxMessagesPerMinute 200
```

> [!NOTE]
> <UL>
> <LI>
> <P>Setting the <EM>PickupDirectoryPath</EM> parameter to the value <CODE>$null</CODE> disables the Pickup directory.</P>
> <LI>
> <P>The directory specified by the <EM>PickupDirectoryPath</EM> parameter and the <EM>ReplayDirectoryPath</EM> parameter can't be the same.</P></LI></UL>

## Use the Shell to configure the Replay directory

To configure the Replay directory, use the following syntax.

```powershell
Set-TransportService <ServerIdentity> -ReplayDirectoryPath "C:\Replay Directory" <LocalFilePath> -PickupDirectoryMaxMessagesPerMinute <Integer>
```

This example makes the following changes to the Replay directory on the Mailbox server named Exchange01:

- The Replay directory location is set to D:\\Replay Directory.

- The maximum rate of message processing for the Pickup and Replay directories is increased to 200 messages per minute.

```powershell
Set-TransportService Exchange01 -ReplayDirectoryPath "D:\Replay Directory" -PickupDirectoryMaxMessagesPerMinute 200
```

> [!NOTE]
> <UL>
> <LI>
> <P>Setting the <EM>ReplayDirectoryPath</EM> parameter to the value <CODE>$null</CODE> disables the Replay directory.</P>
> <LI>
> <P>The directory specified by the <EM>PickupDirectoryPath</EM> parameter and the <EM>ReplayDirectoryPath</EM> parameter can't be the same.</P></LI></UL>

## How do you know this worked?

To verify that you have successfully configured the Pickup and Replay directories, do the following:

1. Run the following command:

   ```powershell
   Get-TransportService <ServerIdentity> | Format-List Pickup*,Replay*
   ```

2. Verify the values displayed are the values you configured.
