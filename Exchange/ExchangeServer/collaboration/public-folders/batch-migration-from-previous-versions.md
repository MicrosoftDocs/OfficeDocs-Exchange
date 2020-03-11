---
localization_priority: Normal
monikerRange: exchserver-2016
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: da808e27-d2b7-4fbd-915c-a600751f526c
ms.reviewer:
ms.collection: exchange-server
description: 'Summary: Learn how to migrate Exchange 2010 public folders to Exchange 2016.'
f1.keywords:
- NOCSH
audience: ITPro
title: Use batch migration to migrate Exchange 2010 public folders to Exchange 2016

---

# Use batch migration to migrate Exchange 2010 public folders to Exchange 2016

Migrate your public folders from Exchange Server 2010 SP3 RU8 to Exchange Server 2016 within the same forest.

We refer to the Exchange 2010 SP3 RU8 or later server as the *legacy Exchange server*.

> [!NOTE]
> The batch migration method described in this article is the only supported method for migrating legacy public folders to Exchange Server. The old serial migration method for migrating public folders is being deprecated and is no longer supported by Microsoft.

You'll perform the migration by using the **\*MigrationBatch** cmdlets, and the **\*PublicFolderMigrationRequest** cmdlets for troubleshooting. In addition, you'll use the following PowerShell scripts:

- `Export-PublicFolderStatistics.ps1`: This script creates the folder name-to-folder size mapping file.

- `Export-PublicFolderStatistics.psd1`: This support file is used by the Export-PublicFolderStatistics.ps1 script and should be downloaded to the same location.

- `PublicFolderToMailboxMapGenerator.ps1`: This script creates the public folder-to-mailbox mapping file.

- `PublicFolderToMailboxMapGenerator.strings.psd1`: This support file is used by the PublicFolderToMailboxMapGenerator.ps1 script and should be downloaded to the same location.

- `Create-PublicFolderMailboxesForMigration.ps1`: This script creates the target public folder mailboxes for the migration. In addition, this script calculates the number of mailboxes necessary to handle the estimated user load, based on the guidelines for the number of user logons per public folder mailbox recommended in [Limits for public folders](limits.md).

- `Create-PublicFolderMailboxesForMigration.strings.psd1`: This support file is used by the Create-PublicFolderMailboxesForMigration.ps1 script and should be downloaded to the same location.

The [Step 1: Download the migration scripts](#step-1-download-the-migration-scripts) section provides details about where to download these scripts. Be sure to download all scripts to the same location.

For additional management tasks related to public folders, see [Public folder procedures](procedures.md).

## What migration pathways are supported for Exchange Server versions?

Exchange supports moving your public folders from the following legacy versions of Exchange Server:

- Exchange 2010 SP3 RU8 or later

You can't migrate public folders directly from Exchange 2003. If you're running Exchange 2003 in your organization, you need to move all public folder databases and replicas to Exchange 2010 SP3 RU8 or later. No public folder replicas can remain on Exchange 2003. Additionally, mail destined for an Exchange 2016 public folder can't be routed through an Exchange 2003 server.

## What do you need to know before you begin?

- Before you begin, we recommend that you read this topic in its entirety as downtime is required for some steps.

- The Exchange 2010 server needs to be running Exchange 2010 SP3 RU8 or later.

- The maximum number of public folders that can be migrated to Exchange 2016 in a single migration is 500,000.

- In Exchange 2016, you need to be a member of the Organization Management role group. For details about how to enable the Organization Management role group, see [Manage role groups](../../permissions/role-groups.md).

- In Exchange 2010, you need to be a member of the Organization Management or Server Management RBAC role groups. For details, see [Add Members to a Role Group](https://go.microsoft.com/fwlink/p/?linkId=299212).

- Before you migrate, you should consider the [Limits for public folders](limits.md).

- Before you migrate, move all user mailboxes to Exchange 2016, because users with Exchange 2010 mailboxes will not have access to public folders on Exchange 2016. For details, see [Mailbox moves in Exchange Server](../../recipients/mailbox-moves.md).

- In a multiple-domain environment, mail-enabled public folders will stop working after migration to Exchange 2016 if Exchange is running in a child domain. This is because in Exchange 2016, mail-enabled public folder objects are required to be under the root domain. To resolve this, you need to mail-disable your mail-enabled public folders and then mail-enable them again, which will allow you to move them to the correct domain location.

- After the migration is complete, if you want external senders to send mail to the migrated mail-enabled public folders, the **Anonymous** user needs to be granted at least the **Create Items** permission. If you don't do this, external senders will receive a delivery failure notification and the messages won't be delivered to the migrated mail-enabled public folder. To read more about how to set permissions on the Anonymous user, see [Mail-enable or mail-disable a public folder](mail-enable-or-disable.md).

- You must use a single migration batch to migrate all of your public folder data. Exchange allows creating only one migration batch at a time. If you attempt to create more than one migration batch simultaneously, the result will be an error.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!IMPORTANT]
> Before you begin your migration, make sure you migrate your arbitration mailbox to the target Exchange server. Otherwise, your migration batch will hang in the **Starting** state. To identify your migration arbitration mailbox, run the following cmdlet:<br/>
`Get-Mailbox -Arbitration -Identity Migration.*`

## Step 1: Download the migration scripts

1. Download all scripts and supporting files from [Public Folders Migration Scripts](https://go.microsoft.com/fwlink/p/?linkId=299838).

2. Save the scripts to the local computer on which you'll be running PowerShell. For example, C:\PFScripts. Make sure all scripts are saved in the same location.

## Step 2: Prepare for the migration

Perform the following prerequisite steps before you begin the migration.

### Prerequisite steps on the Exchange 2010 server

1. For verification purposes at the end of migration, we recommend that you first run the following commands on the Exchange 2010 server to take snapshots of your current public folder deployment:

   - Run the following command to take a snapshot of the original source folder structure:

     ```PowerShell
     Get-PublicFolder -Recurse | Export-CliXML C:\PFMigration\Legacy_PFStructure.xml
     ```

   - Run the following command to take a snapshot of public folder statistics such as item count, size, and owner:

     ```PowerShell
     Get-PublicFolderStatistics | Export-CliXML C:\PFMigration\Legacy_PFStatistics.xml
     ```

   - Run the following command to take a snapshot of the permissions:

     ```PowerShell
     Get-PublicFolder -Recurse | Get-PublicFolderClientPermission | Select-Object Identity,User -ExpandProperty AccessRights | Export-CliXML C:\PFMigration\Legacy_PFPerms.xml
     ```

2. If the name of a public folder contains a backslash ( \\ ), migration will create the migrated public folders in the parent public folder. Before you migrate, we recommend that you rename any public folders that have a backslash in the name.

   To locate public folders in Exchange 2010 that have a backslash in the name, run the following command:

   ```PowerShell
   Get-PublicFolderStatistics -ResultSize Unlimited | Where {($_.Name -like "*\*") -or ($_.Name -like "*/*") } | Format-List Name, Identity
   ```

   If any public folders are returned, you can rename them by running the following command:

   ```PowerShell
   Set-PublicFolder -Identity <public folder identity> -Name <new public folder name>
   ```

3. Make sure there isn't a record of a previously successful migration by running the following command:

   ```PowerShell
   Get-OrganizationConfig | Format-List PublicFoldersLockedforMigration, PublicFolderMigrationComplete
   ```

   A previously successful migration will set the _PublicFoldersLockedforMigration_ or _PublicFolderMigrationComplete_ properties to the value `True`, which will cause your new migration request to fail.

   If the property values are `True`, run the following command to change them to `False`:

   ```PowerShell
   Set-OrganizationConfig -PublicFoldersLockedforMigration $false -PublicFolderMigrationComplete $false
   ```

   > [!NOTE]
   > After resetting these properties, you need to wait for Exchange to detect the new settings. This may take up to two hours to complete.

For detailed syntax and parameter information, see the following topics:

- [Get-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolder)

- [Get-PublicFolderDatabase](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolderdatabase)

- [Set-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/set-publicfolder)

- [Get-PublicFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolderstatistics)

- [Get-PublicFolderClientPermission](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolderclientpermission)

- [Get-OrganizationConfig](https://go.microsoft.com/fwlink/p/?linkid=183212)

- [Set-OrganizationConfig](https://go.microsoft.com/fwlink/p/?linkid=183213)

### Prerequisite steps on the Exchange 2016 server

1. Make sure there are no existing public folder migration requests. If there are, clear them or your own migration request will fail. This step isn't required in all cases; it's only required if you think there may be an existing migration request in the pipeline.

   > [!IMPORTANT]
   > Before removing a migration request, it is important to understand why there was an existing one. Running the following commands will determine when a previous request was made and help you diagnose any problems that may have occurred. You may need to communicate with other administrators in your organization to determine why the change was made.

   - Run the following command to discover any existing batch migration requests:

     ```
     $batch = Get-MigrationBatch | ?{$_.MigrationType.ToString() -eq "PublicFolder"}
     ```

   - Run the following command to remove any existing public folder batch migration requests.

     ```PowerShell
     $batch | Remove-MigrationBatch -Confirm:$false
     ```

2. Make sure no public folders or public folder mailboxes exist on the Exchange 2016 servers by running the following command:

   ```PowerShell
   Get-Mailbox -PublicFolder
   ```

   If the command didn't return any public folder mailboxes, continue to [Step 3: Generate the .csv files](#step-3-generate-the-csv-files). If the command returned any public folders, run the following command to see if any public folders exist:

   ```PowerShell
   Get-PublicFolder
   ```

   If you have any public folders, run the following commands to remove them. Make sure you've saved any information that was in the public folders.

   > [!NOTE]
   > All information contained in the public folders will be permanently deleted when you remove them.

   ```PowerShell
   Get-Mailbox -PublicFolder | Where {$_.IsRootPublicFolderMailbox -eq $false} | Remove-Mailbox -PublicFolder -Force -Confirm:$false
   ```

   ```PowerShell
   Get-Mailbox -PublicFolder | Remove-Mailbox -PublicFolder -Force -Confirm:$false
   ```

For detailed syntax and parameter information, see the following topics:

- [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/get-migrationbatch)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/get-mailbox)

- [Get-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolder)

- [Get-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-mailpublicfolder)

- [Disable-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/disable-mailpublicfolder)

- [Remove-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/remove-publicfolder)

- [Remove-Mailbox](https://docs.microsoft.com/powershell/module/exchange/mailboxes/remove-mailbox)

## Step 3: Generate the .csv files

1. On the Exchange 2010 server, run the `Export-PublicFolderStatistics.ps1` script to create the folder name-to-folder size mapping file. This script needs to be run by a local administrator. The file will contain two columns: **FolderName** and **FolderSize**. The values for the **FolderSize** column will be displayed in bytes. For example, **\\PublicFolder01,10000**.

   ```PowerShell
   .\Export-PublicFolderStatistics.ps1 <Folder to size map path> <FQDN of source server>
   ```

   - _FQDN of source server_ equals the fully qualified domain name of the Mailbox server where the public folder hierarchy is hosted.

   - _Folder to size map path_ equals the file name and path on a network shared folder where you want the .csv file saved. Later in this topic, you'll need to access this file from the Exchange 2016 server. If you specify only the file name, the file will be generated in the current PowerShell directory on the local computer.

2. Run the `PublicFolderToMailboxMapGenerator.ps1` script to create the public folder-to-mailbox mapping file. This file is used to calculate the correct number of public folder mailboxes on the Exchange 2016 server.

   > [!NOTE]
   > If the name of a public folder contains a backslash **\**, the public folders will be created in the parent public folder. We recommend that you review the .csv file and edit any names that contain a backslash.

   ```PowerShell
   .\PublicFolderToMailboxMapGenerator.ps1 <Maximum mailbox size in bytes> <Folder to size map path> <Folder to mailbox map path>
   ```

   - _Maximum mailbox size in bytes_ equals the maximum size you want to set for the new public folder mailboxes. When specifying this setting, be sure to allow for expansion so the public folder mailbox has room to grow.

   - _Folder to size map path_ equals the file path of the .csv file you created when running the `Export-PublicFolderStatistics.ps1` script.

   - _Folder to mailbox map path_ equals the file name and path of the folder-to-mailbox .csv file that you'll create with this step. If you specify only the file name, the file will be generated in the current PowerShell directory on the local computer.

## Step 4: Create the public folder mailboxes in Exchange 2016

Run the following command to create the target public folder mailboxes. The script will create a target mailbox for each mailbox in the .csv file that you generated previously in Step 3 by running the `PublicFoldertoMailboxMapGenerator.ps1` script.

```
.\Create-PublicFolderMailboxesForMigration.ps1 -FolderMappingCsv Mapping.csv -EstimatedNumberOfConcurrentUsers:<estimate>
```

_Mapping.csv_ is the file generated by the `PublicFoldertoMailboxMapGenerator.ps1` script in Step 3. The estimated number of simultaneous user connections browsing a public folder hierarchy is usually less than the total number of users in an organization.

## Step 5: Start the migration request

After you crate the batch migration request in the Exchange Management Shell, you can view the requests and manage them in the Exchange admin center (EAC).

1. On the Exchange 2016 server, run the following command:

   ```
   New-MigrationBatch -Name PFMigration -SourcePublicFolderDatabase (Get-PublicFolderDatabase -Server <Source server name>) -CSVData (Get-Content <Folder to mailbox map path> -Encoding Byte) -NotificationEmails <email addresses for migration notifications>
   ```

   The `NotificationEmails` parameter is optional.

2. Start the migration in the EAC or in the Exchange Management Shell.

   - In the Exchange Management Shell, run the following command:

     ```PowerShell
     Start-MigrationBatch PFMigration
     ```

   - In the EAC:

     1. Log into Exchange Online and open the EAC.

     2. Go to **Recipients** \> **Migration**.

     3. Select the migration batch you just created, and then click the start button.

     In the EAC, the **Status** column will show the initial batch status as **Created**. The status changes to **Syncing** during migration. When the migration request is complete, the status will be **Synced**. You can double-click a batch to view the status of individual mailboxes within the batch. Mailbox jobs begin with a status of **Queued**. When the job begins the status is **Syncing**, and once `InitialSync` is complete, the status will show **Synced**.

You can view and manage the progress and completion of the migration in the **Recipients** \> **Migration** tab in the EAC.

Because the **New-MigrationBatch** cmdlet initiates a mailbox migration request for each public folder mailbox, you can view the status of these requests using the mailbox migration page in the EAC, and you can create migration reports that can be emailed to you.

1. Log into Exchange Online and open the EAC.

2. Go to **Recipients** \> **Migration**.

3. Select the migration request that you just created and then click **View Details** in the **Details** pane.

For detailed syntax and parameter information, see the following topics:

- [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/new-migrationbatch)

- [Get-PublicFolderDatabase](https://docs.microsoft.com/powershell/module/exchange/sharing-and-collaboration/get-publicfolderdatabase)

- [Get-PublicFolderMailboxMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/get-publicfoldermailboxmigrationrequest)

- [Get-PublicFolderMailboxMigrationRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/move-and-migration/get-publicfoldermailboxmigrationrequeststatistics)

## Step 6: Lock down the public folders on the Exchange 2010 server for final migration (downtime required)

Until this point in the migration, users have been able to access public folders. The next steps will log users off from the Exchange 2010 public folders and lock the folders while the migration completes its final synchronization. Users won't be able to access public folders during this process. Also, any mail sent to mail-enabled public folders will be queued and won't be delivered until the public folder migration is complete.

Before you run the `PublicFoldersLockedForMigration` command as described below, make sure that all jobs are in the **Synced** state. You can do this by running the `Get-PublicFolderMailboxMigrationRequest` command. Continue with this step only after you've verified that all jobs are in the **Synced** state.

On the Exchange 2010 server, run the following command to lock the public folders for finalization.

```PowerShell
Set-OrganizationConfig -PublicFoldersLockedForMigration:$true
```


For detailed syntax and parameter information, see [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/organization/set-organizationconfig).

If your organization has multiple public folder databases, you'll need to wait until public folder replication is complete to confirm that all public folder databases have picked up the `PublicFoldersLockedForMigration` property value and any pending changes users recently made to folders have converged across the organization. This may take several hours.

## Step 7: Finalize the public folder migration (downtime required)

First, run the following cmdlet to change the Exchange 2016 deployment type to **Remote**:

```PowerShell
Set-OrganizationConfig -PublicFoldersEnabled Remote
```

Once that is done, you can complete the public folder migration by running the following command:

```PowerShell
Complete-MigrationBatch PFMigration

```

Or, in EAC, you can complete the migration by clicking **Complete this migration batch**.

When you complete the migration, Exchange will perform a final synchronization between the Exchange 2010 server and Exchange 2016. If the final synchronization is successful, the public folders on the Exchange 2016 server will be unlocked and the status of the migration batch will change to **Completing**, and then **Completed**. It is common for the migration batch to take a few hours before its status changes from **Synced** to **Completing**, at which point the final synchronization will begin.

> [!NOTE]
> If for any reason the migration batch file does not finalize (the _PublicFolderMigrationComplete_ property value is `False`) restart the Information Store (IS) on the Exchange 2010 server.

## Step 8: Test and unlock the public folder migration

After you finalize the public folder migration, you should run the following test to make sure that the migration was successful. This allows you to test the migrated public folder hierarchy before you switch to using Exchange 2016 public folders.

1. In PowerShell, run the following command to assign some test mailboxes to use any newly migrated public folder mailbox as the default public folder mailbox.

   ```PowerShell
   Set-Mailbox -Identity <Test User> -DefaultPublicFolderMailbox <Public Folder Mailbox Identity>
   ```

2. Log on to Outlook 2007 or later with the test user identified in the previous step, and then perform the following public folder tests:

   - View the hierarchy.

   - Check permissions.

   - Create and delete public folders.

   - Post content to and delete content from a public folder.

3. If you run into any issues, see [Roll back the migration](#roll-back-the-migration) later in this topic. If the public folder content and hierarchy is acceptable and functions as expected, run the following command to unlock the public folders for all other users.

   ```PowerShell
   Get-Mailbox -PublicFolder | Set-Mailbox -PublicFolder -IsExcludedFromServingHierarchy $false
   ```

   > [!IMPORTANT]
   > Don't use the _IsExcludedFromServingHierarchy_ parameter after initial migration validation is complete as this parameter is used by the automated storage management service for Exchange Online.

4. On the Exchange 2010 server, run the following command to indicate that the public folder migration is complete:

   ```PowerShell
   Set-OrganizationConfig -PublicFolderMigrationComplete:$true
   ```

5. After you've verified that the migration is complete, on the Exchange 2016 server, run the following command:

   ```PowerShell
   Set-OrganizationConfig -PublicFoldersEnabled Local
   ```

6. Finally, if you want external senders to send mail to the migrated mail-enabled public folders, the **Anonymous** user needs to be granted at least the **Create Items** permission. If you don't do this, external senders will receive a delivery failure notification and the messages won't be delivered to the migrated mail-enabled public folder.

   You can use the Exchange Management Shell or Outlook to set the permissions on the Anonymous user. To read more about how to set permissions on the Anonymous user, see [Mail-enable or mail-disable a public folder](mail-enable-or-disable.md).

## How do I know this worked?

In [Step 2: Prepare for the migration](#step-2-prepare-for-the-migration), you were instructed to take snapshots of the public folder structure, statistics, and permissions before the migration began. The following steps will help verify that your public folder migration was successful by taking the same snapshots after the migration is complete. You can then compare the data in both files to verify success.

1. Run the following command to take a snapshot of the new folder structure.

   ```PowerShell
   Get-PublicFolder -Recurse | Export-CliXML C:\PFMigration\Cloud_PFStructure.xml
   ```

2. Run the following command to take a snapshot of the public folder statistics such as item count, size, and owner.

   ```PowerShell
   Get-PublicFolderStatistics -ResultSize Unlimited | Export-CliXML C:\PFMigration\Cloud_PFStatistics.xml
   ```

3. Run the following command to take a snapshot of the permissions.

   ```PowerShell
   Get-PublicFolder -Recurse | Get-PublicFolderClientPermission | Select-Object Identity,User -ExpandProperty AccessRights | Export-CliXML  C:\PFMigration\Cloud_PFPerms.xml
   ```

## Remove public folder databases from the Exchange 2010 servers

After the migration is complete, and you have verified that your Exchange 2016 public folders are working as expected, you should remove the public folder databases on the Exchange 2010 servers.

For details about how to remove public folder databases from Exchange 2010 servers, see [Remove Public Folder Databases](https://go.microsoft.com/fwlink/p/?linkId=81409).

## Roll back the migration

If you run into issues with the migration and need to reactivate your Exchange 2010 public folders, perform the following steps.

> [!CAUTION]
> If you roll your migration back to the Exchange 2010 servers, you will lose any email that was sent to mail-enabled public folders or content that was posted to public folders in Exchange 2016 after the migration. To save this content, you need to export the public folder content to a .pst file and then import it to the Exchange 2010 public folders when the rollback is complete.

1. On the Exchange 2010 server, run the following command to unlock the migrated public folders. This process may take several hours.

   ```PowerShell
   Set-OrganizationConfig -PublicFoldersLockedForMigration $false
   ```

2. On the Exchange 2016 server, run the following commands to remove the public folder mailboxes.

   ```PowerShell
   Get-Mailbox -PublicFolder | Where {$_.IsRootPublicFolderMailbox -eq $false} | Remove-Mailbox -PublicFolder -Force -Permanent $true -Confirm:$false
   ```

   ```PowerShell
   Get-Mailbox -PublicFolder | Remove-Mailbox -PublicFolder -Force -Permanent $true -Confirm:$false
   ```

3. On the Exchange 2010 server, run the following command to set the `PublicFolderMigrationComplete` property value to `False`.

   ```PowerShell
   Set-OrganizationConfig -PublicFolderMigrationComplete $false
   ```
