---
title: 'Perform a dial tone recovery: Exchange 2013 Help'
TOCTitle: Perform a dial tone recovery
ms:assetid: 158817fa-4b17-4fa9-8341-a86609e6a388
ms:mtpsurl: https://technet.microsoft.com/library/Dd979810(v=EXCHG.150)
ms:contentKeyID: 50873788
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Perform a dial tone recovery

_**Applies to:** Exchange Server 2013_

Using dial tone portability, users can have a temporary mailbox for sending and receiving email while their original mailbox is being restored or repaired. The temporary mailbox can be on the same Exchange 2013 Mailbox server or on any other Exchange 2013 Mailbox server in your organization. The process for using dial tone portability is called a dial tone recovery, which involves creating an empty database on a Mailbox server to replace a failed database. To learn more, see [Dial tone portability](dial-tone-portability-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes, plus the time it takes to restore and move the data.

- You must have fewer than the maximum number of databases deployed to create a dial tone database. Exchange 2013 Standard Edition supports a maximum of five databases per server. Exchange 2013 Enterprise Edition supports a maximum of 50 databases per server in RTM and CU1, and 100 databases per server in CU2 and later.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to perform a dial tone recovery on a single server

> [!NOTE]
> You can't use the EAC to perform a dial tone recovery on a single server.

1. Make sure that any existing files for the database being recovered are preserved in case they're needed later for further recovery operations.

2. Use the [New-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/New-MailboxDatabase) cmdlet to create a dial tone database, as shown in this example.

    ```powershell
    New-MailboxDatabase -Name DTDB1 -EdbFilePath D:\DialTone\DTDB1.EDB
    ```

3. Use the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet to rehome the user mailboxes hosted on the database being recovered, as shown in this example.

    ```powershell
    Get-Mailbox -Database DB1 | Set-Mailbox -Database DTDB1
    ```

4. Use the [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/Mount-Database) cmdlet to mount the database so client computers can access the database and send and receive messages, as shown in this example.

    ```powershell
    Mount-Database -Identity DTDB1
    ```

5. Create a recovery database (RDB) and restore or copy the database and log files containing the data you want to recover into the RDB. For detailed steps, see [Create a recovery database](create-a-recovery-database-exchange-2013-help.md).

6. After the data is copied to the RDB, but before mounting the restored database, copy any log files from the failed database to the recovery database log folder so they can be played against the restored database.

7. Mount the RDB, and then use the [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/Dismount-Database) cmdlet to dismount it, as shown in this example.

    ```powershell
    Mount-Database -Identity RDB1
    ```

    ```powershell
    Dismount-Database -Identity RDB1
    ```

8. After the RDB is dismounted, move the current database and log files within the RDB folder to a safe location. This is done in preparation for swapping the recovered database with the dial tone database.

9. Dismount the dial tone database, as shown in this example. Note that your end users will experience an interruption in service when you dismount this database.

    ```powershell
    Dismount-Database -Identity DTDB1
    ```

10. Move the database and log files from the dial tone database folder into the RDB folder.

11. Move the database and log files from the safe location containing the recovered database into the dial tone database folder, and then mount the database, as shown in this example.

    ```powershell
    Mount-Database -Identity DTDB1
    ```

    This ends the service interruption for your end users. They will be able to access their original production database and send and receive messages.

12. Mount the RDB, as shown in this example.

    ```powershell
    Mount-Database -Identity RDB1
    ```

13. Use the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) and [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest) cmdlets to export the data from the RDB and import it into the recovered database, as shown in this example. This will import all the messages sent and received using the dial tone database into the production database.

    ```powershell
    $mailboxes = Get-Mailbox -Database DTDB1
    ```

    ```powershell
    $mailboxes | %{ New-MailboxRestoreRequest -SourceStoreMailbox $_.ExchangeGuid -SourceDatabase RDB1 -TargetMailbox $_ }
    ```

14. After the restore operation is complete, you can dismount and remove the RDB, as shown in this example.

    ```powershell
    Dismount-Database -Identity RDB1
    Remove-MailboxDatabase -Identity RDB1
    ```

For detailed syntax and parameter information, see the following topics:

- [New-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/New-MailboxDatabase)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox)

- [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/Mount-Database)

- [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/Dismount-Database)

- [Remove-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Remove-MailboxDatabase)

## How do you know this worked?

To verify that you've successfully moved a mailbox, do the following:

- Open the mailbox using Outlook Web App.

- Open the mailbox using Microsoft Outlook.
