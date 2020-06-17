---
title: 'Create a database availability group: Exchange 2013 Help'
TOCTitle: Create a database availability group
ms:assetid: d6b98299-e203-488b-af73-50753fe152c8
ms:mtpsurl: https://technet.microsoft.com/library/Dd351172(v=EXCHG.150)
ms:contentKeyID: 48385609
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a database availability group

_**Applies to:** Exchange Server 2013_

A database availability group (DAG) is a set of up to 16 Microsoft Exchange Server 2013 Mailbox servers that provide automatic database-level recovery from a database, server, or network failure. When a Mailbox server is added to a DAG, it works with the other servers in the DAG to provide automatic, database-level recovery from database, server, and network failures.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- When creating a DAG with Mailbox servers running Windows Server 2012, you must pre-stage the cluster name object (CNO) before adding members to the DAG. If you are creating a DAG without an administrative access point with Mailbox servers running Windows Server 2012 R2, then you do not need to pre-stage a CNO for the DAG. For detailed steps, see [Pre-stage the cluster name object for a database availability group](pre-stage-the-cluster-name-object-for-a-database-availability-group-exchange-2013-help.md).

- When creating a DAG, you provide a unique name for the DAG of up to 15 characters. In addition to providing a name for the DAG, you must also assign one or more IP addresses (either IPv4 or both IPv4 and IPv6) to the DAG, unless you are creating a Windows Server 2012 R2 DAG without an administrative access point and you are not assigning any IP addresses to the DAG. Otherwise, the IP addresses you assign must be on each subnet intended for the MAPI network and must be available for use. If you specify one or more IPv4 addresses and your system is configured to use IPv6, the task will also attempt to automatically assign the DAG one or more IPv6 addresses.

- When creating a DAG, you can optionally specify a witness server and witness directory. If you specify a witness server, we recommend that you use a Client Access server that doesn't have the Mailbox server role installed. This allows an Exchange administrator to be aware of the availability of the witness, and it ensures that all of the necessary security permissions needed for using the witness server are in place.

  The following combinations of options and behaviors are available:

  - You can specify only a name for the DAG and leave the **Witness server** and **Witness directory** fields empty. In this scenario, the task will search for a Client Access server that doesn't have the Mailbox server role installed. It will automatically create the default witness directory and share on that Client Access server and configure the DAG to use that server as its witness server.

  - You can specify a name for the DAG, the witness server that you want to use, and the directory you want created and shared on the witness server.

  - You can specify a name for the DAG and the witness server that you want to use, and leave the **Witness directory** field empty. In this scenario, the task will create the default witness directory on the specified witness server.

  - You can specify a name for the DAG, leave the **Witness server** field empty, and specify the directory you want created and shared on the witness server. In this scenario, the wizard will search for a Client Access server that doesn't have the Mailbox server role installed, and it will automatically create the specified witness directory on that server, share the directory, and configure the DAG to use that Client Access server as its witness server.

    > [!IMPORTANT]
    > If the witness server you specify isn't an Exchange 2013 or Exchange 2010 server, you must add the Exchange Trusted Subsystem universal security group to the local Administrators group on the witness server. These security permissions are necessary to ensure that Exchange can create a directory and share on the witness server as needed. If the proper permissions aren't configured, the following error is returned:<BR><CODE>Error: An error occurred during discovery of the database availability group topology. Error: An error occurred while attempting a cluster operation. Error: Cluster API "AddClusterNode() (MaxPercentage=12) failed with 0x80070005. Error: Access is denied."</CODE>

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create a database availability group

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Click ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to create a DAG.

3. On the **new database availability group** page, provide the following information for the DAG:

   - **Database availability group name**: Use this field to type a valid and unique name for the DAG of up to 15 characters. The name is equivalent to a computer name, and a corresponding CNO will be created in Active Directory with that name. This name will be both the name of the DAG and the name of the underlying cluster.

   - **Witness server**: Use this field to specify a witness server for the DAG. If you leave this field blank, the system will attempt to automatically select a Client Access server in the local Active Directory site that isn't installed on a computer with the Mailbox server to be used as the witness server.

     > [!NOTE]
     > If you specify a witness server, you must use either a host name or a fully qualified domain name (FQDN). Using an IP address or a wildcard name isn't supported. In addition, the witness server can't be a member of the DAG.

   - **Witness directory**: Use this field to type the path to a directory on the witness server that will be used to store witness data. If the directory doesn't exist, the system will create it for you on the witness server. If you leave this field blank, the default directory (%SystemDrive%\\DAGFileShareWitnesses\\\<DAG FQDN\>) will be created on the witness server.

   - **Database availability group IP addresses**: Use this field to assign one or more static IPv4 addresses to the DAG. Enter an IPv4 address and click ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add it. Leave this field blank if you want the DAG to use Dynamic Host Configuration Protocol (DHCP) to obtain the necessary IPv4 addresses. Optionally, enter 255.255.255.255 to create a DAG without an IP address or cluster administrative access point, which applies only to DAGs that will contain Mailbox servers running Windows Server 2012 R2.

4. Click **Save** to create the DAG.

## Use the Shell to create a database availability group

This example creates the DAG DAG1 that's configured to use the witness server FILESRV1 and the local directory C:\\DAG1. DAG1 is also configured to use DHCP for the DAG's IP addresses.

```powershell
New-DatabaseAvailabilityGroup -Name DAG1 -WitnessServer FILESRV1 -WitnessDirectory C:\DAG1
```

This example creates the DAG DAG2. The system automatically selects a Client Access server in the local Active Directory site that does not contain the Mailbox server role as the DAG's witness server. DAG2 is assigned a single static IP address because in this example all DAG members have the MAPI network on the same subnet.

```powershell
New-DatabaseAvailabilityGroup -Name DAG2 -DatabaseAvailabilityGroupIPAddresses 10.0.0.8
```

This example creates the DAG DAG3. DAG3 is configured to use the witness server MBX2 and the local directory C:\\DAG3. DAG3 is assigned multiple static IP addresses because its DAG members are on different subnets on the MAPI network.

  ```powershell
  New-DatabaseAvailabilityGroup -Name DAG3 -WitnessServer MBX2 -WitnessDirectory C:\DAG3 -DatabaseAvailabilityGroupIPAddresses 10.0.0.8,192.168.0.8
  ```

This example creates the DAG DAG4 that's configured to use DHCP. In addition, the witness server will be automatically selected by the system, and the default witness directory will be created.

```powershell
New-DatabaseAvailabilityGroup -Name DAG4
```

This example creates the DAG DAG5 that will not have an administrative access point (valid for Windows Server 2012 R2 DAGs only). In addition, MBX4 will be used as the witness server for the DAG, and the default witness directory will be created.

  ```powershell
  New-DatabaseAvailabilityGroup -Name DAG5 -DatabaseAvailabilityGroupIPAddresses ([System.Net.IPAddress]::None) -WitnessServer MBX4
  ```

## How do you know this worked?

To verify that you've successfully created a DAG, do one of the following:

- In the EAC, navigate to **Servers** \> **Database Availability Groups**. The newly created DAG is displayed.

- In the Shell, run the following command to verify the DAG was created and to display DAG property information.

  ```powershell
  Get-DatabaseAvailabilityGroup <DAGName> | Format-List
  ```

## For more information

[Database availability groups (DAGs)](database-availability-groups-dags-exchange-2013-help.md)

[Configure database availability group properties](configure-database-availability-group-properties-exchange-2013-help.md)

[Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup)

[New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroup)

[New-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/New-DatabaseAvailabilityGroupNetwork)

[Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Add-DatabaseAvailabilityGroupServer)
