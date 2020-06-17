---
localization_priority: Normal
description: 'Summary: How to move all active mailbox database copies from their current Mailbox server to one or more other Mailbox servers in an Exchange Server 2016 or Exchange Server 2019 DAG.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ffcefd56-b0a0-4229-9011-fff4197b7c74
ms.reviewer:
title: Perform a server switchover
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Perform a server switchover

A server switchover is part of preparing for a scheduled outage for the current Mailbox server.

## What do you need to know before you begin?

- Estimated time to complete: 30 seconds per database

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to perform a server switchover

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the"Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

1. In the EAC, go to **Servers** \> **servers**.

2. Select the Mailbox server you want to switchover.

3. In the details pane, select **Server Switchover**.

4. On the **Server Switchover** page, do one of the following:

1. Accept the default setting of **Automatically choose a target server** (in which case, the system automatically selects the best Mailbox server for each database being switched over), and then click **save**.

2. Click **Use the specified server as the target for switchover**, click **Browse** to select a Mailbox server, and then click **save**.

5. When the switchover has completed, click **close** to exit the **Server Switchover** page.

## Use the Exchange Management Shell to perform a server switchover

This example performs a server switchover for the server MBX1. The system automatically selects the best Mailbox server for each active database on MBX1.

```powershell
Move-ActiveMailboxDatabase -Server MBX1
```

This example performs a server switchover of the Mailbox server MBX4. When the command completes, MBX5 hosts the active copy of the databases that were previously active on MBX4.

```powershell
Move-ActiveMailboxDatabase -Server MBX4 -ActivateOnServer MBX5
```

For detailed syntax and parameter information, see [Move-ActiveMailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/move-activemailboxdatabase).
