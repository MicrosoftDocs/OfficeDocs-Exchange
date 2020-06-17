---
localization_priority: Normal
description: 'Summary: Automatically restore a database in Exchange Server 2016 or Exchange Server 2019 with pre-provisioned spare disks.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 61f9a8be-070e-4c62-b505-52644fcff0c5
ms.reviewer:
title: AutoReseed
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# AutoReseed

Automatic Reseed, or AutoReseed, is a feature that replaces standard actions administrators take in response to a disk failure, or a database corruption event, or another issue that necessitates a reseed of a database copy.

## Overview of Autoreseed

In an AutoReseed configuration, a standardized storage presentation structure is used, and the administrator picks the starting point. AutoReseed is about restoring redundancy as soon as possible after a drive fails. This involves using mount points to pre-map a set of volumes (including spare volumes) and databases. In the event of a disk failure where the disk is no longer available to the operating system, or is no longer writable, a spare volume is allocated by the system, and the affected database copies are reseeded automatically.

1. The Microsoft Exchange Replication service periodically scans for copies that have a status of FailedAndSuspended. If all database copies on a volume configured for AutoReseed are in a FailedandSuspended state for 15 consecutive minutes, the AutoReseed workflow is initiated.

2. AutoReseed will try to resume the failed and suspended copies up to three times, with a 5-minute sleep in between each attempt. Sometimes, after a FailedandSuspended database copy is resumed, the copy remains in a Failed state. This can happen for a variety of reasons, so this step is designed to handle those cases; AutoReseed will automatically suspend a database copy that has been Failed for 10 consecutive minutes to keep the workflow running. If the suspend and resume actions don't result in a healthy database copy, the workflow continues.

3. When it finds a copy with that status, it performs some prerequisite checks. For example, it will verify that a spare disk is available, that the database and its log files are configured on the same volume, and in the appropriate locations that match the required naming conventions.

4. If the prerequisite checks pass successfully, the Disk Reclaimer function within the Microsoft Exchange Replication service allocates, remaps and formats a spare disk according to the timelines in the table below. AutoReseed will attempt to assign a spare volume up to 5 times, with 1-hour sleeps in between each try.

5. Once a spare has been assigned, AutoReseed will perform an InPlaceSeed operation using the SafeDeleteExistingFiles seeding switch. All databases that were on the affected disk are re-seeded using the active copy of the database as the seeding source.

6. After the seeding operation has been completed, the Microsoft Exchange Replication service verifies that the newly seeded copy is healthy.

Once all retries are exhausted, the workflow stops. If, after 3 days, the database copy is still FailedandSuspended, the workflow state is reset and it starts again from Step 1. This reset/resume behavior is useful (and intentional) since it can take a few days to replace a failed disk, controller, etc.

At this point, if the failure was a disk failure, it would require manual intervention by an operator or administrator to remove and replace the failed disk and reconfigure the replacement disk as a spare.

AutoReseed is configured using three properties of the DAG. Two of the properties refer to the two mount points that are in use. Exchange Server leverages the fact that Windows Server allows multiple mount points per volume. The _AutoDagVolumesRootFolderPath_ property refers to the mount point that contains all of the available volumes. This includes volumes that host databases and spare volumes. The _AutoDagDatabasesRootFolderPath_ property refers to the mount point that contains the databases. A third DAG property, _AutoDagDatabaseCopiesPerVolume_, is used to configure the number of database copies per volume.

An example AutoReseed configuration is illustrated below.

 **Example AutoReseed configuration**

![Example Automatic Reseed Configuration](../../media/ITPro_Mailbox_AutoReseed.gif)

In this example, there are three volumes, two of which will contain databases (VOL1 and VOL2), and one of which is a blank, formatted spare (VOL3).

To configure AutoReseed:

1. All three volumes are mounted under a single mount point. In this example, a mount point of C:\ExchVols is used. This represents the directory used to get storage for Exchange databases.

2. The root directory of the mailbox databases is mounted as another mount point. In this example, a mount point of C:\ExchDBs is used. Next, a directory structure is created so that a parent directory is created for the database, and under the parent directory, two subdirectories are created: one database file and one for the log files.

3. Databases are created. The above example illustrates a simple design using a single database per volume. Thus, on VOL1, there are three directories: the parent directory and two subdirectories (one for MDB1's database file, and one for its logs). Although not depicted in the example image, on VOL2, there would also be three directories: the parent directory, and under that, a directory for MDB2's database file, and one for its log files.

In this configuration, if MDB1 or MDB2 were to experience a failure, a copy of the failed database will be automatically reseeded to VOL3.

### Disk Reclaimer

The AutoReseed component that allocates and formats spare disks is called the *Disk Reclaimer*. The Disk Reclaimer component automatically formats spare disks in preparation for automatic reseeding at different intervals, depending on the state of the disk. In order for the Disk Reclaimer to format a disk, certain conditions must met:

- The Disk Reclaimer must be enabled. It is enabled by default, but it can be disabled using [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/set-databaseavailabilitygroup).

- The volume must have a mount point in the root volumes path (by default, C:\ExchangeVolumes).

- The volume must not have any mount points in the database volumes path (by default, C:\ExchangeDatabases).

- If the volume contains any files, none of the files must have been touched for 24 hours.

In addition to the above conditions, the Disk Reclaimer will only attempt to format a given volume once a day. The following table describes the formatting behavior of the Disk Reclaimer.

|**State of Disk and Database Copies**|**Formatting Interval**|
|:-----|:-----|
|Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source.|1 day|
|Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, but there are no healthy active database copies in the local Active Directory site that can be used as a seeding source.|2 days|
|Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source, but there are unknown files outside of the database file (EDB file) and log files.|2 weeks|
|Disk is unformatted, or formatted but empty, or formatted but contains files that have not been touched for 24 hours, and there are healthy active database copies in the local Active Directory site that can be used as a seeding source, but there are one or more database files (EDB files) for databases that are not present in Active Directory.|2 weeks|
