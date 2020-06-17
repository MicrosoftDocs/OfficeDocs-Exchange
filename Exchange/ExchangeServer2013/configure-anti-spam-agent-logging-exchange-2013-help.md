---
title: 'Configure anti-spam agent logging: Exchange 2013 Help'
TOCTitle: Configure anti-spam agent logging
ms:assetid: df157ca3-ad8e-4302-acbc-5fbb8570c21d
ms:mtpsurl: https://technet.microsoft.com/library/Bb691337(v=EXCHG.150)
ms:contentKeyID: 49289436
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure anti-spam agent logging

_**Applies to:** Exchange Server 2013_

Agent logging records the actions performed by specific Exchange anti-spam agents. The information written to the agent log depends on the agent, the SMTP event, and the action performed on the message.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Service" and "Edge Transport server" entries in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- By default, anti-spam features aren't enabled in the Transport service on a Mailbox server. Typically, you only enable the anti-spam features on a Mailbox server if your Exchange organization doesn't do any prior anti-spam filtering before accepting incoming messages. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

  - You can only use the Shell to perform this procedure.

  - For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure anti-spam agent logging

Run the following command:

```powershell
Set-TransportService <ServerIdentity> -AgentLogEnabled <$true | $false> -AgentLogMaxAge <dd.hh:mm:ss> -AgentLogMaxDirectorySize <Size> -AgentLogMaxFileSize <Size> -AgentLogPath <LocalFilePath>
```

This example sets the following agent log settings on the Mailbox server named Mailbox01:

- Sets the location of the agent log files to D:\\Anti-Spam Agent Log. Note that if the folder doesn't exist, it will be created for you.

- Sets the maximum size of an agent log file to 20 MB.

- Sets the maximum size of the agent log directory to 400 MB.

- Sets the maximum age of an agent log file to 14 days.

```powershell
Set-TransportService Mailbox01 -AgentLogPath "D:\Anti-Spam Agent Log" -AgentLogMaxFileSize 20MB -AgentLogMaxDirectorySize 400MB -AgentLogMaxAge 14.00:00:00
```

> [!NOTE]
> <UL>
> <LI>
> <P>If you set the <EM>AgentLogPath</EM> parameter to the value <CODE>$null</CODE>, you effectively disable agent logging. However, if you set <EM>AgentLogPath</EM> to <CODE>$null</CODE> when the value of the <EM>AgentLogEnabled</EM> parameter is <CODE>$true</CODE>, event log errors are generated. The preferred method to disable agent logging is to set <EM>AgentLogEnabled</EM> to <CODE>$false</CODE>.</P>
> <LI>
> <P>Setting the <EM>AgentLogMaxAge</EM> parameter to the value <CODE>00:00:00</CODE> prevents the automatic removal of agent log files because of their age.</P></LI></UL>

For detailed syntax and parameter information, see the *AgentLog* parameters in [Set-TransportService](https://docs.microsoft.com/powershell/module/exchange/Set-TransportService).

## How do you know this worked?

To verify that you have successfully configured anti-spam agent logging, do the following:

1. In the Shell, run the following command:

   ```powershell
   Get-TransportService <ServerIdentity> | Format-List AgentLog*
   ```

2. Verify the values displayed are the values you configured.
