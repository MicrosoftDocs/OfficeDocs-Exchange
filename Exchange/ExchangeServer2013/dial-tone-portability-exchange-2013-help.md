---
title: 'Dial tone portability: Exchange 2013 Help'
TOCTitle: Dial tone portability
ms:assetid: ea62fae0-5e0a-460c-beb6-52532c8c8dbc
ms:mtpsurl: https://technet.microsoft.com/library/Dd876950(v=EXCHG.150)
ms:contentKeyID: 50873816
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Dial tone portability

_**Applies to:** Exchange Server 2013_

Dial tone portability is a feature of Microsoft Exchange Server 2013 that provides a limited business continuity solution for failures that affect a mailbox database, a server, or an entire site. Dial tone portability enables users to have a temporary mailbox for sending and receiving email while their original mailbox is being restored or repaired. The temporary mailbox can be on the same Exchange 2013 Mailbox server or on any other Exchange 2013 Mailbox server in your organization that has databases with the same database schema version. This allows an alternative server to host the mailboxes of users who were previously on a server that is no longer available. Clients that support Autodiscover are automatically redirected to the new server without having to manually update the user's desktop profile. After the user's original mailbox data has been restored, an administrator can merge a user's recovered mailbox and the user's dial tone mailbox into a single, up-to-date mailbox.

The process for using dial tone portability is called a *dial tone recovery*. A dial tone recovery involves creating an empty database on a Mailbox server to replace a failed database. This empty database, referred to as a *dial tone database*, allows users to send and receive email messages while the failed database is recovered.

There are three options for performing a dial tone recovery:

  - **Dial tone recovery on the server with the failed database**: If the server hosting the failed database is still functional, we recommend that you perform a dial tone recovery on that server. This means less downtime because you don't need to move database files between servers. In addition, you won't need to reconfigure messaging profiles for clients that don't support Autodiscover.

  - **Dial tone recovery using an alternate server for the dial tone database**: If a server fails and needs to be rebuilt, the most efficient way to give users basic mail functionality is to create a dial tone database on another server, and use database portability to move the users' mailbox configuration to that new server. Because this process involves moving the dial tone database back to the original (recovered) server, this option adds more time to the overall recovery process. In addition, this process is more complex than performing a dial tone recovery on the original server. When performing this process, the server hosting the dial tone database must have sufficient resources to support the added load of the additional users. In addition, if the users' client doesn't support Autodiscover, their messaging profile will need to be reconfigured to point to the dial tone server.

  - **Dial tone recovery using and staying on an alternate server for the dial tone database**: This is similar to the preceding option, except that you don't revert back to the original server. We recommend this option for situations in which it isn't possible or feasible to recover the failed server. In this scenario, users typically remain on an alternate server after the recovery operation has completed. When performing this process, the server hosting the dial tone database must have sufficient resources to support the added load of the additional users. In addition, if the users' client doesn't support Autodiscover, their messaging profile will need to be reconfigured to point to the dial tone server.

All three options follow the same basic steps:

1. **Create an empty dial tone database to replace the failed database.**

    This new database will allow users who had mailboxes on the failed database to send and receive new messages. Dial tone portability allows you to point a user to a different database without moving the mailbox. If you created the dial tone database on a different server than the server that housed the failed database, you need to move the mailbox configuration to that new server.

2. **Restore the old database.**

    Use the backup and recovery software you typically use to restore the failed database. If there is no backup of the failed database, recover the failed database using other means if possible. If you're using the same server for dial tone recovery, you need to restore the database to a recovery database (RDB).

3. **Swap the dial tone database with the restored database.**

    After the failed database is restored, swap it with the dial tone database. This gives the users the ability to send and receive email and access all the data in the restored database. If users were moved to a dial tone database on another server, you need to move the mailbox configuration back to the original server.

4. **Merge the databases.**

    To get the data from the dial tone database into the restored database, you merge the data using the [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest) cmdlet.

For detailed steps about how to perform a dial tone recovery, see [Perform a dial tone recovery](perform-a-dial-tone-recovery-exchange-2013-help.md).
