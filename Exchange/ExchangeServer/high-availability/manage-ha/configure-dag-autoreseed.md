---
localization_priority: Normal
description: 'Summary: AutoReseed is a feature for quickly restoring database redundancy after a disk failure. If a disk fails, the database copies stored on that disk are automatically reseeded to a preconfigured spare disk on the Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4a8bd779-b52a-40ed-8040-4d76eabeb41e
ms.reviewer:
title: Configure AutoReseed for a database availability group
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure AutoReseed for a database availability group

Use the steps in this topic to configure AutoReseed for a database availability group (DAG) in Exchange Server.

> [!CAUTION]
> The AutoReseed feature doesn't perform any prerequisite configuration tasks for you. Installing disks correctly, adding spare disks to the system, replacing bad disks, and formatting new disks must be done manually by an administrator.

For additional management tasks related to DAGs, see [Manage database availability groups](manage-dags.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 10 minutes.

- To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Database availability groups" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- A single logical disk/partition per physical disk must be created.

- The specific database and log folder structure described in the steps below must be used.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Configure the root paths for databases and volumes

The first step involves configuring the root directories for the databases (_AutoDagDatabasesRootFolderPath_) and volumes (_AutoDagVolumesRootFolderPath_) used by the DAG. The defaults are C:\ExchangeDatabases, and C:\ExchangeVolumes, respectively. You can omit this step if you're using the default paths.

This example illustrates how to configure the root path for the databases.

```powershell
Set-DatabaseAvailabilityGroup DAG1 -AutoDagDatabasesRootFolderPath "C:\ExchDbs"
```

This example illustrates how to configure the root path for the storage volumes.

```powershell
Set-DatabaseAvailabilityGroup DAG1 -AutoDagVolumesRootFolderPath "C:\ExchVols"
```

### How do you know this step worked?

To verify that you've successfully configured the root paths for databases and volumes, run the following command.

```powershell
Get-DatabaseAvailabilityGroup DAG1 | Format-List *auto*
```

The output for _AutoDagDatabasesRootFolderPath_ and _AutoDagVolumesRootFolderPath_ should reflect the configured paths.

## Step 2: Configure the number of databases per volume

Next, configure the number of databases per volume (_AutoDagDatabaseCopiesPerVolume_) for the DAG.

This example illustrates how to configure this AutoReseed setting for a DAG configured with 4 databases per volume.

```powershell
Set-DatabaseAvailabilityGroup DAG1 -AutoDagDatabaseCopiesPerVolume 4
```

### How do you know this step worked?

To verify that you've successfully configured the number of databases per volume, run the following command.

```powershell
Get-DatabaseAvailabilityGroup DAG1 | Format-List *auto*
```

The output for _AutoDagDatabaseCopiesPerVolume_ should reflect the configured value.

## Step 3: Create the root directories for databases and volumes

Next, create the directories that correspond to the root directories you configured in Step 1. This example shows how to create the default directories using the command prompt.

```powershell
md C:\ExchangeDatabases
md C:\ExchangeVolumes
```

### How do you know this step worked?

To verify that you've successfully configured the root directories for databases and volumes, run the following command.

```powershell
Dir C:\
```

The created directories should appear in the output list.

## Step 4: Mount the volume folders

For every volume that will be used for databases (including spare volumes), use the Windows Disk Management application (diskmgmt.msc) to mount each volume in a mounted folder under C:\ExchangeVolumes\. For example, if there are 2 volumes with databases and 1 spare volume, mount the volumes to the following mounted folders:

- C:\ExchangeVolumes\Volume1

- C:\ExchangeVolumes\Volume2

- C:\ExchangeVolumes\Volume3

The names of the mounted folders can be any folder name, as long as the folders are mounted under the root volume's path.

### How do you know this step worked?

To verify that you've successfully mounted the volume folders, run the following command.

```powershell
Dir C:\
```

The mounted volumes should appear in the output list.

## Step 5: Create the database folders

Next, create the database directories under the root path C:\ExchangeDatabases. This example illustrates how to create directories for a storage configuration with 4 databases on each volume.

```powershell
md c:\ExchangeDatabases\db001
```

```powershell
md c:\ExchangeDatabases\db002
```

```powershell
md c:\ExchangeDatabases\db003
```

```powershell
md c:\ExchangeDatabases\db004
```

### How do you know this step worked?

To verify that you've successfully mounted the database folders, run the following command.

```powershell
Dir C:\ExchangeDatabases
```

The created directories should appear in the output list.

## Step 6: Create the mount points for the databases

Create the mount points for each database and link the mount point to the correct volume. For example, the mounted folder for db001 should be at C:\ExchangeDatabases\db001. You can use diskmgmt.msc or mountvol.exe to do this. This example illustrates how to mount db001 to C:\ExchangeDatabases\db001 using mountvol.exe.

```powershell
Mountvol.exe c:\ExchangeDatabases\db001 \\?\Volume (GUID)
```

### How do you know this step worked?

To verify that you've successfully created the mount points for the database, run the following command.

```powershell
Mountvol.exe C:\ExchangeDatabases\db001 /L
```

The mounted volume should appear in the mount point list.

## Step 7: Create the database directory structure

Next, create two directories underneath the folders you created in Step 5, one for each database and one for each of the database's log stream that will be stored on the same volume. You must use the following format for your directory structure:

C:\\<*DatabaseFolderName* \>\ *DatabaseName* \\<*DatabaseName* \>.db

C:\\<*DatabaseFolderName* \>\ *DatabaseName* \\<*DatabaseName* \>.log

This example illustrates how to create directories for 4 databases that will be stored on Volume 1:

```powershell
md c:\ExchangeDatabases\db001\db001.db
```

```powershell
md c:\ExchangeDatabases\db001\db001.log
```

```powershell
md c:\ExchangeDatabases\db002\db002.db
```

```powershell
md c:\ExchangeDatabases\db002\db002.log
```

```powershell
md c:\ExchangeDatabases\db003\db003.db
```

```powershell
md c:\ExchangeDatabases\db003\db003.log
```

```powershell
md c:\ExchangeDatabases\db004\db004.db
```

```powershell
md c:\ExchangeDatabases\db004\db004.log
```

Repeat the preceding commands for databases on every volume.

### How do you know this step worked?

To verify that you've successfully created the database directory structure, run the following command.

```powershell
Dir C:\ExchangeDatabases /s
```

The created directories should appear in the output list.

## Step 8: Create databases

Create databases with log and database paths configured with the appropriate folders. This example illustrates how to create a database that's stored in the newly created directory and mount point structure.

```powershell
New-MailboxDatabase -Name db001 -Server MBX1 -LogFolderPath C:\ExchangeDatabases\db001\db001.log -EdbFilePath C:\ExchangeDatabases\db001\db001.db\db001.edb
```

### How do you know this step worked?

To verify that you've successfully created databases in the appropriate folder, run the following command.

```powershell
Get-MailboxDatabase db001 | Format List *path*
```

Database properties that are returned should indicate that the database file and log files are being stored in the above folders.

## How do you know this task worked?

To verify that you've configured AutoReseed for a DAG, do the following:

1. Run the following command to verify the DAG is configured correctly.

   ```powershell
   Get-DatabaseAvailabilityGroup DAG1 | Format-List *auto*
   ```

2. Run the following command to verify the directory structure is configured correctly (below are the default paths; if necessary, substitute the paths for the paths you're using).

   ```powershell
   Dir c:\ExchangeDatabases /s
   ```

   ```powershell
   Dir c:\ExchangeVolumes /s
   ```
