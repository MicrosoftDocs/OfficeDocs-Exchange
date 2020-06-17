---
localization_priority: Normal
description: 'Summary: Step-by-step guidance for restoring a recovery database in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d64c18e7-16af-4bd8-a5c5-01206984d4d1
ms.reviewer:
title: Restore data using a recovery database
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Restore data using a recovery database

A recovery database (RDB) is a special kind of mailbox database that allows you to mount and extract data from a restored mailbox database as part of a recovery operation. RDBs allow you to recover data from a backup or copy of a database without disrupting user access to current data.

After you create an RDB, you can restore a mailbox database into the RDB by using a backup application or by copying a database and its log files into the RDB folder structure. Then you can use the [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/new-mailboxrestorerequest) cmdlet to extract data from the recovered database. Once extracted, the data can then be exported to a folder or merged into an existing mailbox.

For additional management tasks related to RDBs, see [Recovery databases](recovery-databases.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute, plus the time it takes to put the database into a clean shutdown state and to extract the data.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- Some backup applications have the ability to restore Exchange data directly to a recovery database. Windows Server Backup can restore only file-level backups to a recovery database. It cannot be used to restore application-level backups to a recovery database.

- The database and log files containing the recovered data must be restored or copied into the RDB folder structure.

- The database must be in a clean shutdown state. Because an RDB is an alternate restore location for all databases, all restored databases will be in a dirty shutdown state. You must use **Eseutil /R** to put restored databases into a clean shutdown state.

## Use the Exchange Management Shell to recover data using a recovery database

1. Copy a recovered database and its log files, or restore a database and it log files, to the location you will use for your recovery database.

2. Use Eseutil to bring that database into a clean shutdown state. In the following example, EXX is the log generation prefix for the database (for example, E00, E01, E02, and so on).

   ```powershell
   Eseutil /R EXX /l <RDBLogFilePath> /d <RDBEdbFolder>
   ```

   The following example illustrates a log generation prefix of E01 and a recovery database and log file path of E:\Databases\RDB1:

   ```powershell
   Eseutil /R E01 /l E:\Databases\RDB1 /d E:\Databases\RDB1
   ```

3. Create a recovery database. Give the recovery database a unique name, but use the name and path of the database file for the EdbFilePath parameter, and the location of the recovered log files for the LogFolderPath parameter.

   ```powershell
   New-MailboxDatabase -Recovery -Name <RDBName> -Server <ServerName> -EdbFilePath <RDBPathandFileName> -LogFolderPath <LogFilePath>
   ```

   The following example illustrates creating a recovery database that will be used to recover DB1.edb and its log files, which are located at E:\Databases\RDB1.

   ```powershell
   New-MailboxDatabase -Recovery -Name <RDBName> -Server <ServerName> -EdbFilePath "E:\Databases\RDB1\DB1.EDB" -LogFolderPath "E:\Databases\RDB1"
   ```

4. Restart the Microsoft Exchange Information Store service:

   ```powershell
   Restart-Service MSExchangeIS
   ```

5. Mount the recovery database:

   ```powershell
   Mount-database <RDBName>
   ```

6. Verify that the mounted database contains the mailbox(es) you want to restore:

   ```powershell
   Get-MailboxStatistics -Database <RDBName> | Format-Table DisplayName,MailboxGUID -AutoSize
   ```

7. Use the New-MailboxRestoreRequest cmdlet to restore a mailbox or items from the recovery database to a production mailbox.

   The following example restores the source mailbox that has the MailboxGUID 1d20855f-fd54-4681-98e6-e249f7326ddd on mailbox database DB1 to the target mailbox with the alias Morris.

   ```powershell
   New-MailboxRestoreRequest -SourceDatabase DB1 -SourceStoreMailbox 1d20855f-fd54-4681-98e6-e249f7326ddd -TargetMailbox Morris
   ```

   The following example restores the content of the source mailbox that has the display name Morris Cornejo on mailbox database DB1 to the archive mailbox for Morris@contoso.com.

   ```powershell
   New-MaiboxRestoreRequest -SourceDatabase DB1 -SourceStoreMailbox "Morris Cornejo" -TargetMailbox Morris@contoso.com -TargetIsArchive
   ```

8. Periodically check the status of the Mailbox restore request using [Get-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/get-mailboxrestorerequest).

   Once the restore has a status of Completed, remove the restore request using [Remove-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxrestorerequest). For example:

   ```powershell
   Get-MailboxRestoreRequest -Status Completed | Remove-MailboxRestoreRequest
   ```

## How do you know this worked?

To verify that you have successfully recovered the mailbox data, open the target mailbox using Outlook or Outlook Web App and verify that the recovered data is present.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
