---
title: 'AutoReseed: Exchange 2013 Help'
TOCTitle: AutoReseed
ms:assetid: 61f9a8be-070e-4c62-b505-52644fcff0c5
ms:mtpsurl: https://technet.microsoft.com/library/Dn789209(v=EXCHG.150)
ms:contentKeyID: 62523707
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# AutoReseed

_**Applies to:** Exchange Server 2013 SP1_

Automatic Reseed, or AutoReseed, is a feature that's the replacement for what is normally administrator-driven action in response to a disk failure, database corruption event, or other issue that necessitates a reseed of a database copy. AutoReseed is designed to automatically restore database redundancy after a disk failure by using spare disks that have been provisioned on the system.

## Overview of Autoreseed

In an AutoReseed configuration, a standardized storage presentation structure is used, and the administrator picks the starting point. AutoReseed is about restoring redundancy as soon as possible after a drive fails. This involves pre-mapping a set of volumes (including spare volumes) and databases using mount points. In the event of a disk failure where the disk is no longer available to the operating system, or is no longer writable, a spare volume is allocated by the system, and the affected database copies are reseeded automatically.

1. The Microsoft Exchange Replication service periodically scans for copies that have a status of FailedAndSuspended. If all database copies on a volume configured for AutoReseed are in a FailedandSuspended state for 15 consecutive minutes, the AutoReseed workflow is initiated.

2. AutoReseed will try to resume the failed and suspended copies up to three times, with a 5-minute sleep in between each attempt. Sometimes, after a FailedandSuspended database copy is resumed, the copy remains in a Failed state. This can happen for a variety of reasons, so this step is designed to handle those cases; AutoReseed will automatically suspend a database copy that has been Failed for 10 consecutive minutes to keep the workflow running. If the suspend and resume actions don't result in a healthy database copy, the workflow continues.

3. When it finds a copy with that status, it performs some prerequisite checks. For example, it will verify that a spare disk is available, that the database and its log files are configured on the same volume, and in the appropriate locations that match the required naming conventions.

4. If the prerequisite checks pass successfully, the Disk Reclaimer function within the Microsoft Exchange Replication service allocates, remaps and formats a spare disk according to the timelines in the table below. AutoReseed will attempt to assign a spare volume up to 5 times, with 1-hour sleeps in between each try.

5. Once a spare has been assigned, AutoReseed will perform an InPlaceSeed operation using the SafeDeleteExistingFiles seeding switch. All databases that were on the affected disk are re-seeded using the active copy of the database as the seeding source.

6. After the seeding operation has been completed, the Microsoft Exchange Replication service verifies that the newly seeded copy is healthy.

Once all retries are exhausted, the workflow stops. If, after 3 days, the database copy is still FailedandSuspended, the workflow state is reset and it starts again from Step 1. This reset/resume behavior is useful (and intentional) since it can take a few days to replace a failed disk, controller, etc.

At this point, if the failure was a disk failure, it would require manual intervention by an operator or administrator to remove and replace the failed disk and reconfigure the replacement disk as a spare.

AutoReseed is configured using three properties of the DAG. Two of the properties refer to the two mount points that are in use. Exchange 2013 leverages the fact that Windows Server allows multiple mount points per volume. The *AutoDagVolumesRootFolderPath* property refers to the mount point that contains all of the available volumes. This includes volumes that host databases and spare volumes. The *AutoDagDatabasesRootFolderPath* property refers to the mount point that contains the databases. A third DAG property, *AutoDagDatabaseCopiesPerVolume*, is used to configure the number of database copies per volume.

An example AutoReseed configuration is illustrated below.

**Example AutoReseed configuration**

![Example Automatic Reseed Configuration](images/Dn789209.e3af7306-f5b4-4ec4-9ccf-222ec452699b(EXCHG.150).gif "Example Automatic Reseed Configuration")

In this example, there are three volumes, two of which will contain databases (VOL1 and VOL2), and one of which is a blank, formatted spare (VOL3).

To configure AutoReseed:

1. All three volumes are mounted under a single mount point. In this example, a mount point of C:\\ExchVols is used. This represents the directory used to get storage for Exchange databases.

2. The root directory of the mailbox databases is mounted as another mount point. In this example, a mount point of C:\\ExchDBs is used. Next, a directory structure is created so that a parent directory is created for the database, and under the parent directory, two subdirectories are created: one database file and one for the log files.

3. Databases are created. The above example illustrates a simple design using a single database per volume. Thus, on VOL1, there are three directories: the parent directory and two subdirectories (one for MDB1's database file, and one for its logs). Although not depicted in the example image, on VOL2, there would also be three directories: the parent directory, and under that, a directory for MDB2's database file, and one for its log files.

In this configuration, if MDB1 or MDB2 were to experience a failure, a copy of the failed database will be automatically reseeded to VOL3.

## Disk Reclaimer

The AutoReseed component that allocates and formats spare disks is called the *Disk Reclaimer*. The Disk Reclaimer component automatically formats spare disks in preparation for automatic reseeding at different intervals, depending on the state of the disk. In order for the Disk Reclaimer to format a disk, certain conditions must met:

  - The Disk Reclaimer must be enabled. It is enabled by default, but it can be disabled using [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup).

  - The volume must have a mount point in the root volumes path (by default, C:\\ExchangeVolumes).

  - The volume must not have any mount points in the database volumes path (by default, C:\\ExchangeDatabases).

  - If the volume contains any files, none of the files must have been touched for 24 hours.

In addition to the above conditions, the Disk Reclaimer will only attempt to format a given volume once a day. The following table describes the formatting behavior of the Disk Reclaimer.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>State of Disk and Database Copies</th>
<th>Formatting Interval</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source.</p></td>
<td><p>1 day</p></td>
</tr>
<tr class="even">
<td><p>Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, but there are no healthy active database copies in the local Active Directory site that can be used as a seeding source.</p></td>
<td><p>2 days</p></td>
</tr>
<tr class="odd">
<td><p>Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source, but there are unknown files outside of the database file (EDB file) and log files.</p></td>
<td><p>2 weeks</p></td>
</tr>
<tr class="even">
<td><p>Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source, but there are one or more database files (EDB files) for databases that are not present in Active Directory.</p></td>
<td><p>2 weeks</p></td>
</tr>
</tbody>
</table>
