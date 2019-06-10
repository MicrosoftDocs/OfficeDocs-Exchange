---
localization_priority: Normal
description: 'Summary: An overview of using Windows Server Backup with Exchange Server 2016 or Exchange Server 2019.'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: 0fac891a-5713-42b6-afd5-c91b2b88f966
ms.date: 7/9/2018
ms.reviewer: 
title: Using Windows Server Backup to back up and restore Exchange data
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Using Windows Server Backup to back up and restore Exchange data

Microsoft's [preferred architecture](https://blogs.technet.com/b/exchange/archive/2014/04/21/the-preferred-architecture.aspx) for Exchange Server leverages a concept known as Exchange Native Data Protection. Exchange Native Data Protection relies on native Exchange features to protect your mailbox data, without the use of traditional backups. But if you want to create backups, Exchange includes a plug-in for Windows Server Backup (WSB) that enables you to create Exchange-aware Volume Shadow Copy Service (VSS)-based backups of Exchange data. To take Exchange-aware backups, you must have the WSB feature installed.

The plug-in, WSBExchange.exe, runs as a service named Microsoft Exchange Server Extension for Windows Server Backup (the short name for this service is WSBExchange). This service is automatically installed and configured for manual startup on all Mailbox servers. The plug-in enables WSB to create Exchange-aware VSS backups.

Before using WSB to back up Exchange data, we recommend that you familiarize yourself with the following features and options for the plug-in:

- Backups taken with WSB occur at the volume level, and the only way to perform an application-level backup or restore is to select an entire volume. To back up a database and its log stream, you must back up the entire volume containing the database and logs, not just the individual folders. You can't back up any data without backing up the entire volume containing the data.

- The backup must be run locally on the server being backed up, and you can't use the plug-in to take remote VSS backups. There is no remote administration of WSB or the plug-in. You can, however, use Remote Desktop Services or Terminal Services to remotely manage backups.

- The backup can be created on a local drive or on a remote network share.

- Only full backups should be taken. Log truncation will occur only after a successful completion of a VSS full backup of a volume or folders containing an Exchange database.

- When restoring data, it's possible to restore only Exchange data. This data can be restored to its original location or to an alternate location. If you restore the data to its original location, WSB and the plug-in automatically handle the recovery process, including dismounting any existing database and replaying logs into the restored database.

- The restore process doesn't support the Exchange recovery database (RDB). If you want to use an RDB, you must restore the data to an alternate location and then manually copy or move the restored data from that location into the RDB folder structure.

- When restoring Exchange data, all backed up databases must be restored together. You can't restore a single database.

- Bare metal restores are supported when using WSB; however, the recommended recovery approach for Exchange servers is to recover the Exchange server and then restore the data. If you're using a third-party backup application (e.g., non-Microsoft), then support for bare metal restores of Exchange may be available from your backup application vendor.

The following table describes the supportability of the backup and recovery options available for Exchange Server with WSB.

|**If you...**|**Then...**|
|:-----|:-----|
|Back up the full server...|A VSS copy backup will be performed, and the transaction logs for the databases on the server will not be truncated.|
|Perform a custom backup and select one or more volumes to back up...|A VSS full backup can be selected, allowing the transaction logs for the databases on the selected volumes to be truncated at the completion of a successful backup.|
|Perform a custom backup and select one or more folders to back up...|A VSS full backup can be selected and the log files will be truncated; however, restoration of the backup will be limited to file restore, as an Application level restore will not be available as an option.|

For detailed steps to back up Exchange using WSB, see [Use Windows Server Backup to back up Exchange](backup-with-windows-server-backup.md).

For detailed steps to restore data from a backup taken with WSB, see [Use Windows Server Backup to restore a backup of Exchange](restore-with-windows-server-backup.md).
