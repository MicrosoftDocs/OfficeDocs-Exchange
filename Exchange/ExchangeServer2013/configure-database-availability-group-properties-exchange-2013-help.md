---
title: 'Configure database availability group properties: Exchange 2013 Help'
TOCTitle: Configure database availability group properties
ms:assetid: 50daeac5-a16f-4362-a325-19e0fe25d59d
ms:mtpsurl: https://technet.microsoft.com/library/Dd297985(v=EXCHG.150)
ms:contentKeyID: 48385082
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure database availability group properties

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to configure the properties of a database availability group (DAG), including DAG IP address configuration and the witness server and witness directory used by the DAG. The Shell enables you to configure DAG properties that aren't available in the EAC, such as alternate witness server and alternate witness directory information, the TCP port used for replication, and datacenter activation coordination (DAC) mode.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- DAG property values are stored in both Active Directory and the cluster database. However, some properties are stored only in the cluster database. As a result, the underlying cluster for the DAG must be running and have quorum to set the properties for:

  - ReplicationPort

  - NetworkCompression

  - NetworkEncryption

  - DiscoverNetworks

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure database availability group properties

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Select the DAG you want to configure and click ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. Use the **General** page to view DAG membership and operational status, and to configure the DAG's witness server, witness directory, and automatic network configuration:

   - **Witness server**: The host name or fully qualified domain name (FQDN) of the witness server for the DAG. Although this is a required property for all DAGs, the witness server is used when there is an even number of DAG members and the quorum model in use by the cluster is Node and File Share Majority.

   - **Witness directory**: The full path of the directory used to store the witness.log file on the witness server. Although this is a required property for all DAGs, the witness directory is used only when the DAG's witness server is in use.

   - **Operational servers**: A read-only field that displays a list of DAG members and their current operational status.

   - **Configure the database group network manually**: A check box that you select when you want to configure all DAG networks manually. When you leave the check box clear, the system configures DAG networks automatically based on network interface configuration. If the check box is clear, the **Set-DatabaseAvailabilityGroupNetwork** and **New-DatabaseAvailabilityGroupNetwork** cmdlets are disabled for administrative use against the DAG.

4. Use the **IP Addresses** page to view and modify the IP addresses assigned to the DAG:

   - Select an existing IP address and click ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon") to modify it.

   - Select an existing IP address and click the minus icon (delete) to remove it.

   - Enter an IP address and click ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add it to the DAG.

5. Click **Save** to save any changes that were made.

## Use the Shell to configure database availability group properties

This example sets the witness directory to C:\\DAG1DIR for the DAG DAG1.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -WitnessDirectory C:\DAG1DIR
```

This example preconfigures an alternate witness server of CAS3 and an alternate witness directory of C:\\DAGFileShareWitnesses\\DAG1.contoso.com for the DAG DAG1.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG1 -AlternateWitnessDirectory C:\DAGFileShareWitnesses\DAG1.contoso.com -AlternateWitnessServer CAS3
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

- In the Shell, run the following command to display DAG configuration settings and verify the DAG was configured successfully.

  ```powershell
  Get-DatabaseAvailabilityGroup <DAGName> | Format-List
  ```

## For more information

[Create a database availability group](create-a-database-availability-group-exchange-2013-help.md)

[Remove a database availability group](remove-a-database-availability-group-exchange-2013-help.md)

[Create a database availability group network](create-a-database-availability-group-network-exchange-2013-help.md)

[Manage database availability group membership](manage-database-availability-group-membership-exchange-2013-help.md)

[Get-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Get-DatabaseAvailabilityGroup)

[Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup)
