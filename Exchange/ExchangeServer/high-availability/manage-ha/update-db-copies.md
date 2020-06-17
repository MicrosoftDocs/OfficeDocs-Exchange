---
localization_priority: Normal
description: 'Summary: How to update, or seed , a mailbox database copy in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bead3cc5-7d50-446f-95b7-e432bcb7968e
ms.reviewer:
title: Update a mailbox database copy
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Update a mailbox database copy

Updating, also known as *seeding*, is the process in which a copy of a mailbox database is added to another Mailbox server in a database availability group (DAG). The newly added copy becomes the baseline database for the passive copy into which log files copied from the active copy are replayed. Seeding is required under the following conditions:

- When a new passive copy of a database is created. Seeding can be postponed for a new mailbox database copy, but eventually each passive database copy must be seeded to function as a redundant database copy.

- After a failover occurs in which data is lost as a result of the passive database copy having become diverged and unrecoverable.

- When the system has detected a corrupted log file that can't be replayed into the passive copy of the database.

- After an offline defragmentation of any copy of the database occurs.

- After the log generation sequence for the database has been reset back to 1.

You can perform seeding by using the following methods:

- **Automatic seeding**: An automatic seed produces a passive copy of the active database on the target Mailbox server. Automatic seeding occurs during the creation of a database.

- **Seeding using the Update-MailboxDatabaseCopy cmdlet**: You can use the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet in the Exchange Management Shell to seed a database copy at any time.

- **Seeding using the Update Mailbox Database Copy wizard**: You can use the Update Mailbox Database Copy wizard in the EAC to seed a database copy at any time.

- **Manually copying the offline database**: You can dismount the active copy of the database and copy the database file to the same location on another Mailbox server in the same DAG. If you use this method, there will be an interruption in service because the process requires you to dismount the database.

Updating a database copy can take a long time, especially if the database being copied is large, or if there is high network latency or low network bandwidth. After the seeding process has started, don't close the EAC or the Exchange Management Shell until the process has completed. If you do, the seeding operation will be terminated.

A database copy can be seeded using either the active copy or an up-to-date passive copy as the source for the seed. When seeding from a passive copy, be aware that the seed operation will terminate with a network communication error under the following circumstances:

- If the status of the seeding source copy changes to Failed or FailedAndSuspended.

- If the database fails over to another copy.

Multiple database copies can be seeded simultaneously. However, when seeding multiple copies simultaneously, you must seed only the database file, and omit the content index catalog. You can do this by using the _DatabaseOnly_ parameter with the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet.

> [!NOTE]
> If you don't use the _DatabaseOnly_ parameter when seeding multiple targets from the same source, the task will fail with `SeedInProgressException` error `FE1C6491`.

Looking for other management tasks related to mailbox database copies? Check out [Manage mailbox database copies](manage-database-copies.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 2 minutes, plus the time to seed the database copy, which depends on a variety of factors, such as the size of the database, the speed, available bandwidth and latency of the network, and storage speeds.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- The mailbox database copy must be suspended. For detailed steps, see [Suspend or resume a mailbox database copy](suspend-resume-db-copies.md).

- The Remote Registry service must be running on the server hosting the passive database copy you're updating.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Update a mailbox database copy

### Use the EAC to update a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the mailbox database whose passive copy you want to update.

3. In the Details pane, under **Database Copies**, click **Suspend** under the passive database copy you want to seed. Provide any optional comments, and click **save**.

4. In the Details pane, under **Database Copies**, click **Update** under the passive database copy you want to seed.

5. By default, the active copy of the database is used as the source database for seeding. If you prefer to use a passive copy of the database for seeding, click **browse...** to select the server containing the passive database copy you want to use for the source.

6. Click **save** to update the passive database copy.

### Use the Exchange Management Shell to update a mailbox database copy

This example shows how to seed a copy of the database DB1 on MBX1.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1
```

This example shows how to seed a copy of the database DB1 on MBX1 using MBX2 as the source Mailbox server for the seed.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1 -SourceServer MBX2
```

This example shows how to seed a copy of the database DB1 on MBX1 without seeding the content index catalog.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1 -DatabaseOnly
```

This example shows how to seed the content index catalog for the copy of the database DB1 on MBX1 without seeding the database file.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1 -CatalogOnly
```

## Manually copy an offline database

1. If circular logging is enabled for the database, it must be disabled before proceeding. You can disable circular logging for a mailbox database by using the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet, as shown in this example.

   ```powershell
   Set-MailboxDatabase DB1 -CircularLoggingEnabled $false
   ```

2. Dismount the database. You can use the [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/dismount-database) cmdlet, as shown in this example.

   ```powershell
   Dismount-Database DB1 -Confirm $false
   ```

3. Manually copy the database files (the database file and all log files) to a second location, such as an external disk drive or a network share.

4. Mount the database. You can use the [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/mount-database) cmdlet, as shown in this example.

   ```powershell
   Mount-Database DB1
   ```

5. On the server that will host the copy, copy the database files from the external drive or network share to the same path as the active database copy. For example, if the active copy database path is D:\DB1\DB1.edb and log file path is D:\DB1, you would copy the database files to D:\DB1 on the server that will host the copy.

6. Add the mailbox database copy by using the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) cmdlet with the _SeedingPostponed_ parameter, as shown in this example.

   ```powershell
   Add-MailboxDatabaseCopy -Identity DB1 -MailboxServer MBX3 -SeedingPostponed
   ```

7. If circular logging is enabled for the database, enable it again by using the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet, as shown in this example.

   ```powershell
   Set-MailboxDatabase DB1 -CircularLoggingEnabled $true
   ```

## How do you know this worked?

To verify that you've successfully seeded a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the database that was seeded. In the Details pane, the status of the database copy and its content index are displayed, along with the current copy queue length.

- In the Exchange Management Shell, run the following command to verify the mailbox database copy was seeded successfully and is healthy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName>
  ```

  The Status and Content Index State should both be Healthy.
