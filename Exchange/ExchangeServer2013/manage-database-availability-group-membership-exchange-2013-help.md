---
title: 'Manage database availability group membership: Exchange 2013 Help'
TOCTitle: Manage database availability group membership
ms:assetid: fb2ea15e-96d5-4045-b75b-b0aa5fc60479
ms:mtpsurl: https://technet.microsoft.com/library/Dd351278(v=EXCHG.150)
ms:contentKeyID: 48385730
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage database availability group membership

_**Applies to:** Exchange Server 2013_

When you add a server to a database availability group (DAG), it works with the other servers in the DAG to provide automatic database-level recovery from database, server, or network failures. When you remove a server from a DAG, it's no longer automatically protected from failures.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes per server

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- DAGs use Windows Failover Clustering (WFC) technologies. Each Mailbox server that's a member of a DAG is also a node in the underlying cluster used by the DAG. As a result, at any specific time, a Mailbox server can be a member of only one DAG. Because DAGs use WFC technology, all servers added to a DAG must be running the same operating system: either Windows Server 2008 R2 Enterprise or Datacenter Edition, or the Standard or Datacenter Edition of Windows Server 2012 or Windows Server 2012 R2.

- If you're adding Mailbox servers running Windows Server 2012, you must pre-stage the cluster name object (CNO) for the DAG. If you're adding Mailbox servers running Windows Server 2012 R2, and your DAG does not have an administrative access point, then you do not need to pre-stage a CNO, as DAGs without administrative access points do not have a CNO. For detailed steps, see [Pre-stage the cluster name object for a database availability group](pre-stage-the-cluster-name-object-for-a-database-availability-group-exchange-2013-help.md).

- Before you can add members to a DAG, you must first create a DAG. For detailed steps, see [Create a database availability group](create-a-database-availability-group-exchange-2013-help.md).

- You must remove all replicated database copies from the server before you can remove it from a DAG.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to manage database availability group membership

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure, and then click ![Manage DAG members](images/Dd351278.d567ae56-d6cd-4edb-ab67-ad8f7c58f337(EXCHG.150).gif "Manage DAG members").

   - To add one or more Mailbox servers to the DAG, click ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"), select the servers from the list, click **Add**, and then click **OK**.

   - To remove one or more Mailbox servers from the DAG, select the servers, and then click the minus (-) icon.

3. Click **Save** to save the changes.

4. When the task has completed successfully, click **Close**.

## Use the Shell to manage database availability group membership

This example adds the Mailbox server MBX1 to the DAG DAG1.

```powershell
Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
```

This example removes the Mailbox server MBX1 from the DAG DAG1. Before running this command, make sure that no replicated databases exist on the Mailbox server.

```powershell
Remove-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
```

This example removes the configuration settings for the Mailbox server MBX4 from the DAG DAG2. MBX4 is expected to be offline for an extended period, so its configuration is being removed from the DAG while it's offline to establish quorum with the remaining online DAG members.

```powershell
Remove-DatabaseAvailabilityGroupServer -Identity DAG2 -MailboxServer MBX4 -ConfigurationOnly
```

## How do you know this worked?

To verify that you've successfully managed DAG membership, do one of the following:

- In the EAC, navigate to **Servers** \> **Database Availability Groups**. The current DAG membership is displayed in the **Member Servers** column.

- In the Shell, run the following command to display DAG membership information.

    ```powershell
    Get-DatabaseAvailabilityGroup <DAGName> | Format-List Servers
    ```

## For more information

[Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Add-DatabaseAvailabilityGroupServer)

[Remove-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Remove-DatabaseAvailabilityGroupServer)
