---
localization_priority: Normal
description: 'Summary: Database portability is a feature that enables an Exchange Server 2016 or Exchange 2019 mailbox database to be moved to or mounted on any other Mailbox server in the same organization running Exchange 2016 or Exchange 2019 respectively, provided the target Mailbox server has databases with the same database schema version.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a765ead1-43bc-4786-ae93-1835cacfc8fc
ms.reviewer:
title: Move a mailbox database using database portability
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Move a mailbox database using database portability

Database portability can help reduce overall recovery times for some failure scenarios. By using database portability, reliability is improved by removing several error-prone, manual steps from the recovery processes. Note that Mailbox databases from previous versions of Exchange can't be moved to a Mailbox server running Exchange 2016 or Exchange 2019.

> [!NOTE]
> When using database portability to recover a mailbox database, the operating system version and the Exchange Server version on the source and target Exchange servers must be the same. For example, if an Exchange 2016 mailbox database was previously mounted on a server running Windows Server 2016, database portability will only work when migrating the database to a server also running Windows Server 2016 and Exchange 2016.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes, plus the time it takes to restore the data, move the database files, and wait for Active Directory replication to complete.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- You can't use the EAC to move user mailboxes to a recovered or dial tone database using database portability.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to move user mailboxes to a recovered or dial tone database using database portability

1. Verify that the database to be moved is in a clean shutdown state. If the database isn't in a clean shutdown state, perform a soft recovery.

   > [!NOTE]
   > When you perform a soft recovery, any uncommitted log files are committed to the database. If you don't have all of the required log files, you can't complete the soft recovery process. Proceed to step 2.

   To commit all uncommitted log files to the database, from a command prompt, run the following command.

   ```powershell
   ESEUTIL /R <Enn>
   ```

   > [!NOTE]
   > \<E _nn_\> specifies the log file prefix for the database into which you intend to replay the log files. The log file prefix specified by \<E _nn_\> is a required parameter for Eseutil /r.

2. Create a database on a server using the following syntax:

   ```powershell
   New-MailboxDatabase -Name <DatabaseName> -Server <ServerName> -EdbFilePath <DatabaseFileNameandPath> -LogFolderPath <LogFilesPath>
   ```

3. Set the _This database can be over written by restore_ attribute using the following syntax:

   ```powershell
   Set-MailboxDatabase <DatabaseName> -AllowFileRestore $true
   ```

4. Move the original database files (.edb file, log files, and Exchange Search catalog) to the database folder you specified when you created the new database above.

5. Mount the database using the following syntax:

   ```powershell
   Mount-Database <DatabaseName>
   ```

6. After the database is mounted, modify the user account settings with the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet so that the account points to the mailbox on the new mailbox server. To move all of the users from the old database to the new database, use the following syntax.

   ```powershell
   Get-Mailbox -Database <SourceDatabase> |where {$_.ObjectClass -NotMatch '(SystemAttendantMailbox|ExOleDbSystemMailbox)'}| Set-Mailbox -Database <TargetDatabase>
   ```

7. Trigger delivery of any messages remaining in queues using the following syntax.

   ```powershell
   Get-Queue <QueueName> | Retry-Queue -Resubmit $true
   ```

After Active Directory replication is complete, all users can access their mailboxes on the new Exchange server. Most clients are redirected via Autodiscover. Outlook on the web users are also automatically redirected.

## How do you know this worked?

To verify that you've successfully moved a mailbox, do the following:

- Open the mailbox using Outlook on the web.

- Open the mailbox using Microsoft Outlook.
