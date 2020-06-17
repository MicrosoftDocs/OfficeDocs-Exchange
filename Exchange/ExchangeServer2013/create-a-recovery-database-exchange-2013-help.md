---
title: 'Create a recovery database: Exchange 2013 Help'
TOCTitle: Create a recovery database
ms:assetid: 34d87491-b7b7-44a9-8d69-e1a9c1fe5852
ms:mtpsurl: https://technet.microsoft.com/library/Ee332321(v=EXCHG.150)
ms:contentKeyID: 48384961
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a recovery database

_**Applies to:** Exchange Server 2013_

You can use the Shell to create a recovery database, a special kind of mailbox database that's used to mount and extract data from the restored database as part of a recovery operation. After you create a recovery database, you can move a recovered or restored mailbox database into the recovery database, and then use the [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest) cmdlet to extract data from the recovered database. After extraction, the data can then be exported to a folder or merged into an existing mailbox. Using recovery databases, you can recover data from a backup or copy of a database without disrupting user access to current data.

Looking for other management tasks related to recovery databases? Check out [Recovery databases](recovery-databases-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to create a recovery database

This example creates the recovery database RDB1 on the Mailbox server MBX2.

```powershell
New-MailboxDatabase -Recovery -Name RDB1 -Server MBX2
```

This example creates the recovery database RDB2 on the Mailbox server MBX1 using a custom path for the database file and log folder.

```powershell
New-MailboxDatabase -Recovery -Name RDB2 -Server MBX1 -EdbFilePath "C:\Recovery\RDB2\RDB2.EDB" -LogFolderPath "C:\Recovery\RDB2"
```

For detailed syntax and parameter information, see [New-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/New-MailboxDatabase).

## How do you know this worked?

To verify that you've successfully created a recovery database, do the following:

- In the Shell, run the following command to display configuration information for the recovery database.

  ```powershell
  Get-MailboxDatabase <RecoveryDatabaseName> | Format-List
  ```

## Other Tasks

After you create a recovery database, you may also want to restore data using a recovery database. For detailed steps, see [Restore data using a recovery database](restore-data-using-a-recovery-database-exchange-2013-help.md).
