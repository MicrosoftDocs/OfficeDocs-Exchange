---
localization_priority: Normal
description: 'Summary: You can use the EAC or the Exchange Management Shell to configure the properties of a database availability group (DAG), including DAG IP address configuration, the witness server, and the witness directory.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 50daeac5-a16f-4362-a325-19e0fe25d59d
ms.reviewer:
title: Configure database availability group properties
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure database availability group properties

The Exchange Management Shell enables you to configure DAG properties that aren't available in the EAC, such as alternate witness server and alternate witness directory information, the TCP port used for replication, and datacenter activation coordination (DAC) mode.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- DAG property values are stored in both Active Directory and the cluster database. However, some properties are stored only in the cluster database. As a result, the underlying cluster for the DAG must be running and have quorum to set the properties for:

  - ReplicationPort

  - NetworkCompression

  - NetworkEncryption

  - DiscoverNetworks

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure database availability group properties

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure and click ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. Use the **General** page to view DAG membership and operational status, and to configure the DAG's witness server, witness directory, and automatic network configuration:

   - **Witness server**: The host name or fully qualified domain name (FQDN) of the witness server for the DAG. Although this is a required property for all DAGs, the witness server is used when there is an even number of DAG members and the quorum model in use by the cluster is Node and File Share Majority.

   - **Witness directory**: The full path of the directory used to store the witness.log file on the witness server. Although this is a required property for all DAGs, the witness directory is used only when the DAG's witness server is in use.

   - **Database availability group members**: A read-only field that displays a list of DAG members and their current operational status.

   - **Configure database availability group networks manually**: A check box that you select when you want to configure all DAG networks manually. When the check box is clear, the system configures DAG networks automatically based on network interface configuration, and the **Set-DatabaseAvailabilityGroupNetwork** and **New-DatabaseAvailabilityGroupNetwork** cmdlets are disabled for the DAG.

4. Use the **IP addresses** page to view and modify the IP addresses assigned to the DAG:

   - Select an existing IP address and click ![Edit icon](../../media/ITPro_EAC_EditIcon.png) to modify it.

   - Select an existing IP address and click the minus icon (delete) to remove it.

   - Enter an IP address and click ![Add icon](../../media/ITPro_EAC_AddIcon.png) to add it to the DAG.

5. Click **Save** to save any changes that were made.

## Use the Exchange Management Shell to configure database availability group properties

This example sets the witness directory to C:\DAG1DIR for the DAG DAG1.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -WitnessDirectory C:\DAG1DIR
```

This example preconfigures an alternate witness server of MBX3 and an alternate witness directory of C:\DAGFileShareWitnesses\DAG1.contoso.com for the DAG DAG1.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -AlternateWitnessDirectory C:\DAGFileShareWitnesses\DAG1.contoso.com -AlternateWitnessServer MBX3
```

This example configures the DAG DAG1 to use Dynamic Host Configuration Protocol (DHCP) to obtain an IP address.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -DatabaseAvailabilityGroupIPAddresses 0.0.0.0
```

This example configures the DAG DAG1 to use a static IP address of 10.0.0.8.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -DatabaseAvailabilityGroupIPAddresses 10.0.0.8
```

This example configures the multi-subnet DAG DAG1 with multiple static IP addresses.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -DatabaseAvailabilityGroupIPAddresses 10.0.0.8,10.0.1.8
```

This example configures the DAG DAG1 for DAC mode.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -DatacenterActivationMode DagOnly
```

This example configures the replication port for the DAG DAG1 to be 63132.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -ReplicationPort 63132
```

> [!NOTE]
> After changing the default replication port for a DAG, you must manually modify the Windows Firewall exceptions on each member of the DAG to allow communication to occur over the specified port.

## How do you know this worked?

To verify that you've successfully configured the DAG, do the following:

- In the Exchange Management Shell, run the following command to display DAG configuration settings and verify the DAG was configured successfully.

  ```powershell
  Get-DatabaseAvailabilityGroup <DAGName> | Format-List
  ```

## For more information

[Create a database availability group](create-dags.md)

[Remove a database availability group](remove-dags.md)

[Create a database availability group network](create-dag-networks.md)

[Manage database availability group membership](dag-memberships.md)

[Get-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/get-databaseavailabilitygroup)

[Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup)
