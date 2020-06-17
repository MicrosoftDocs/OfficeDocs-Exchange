---
localization_priority: Normal
description: 'Summary: Create additional networks, if needed, for use in a database availability group (DAG).'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6caec7be-788a-4058-87a7-f31c575b870c
ms.reviewer:
title: Create a database availability group network
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a database availability group network

You can use the EAC or the Exchange Management Shell to create a DAG network.

Looking for other management tasks related to DAGs? Check out [Manage database availability groups](manage-dags.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- You can create a DAG network only when automatic network configuration has been disabled for a DAG. For detailed steps about how to disable automatic network configuration for a DAG, see [Configure database availability group properties](configure-dag-properties.md).

- When creating a DAG network, you must assign unique subnets that aren't in use by another DAG network. If you use subnets that are assigned to an existing DAG network, they will be removed from that DAG network and added to the newly created DAG network.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a database availability group network

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure, and then click ![Add DAG network](../../media/ITPro_EAC_AddDagNetwork.png).

3. On the **new database availability group network** page, provide the following information:

   - **Database availability group network name**: Use this field to type a name for the network that's unique in the DAG.

   - **Description**: Use this field to provide a text description of the DAG network.

   - **Subnets**: Use this field to associate one or more subnets with the DAG network. Click ![Add icon](../../media/ITPro_EAC_AddIcon.png) to add a subnet, click ![Edit icon](../../media/ITPro_EAC_EditIcon.png) to edit a subnet, and click minus (-) to remove a subnet.

4. Click **Save** to create the DAG network.

## Use the Exchange Management Shell to create a database availability group network

This example creates the network ReplicationDagNetwork02 with a subnet of 10.0.0.0 and a bitmask of 8 in the DAG DAG1. Replication is enabled for the network, and an optional description of the network is also being added.

```powershell
New-DatabaseAvailabilityGroupNetwork -DatabaseAvailabilityGroup DAG1 -Name ReplicationDagNetwork02 -Description "Replication network 2" -Subnets 10.0.0.0/8 -ReplicationEnabled:$True
```

## How do you know this worked?

To verify that you've successfully created a DAG network, do one of the following:

- In the EAC, navigate to **Servers** \> **Database Availability Groups**. Select the appropriate DAG, and the newly created DAG network is displayed in the details pane.

- In the Exchange Management Shell, run the following command to verify the DAG network was created and to display DAG network configuration information.

  ```powershell
  Get-DatabaseAvailabilityGroupNetwork <DAGNetworkName> | Format-List
  ```

## For more information

[Set-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroupnetwork)

[Get-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/get-databaseavailabilitygroupnetwork)

[New-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/new-databaseavailabilitygroupnetwork)

[Remove-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/remove-databaseavailabilitygroupnetwork)
