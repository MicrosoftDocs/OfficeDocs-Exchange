---
title: 'Use batch migration to migrate public folders to Exchange 2013 from previous versions'
TOCTitle: Use batch migration to migrate public folders to Exchange 2013 from previous versions
ms:assetid: da808e27-d2b7-4fbd-915c-a600751f526c
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
ms:mtpsurl: https://technet.microsoft.com/library/Dn912663(v=EXCHG.150)
ms:contentKeyID: 64568564
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Use batch migration to migrate public folders to Exchange 2013 from previous versions

**Summary:** This article tells you how to move public folders from Exchange 2007 or Exchange 2010 to Exchange 2013.

This article describes how to migrate your public folders from Exchange Server 2010 SP3 RU8 or Exchange 2007 SP3 RU15 to Microsoft Exchange Server 2013 CU7 or later within the same forest.

We refer to the Exchange 2010 SP3 RU8 and Exchange 2007 SP3 RU15 servers as the *legacy Exchange server*.

> [!NOTE]
> The batch migration method described in this article is the only supported method for migrating legacy public folders to Exchange 2013. The old serial migration method for migrating public folders is being deprecated and is no longer supported by Microsoft.

You'll perform the migration by using the **\*MigrationBatch** cmdlets, and the **\*PublicFolderMigrationRequest** cmdlets for troubleshooting. In addition, you will use the following PowerShell scripts:

- `Export-PublicFolderStatistics.ps1`: This script creates the folder name-to-folder size mapping file.

- `Export-PublicFolderStatistics.psd1`: This support file is used by the Export-PublicFolderStatistics.ps1 script and should be downloaded to the same location.

- `PublicFolderToMailboxMapGenerator.ps1`: This script creates the public folder-to-mailbox mapping file.

- `PublicFolderToMailboxMapGenerator.strings.psd1`: This support file is used by the PublicFolderToMailboxMapGenerator.ps1 script and should be downloaded to the same location.

- `Create-PublicFolderMailboxesForMigration.ps1`: This script creates the target public folder mailboxes for the migration. In addition, this script calculates the number of mailboxes necessary to handle the estimated user load, based on the guidelines for the number of user logons per public folder mailbox recommended in [Limits for public folders](limits-for-public-folders-exchange-2013-help.md).

- `Create-PublicFolderMailboxesForMigration.strings.psd1`: This support file is used by the Create-PublicFolderMailboxesForMigration.ps1 script and should be downloaded to the same location.

Step 1: Download the migration scripts provides details about where to download these scripts. Make sure all scripts are downloaded to the same location.

For additional management tasks related to public folders, see [Public folder procedures](public-folder-procedures-exchange-2013-help.md).

## What versions of Exchange are supported for migrating public folders to Exchange 2013?

Exchange supports moving your public folders from the following legacy versions of Exchange Server:

- Exchange 2010 SP3 RU8 or later

- Exchange 2007 SP3 RU15 or later

If you need to move your public folders to Exchange 2013 but your on-premises servers aren't running the minimum support versions of Exchange 2010 or Exchange 2007, check out [Use serial migration to migrate public folders to Exchange 2013 from previous versions](https://docs.microsoft.com/previous-versions/exchange-server/exchange-150/jj150486(v=exchg.150)). While serial migration is an option, we strongly recommend that you upgrade your on-premises servers and use batch migration. Batch migration allows for significantly faster and greater reliability.

You can't migrate public folders directly from Exchange 2003. If you're running Exchange 2003 in your organization, you need to move all public folder databases and replicas to Exchange 2010 SP3 RU8 or later, or to Exchange 2007 SP3 RU15 or later. No public folder replicas can remain on Exchange 2003. Additionally, mail destined for an Exchange 2013 public folder can't be routed through an Exchange 2003 server.

## What do you need to know before you begin?

- Before you begin, we recommend that you read this topic in its entirety as downtime is required for some steps.

- The Exchange 2010 server needs to be running Exchange 2010 SP3 RU8 or later.

- The Exchange 2007 server needs to be running Exchange 2007 SP3 RU15 or later.

- The maximum number of public folders that can be migrated to Exchange 2013 in a single migration is 500,000.

- In Exchange 2013, you need to be a member of the Organization Management role group. For details about how to enable the Organization Management role group, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

- In Exchange 2010, you need to be a member of the Organization Management or Server Management RBAC role groups. For details, see [Add Members to a Role Group](https://go.microsoft.com/fwlink/?linkid=299212).

- In Exchange 2007, you need to be assigned the Exchange Organization Administrator role or the Exchange Server Administrator role. In addition, you need to be assigned the Public Folder Administrator role and local Administrators group for the target server. For details, see [How to Add a User or Group to an Administrator Role](https://go.microsoft.com/fwlink/p/?linkid=81779).

- On the Exchange 2007 server, upgrade to [Windows PowerShell 2.0 and WinRM 2.0 for Windows Server 2008 x64 Edition](https://support.microsoft.com/help/968930).

- Before you migrate, you should consider the [Limits for public folders](limits-for-public-folders-exchange-2013-help.md).

- Before you migrate, move all user mailboxes to Exchange 2013, because users with Exchange 2007 or Exchange 2010 mailboxes will not have access to public folders on Exchange 2013. For details, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

- In a multiple-domain environment, mail-enabled public folders will stop working after migration to Exchange 2013 if Exchange is running in a child domain. This is because in Exchange 2013, mail-enabled public folder objects are required to be under the root domain. To resolve this, you need to mail-disable your mail-enabled public folders and then mail-enable them again, which will allow you to move them to the correct domain location.

- After the migration is complete, if you want external senders to send mail to the migrated mail-enabled public folders, the **Anonymous** user needs to be granted at least the **Create Items** permission. If you don't do this, external senders will receive a delivery failure notification and the messages won't be delivered to the migrated mail-enabled public folder. To read more about how to set permissions on the Anonymous user, see [Mail-enable or mail-disable a public folder](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/enable-or-disable-mail-for-public-folder).

- You must use a single migration batch to migrate all of your public folder data. Exchange allows creating only one migration batch at a time. If you attempt to create more than one migration batch simultaneously, the result will be an error.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!IMPORTANT]
> Before you begin your migration, make sure you migrate your arbitration mailbox to the Exchange 2013 server. Otherwise, your migration batch will hang in the **Starting** state. To identify your migration arbitration mailbox, run the following cmdlet:<br/>
`((get-mailbox -Arbitration -Identity Migration.*).servername -eq (hostname))`

## Step 1: Download the migration scripts

1. Download all scripts and supporting files from [Public Folders Migration Scripts](https://go.microsoft.com/fwlink/?linkid=299838).

2. Save the scripts to the local computer on which you'll be running PowerShell. For example, C:\\PFScripts. Make sure all scripts are saved in the same location.

## Step 2: Prepare for the migration

Perform the following prerequisite steps before you begin the migration.

### Prerequisite steps on the legacy Exchange server

1. For verification purposes at the end of migration, we recommend that you first run the following commands on the legacy Exchange server to take snapshots of your current public folder deployment:

   - Run the following command to take a snapshot of the original source folder structure:

     ```powershell
     Get-PublicFolder -Recurse -ResultSize Unlimited | Export-CliXML C:\PFMigration\Legacy_PFStructure.xml
     ```

   - Run the following command to take a snapshot of public folder statistics such as item count, size, and owner:

     ```powershell
     Get-PublicFolderStatistics -ResultSize Unlimited | Export-CliXML C:\PFMigration\Legacy_PFStatistics.xml
     ```

   - Run the following command to take a snapshot of the permissions:

     ```powershell
     Get-PublicFolder -Recurse -ResultSize Unlimited | Get-PublicFolderClientPermission | Select-Object Identity,User -ExpandProperty AccessRights | Export-CliXML C:\PFMigration\Legacy_PFPerms.xml
     ```

   Save the information from the preceding commands for comparison purposes after your migration is complete.

2. If the name of a public folder contains a backslash **\\**, the public folders will be created in the parent public folder when migration occurs. Before you migrate, we recommend that you rename any public folders that have a backslash in the name.

   1. In Exchange 2010, to locate public folders that have a backslash in the name, run the following command:

      ```powershell
      Get-PublicFolderStatistics -ResultSize Unlimited | Where {($_.Name -like "*\*") -or ($_.Name -like "*/*") } | Format-List Name, Identity
      ```

   2. In Exchange 2007, to locate public folders that have a backslash in the name, run the following command:

      ```powershell
      Get-PublicFolderDatabase | ForEach {Get-PublicFolderStatistics -Server $_.Server | Where {$_.Name -like "*\*"}}
      ```

   3. If any public folders are returned, you can rename them by running the following command:

      ```powershell
      Set-PublicFolder -Identity <public folder identity> -Name <new public folder name>
      ```

3. Make sure there isn't a previous record of a successful migration.

   1. The following example checks the public folder migration status.

      ```powershell
      Get-OrganizationConfig | Format-List PublicFoldersLockedforMigration, PublicFolderMigrationComplete
      ```

      If there has been a previous successful migration, the value of the *PublicFoldersLockedforMigration* or *PublicFolderMigrationComplete* properties is `$true`. Use the command in step 3b to set the value to `$false`. If the value is set to `$true`, your migration request will fail.

   2. If the status of the *PublicFoldersLockedforMigration* or *PublicFolderMigrationComplete* properties is `$true`, run the following command to set the value to `$false`.

      ```powershell
      Set-OrganizationConfig -PublicFoldersLockedforMigration:$false -PublicFolderMigrationComplete:$false
      ```

   > [!WARNING]
   > After resetting these properties, you need to wait for Exchange to detect the new settings. This may take up to two hours to complete.

For detailed syntax and parameter information, see the following topics:

- [Get-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/get-publicfolder)

- [Get-PublicFolderDatabase](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderdatabase)

- [Set-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/set-publicfolder)

- [Get-PublicFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderstatistics)

- [Get-PublicFolderClientPermission](https://docs.microsoft.com/powershell/module/exchange/get-publicfolderclientpermission)

- [Get-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/Get-OrganizationConfig)

- [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/Set-OrganizationConfig)

### Prerequisite steps on the Exchange 2013 server

1. Make sure there are no existing public folder migration requests. If there are, clear them or your own migration request will fail. This step isn't required in all cases; it's only required if you think there may be an existing migration request in the pipeline.

   An existing migration request can be one of two types: batch migration or serial migration. The commands for detecting requests for each type and for removing requests of each type are as follows.

   > [!IMPORTANT]
   > • Before removing a migration request, it is important to understand why there was an existing one. Running the following commands will determine when a previous request was made and help you diagnose any problems that may have occurred. You may need to communicate with other administrators in your organization to determine why the change was made. <br/><br/>• Support for serial migration of public folders (**\*-PublicFolderMigrationRequest\*** cmdlets) ended in Exchange 2013 Cumulative Update 8 (CU8).

   The following example will discover any existing serial migration requests.

   ```powershell
   Get-PublicFolderMigrationRequest | Get-PublicFolderMigrationRequestStatistics -IncludeReport | Format-List
   ```

   The following example removes any existing public folder serial migration requests.

   ```powershell
   Get-PublicFolderMigrationRequest | Remove-PublicFolderMigrationRequest
   ```

   The following example will discover any existing batch migration requests.

   ```powershell
   $batch = Get-MigrationBatch | ?{$_.MigrationType.ToString() -eq "PublicFolder"}
   ```

   The following example removes any existing public folder batch migration requests.

   ```powershell
   $batch | Remove-MigrationBatch -Confirm:$false
   ```

2. Make sure no public folders or public folder mailboxes exist on the Exchange 2013 servers.

   1. Run the following command to see if any public folders mailboxes exist.

      ```powershell
      Get-Mailbox -PublicFolder
      ```

   2. If the command didn't return any public folder mailboxes, continue to Step 3: Generate the .csv files. If the command returned any public folders, run the following command to see if any public folders exist:

      ```powershell
      Get-PublicFolder
      ```

   3. If you have any public folders, run the following PowerShell commands to remove them. Make sure you've saved any information that was in the public folders.

      > [!NOTE]
      > All information contained in the public folders will be permanently deleted when you remove them.

      ```powershell
      Get-Mailbox -PublicFolder | Where{$_.IsRootPublicFolderMailbox -eq $false} | Remove-Mailbox -PublicFolder -Force -Confirm:$false
      ```

      ```powershell
      Get-Mailbox -PublicFolder | Remove-Mailbox -PublicFolder -Force -Confirm:$false
      ```

For detailed syntax and parameter information, see the following topics:

- [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationBatch)

- [Get-PublicFolderMailboxMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxmigrationrequest)

- [Remove-PublicFolderMailboxMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/remove-publicfoldermailboxmigrationrequest)

- [Get-PublicFolderMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/Get-PublicFolderMigrationRequest)

- [Remove-PublicFolderMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/Remove-PublicFolderMigrationRequest)

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox)

- [Get-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/Get-PublicFolder)

- [Get-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/Get-MailPublicFolder)

- [Disable-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/Disable-MailPublicFolder)

- [Remove-PublicFolder](https://docs.microsoft.com/powershell/module/exchange/Remove-PublicFolder)

- [Remove-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Remove-Mailbox)

## Step 3: Generate the .csv files

1. On the legacy Exchange server, run the `Export-PublicFolderStatistics.ps1` script to create the folder name-to-folder size mapping file. This script needs to be run by a local administrator. The file will contain two columns: **FolderName** and **FolderSize**. The values for the **FolderSize** column will be displayed in bytes. For example, **\\PublicFolder01,10000**.

   ```powershell
   .\Export-PublicFolderStatistics.ps1  <Folder to size map path> <FQDN of source server>
   ```

   - *FQDN of source server* equals the fully qualified domain name of the Mailbox server where the public folder hierarchy is hosted.

   - *Folder to size map path* equals the file name and path on a network shared folder where you want the .csv file saved. Later in this topic, you'll need to access this file from the Exchange 2013 server. If you specify only the file name, the file will be generated in the current PowerShell directory on the local computer.

2. Run the `PublicFolderToMailboxMapGenerator.ps1` script to create the public folder-to-mailbox mapping file. This file is used to calculate the correct number of public folder mailboxes on the Exchange 2013 Mailbox server.

   > [!NOTE]
   > If the name of a public folder contains a backslash **\\**, the public folders will be created in the parent public folder. We recommend that you review the .csv file and edit any names that contain a backslash.

   ```powershell
   .\PublicFolderToMailboxMapGenerator.ps1 <Maximum mailbox size in bytes> <Folder to size map path> <Folder to mailbox map path>
   ```

   - *Maximum mailbox size in bytes* equals the maximum size you want to set for the new public folder mailboxes. When specifying this setting, be sure to allow for expansion so the public folder mailbox has room to grow.

   - *Folder to size map path* equals the file path of the .csv file you created when running the `Export-PublicFolderStatistics.ps1` script.

   - *Folder to mailbox map path* equals the file name and path of the folder-to-mailbox .csv file that you'll create with this step. If you specify only the file name, the file will be generated in the current PowerShell directory on the local computer.

## Step 4: Create the public folder mailboxes in Exchange 2013

1. Run the following command to create the target public folder mailboxes. The script will create a target mailbox for each mailbox in the .csv file that you generated previously in Step 3, by running the PublicFoldertoMailboxMapGenerator.ps1 script.

   ```powershell
   .\Create-PublicFolderMailboxesForMigration.ps1 -FolderMappingCsv Mapping.csv -EstimatedNumberOfConcurrentUsers:<estimate>
   ```

   *Mapping.csv* is the file generated by the PublicFoldertoMailboxMapGenerator.ps1 script in Step 3. The estimated number of simultaneous user connections browsing a public folder hierarchy is usually less than the total number of users in an organization.

## Step 5: Start the migration request

The steps for migrating Exchange 2007 public folders are different from the steps for migrating Exchange 2010 public folders.

> [!TIP]
> Whether migrating from Exchange 2007 or Exchange 2010, once batch migration requests are created with the appropriate cmdlet, you can then view the requests and manage them in the EAC.

### Migrate Exchange 2007 public folders

1. Legacy system public folders such as OWAScratchPad and the schema-root folder subtree in Exchange 2007 won't be recognized by Exchange 2013 and will therefore be treated as "bad" items. This will cause the migration to fail. As part of the migration request, you must specify a value for the `BadItemLimit` parameter. This value will vary depending on the number of public folder databases you have. The following commands will determine how many public folder databases you have and compute the `BadItemLimit` for the migration request.

   ```powershell
   $PublicFolderDatabasesInOrg = @(Get-PublicFolderDatabase)
   ```

   ```powershell
   $BadItemLimitCount = 5 + ($PublicFolderDatabasesInOrg.Count -1)
   ```

2. On the Exchange 2013 server, run the following command:

   ```powershell
   New-MigrationBatch -Name PFMigration -SourcePublicFolderDatabase (Get-PublicFolderDatabase -Server <Source server name>) -CSVData (Get-Content <Folder to mailbox map path> -Encoding Byte) -NotificationEmails <email addresses for migration notifications> -BadItemLimit $BadItemLimitCount
   ```

3. Start the migration using the following command:

   ```powershell
   Start-MigrationBatch PFMigration
   ```

### Migrate Exchange 2010 public folders

1. On the Exchange 2013 server, run the following command.

   ```powershell
   New-MigrationBatch -Name PFMigration -SourcePublicFolderDatabase (Get-PublicFolderDatabase -Server <Source server name>) -CSVData (Get-Content <Folder to mailbox map path> -Encoding Byte) -NotificationEmails <email addresses for migration notifications>
   ```

   The *NotificationEmails* parameter is optional.

2. Start the migration using the following command:

   ```powershell
   Start-MigrationBatch PFMigration
   ```

   Or:

   You can start the migration in the EAC.

   1. Log into Exchange Online and open the EAC.

   2. Navigate to **Recipients** \> **Migration**.

   3. Select the migration batch you just created, and then click the start button.

The **Status** column will show the initial batch status as **Created**. The status changes to **Syncing** during migration. When the migration request is complete, the status will be **Synced**. You can double-click a batch to view the status of individual mailboxes within the batch. Mailbox jobs begin with a status of **Queued**. When the job begins the status is **Syncing**, and once `InitialSync` is complete, the status will show **Synced**.

The progress and completion of the migration can be viewed and managed in the EAC. Because the **New-MigrationBatch** cmdlet initiates a mailbox migration request for each public folder mailbox, you can view the status of these requests using the mailbox migration page. You can get to the mailbox migration page, and create migration reports that can be emailed to you, by doing the following:

1. Log into Exchange Online and open the EAC.

2. Navigate to **Mailbox** \> **Migration**.

3. Select the migration request that was just created and then click **View Details** in the **Details** pane.

For detailed syntax and parameter information, see the following topics:

- [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/new-migrationbatch)

- [Get-PublicFolderDatabase](https://docs.microsoft.com/powershell/module/exchange/Get-PublicFolderDatabase)

- [Get-PublicFolderMailboxMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxmigrationrequest)

- [Get-PublicFolderMailboxMigrationRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxmigrationrequeststatistics)

## Step 6: Lock down the public folders on the legacy Exchange server for final migration (downtime required)

Until this point in the migration, users have been able to access public folders. The next steps will log users off from the legacy public folders and lock the folders while the migration completes its final synchronization. Users won't be able to access public folders during this process. Also, any mail sent to mail-enabled public folders will be queued and won't be delivered until the public folder migration is complete.

Before you run the `PublicFoldersLockedForMigration` command as described below, make sure that all jobs are in the **Synced** state. You can do this by running the `Get-PublicFolderMailboxMigrationRequest` command. Continue with this step only after you've verified that all jobs are in the **Synced** state.

On the legacy Exchange server, run the following command to lock the legacy public folders for finalization.

```powershell
Set-OrganizationConfig -PublicFoldersLockedForMigration:$true
```

For detailed syntax and parameter information, see [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/Set-OrganizationConfig).

If your organization has multiple public folder databases, you'll need to wait until public folder replication is complete to confirm that all public folder databases have picked up the `PublicFoldersLockedForMigration` flag and any pending changes users recently made to folders have converged across the organization. This may take several hours.

## Step 7: Finalize the public folder migration (downtime required)

First, run the following cmdlet to change the Exchange 2013 deployment type to **Remote**:

```powershell
Set-OrganizationConfig -PublicFoldersEnabled Remote
```

Once that is done, you can complete the public folder migration by running the following command:

```powershell
Complete-MigrationBatch PFMigration
```

Or, in EAC, you can complete the migration by clicking **Complete this migration batch**.

> [!NOTE]
> If for any reason the migration batch file does not finalize (<STRONG>PublicFolderMigrationComplete</STRONG> displays <STRONG>False</STRONG>), on the legacy server, restart the Information Store (IS).

When you complete the migration, Exchange will perform a final synchronization between the legacy Exchange server and Exchange 2013. If the final synchronization is successful, the public folders on the Exchange 2013 server will be unlocked and the status of the migration batch will change to **Completing**, and then **Completed**. It is common for the migration batch to take a few hours before its status changes from **Synced** to **Completing**, at which point the final synchronization will begin.

## Step 8: Test and unlock the public folder migration

After you finalize the public folder migration, you should run the following test to make sure that the migration was successful. This allows you to test the migrated public folder hierarchy before you switch to using Exchange 2013 public folders.

1. In PowerShell, run the following command to assign some test mailboxes to use any newly migrated public folder mailbox as the default public folder mailbox.

   ```powershell
   Set-Mailbox -Identity <Test User> -DefaultPublicFolderMailbox <Public Folder Mailbox Identity>
   ```

2. Log on to Outlook 2007 or later with the test user identified in the previous step, and then perform the following public folder tests:

   - View the hierarchy.

   - Check permissions.

   - Create and delete public folders.

   - Post content to and delete content from a public folder.

3. If you run into any issues, see Roll back the migration later in this topic. If the public folder content and hierarchy is acceptable and functions as expected, run the following command to unlock the public folders for all other users.

   ```powershell
   Get-Mailbox -PublicFolder | Set-Mailbox -PublicFolder -IsExcludedFromServingHierarchy $false
   ```

   > [!IMPORTANT]
   > Don't use the <EM>IsExcludedFromServingHierarchy</EM> parameter after initial migration validation is complete as this parameter is used by the automated storage management service for Exchange Online.

4. On the legacy Exchange server, run the following command to indicate that the public folder migration is complete:

   ```powershell
   Set-OrganizationConfig -PublicFolderMigrationComplete:$true
   ```

5. After you've verified that the migration is complete, run the following command:

   ```powershell
   Set-OrganizationConfig -PublicFoldersEnabled Local
   ```

6. Finally, if you want external senders to send mail to the migrated mail-enabled public folders, the **Anonymous** user needs to be granted at least the **Create Items** permission. If you don't do this, external senders will receive a delivery failure notification and the messages won't be delivered to the migrated mail-enabled public folder.

   You can use the Shell or Outlook to set the permissions on the Anonymous user. To read more about how to set permissions on the Anonymous user, see [Mail-enable or mail-disable a public folder](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/enable-or-disable-mail-for-public-folder).

## How do I know this worked?

In [Step 2: Prepare for the migration](#step-2-prepare-for-the-migration), you were instructed to take snapshots of the public folder structure, statistics, and permissions before the migration began. The following steps will help verify that your public folder migration was successful by taking the same snapshots after the migration is complete. You can then compare the data in both files to verify success.

1. Run the following command to take a snapshot of the new folder structure.

    ```powershell
    Get-PublicFolder -Recurse -ResultSize Unlimited | Export-CliXML C:\PFMigration\Cloud_PFStructure.xml
    ```

2. Run the following command to take a snapshot of the public folder statistics such as item count, size, and owner.

    ```powershell
    Get-PublicFolderStatistics -ResultSize Unlimited | Export-CliXML C:\PFMigration\Cloud_PFStatistics.xml
    ```

3. Run the following command to take a snapshot of the permissions.

    ```powershell
    Get-PublicFolder -Recurse -ResultSize Unlimited | Get-PublicFolderClientPermission | Select-Object Identity,User -ExpandProperty AccessRights | Export-CliXML  C:\PFMigration\Cloud_PFPerms.xml
    ```

## Remove public folder databases from the legacy Exchange servers

After the migration is complete, and you have verified that your Exchange 2013 public folders are working as expected, you should remove the public folder databases on the legacy Exchange servers.

- For details about how to remove public folder databases from Exchange 2007 servers, see [Removing Public Folder Databases](https://go.microsoft.com/fwlink/?linkid=123678).

- For details about how to remove public folder databases from Exchange 2010 servers, see [Remove Public Folder Databases](https://go.microsoft.com/fwlink/?linkid=81409).

## Roll back the migration

If you run into issues with the migration and need to reactivate your legacy Exchange public folders, perform the following steps.

> [!WARNING]
> If you roll your migration back to the legacy Exchange servers, you will lose any email that was sent to mail-enabled public folders or content that was posted to public folders in Exchange 2013 after the migration. To save this content, you need to export the public folder content to a .pst file and then import it to the legacy public folders when the rollback is complete.

1. On the legacy Exchange server, run the following command to unlock the legacy Exchange public folders. This process may take several hours.

   ```powershell
   Set-OrganizationConfig -PublicFoldersLockedForMigration:$False
   ```

2. On the Exchange 2013 server, run the following commands to remove the public folder mailboxes.

   ```powershell
   Get-Mailbox -PublicFolder | Where{$_.IsRootPublicFolderMailbox -eq $false} | Remove-Mailbox -PublicFolder -Force -Confirm:$false
   ```

   ```powershell
   Get-Mailbox -PublicFolder | Remove-Mailbox -PublicFolder -Force -Confirm:$false
   ```

3. On the legacy Exchange server, run the following command to set the `PublicFolderMigrationComplete` flag to `$false`.

   ```powershell
   Set-OrganizationConfig -PublicFolderMigrationComplete:$False
   ```
