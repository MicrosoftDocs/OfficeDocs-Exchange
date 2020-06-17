---
localization_priority: Normal
description: 'Summary: How to create a DAG in Exchange Server, through the Exchange admin center (EAC) or the Exchange Management Shell.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d6b98299-e203-488b-af73-50753fe152c8
ms.reviewer:
title: Create a database availability group
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a database availability group

A database availability group (DAG) is a set of up to 16 Microsoft Exchange Server Mailbox servers that provide automatic database-level recovery from a database, server, or network failure. When a Mailbox server is added to a DAG, it works with the other servers in the DAG to provide automatic, database-level recovery from database, server, and network failures.

> [!IMPORTANT]
> All servers within a DAG must be running the same version of Exchange. You can't mix Exchange 2013 servers and Exchange servers in the same DAG.

Looking for other management tasks related to DAGs? Check out [Manage database availability groups](manage-dags.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- When creating a DAG with Mailbox servers running Windows Server 2012, you must pre-stage the cluster name object (CNO) before adding members to the DAG. If you're creating a DAG without an administrative access point with Mailbox servers running Windows Server 2012 R2, then you do not need to pre-stage a CNO for the DAG. For detailed steps, see [Pre-stage the cluster name object for a database availability group](pre-stage-dag-cnos.md).

- When creating a DAG, you provide a unique name for the DAG of up to 15 characters. In addition to providing a name for the DAG, you must also assign one or more IP addresses (either IPv4 or both IPv4 and IPv6) to the DAG, unless you're creating a Windows Server 2012 R2 DAG without an administrative access point and you aren't assigning any IP addresses to the DAG. Otherwise, the IP addresses you assign must be on each subnet intended for the MAPI network and must be available for use. If you specify one or more IPv4 addresses and your system is configured to use IPv6, the task will also attempt to automatically assign the DAG one or more IPv6 addresses.

- When creating a DAG, you can optionally specify a witness server and witness directory. If you specify a witness server, we recommend that you use an Exchange server with Client Access services. This allows an Exchange administrator to be aware of the availability of the witness, and it ensures that all of the necessary security permissions needed for using the witness server are in place.

  The following combinations of options and behaviors are available:

  - You can specify only a name for the DAG and leave the **Witness server** and **Witness directory** fields empty. In this scenario, the task will search for an Exchange server with Client Access services. It will automatically create the default witness directory and share on that Exchange server with Client Access services, and it will configure the DAG to use that server as its witness server.

  - You can specify a name for the DAG, the witness server that you want to use, and the directory you want created and shared on the witness server.

  - You can specify a name for the DAG and the witness server that you want to use, and leave the **Witness directory** field empty. In this scenario, the task will create the default witness directory on the specified witness server.

  - You can specify a name for the DAG, leave the **Witness server** field empty, and specify the directory you want created and shared on the witness server. In this scenario, the wizard will search for an Exchange server with Client Access services, and it will automatically create the specified witness directory on that server, share the directory, and configure the DAG to use that Exchange server with Client Access services as its witness server.

    **Note**: If the witness server you specify isn't an Exchange server in your organization, you must add the Exchange Trusted Subsystem universal security group to the local Administrators group on the witness server. These security permissions are necessary to ensure that Exchange can create a directory and share on the witness server as needed. If the proper permissions aren't configured, the following error is returned:

    `Error: An error occurred during discovery of the database availability group topology. Error: An error occurred while attempting a cluster operation. Error: Cluster API "AddClusterNode() (MaxPercentage=12) failed with 0x80070005. Error: Access is denied."`

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a database availability group

1. In the EAC, go to **Servers** \> **Database Availability Groups**.

2. Click ![Add icon](../../media/ITPro_EAC_AddIcon.png) to create a DAG.

3. On the **new database availability group** page, provide the following information for the DAG:

   - **Database availability group name**: Use this field to type a valid and unique name for the DAG of up to 15 characters. The name is equivalent to a computer name, and a corresponding CNO will be created in Active Directory with that name. This name will be both the name of the DAG and the name of the underlying cluster.

   - **Witness server**: Use this field to specify a witness server for the DAG. If you leave this field blank, the system will attempt to automatically select an Exchange server with Client Access services that is in the local Active Directory site.

     **Note**: If you specify a witness server, you must use either a host name or a fully qualified domain name (FQDN). Using an IP address or a wildcard name isn't supported. In addition, the witness server can't be a member of the DAG.

   - **Witness directory**: Use this field to type the path to a directory on the witness server that will be used to store witness data. If the directory doesn't exist, the system will create it for you on the witness server. If you leave this field blank, the default directory (%SystemDrive%\DAGFileShareWitnesses\\<DAG FQDN\>) will be created on the witness server.

   - **Database availability group IP addresses**: Use this field to assign one or more static IPv4 addresses to the DAG. Enter an IPv4 address and click ![Add icon](../../media/ITPro_EAC_AddIcon.png) to add it. Leave this field blank if you want the DAG to use Dynamic Host Configuration Protocol (DHCP) to obtain the necessary IPv4 addresses. Optionally, enter 255.255.255.255 to create a DAG without an IP address or cluster administrative access point, which applies only to DAGs that will contain Mailbox servers running Windows Server 2012 R2.

4. Click **Save** to create the DAG.

## Use the Exchange Management Shell to create a database availability group

The following example creates a DAG named DAG1, which is configured to use the witness server FILESRV1 and the local directory C:\DAG1. DAG1 is also configured to use DHCP for the DAG's IP addresses.

```powershell
New-DatabaseAvailabilityGroup -Name DAG1 -WitnessServer FILESRV1 -WitnessDirectory C:\DAG1
```

The next example creates a DAG named DAG2. For the DAG's witness server, the system automatically selects an Exchange server with Client Access services that is in the local Active Directory site. DAG2 is assigned a single static IP address because in this example all DAG members have the MAPI network on the same subnet.

```powershell
New-DatabaseAvailabilityGroup -Name DAG2 -DatabaseAvailabilityGroupIPAddresses 10.0.0.8
```

This example creates the DAG DAG3. DAG3 is configured to use the witness server MBX2 and the local directory C:\DAG3. DAG3 is assigned multiple static IP addresses because its DAG members are on different subnets on the MAPI network.

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

- In the Exchange Management Shell, run the following command to verify the DAG was created and to display DAG property information.

  ```powershell
  Get-DatabaseAvailabilityGroup <DAGName> | Format-List
  ```

## For more information

[Database availability groups](../../high-availability/database-availability-groups/database-availability-groups.md)

[Configure database availability group properties](configure-dag-properties.md)

[Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup)

[New-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/new-databaseavailabilitygroup)

[New-DatabaseAvailabilityGroupNetwork](https://docs.microsoft.com/powershell/module/exchange/new-databaseavailabilitygroupnetwork)

[Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/add-databaseavailabilitygroupserver)
