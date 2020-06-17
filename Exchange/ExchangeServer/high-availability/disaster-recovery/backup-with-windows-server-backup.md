---
localization_priority: Normal
description: 'Summary: Step-by-step guidance for backing up your Exchange Server 2016 and Exchange Server 2019 Sdata.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 188a8291-0a41-4ca2-b6d2-94242e2b1ffc
ms.reviewer: 
title: Use Windows Server Backup to back up Exchange
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Use Windows Server Backup to back up Exchange

You can use Windows Server Backup to back up and restore Exchange databases. Exchange includes a plug-in for Windows Server Backup that allows you to make Volume Shadow Copy Service (VSS)-based backups of Exchange data.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute, plus the time it takes to back up the data

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- The Windows Server Backup feature must be installed on the local computer.

- During the backup operation, a consistency check of the Exchange data files is run to make sure that the files are in a good state and can be used for recovery. If the consistency check succeeds, Exchange data is available for recovery from that backup. If the consistency check fails, the Exchange data isn't available for recovery. Windows Server Backup runs the consistency check on the snapshot taken for the backup. As a result, before copying files from the snapshot to backup media, the consistency of the backup is known, and the user is notified of the consistency check results.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Windows Server Backup to back up Exchange

1. Start Windows Server Backup.

2. Select **Local Backup**.

3. In the Actions pane, click **Backup Once...** to start the Backup Once Wizard.

4. On the **Backup Options** page, select **Different options**, and then click **Next**.

5. On the **Select Backup Configuration** page, select **Custom**, and then click **Next**.

6. On the **Select Items for Backup** page, click **Add Items** to select the volume(s) to be backed up, and then click **OK**.

   > [!NOTE]
   > Choose volumes and not individual folders. The only way to perform an application-level backup or restore is to select an entire volume.

7. Click **Advanced Settings**. On the **Exclusions** tab, click **Add Exclusion** to add any files or file types you want to exclude from the backup.

   > [!NOTE]
   > By default, volumes that contain operating system components or applications are included in the backup and can't be excluded.

8. On the **VSS Settings tab**, select **VSS full Backup**, and then click **OK**, and then click **Next**.

9. On the **Specify Destination Type** page, select the location where you want to store the backup, and then click **Next**.

   - If you choose **Local drives**, the **Select Backup Destination** page appears. Select an option from the **Backup destination** dropdown, and then click **Next**.

   - If you choose **Remote shared folder**, the **Specify remote folder** page appears. Specify a UNC path for the backup files, configure access control settings. Choose **Do not inherit** if you want the backup to be accessible only through a specific account. Provide a username and password for an account that has write permissions on the computer hosting the remote folder, and then click **OK**. Alternatively, choose **Inherit** if you want the backup to be accessible by everyone who has access to the remote folder. Click **Next**.

10. On the **Confirmation** page, review the backup settings, and then click **Backup**.

11. On the **Backup Progress** page, you can view the status and progress of the backup operation.

12. Click **Close** to exit the **Backup Progress** page at any time. Any backup in progress will continue to run in the background.

## How do you know this worked?

To verify that you've successfully backed up the data, do any of the following:

- On the server on which Windows Server Backup was run, the last backup status will be displayed, which should say Successful. You can also verify that the backup completed successfully by viewing the Windows Server Backup logs.

- Open Event Viewer and verify that a backup completion event was logged in the Application event log.

- Run the following command in the Exchange Management Shell to verify that each database on the selected volume(s) was backed up successfully:

  ```powershell
  Get-MailboxDatabase -Server <ServerName> -Status | Format-List Name,*FullBackup
  ```

  The _SnapshotLastFullBackup_ and _LastFullBackup_ properties of the database indicate when the last successful backup was taken, and if it was a VSS full backup.
