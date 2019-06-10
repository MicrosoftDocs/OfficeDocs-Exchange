---
localization_priority: Normal
description: 'Summary: Step-by-step guidance for restoring Exchange data from a previous back up.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 2d0f31dc-eb32-451a-8852-591269026506
ms.date: 2/19/2016
ms.reviewer: 
title: Use Windows Server Backup to restore a backup of Exchange
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Use Windows Server Backup to restore a backup of Exchange

You can use Windows Server Backup to back up and restore Exchange databases. Exchange includes a plug-in for Windows Server Backup that allows you to make and restore Volume Shadow Copy Service (VSS)-based backups of Exchange data. For additional information, see [Using Windows Server Backup to back up and restore Exchange data](windows-server-backup.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute, plus the time it takes to restore the data

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- The Windows Server Backup feature must be installed on the local computer.

- When restoring a database to its original location, the database can remain in a dirty shutdown state and be mountable by the system. When restoring to an alternate location (for example, in preparation to use a recovery database), the database must be manually brought into a clean shutdown state by using Exchange Server Database Utilities (Eseutil.exe).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351)..

## Use Windows Server Backup to restore a backup of Exchange

1. Start Windows Server Backup.

2. Select **Local Backup**.

3. In the Actions pane, click **Recover...** to start the Recovery Wizard.

4. On the **Getting Started** page, do either of the following:

  - If the data being recovered was backed up on the local server, select **This server (ServerName)**, and then click **Next**.

  - If the data being recovered is from another server, or if the backup being recovered is located on another computer, select **Another server**, and then click **Next**. On the **Specify location type** page, select **Local drives** or **Remote shared folder**, and then click **Next**. If you select **Local drives**, select the drive containing the backup on the **Select backup location** page, and then click **Next**. If you select **Remote shared folder**, enter the UNC path for the backup data on the **Specify remote folder** page, and then click **Next**.

5. On the **Select Backup Date** page, select the date and time of the backup that you want to recover, and then click **Next**.

6. On the **Select Recovery Type** page, select **Applications**, and then click **Next**.

    > [!NOTE]
    > If **Applications** is not available as a selection, it indicates that the backup selected for restore was a folder-level backup, and not a volume level backup. You must perform backups at the volume level when backing up Exchange data with Windows Server Backup.

7. On the **Select Application** page, verify that Exchange is selected in the **Applications** field. Click **View Details** to view the application components of the backups. If the backup that you're recovering is the most recent, the **Do not perform a roll-forward recovery of the application database** check box is displayed. Select this check box if you want to prevent Windows Server Backup from rolling forward the database being recovered by committing all uncommitted transaction logs. Click **Next**.

8. On the **Specify Recovery Options** page, specify where you want to recover the data, and then click **Next**:

  - Choose **Recover to original location** if you want to restore the Exchange data directly to its original location. If you use this option, you can't choose which databases are restored; all backed up databases on the volume will be restored to their original locations.

  - Choose **Recover to another location** if you want to restore individual databases and their files to a specified location. Click **Browse** to specify the alternate location. If you use this option, you can choose which databases are restored. After being restored, the data files can then be moved into a recovery database, manually moved back to their original location, or mounted somewhere else in the Exchange organization using [Database Portability](http://technet.microsoft.com/library/387b727a-ce51-4910-b5c4-613c693fa5bd.aspx). When you restore a database to an alternate location, the restored database will be in a dirty shutdown state. After the restore process has completed, you will need to manually put the database into a clean shutdown state using Eseutil.exe.

9. On the **Confirmation** page, review the recovery settings, and then click **Recover**.

10. On the **Recovery Progress** page, you can view the status and progress of the recovery operation.

11. Click **Close** when the recovery operation has completed.

## How do you know this worked?

The **Recovery Progress** page will indicate whether or not the recovery process completed successfully. To further verify that you've successfully restored the data, do any of the following:

- Examine the target directory of the backup and verify that the restored data exists.

- On the server on which Windows Server Backup was run, verify that the job completed successfully by viewing the backup logs.

- Open Event Viewer and verify that a restore completion event was logged in the Application event log.
