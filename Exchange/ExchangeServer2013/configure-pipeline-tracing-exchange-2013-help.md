---
title: 'Configure pipeline tracing: Exchange 2013 Help'
TOCTitle: Configure pipeline tracing
ms:assetid: 10293c83-2157-474e-840d-942e064a4672
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ916678(v=EXCHG.150)
ms:contentKeyID: 50934211
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure pipeline tracing

_**Applies to:** Exchange Server 2013_

Pipeline tracing captures copies of email messages as they move through the transport pipeline in the Transport service or the Mailbox Transport service on Mailbox server and on Edge Transport servers.

## What do you need to know before you begin?

- Estimated time to complete this procedure: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Transport Service" and "Mailbox Transport Service" entries in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- Pipeline tracing copies the complete contents of email messages that are sent from the sender's email address. To avoid unwanted exposure of confidential information, you need to set appropriate security permissions on the location of the pipeline tracing folder.

- Don't enable pipeline tracing for long periods of time. Pipeline tracing creates multiple message snapshot files that accumulate quickly. Always monitor available disk space when pipeline tracing is enabled.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Enable and configure pipeline tracing

## Step 1: Use the Shell to configure the pipeline tracing sender address

Use the following syntax to configure the pipeline tracing sender address.

```powershell
<Set-TransportService | Set-MailboxTransportService> <ServerIdentity> -PipelineTracingSenderAddress <SMTPAddress | "<>">
```

This example configures pipeline tracing to capture snapshots of all messages sent by the sender chris@contoso.com in the Transport service on the Mailbox server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -PipelineTracingSenderAddress chris@contoso.com
```

This example configures pipeline tracing to capture snapshots of all the system-generated messages received by the Transport service on the Mailbox server named Mailbox02.

```powershell
Set-TransportService Mailbox02 -PipelineTracingSenderAddress "<>"
```

> [!WARNING]
> Configuring pipeline tracing to capture all server-generated messages in a transport service may place a significant load on the server and may quickly consume available disk space. Always monitor available disk space when pipeline tracing is enabled.

## Step 2: (Optional) Use the Shell to specify a custom pipeline tracing folder

The default pipeline tracing folder doesn't exist until after you enable pipeline tracing, and messages that meet the criteria you specify using the *PipelineTracingSenderAddress* parameter flow through the transport service on the server. For the Transport service on a Mailbox server, the default location is `%ExchangeInstallPath%TransportRoles\Logs\Hub\PipelineTracing`. For the Mailbox Transport service on a Mailbox server, the default location is `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\PipelineTracing`. If you specify a custom path, the path must be on the local Exchange server.

Use the following syntax to configure the pipeline tracing folder.

```powershell
<Set-TransportService | Set-MailboxTransportService> <ServerIdentity> -PipelineTracingPath <LocalFilePath>
```

This example sets the pipeline tracing folder for the Transport service on the Mailbox server named Mailbox01 to D:\\Hub\\Pipeline Tracing.

```powershell
Set-TransportService Mailbox01 -PipelineTracingPath "D:\Hub\Pipeline Tracing"
```

## Step 3: Use the Shell to enable pipeline tracing

By default, pipeline tracing is disabled on all Exchange servers. When you enable pipeline tracing, you are enabling pipeline tracing in the specified transport service on the specified Exchange server only. Before you enable pipeline tracing, you need to specify the sender address as described in Step 1.

Use the following syntax to enable pipeline tracing.

```powershell
<Set-TransportService | Set-MailboxTransportService> <ServerIdentity> -PipelineTracingEnabled $true
```

This example enables pipeline tracing in the Transport service on the Mailbox server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -PipelineTracingEnabled $true
```

## How do you know this worked?

To verify that you have successfully configured pipeline tracing, do the following:

1. Run the following command:

   ```powershell
   <Get-TransportService | Get-MailboxTransportService> <ServerIdentity> | Format-List PipelineTracing*
   ```

2. Verify the values displayed are the values you configured.

3. Check the pipeline tracing folder for the Transport service or the Mailbox Transport service, and verify message snapshot files are being created in the folder.

## Disable pipeline tracing

Because of the disk space and security concerns associated with pipeline tracing, pipeline tracing is a temporary action for diagnostic or troubleshooting purposes. Whenever you enable pipeline tracing, always remember to disable it when you are finished.

Use the following syntax to disable pipeline tracing.

```powershell
<Set-TransportService | Set-MailboxTransportService> <ServerIdentity> -PipelineTracingEnabled $false
```

This example disables pipeline tracing in the Transport service on the Mailbox server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -PipelineTracingEnabled $false
```

## How do you know this worked?

To verify that you have successfully disabled pipeline tracing, do the following:

1. Run the following command:

   ```powershell
   <Get-TransportService | Get-MailboxTransportService> <ServerIdentity> | Format-List PipelineTracingEnabled
   ```

2. Verify the value of the *PipelineTracingEnabled* parameter is $false.

3. Check the pipeline tracing folder, and verify message snapshot files are no longer being created in the folder.
