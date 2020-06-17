---
title: 'Create a database availability group network: Exchange 2013 Help'
TOCTitle: Create a database availability group network
ms:assetid: 6caec7be-788a-4058-87a7-f31c575b870c
ms:mtpsurl: https://technet.microsoft.com/library/Dd298051(v=EXCHG.150)
ms:contentKeyID: 48385202
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a database availability group network

_**Applies to:** Exchange Server 2013_

If needed, you can create additional networks for use in a database availability group (DAG). You can use the EAC or the Shell to create a DAG network.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- You can create a DAG network only when automatic network configuration has been disabled for a DAG. For detailed steps about how to disable automatic network configuration for a DAG, see [Configure database availability group properties](configure-database-availability-group-properties-exchange-2013-help.md).

- When creating a DAG network, you must assign unique subnets that aren't in use by another DAG network. If you use subnets that are assigned to an existing DAG network, they will be removed from that DAG network and added to the newly created DAG network.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create a database availability group network

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure, and then click ![Add DAG network](images/Dd298051.befcdc4e-7f7a-451d-a0a8-608c79f5d186(EXCHG.150).gif "Add DAG network").

3. On the **new database availability group network** page, provide the following information:

   - **Database availability group network name**: Use this field to type a name for the network that's unique in the DAG.

   - **Description**: Use this field to provide a text description of the DAG network.

   - **Subnets**: Use this field to associate one or more subnets with the DAG network. Click ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add a subnet, click ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon") to edit a subnet, and click minus (-) to remove a subnet.

4. Click **Save** to create the DAG network.

## Use the Shell to create a database availability group network

This example creates the network ReplicationDagNetwork02 with a subnet of 10.0.0.0 and a bitmask of 8 in the DAG DAG1. Replication is enabled for the network, and an optional description of the network is also being added.

```powershell
New-DatabaseAvailabilityGroupNetwork -DatabaseAvailabilityGroup DAG1 -Name ReplicationDagNetwork02 -Description "Replication network 2" -Subnets 10.0.0.0/8 -ReplicationEnabled:$True
```

## How do you know this worked?

To verify that you've successfully created a DAG network, do one of the following:

- In the EAC, navigate to **Servers** \> **Database Availability Groups**. Select the appropriate DAG, and the newly created DAG network is displayed in the details pane.

- In the Shell, run the following command to verify the DAG network was created and to display DAG network configuration information.

    ```powershell
    Get-DatabaseAvailabilityGroupNetwork <DAGNetworkName> | Format-List
    ```

## For more information

[Set-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroupNetwork)

[Get-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Get-DatabaseAvailabilityGroupNetwork)

[New-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroupNetwork)

[Remove-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Remove-DatabaseAvailabilityGroupNetwork)
