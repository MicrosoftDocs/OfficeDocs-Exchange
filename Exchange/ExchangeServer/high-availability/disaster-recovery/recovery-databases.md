---
localization_priority: Normal
description: 'Summary: An overview of recovery databases in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f3c6fd0b-2e25-442e-a0fc-46f663130c3e
ms.reviewer:
title: Recovery databases
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recovery databases

A recovery database (RDB) is a special kind of mailbox database that allows you to mount a restored mailbox database and extract data from the restored database as part of a recovery operation. You can use the [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/new-mailboxrestorerequest) cmdlet to extract data from an RDB. After extraction, the data can be exported to a folder or merged into an existing mailbox. RDBs enable you to recover data from a backup or copy of a database without disturbing user access to current data.

Microsoft Exchange Server supports the ability to restore data directly to a recovery database. Mounting the recovered data as a recovery database allows the administrator to restore individual mailboxes or individual items in a mailbox. Restoring to a recovery database can be accomplished in two ways:

- If a recovery database already exists, the application can dismount the database, restore the data onto the recovery database and log files, and then remount the database.

- The database and log files can be restored to any disk location. Exchange analyzes the restored data and replays the transaction logs to bring the databases up to date, and then a recovery database can be configured to point to already recovered database files.

## Difference between a mailbox database and a recovery database

RDBs are different from standard mailbox databases in several respects:

- An RDB is created by using the Exchange Management Shell.

- Mail can't be sent to or from an RDB. All client protocol access to an RDB (including SMTP, POP3, and IMAP4) is blocked. This design prevents using an RDB to insert mail into or remove mail from the messaging system.

- Client MAPI access using Microsoft Outlook or Outlook on the web is blocked. MAPI access is supported for an RDB, but only by recovery tools and applications. Both the mailbox GUID and the database GUID must be specified when using MAPI to log into a mailbox in an RDB.

- Mailboxes in an RDB can't be connected to user accounts. To allow a user to access the data in a mailbox in an RDB, the mailbox must be merged into an existing mailbox, or exported to a folder.

- System and mailbox management policies aren't applied. This design prevents items in an RDB from being deleted by the system during the recovery process.

- Online maintenance isn't performed for RDBs.

- Circular logging can't be enabled for RDBs.

- Only one RDB can be mounted at any time on a Mailbox server. The use of an RDB doesn't count against the database limit per Mailbox server.

- You can't create mailbox database copies of an RDB.

- An RDB can be used as a target for restore operations, but not backup operations.

- A recovered database mounted as an RDB isn't tied to the original mailbox in any way.

## Using a recovery database

Before you can use an RDB, there are certain requirements that must be met. An RDB can be used for Exchange 2016 and later mailbox databases only. Mailbox databases from previous versions of Exchange aren't supported. In addition, the target mailbox used for data merges and extraction must be in the same Active Directory forest as the database mounted in the RDB.

An RDB can be used to recover data in several situations, such as:

- **Same server dial tone recovery**: You can perform a recovery from an RDB after the original database has been restored from backup, as part of a dial tone recovery operation.

- **Alternate server dial tone recovery**: You can use an alternate server to host the dial tone database, and then later recover data from an RDB after the original database has been restored from backup.

- **Mailbox recovery**: You can recover an individual mailbox from backup when the deleted mailbox retention period has elapsed. You then extract data from the restored mailbox and copy it to a target folder or merge it with another mailbox.

- **Specific item recovery**: You can restore from backup data that has been deleted or purged from a mailbox.

> [!NOTE]
> Folder access control lists (ACLs) aren't preserved when recovering content into an active mailbox. Because the recovery process typically involves recovering mailbox data and merging the content back into the original database, there should be no need to recover or copy ACLs.

An RDB is designed for mailbox database recovery under the following conditions and scenarios:

- The logical information about the original database and the mailboxes in that database remains intact and unchanged in Active Directory.

- You need to recover a single mailbox or a single database. Recovery scenarios include:

  - Recovering or repairing a database while a dial tone database is in use, with the goal of merging the two databases.

  - Recovering a database on a server other than the original server for that database. If needed, you can then merge the recovered data back to the original server.

  - Recovering deleted items that users previously deleted from their mailbox, after the deleted item retention period has expired.

RDBs are generally not designed for scenarios in which you have to restore entire servers, when you have to restore multiple databases, or when you're in an emergency situation that requires changing or rebuilding your Active Directory topology.

For detailed steps about how to create an RDB, see [Create a recovery database](create-recovery-dbs.md). For detailed steps about how to use an RDB, see [Restore data using a recovery database](restore-data-using-recovery-dbs.md).
