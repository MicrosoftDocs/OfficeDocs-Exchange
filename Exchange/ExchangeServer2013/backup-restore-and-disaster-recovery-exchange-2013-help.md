---
title: 'Backup, restore, and disaster recovery: Exchange 2013 Help'
TOCTitle: Backup, restore, and disaster recovery
ms:assetid: 394fc4ed-fa02-41fa-9159-cc2754ff8875
ms:mtpsurl: https://technet.microsoft.com/library/Dd876874(v=EXCHG.150)
ms:contentKeyID: 48384978
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Backup, restore, and disaster recovery

_**Applies to:** Exchange Server 2013_

As part of your data protection planning, it's important that you understand the ways in which data can be protected, and to determine which method best suits your organization's needs. Data protection planning is a complex process that relies on many decisions that you make during the planning phase of your deployment.

Traditionally, backups have been used for the following scenarios:

  - **Disaster recovery**: In the event of a hardware or software failure, multiple database copies in a DAG enable high availability with fast failover and little or no data loss. This eliminates downtime and the resulting lost productivity that's a significant cost of recovering from a past point-in-time backup to disk or tape. DAGs can be extended to multiple sites and can provide resilience against disk, server, network, and datacenter failures.

  - **Recovery of accidentally deleted items**: Historically, in a situation where a user deleted items that later needed to be recovered, it involved finding the backup media on which the data that needed to be recovered was stored, and then somehow obtaining the desired items and providing them to the user. With the new Recoverable Items folder in Exchange 2013 and the Hold Policy that can be applied to it, it's possible to retain all deleted and modified data for a specified period of time, so recovery of these items is easier and faster. This reduces the burden on Exchange administrators and the IT help desk by enabling end users to recover accidentally deleted items themselves, thereby reducing the complexity and administrative costs associated with single item recovery. For more information, see [Messaging policy and compliance](messaging-policy-and-compliance-exchange-2013-help.md) and [Data loss prevention](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention).

  - **Long-term data storage**: Backups have also been used as an archive, and typically tape is used to preserve point-in-time snapshots of data for extended periods of time as governed by compliance requirements. The new archiving, multiple-mailbox search, and message retention features in Exchange 2013 provide a mechanism to efficiently preserve data in an end-user accessible manner for extended periods of time. This eliminates expensive restores from tape, and increases productivity. For more information, see [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md), [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery), and [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds).

  - **Point-in-time database snapshot**: If a past point-in-time copy of mailbox data is a requirement for your organization, Exchange provides the ability to create a lagged database copy in a DAG environment. This can be useful in the rare event that store logical corruption replicates to multiple database copies in the DAG, resulting in a need to return to a previous point in time. It may also be useful if an administrator accidentally deletes mailboxes or user data. Recovery from a lagged copy can be faster than restoring from a backup because lagged copies don't require a time-consuming copy process from the backup server to the Exchange server. This can significantly lower total cost of ownership by reducing downtime.

Because there are native Exchange 2013 features that meet each of these scenarios in an efficient and cost effective manner, you may be able to reduce or eliminate the use of traditional backups in your environment.

Exchange Native Data Protection

Supported Backup Technologies

Exchange 2013 VSS Writer

Server Recovery

Unified Contact Store Recovery

Recovery Database

Database Portability

Dial Tone Portability

## Exchange Native Data Protection

Microsoft's [preferred architecture](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/The-Preferred-Architecture/ba-p/586755) for Exchange Server 2013 leverages a concept known as Exchange Native Data Protection. Exchange Native Data Protection relies on built-in Exchange features to protect your mailbox data, without the use of backups (although you can still use those features and make backups). Exchange 2013 includes several new features and core changes that, when deployed and configured correctly, can provide native data protection that eliminates the need to make traditional backups of your data. Using the high availability features built into Exchange 2013 to minimize downtime and data loss in the event of a disaster can also reduce the total cost of ownership of the messaging system. By combining these features with other built-in features, such as Legal Hold, you can reduce or eliminate your use of traditional point-in-time backups and reduce the associated costs.

In addition to determining whether Exchange 2013 enables you to move away from traditional point-in-time backups, we recommend that you evaluate the cost of your current backup infrastructure. Consider the cost of end-user downtime and data loss when attempting to recover from a disaster using your existing backup infrastructure. Also, include hardware, installation, and license costs, as well as the management cost associated with recovering data and maintaining the backups. Depending on the requirements of your organization, it's quite likely that a pure Exchange 2013 environment with at least three mailbox database copies will provide lower total cost of ownership than one with backups.

There are several issues that you should consider before using the features built into Exchange 2013 as a replacement for traditional backups. There may also be considerations unique to your organization. Consider the following issues, and note that this isn't an exhaustive list:

  - You should determine how many copies of the database need to be deployed. We strongly recommend deploying a minimum of three (non-lagged) copies of a mailbox database before eliminating traditional forms of protection for the database, such as Redundant Array of Independent Disks (RAID) or traditional VSS-based backups.

  - You should clearly define the recovery time objective and recovery point objective goals, and you should establish that using a combined set of built-in features in lieu of traditional backups to enable you to meet these goals.

  - You should determine how many copies of each database are needed to cover the various failure scenarios against which your system is designed to protect.

  - You should determine whether eliminating the use of a DAG or some of its members captures sufficient costs to support a traditional backup solution. If so, you should determine whether that solution improves your recovery time objective or recovery point objective service level agreements (SLAs).

  - You should determine whether you can afford to lose a point-in-time copy if the DAG member hosting the copy experiences a failure that affects the copy or the integrity of the copy.

  - Exchange 2013 allows you to deploy much larger mailboxes, with a recommended maximum mailbox database size of 2 terabytes (when two or more highly available mailbox database copies are being used). Based on the larger mailboxes that most organizations are likely to deploy, you should determine your recovery point objective if you have to replay a large number of log files when activating a database copy or a lagged database copy.

  - You should determine how you'll detect and prevent logical corruption in an active database copy from replicating to the passive copies of the database. This includes determining the recovery plan for this situation and how frequently this scenario has occurred in the past. If logical corruption occurs frequently in your organization, we recommend that you factor that scenario into your design by using one or more lagged copies, with a sufficient replay lag window to allow you to detect and act on logical corruption when it occurs, but before that corruption is replicated to other database copies.

One of the functions performed at the end of a successful full or incremental backup is the truncation of transaction log files that are no longer needed for database recovery. If backups aren't being taken, log truncation won't occur. To prevent a buildup of log files, you enable circular logging for your replicated databases. When you combine circular logging with continuous replication, you have a new type of circular logging called continuous replication circular logging (CRCL), which is different from Extensible Storage Engine (ESE) circular logging. Whereas ESE circular logging is performed and managed by the Microsoft Exchange Information Store service, CRCL is performed and managed by the Microsoft Exchange Replication service. When enabled, ESE circular logging doesn't generate additional log files and instead overwrites the current log file when needed. However, in a continuous replication environment, log files are needed for log shipping and replay. As a result, when you enable CRCL, the current log file isn't overwritten and closed log files are generated for the log shipping and replay process.

Specifically, the Microsoft Exchange Replication service manages CRCL so that log continuity is maintained and logs aren't deleted if they're still needed for replication. The Microsoft Exchange Replication service and the Microsoft Exchange Information Store service communicate by using remote procedure calls (RPCs) regarding which log files can be deleted.

For truncation to occur on highly available (non-lagged) mailbox database copies, the following must be true:

  - The log file has been backed up, or CRCL is enabled.

  - The log file is below the checkpoint.

  - The other non-lagged copies of the database agree with deletion.

  - The log file has been inspected by all lagged copies of the database.

For truncation to occur on lagged database copies, the following must be true:

  - The log file is below the checkpoint.

  - The log file is older than ReplayLagTime + TruncationLagTime.

  - The log file is deleted on the active copy of the database.

## Supported Backup Technologies

Exchange 2013 supports only Exchange-aware, VSS-based backups. Exchange 2013 includes a plug-in for Windows Server Backup that enables you to make and restore VSS-based backups of Exchange data. To back up and restore Exchange 2013, you must use an Exchange-aware application that supports the VSS writer for Exchange 2013, such as Windows Server Backup (with the VSS plug-in), Microsoft System Center 2012 - Data Protection Manager, or a third-party Exchange-aware VSS-based application.

For detailed steps about how to back up and restore Exchange data using Windows Server Backup, see [Using Windows Server Backup to back up and restore Exchange data](using-windows-server-backup-to-back-up-and-restore-exchange-data-exchange-2013-help.md).

## Exchange 2013 VSS Writer

Exchange 2013 introduces a significant change from the VSS writer architecture used in Exchange 2010 and Exchange 2007. These earlier versions of Exchange included two VSS writers: one inside the Microsoft Exchange Information Store service (store.exe) and one inside the Microsoft Exchange Replication service (msexchangerepl.exe). In Exchange, the VSS writer functionality previously found in the Microsoft Exchange Information Store service has been moved to the Microsoft Exchange Replication service. The new writer, which is named Microsoft Exchange Writer, is now used by Exchange-aware VSS-based applications to back up active and passive database copies, and to restore backed up database copies. Although the new writer runs in the Microsoft Exchange Replication service, it requires the Microsoft Exchange Information Store service to be running for the writer to be advertised. As a result, both services are required to back up or restore Exchange databases.

## Exchange Server Recovery

Almost all of the configuration settings for Mailbox and Client Access servers are stored in Active Directory. As with previous versions of Exchange, Exchange 2013 includes a Setup parameter for recovering lost servers. This parameter, */m:RecoverServer*, is used to rebuild and re-create a lost server by using the settings and configuration information stored in Active Directory. However, be aware that there are several settings which are not restored, such as changes to local web.config and other configuration files. In addition, custom registry entries are not restored. We recommend that you use a reliable change management process to track and recreate these changes.

For detailed steps about how to perform a server recovery of a lost Exchange 2013 server, see [Recover an Exchange Server](recover-an-exchange-server-exchange-2013-help.md). For detailed steps about how to recover a lost server that's a member of a database availability group (DAG), see [Recover a database availability group member server](recover-a-database-availability-group-member-server-exchange-2013-help.md).

## Unified Contact Store Recovery

When Microsoft Lync Server 2013 is used in an Exchange 2013 environment, the user's Lync contact information is stored in a special contact folder in the user's mailbox. This is referred to as the unified contact store (UCS). If you restore a UCS-migrated mailbox, the instant messaging contact list for the target user may be affected. If the user was migrated after the last backup, restoring the mailbox will result in a complete loss of the user's contact list. In less severe cases, modifications to the contact list made by the user since the last backup will be lost. To mitigate this potential data loss, ensure the user is migrated back to the instant messaging server prior to restoring the mailbox.

## Recovery Database

A recovery database is a special kind of mailbox database that allows you to mount a restored mailbox database and extract data from the restored database as part of a recovery operation. You can use the [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest) cmdlet to extract data from a recovery database. After extraction, the data can be exported to a folder or merged into an existing mailbox. Recovery databases enable you to recover data from a backup or copy of a database without disturbing user access to current data.

Using a recovery database for a Mailbox database from any previous version of Exchange isn't supported. In addition, the target mailbox used for data merges and extraction must be in the same Active Directory forest as the database mounted in the recovery database.

For more information, see [Recovery databases](recovery-databases-exchange-2013-help.md). For detailed steps about how to create a recovery database, see [Create a recovery database](create-a-recovery-database-exchange-2013-help.md). For detailed steps about how to use a recovery database, see [Restore data using a recovery database](restore-data-using-a-recovery-database-exchange-2013-help.md).

## Database Portability

Database portability is a feature that enables an Exchange 2013 mailbox database to be moved to and mounted on any other Exchange 2013 Mailbox server in the same organization. By using database portability, reliability is improved by removing several error-prone, manual steps from the recovery processes. In addition, database portability reduces the overall recovery times for various failure scenarios.

For more information, see [Database portability](database-portability-exchange-2013-help.md). For detailed steps to use database portability, see [Move a mailbox database using database portability](move-a-mailbox-database-using-database-portability-exchange-2013-help.md).

## Dial Tone Portability

Dial tone portability is a feature that provides a limited business continuity solution for failures that affect a mailbox database, a server, or an entire site. Dial tone portability enables a user to have a temporary mailbox for sending and receiving e-mail while the original mailbox is being restored or repaired. The temporary mailbox can be on the same Exchange 2013 Mailbox server or on any other Exchange 2013 Mailbox server in your organization. This allows an alternative server to host the mailboxes of users who were previously on a server that's no longer available. Clients that support Autodiscover, such as Microsoft Outlook, are automatically redirected to the new server without having to manually update the user's desktop profile. After the user's original mailbox data has been restored, an administrator can merge a user's recovered mailbox and the user's dial tone mailbox into a single, up-to-date mailbox.

The process for using dial tone portability is called a *dial tone recovery*. A dial tone recovery involves creating an empty database on a Mailbox server to replace a failed database. This empty database, referred to as a *dial tone database*, allows users to send and receive e-mail while the failed database is recovered. After the failed database is recovered, the dial done database and the recovered database are swapped, and then the data from the dial tone database is merged into the recovered database.

For more information, see [Dial tone portability](dial-tone-portability-exchange-2013-help.md). For detailed steps to perform a dial tone recovery, see [Perform a dial tone recovery](perform-a-dial-tone-recovery-exchange-2013-help.md).
