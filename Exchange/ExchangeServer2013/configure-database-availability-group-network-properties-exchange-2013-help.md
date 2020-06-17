---
title: 'Configure database availability group network properties: Exchange 2013 Help'
TOCTitle: Configure database availability group network properties
ms:assetid: 41197639-988f-476c-9788-51d5191a7dce
ms:mtpsurl: https://technet.microsoft.com/library/Dd297927(v=EXCHG.150)
ms:contentKeyID: 48385020
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure database availability group network properties

_**Applies to:** Exchange Server 2013_

Each database availability group (DAG) network has several properties that you can configure, including the name of the DAG network, a description field for the DAG network, a list of subnets that are used by the DAG network, and whether the DAG network is enabled for replication.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- You can configure a DAG network only when automatic network configuration has been disabled for a DAG. For detailed steps about how to disable automatic network configuration for a DAG, see [Configure database availability group properties](configure-database-availability-group-properties-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure database availability group network properties

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure, and in the Details pane, under the DAG network you want to configure, choose:

   - **Disable Replication** or **Enable Replication**: Configures the replication settings for the DAG network.

   - **Remove**: Removes a DAG network. Before you can remove a DAG network, you must first remove all associated subnets from the DAG network.

   - **View details**: Configures DAG network properties, such as the name, description, and associated subnets for the DAG network. You can also view the network interfaces associated with those subnets, and enable or disable replication for the DAG network.

## Use the Shell to configure database availability group network properties

This example adds a subnet of 10.0.0.0 and subnet mask of 255.0.0.0 to the DAG network MapiDagNetwork in the DAG DAG1.

```powershell
Set-DatabaseAvailabilityGroupNetwork -Subnets 10.0.0.0/8 -Identity DAG1\MapiDagNetwork
```

## How do you know this worked?

To verify that you've successfully configured the DAG network, do the following:

- In the Shell, run the following command to display DAG network configuration settings and verify the DAG network was configured successfully.

  ```powershell
  Get-DatabaseAvailabilityGroupNetwork <DAGNetworkName> | Format-List
  ```

## For more information

[Set-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroupNetwork)

[Get-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Get-DatabaseAvailabilityGroupNetwork)

[New-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroupNetwork)

[Remove-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/Remove-DatabaseAvailabilityGroupNetwork)
