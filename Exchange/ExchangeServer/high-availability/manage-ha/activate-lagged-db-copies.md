---
localization_priority: Normal
description: 'Summary: About lagged mailbox database copies and how to activate them in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 493d9c40-644d-49d6-9291-949acbcfdcb6
ms.reviewer:
title: Activate a lagged mailbox database copy
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Activate a lagged mailbox database copy

A lagged mailbox database copy is a mailbox database copy configured with a replay lag time value greater than 0. If you want the database to replay all log files and make the database copy current, activating and recovering a lagged mailbox database copy is a simple process. However, if you want to replay log files up to a specific point in time, it's a more difficult operation because you have to manually manipulate log files and run Eseutil.

Looking for other information related to lagged mailbox database copies? Check out [Manage mailbox database copies](manage-database-copies.md)

> [!NOTE]
> The amount of time it takes to activate a lagged mailbox database copy directly depends on how many log files need to be replayed and how fast the hardware can replay them. At a minimum, you should experience a log replay rate of at least two logs per second per database.

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute, plus the time it takes to duplicate the lagged copy, replay the necessary log files, and extract the data or mount the database for client activity.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- The mailbox database copy being activated must be configured with a replay lag time greater than 0.

- The mailbox database copy being activated must have all log files to the point in time to which you want to recover it. Keep in mind that database transactions can span multiple log files when determining the point in time to which you want to recover.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to activate a lagged mailbox database copy to a specific point in time

> [!NOTE]
> You can't use the EAC to activate a lagged mailbox database copy to a specific point in time. Instead, you perform a series of steps using the Exchange Management Shell and the command line.

1. This example suspends replication for the lagged copy being activated by using the [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/suspend-mailboxdatabasecopy) cmdlet.

   ```powershell
   Suspend-MailboxDatabaseCopy DB1\EX3 -SuspendComment "Activate lagged copy of DB1 on Server EX3" -Confirm:$false
   ```

2. Optionally (to preserve a lagged copy), make a copy of the database copy and its log files.

   > [!NOTE]
   > At this point, continuing to perform this procedure on the existing volume would incur a copy on write performance penalty. As an alternative, you can copy the database and log files to another volume to perform the recovery.

3. Determine which log files are required to replay into the database to meet your point-in-time requirement for this recovery (based on log file date and time, as shown in Windows Explorer). All logs created after this point should be moved to a different directory, until the recovery process is completed, and the logs are no longer needed.

4. Delete the checkpoint (.chk) file for the database.

5. This example uses Eseutil to perform the recovery operation.

   ```powershell
   Eseutil.exe /r eXX /a
   ```

   > [!NOTE]
   > • In the preceding example, e _XX_ is the log generation prefix for the database (for example, E00, E01, E02, and so on). <br/><br/>• This step may take a considerable amount of time, depending on several factors, such as the length of the replay lag time, the number of log files generated during that period, and the speed at which your hardware can replay those logs into the database being recovered.

6. After log replay is finished, the database is in a clean shutdown state and can be copied and used for recovery purposes.

7. After the recovery process is complete, this example resumes replication for the database that was used as part of the recovery process.

   ```powershell
   Resume-MailboxDatabaseCopy DB1\EX3
   ```

For detailed syntax and parameter information, see [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/suspend-mailboxdatabasecopy) or [Resume-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/resume-mailboxdatabasecopy).

## Use the Exchange Management Shell to activate a lagged mailbox database copy by replaying all uncommitted log files

1. Optionally (to preserve a lagged copy), make a copy of the database copy and its log files.

1. This example suspends replication for the lagged copy being activated by using the [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/suspend-mailboxdatabasecopy) cmdlet.

   ```powershell
   Suspend-MailboxDatabaseCopy DB1\EX3 -SuspendComment "Activate lagged copy of DB1 on Server EX3" -Confirm:$false
   ```

2. Optionally (to preserve a lagged copy), make a copy of the database copy and its log files.

     > [!NOTE]
     > At this point, continuing to perform this procedure on the existing volume would incur a copy on write performance penalty. If this isn't desirable, you can copy the database and log files to another volume to perform the recovery.

2. This example activates the lagged mailbox database copy using the [Move-ActiveMailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/move-activemailboxdatabase) cmdlet with the _SkipLagChecks_ parameter.

  ```powershell
  Move-ActiveMailboxDatabase DB1 -ActivateOnServer EX3 -SkipLagChecks
  ```

## Use the Exchange Management Shell to activate a lagged mailbox database copy by using SafetyNet recovery

1. Optionally (to preserve a lagged copy), take a file system-based (non-Exchange aware) Volume Shadow Copy Service (VSS) snapshot of the volumes containing the database copy and its log files.

2. This example suspends replication for the lagged copy being activated by using the [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/suspend-mailboxdatabasecopy) cmdlet.

   ```powershell
   Suspend-MailboxDatabaseCopy DB1\EX3 -SuspendComment "Activate lagged copy of DB1 on Server EX3" -Confirm:$false
   ```

3. Optionally (to preserve a lagged copy), make a copy of the database copy and its log files.

   > [!NOTE]
   > At this point, continuing to perform this procedure on the existing volume would incur a copy-on-write performance penalty. If this isn't desirable, you can copy the database and log files to another volume to perform the recovery.

4. Determine the required logs for the lagged database copy by looking for the "Log Required:" value in ESEUTIL database header output

   ```powershell
   Eseutil /mh <DBPath> | findstr /c:"Log Required"
   ```

   Take note of the hexadecimal numbers in parentheses. The first number is the lowest generation required (referred to as LowGeneration), and the second number is the highest generation required (referred to as HighGeneration). Move all log generation files that have a generation sequence greater than HighGeneration to a different location so that they are not replayed into the database.

5. On the server hosting the active copy of database, either delete the log files for the lagged copy being activated from the active copy, or stop the Microsoft Exchange Replication service.

4. Perform a database switchover and activate the lagged copy. This example activates the database by using the [Move-ActiveMailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/move-activemailboxdatabase) cmdlet with several parameters.

   ```powershell
   Move-ActiveMailboxDatabase DB1 -ActivateOnServer EX3 -MountDialOverride BestEffort -SkipActiveCopyChecks -SkipClientExperienceChecks -SkipHealthChecks -SkipLagChecks
   ```

6. At this point, the database will automatically mount and request redelivery of missing messages from SafetyNet.

## How do you know this worked?

To verify that you've successfully activated a lagged mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the appropriate database, and in the Details pane, click **View details** to view the database copy properties.

- In the Exchange Management Shell, run the following command to display status information for a database copy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName> | Format-List
  ```
