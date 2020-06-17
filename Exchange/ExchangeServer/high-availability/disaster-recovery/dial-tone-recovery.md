---
localization_priority: Normal
description: 'Summary: How to perform a dial tone recovery in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 158817fa-4b17-4fa9-8341-a86609e6a388
ms.reviewer:
title: Perform a dial tone recovery
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Perform a dial tone recovery

The process for using dial tone portability is called a dial tone recovery, which involves creating an empty database on a Mailbox server to replace a failed database. To learn more, see [Dial tone portability](dial-tone-portability.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes, plus the time it takes to restore and move the data.

- You must have fewer than the maximum number of databases deployed to create a dial tone database (a maximum of five databases per server for Exchange Standard Edition, a maximum of 100 databases per server for Exchange Enterprise Edition).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to perform a dial tone recovery on a single server

> [!NOTE]
> You can't use the EAC to perform a dial tone recovery on a single server.

1. Make sure that any existing files for the database being recovered are preserved in case they're needed later for further recovery operations.

2. Use the [New-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/new-mailboxdatabase) cmdlet to create a dial tone database, as shown in this example.

   ```powershell
   New-MailboxDatabase -Name DTDB1 -EdbFilePath D:\DialTone\DTDB1.EDB
   ```

3. Use the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet to rehome the user mailboxes hosted on the database being recovered, as shown in this example.

   ```powershell
   Get-Mailbox -Database DB1 | Set-Mailbox -Database DTDB1
   ```

4. Use the [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/mount-database) cmdlet to mount the database so client computers can access the database and send and receive messages, as shown in this example.

   ```powershell
   Mount-Database -Identity DTDB1
   ```

5. Create a recovery database (RDB) and restore or copy the database and log files containing the data you want to recover into the RDB. For detailed steps, see [Create a recovery database](create-recovery-dbs.md).

6. After the data is copied to the RDB, but before mounting the restored database, copy any log files from the failed database to the recovery database log folder so they can be played against the restored database.

7. Mount the RDB, and then use the [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/dismount-database) cmdlet to dismount it, as shown in this example.

   ```powershell
   Mount-Database -Identity RDB1
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

13. Use the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) and [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/new-mailboxrestorerequest) cmdlets to export the data from the RDB and import it into the recovered database, as shown in this example. This will import all the messages sent and received using the dial tone database into the production database.

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

- [New-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/new-mailboxdatabase)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- [Mount-Database](https://docs.microsoft.com/powershell/module/exchange/mount-database)

- [Dismount-Database](https://docs.microsoft.com/powershell/module/exchange/dismount-database)

- [Remove-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxdatabase)

## How do you know this worked?

To verify that you've successfully moved a mailbox, do the following:

- Open the mailbox using Outlook on the web.

- Open the mailbox using Microsoft Outlook.
