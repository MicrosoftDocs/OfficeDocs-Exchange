---
localization_priority: Normal
description: 'Summary: How to perform a dial tone recovery in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 158817fa-4b17-4fa9-8341-a86609e6a388
ms.date: 7/9/2018
ms.reviewer: 
title: Perform a dial tone recovery
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

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

2. Use the [New-MailboxDatabase](http://technet.microsoft.com/library/5008090b-e776-4ff6-807c-208e00f4daab.aspx) cmdlet to create a dial tone database, as shown in this example.

  ```
  New-MailboxDatabase -Name DTDB1 -EdbFilePath D:\DialTone\DTDB1.EDB
  ```

3. Use the [Set-Mailbox](http://technet.microsoft.com/library/a0d413b9-d949-4df6-ba96-ac0906dedae2.aspx) cmdlet to rehome the user mailboxes hosted on the database being recovered, as shown in this example.

  ```
  Get-Mailbox -Database DB1 | Set-Mailbox -Database DTDB1
  ```

4. Use the [Mount-Database](http://technet.microsoft.com/library/76a57f6a-a6c6-4c65-abf8-190522d47037.aspx) cmdlet to mount the database so client computers can access the database and send and receive messages, as shown in this example.

  ```
  Mount-Database -Identity DTDB1
  ```

5. Create a recovery database (RDB) and restore or copy the database and log files containing the data you want to recover into the RDB. For detailed steps, see [Create a recovery database](create-recovery-dbs.md).

6. After the data is copied to the RDB, but before mounting the restored database, copy any log files from the failed database to the recovery database log folder so they can be played against the restored database.

7. Mount the RDB, and then use the [Dismount-Database](http://technet.microsoft.com/library/e261955b-a9f0-4d87-bf56-f9e67ea5ba3f.aspx) cmdlet to dismount it, as shown in this example.

  ```
  Mount-Database -Identity RDB1
  Dismount-Database -Identity RDB1
  ```

8. After the RDB is dismounted, move the current database and log files within the RDB folder to a safe location. This is done in preparation for swapping the recovered database with the dial tone database.

9. Dismount the dial tone database, as shown in this example. Note that your end users will experience an interruption in service when you dismount this database.

  ```
  Dismount-Database -Identity DTDB1
  ```

10. Move the database and log files from the dial tone database folder into the RDB folder.

11. Move the database and log files from the safe location containing the recovered database into the dial tone database folder, and then mount the database, as shown in this example.

  ```
  Mount-Database -Identity DTDB1
  ```

    This ends the service interruption for your end users. They will be able to access their original production database and send and receive messages.

12. Mount the RDB, as shown in this example.

  ```
  Mount-Database -Identity RDB1
  ```

13. Use the [Get-Mailbox](http://technet.microsoft.com/library/8a5a6eb9-4a75-47f9-ae3b-a3ba251cf9a8.aspx) and [New-MailboxRestoreRequest](http://technet.microsoft.com/library/0b67defd-3c6c-4470-acfa-7f22a6c1d2bd.aspx) cmdlets to export the data from the RDB and import it into the recovered database, as shown in this example. This will import all the messages sent and received using the dial tone database into the production database.

  ```
  $mailboxes = Get-Mailbox -Database DTDB1
  ```

  ```
  $mailboxes | %{ New-MailboxRestoreRequest -SourceStoreMailbox $_.ExchangeGuid -SourceDatabase RDB1 -TargetMailbox $_ }
  ```

14. After the restore operation is complete, you can dismount and remove the RDB, as shown in this example.

  ```
  Dismount-Database -Identity RDB1
  Remove-MailboxDatabase -Identity RDB1
  ```

For detailed syntax and parameter information, see the following topics:

- [New-MailboxDatabase](http://technet.microsoft.com/library/5008090b-e776-4ff6-807c-208e00f4daab.aspx)

- [Get-Mailbox](http://technet.microsoft.com/library/8a5a6eb9-4a75-47f9-ae3b-a3ba251cf9a8.aspx)

- [Set-Mailbox](http://technet.microsoft.com/library/a0d413b9-d949-4df6-ba96-ac0906dedae2.aspx)

- [Mount-Database](http://technet.microsoft.com/library/76a57f6a-a6c6-4c65-abf8-190522d47037.aspx)

- [Dismount-Database](http://technet.microsoft.com/library/e261955b-a9f0-4d87-bf56-f9e67ea5ba3f.aspx)

- [Remove-MailboxDatabase](http://technet.microsoft.com/library/4d07d736-1dd7-43af-9f54-37d7c648572e.aspx)

## How do you know this worked?

To verify that you've successfully moved a mailbox, do the following:

- Open the mailbox using Outlook on the web.

- Open the mailbox using Microsoft Outlook.
