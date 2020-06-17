---
localization_priority: Normal
description: 'Summary: When moving a mailbox database that has been copied to at least one other location, follow the procedures in this topic to move the path for the copy.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 324f255c-d95d-4a8a-a134-c8cee5c5b9cb
ms.reviewer:
title: Move the mailbox database path for a mailbox database copy
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Move the mailbox database path for a mailbox database copy

If the mailbox database being moved is replicated to one or more mailbox database copies, you must follow the procedure in this topic to move the mailbox database path. All copies of a mailbox database must be located in the same path on each server that hosts a copy. For example, if database DB1 is located at C:\mountpoints\DB1 on server EX1, copies of DB1 on servers EX2, EX3, and so on, must also be located at C:\mountpoints\DB1.

> [!NOTE]
> After you create a new mailbox database, you can move it to another volume, folder, location, or path by using either the EAC or the Exchange Management Shell. For step-by-step instructions about how to move the database path for a **non-replicated** mailbox database, see [Manage mailbox databases in Exchange Server](../../architecture/mailbox-servers/manage-databases.md).

Looking for other management tasks related to mailbox database copies? Check out [Managing mailbox database copies](https://docs.microsoft.com/exchange/high-availability/manage-ha/manage-database-copies).

## What do you need to know before you begin?

- Estimated time to complete this task: 2 minutes, plus the time to move the data, which depends on a variety of factors, such as the size of the database, the speed, available bandwidth and latency of the network, and storage speeds.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- To perform the move operation, the database must be temporarily dismounted, making it inaccessible to all users. If the database is currently dismounted, it isn't remounted upon completion.

- To perform the move operation, replication for the database must be disabled for all copies. It's not enough to suspend replication; you must disable it by using the [Remove-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxdatabasecopy) cmdlet to remove the database copies.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to move a replicated mailbox database to a new path

> [!NOTE]
> You can't use the Exchange admin center (EAC) to move a replicated mailbox database to a new path.

1. Note any replay lag or truncation lag settings for all copies of the mailbox database being moved. You can obtain this information by using the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase) cmdlet, as shown in this example.

   ```powershell
   Get-MailboxDatabase DB1 | Format-List *lag*
   ```

2. If circular logging is enabled for the database, it must be disabled before proceeding. You can disable circular logging for a mailbox database by using the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet, as shown in this example.

   ```powershell
   Set-MailboxDatabase DB1 -CircularLoggingEnabled $false
   ```

3. Remove all mailbox database copies for the database being moved. For detailed steps, see [Remove a mailbox database copy](remove-db-copies.md). After all copies are removed, preserve the database and transaction log files from each server from which the database copy is being removed by moving them to another location. These files are being preserved so the database copies don't require re-seeding after they have been re-added.

4. Move the mailbox database path to the new location. For detailed steps, see [Move a mailbox database path](../../architecture/mailbox-servers/manage-databases.md#move-a-mailbox-database-path).

   > [!IMPORTANT]
   > During the move operation, the database being moved must be dismounted. Until the move is complete, this process will cause an interruption in service and an outage for all users with mailboxes on the database being moved. After the move operation completes, the database is automatically mounted.

5. Create the necessary folder structure on each Mailbox server that previously contained a passive copy of the moved mailbox database. For example, if you moved the database to C:\mountpoints\DB1, you must create this same path on each Mailbox server that will host a mailbox database copy.

6. After creating the folder structure, move the passive copy of the mailbox database and its log stream to the new location. These are the files that were left from and preserved after Step 3. Repeat this process for each database copy that was removed in Step 3.

7. Add all of the database copies that were removed in Step 3. For detailed steps, see [Add a mailbox database copy](add-db-copies.md).

8. On each server that contains a copy of the mailbox database being moved, run the following command to stop and restart the content index services.

   ```powershell
   Restart-Service MSExchangeFastSearch
   ```

9. Optionally, enable circular logging by using the [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase) cmdlet, as shown in this example.

   ```powershell
   Set-MailboxDatabase DB1 -CircularLoggingEnabled $true
   ```

10. Reconfigure any previously set values for replay lag time and truncation lag time by using the [Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabasecopy) cmdlet, as shown in this example.

    ```powershell
    Set-MailboxDatabaseCopy DB1\MBX2 -ReplayLagTime 00:15:00
    ```

11. As each copy is added, we recommend that you verify the health and status of the copy prior to adding the next copy. You can verify the health and status by:

    1. Examining the event log for any error or warning events related to the database or the database copy.

    2. Using the [Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabasecopystatus) cmdlet to check the health and status of continuous replication for the database copy.

    3. Using the [Test-ReplicationHealth](https://docs.microsoft.com/powershell/module/exchange/test-replicationhealth) cmdlet to verify the health and status of the database availability group and continuous replication.

For detailed syntax and parameter information, see the following topics:

- [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase)

- [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabase)

- [Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabasecopy)

- [Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabasecopystatus)

- [Test-ReplicationHealth](https://docs.microsoft.com/powershell/module/exchange/test-replicationhealth)

## How do you know this worked?

To verify that you've successfully moved the path for a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the database that was copied. In the Details pane, the status of the database copy and its content index are displayed, along with the current copy queue length. Verify that the status is Healthy.

- In the Exchange Management Shell, run the following command to verify the mailbox database copy was created and is healthy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName>
  ```

    The Status and Content Index State should both be Healthy.
