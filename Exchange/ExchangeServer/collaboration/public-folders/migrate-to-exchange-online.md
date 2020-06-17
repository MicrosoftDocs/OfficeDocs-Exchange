---
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.collection:
- Strat_EX_EXOBlocker
- exchange-server
description: 'Summary: This article tells you how to move modern public folders from Exchange Server to Office 365.'
f1.keywords:
- NOCSH
audience: ITPro
title: Exchange public folder migration, migrate public folders to Office 365, public folder migration Exchange to Office 365, migrate Exchange public folders to Microsoft 365

---

# Use batch migration to migrate Exchange Server public folders to Exchange Online

**Applies to: Exchange Server 2013**, **Exchange Server 2016**, and **Exchange Server 2019**

Migrating your Exchange Server public folders to Exchange Online requires Exchange Server 2013 CU15 or later, or Exchange Server 2016 CU4 or later, to be running in your on-premises environment. All versions of Exchange Server 2019 are supported for batch migrations of public folders.

If you have a mixed environment of both Exchange 2013 and Exchange 2016/2019 public folders in your organization, and you want to move them all to Exchange Online, the instructions in this article will work for you, provided your Exchange 2013 servers have CU15 or later installed.

For instructions on migrating Exchange Server 2010 public folders to Exchange Online, see [Use batch migration to migrate legacy public folders to Exchange Online](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/batch-migration-of-legacy-public-folders).

## What do you need to know before you begin?

- We strongly recommend you review [FAQ: Public folders](faq.md) before you attempt a migration.

- When you upgrade to Exchange Server 2013 CU15 or later, or to Exchange Server 2016 CU4 or later, you must also prepare Active Directory or your public folder migration will fail. This Active Directory preparation ensures that all relevant PowerShell cmdlets and parameters are available to you for preparing for and running the migration. See [Prepare Active Directory and domains](../../plan-and-deploy/prepare-ad-and-domains.md) for more information.

- In Exchange Online, you need to be a member of the Organization Management role group. This role group is different from the permissions assigned to you when you subscribe to Office 365 or Exchange Online. For details about how to enable the Organization Management role group, see [Manage role groups](../../permissions/role-groups.md).

- In Exchange Server, you need to be a member of the Organization Management or Server Management RBAC role groups. For details, see [Add Members to a Role Group](https://go.microsoft.com/fwlink/p/?linkId=299212).

- Before you begin the public folder migration, if any single public folder in your organization is larger than 25 GB, we recommend that you delete content from that folder to make it smaller, or divide the public folder's content into multiple, smaller public folders. Note that the 25 GB limit cited here only applies to the public folder and not to any child or sub-folders the folder in question may have. If neither option is feasible, we recommend that you do not move your public folders to Exchange Online. See [Exchange Online Limits](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits) for more information.

    > [!NOTE]
    > If your current public folder quotas in Exchange Online are less than 25 GB, you can use the [Set-OrganizationConfig](https://docs.microsoft.com/powershell/module/exchange/Set-OrganizationConfig) cmdlet to increase them with the DefaultPublicFolderIssueWarningQuota and DefaultPublicFolderProhibitPostQuota parameters.

- In Office 365 and Exchange Online, you can create a maximum of 1000 public folder mailboxes. However, a maximum of 100 public folders is supported for migration from Exchange Server.

- If you intend to migrate users to Office 365, you should complete your user migration prior to migrating your public folders. For more information, see [Ways to migrate multiple email accounts to Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/mailbox-migration).

- MRS Proxy needs to be enabled on at least one Exchange server, a server that is also hosting public folder mailboxes. See [Enable the MRS Proxy endpoint for remote moves](https://docs.microsoft.com/Exchange/architecture/mailbox-servers/mrs-proxy-endpoint) for details.

- To perform the migration procedures in this article, you can't use the Exchange admin center (EAC). Instead, you need to use the Exchange Management Shell on your Exchange servers. In Exchange Online, you need to use Exchange Online PowerShell. For more information, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=842801).

- To run the migration scripts in this article, you must use an account that has basic authentication enabled. Accounts that use multi-factor authentication (MFA) are currently not supported.

- Skipping the migration of deleted items and deleted folders from Exchange Server to Exchange Online is supported. For more information, see the Exchange Team blog post about [modern public folder migrations without dumpster data](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Announcing-the-support-for-modern-public-folder-migrations/ba-p/608004).

- You must use a single migration batch to migrate all of your public folder data. Exchange allows creating only one migration batch at a time. If you attempt to create more than one migration batch simultaneously, the result will be an error. Also note that once the migration batch has a status of "Completed," no more data can be copied over from the source environment.

- We recommend that you don't use Outlook's PST export feature to migrate public folders to Office 365 or Exchange Online. Public folder mailbox growth in Exchange Online is managed using an auto-split feature that splits the public folder mailbox when it exceeds size quotas. Auto-split can't handle the sudden growth of public folder mailboxes when you use PST export to migrate your public folders, and you may have to wait for up to two weeks for auto-split to move the data from the primary mailbox. We recommend that instead you use the cmdlet-based instructions in this article to migrate your public folders. If you still decide to migrate public folders using PST export, see [Migrate Public Folders to Office 365 by using Outlook PST export](#migrate-public-folders-to-office-365-by-using-outlook-pst-export)  later in this article.

- Before you begin, please read this article in its entirety. For some steps there is downtime required. During this downtime, public folders will not be accessible by anyone. Please also review the list of [known issues](#known-issues).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Download the migration scripts

1. Download all scripts and supporting files from [Exchange 2013/2016/2019 Public Folders Migration Scripts](https://www.microsoft.com/download/details.aspx?id=54855) and [Exchange 2010/2013/2016/EXO Public Folders to Office365 Pre-Migration Scripts](https://www.microsoft.com/download/details.aspx?id=100414).

2. Save the scripts to the local computer on which you'll be running PowerShell. For example, C:\PFScripts. Make sure all scripts are saved in the same location.

The scripts and files you're downloading are:

- `SourceSideValidations.ps1`:  Source Side Validation script scans the public folders at source and reports issues found along with actions required to fix the issues. You'll run this script on the Exchange server on-premises.

- `Sync-ModernMailPublicFolders.ps1` This script synchronizes mail-enabled public folder objects between your Exchange on-premises environment and Office 365. You'll run this script on an on-premises Exchange server.

- `SyncModernMailPublicFolders.strings.psd1` This support file is used by the Sync-ModernMailPublicFolders.ps1 script and should be downloaded to the same location.

- `Export-ModernPublicFolderStatistics.ps1` This script creates the folder name-to-folder size and deleted item size mapping file. You'll run this script on an on-premises Exchange server.

- `Export-ModernPublicFolderStatistics.strings.psd1` This support file is used by the Export-ModernPublicFolderStatistics.ps1 script and should be downloaded to the same location.

- `ModernPublicFolderToMailboxMapGenerator.ps1` This script creates the public folder-to-mailbox mapping file by using the output from the Export-ModernPublicFolderStatistics.ps1 script. You'll run this script on an on-premises Exchange server.

- `ModernPublicFolderToMailboxMapGenerator.strings.psd1` This support file is used by the ModernPublicFolderToMailboxMapGenerator.ps1 script and should be downloaded to the same location.

- `SetMailPublicFolderExternalAddress.ps1` This script updates the `ExternalEmailAddress` of mail-enabled public folders in your on-premises environment to that of their Exchange Online counterparts, so that emails addressed to your mail-enabled public folders post-migration are properly routed to Exchange Online. You need to run this script on an on-premises Exchange server.

- `SetMailPublicFolderExternalAddress.strings.psd1` This support file is used by the Create-PublicFolderMailboxesForMigration.ps1 script and should be downloaded to the same location.

## Step 2: Prepare for the migration

> [!NOTE]
> We strongly recommend running the Source Side Validation script from an on-premises Exchange Mailbox server. The script will scan and report issues that are known to cause migration to be slow, along with guidance to fix these issues. Use the examples as documented [here](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Making-your-public-folder-migrations-faster-and-more-reliable/ba-p/917622). The script will perform all the following prerequisites.

Perform all prerequisite steps in the following sections before you begin the public folder migration.

### General prerequisite steps

For your migration to be successful, you should:

- Make sure that there are no orphaned public folder mail objects in Active Directory. These are objects in Active Directory without a corresponding Exchange object.

- Confirm that the SMTP email addresses configured for public folders in Active Directory match the SMTP email addresses on the Exchange objects.

- Confirm that there are no duplicate public folder objects in Active Directory. This is necessary to avoid having two or more Active Directory objects that are pointing to the same mail-enabled public folder.

### Prerequisite steps in the on-premises Exchange 2013, Exchange 2016, or Exchange 2019 server environment

In Exchange Management Shell (on-premises) perform the following steps:

1. Once your migration is complete, it will take some time for DNS caches across the Internet to direct messages to your mail-enabled public folders in their new location in Exchange Online. You can ensure that your newly migrated mail-enabled public folders receive messages during this DNS transition period by creating an accepted domain with a well-known name. To do this, run the following command in your Exchange on-premises environment. In this example, `target domain` is your Office 365 or Exchange Online domain, for which a send connector has already been configured by the Hybrid Configuration Wizard.

   ```PowerShell
   New-AcceptedDomain -Name PublicFolderDestination_78c0b207_5ad2_4fee_8cb9_f373175b3f99 -DomainName <target domain> -DomainType InternalRelay
   ```

   **Example**:

   ```PowerShell
   New-AcceptedDomain -Name PublicFolderDestination_78c0b207_5ad2_4fee_8cb9_f373175b3f99 -DomainName "contoso.mail.onmicrosoft.com" -DomainType InternalRelay
   ```

   If the accepted domain already exists in your on-premises environment, rename it to `PublicFolderDestination_78c0b207_5ad2_4fee_8cb9_f373175b3f99` and leave the other attributes intact.

   To check if the accepted domain is already present in your on-premises environment, run the following:

   ```PowerShell
   Get-AcceptedDomain | Where {$_.DomainName -eq "<target domain>"}
   ```

   To rename the accepted domain to `PublicFolderDestination_78c0b207_5ad2_4fee_8cb9_f373175b3f99`, run the following:

   ```PowerShell
   Get-AcceptedDomain | Where {$_.DomainName -eq "<target domain>"} | Set-AcceptedDomain -Name PublicFolderDestination_78c0b207_5ad2_4fee_8cb9_f373175b3f99
   ```

   > [!NOTE]
   > If you're expecting your mail-enabled public folders in Exchange Online to receive external emails from the Internet, you have to disable Directory Based Edge Blocking (DBEB) in Exchange Online and Exchange Online Protection (EOP). See [Use Directory Based Edge Blocking to Reject Messages Sent to Invalid Recipients](https://docs.microsoft.com/exchange/mail-flow-best-practices/use-directory-based-edge-blocking) for more information.

2. If the name of a public folder contains a backslash **\\** or a forward slash **/**, it may not get migrated to its designated mailbox during the migration process. Before you migrate, rename any such folders to remove these characters.

   a. To locate public folders that have a backslash in the name, run the following command:

      ```PowerShell
      Get-PublicFolder -Recurse -ResultSize Unlimited | Where {$_.Name -like "*\*" -or $_.Name -like "*/*"} | Format-List Name, Identity, EntryId
      ```

   b. If any public folders are returned, you can rename them by running the following command:

      ```PowerShell
      Set-PublicFolder -Identity "<public folder EntryId>" -Name "<new public folder name>"
      ```

3. (This step is only required only if you are re-doing a previous migration attempt for some reason. If this is not the case, skip to the next step.) Run the following cmdlets to confirm there isn't a record of a previous, successful migration in your organization. If there is, you need to set that value to `$false`.

   Before changing the values, please confirm that the previous migration attempt can be discarded so that you don't accidentally perform a second migration.

   a. Run the following command to check for any previous migrations, and the status of those migrations:

      ```PowerShell
      Get-OrganizationConfig | Format-List  PublicFolderMailboxesLockedForNewConnections, PublicFolderMailboxesMigrationComplete
      ```

   b. If any of the above is returned with a value set to `$true`, make them `$false` by running:

      ```PowerShell
      Set-OrganizationConfig -PublicFolderMailboxesLockedForNewConnections:$false -PublicFolderMailboxesMigrationComplete:$false
      ```

4. For the purpose of verifying the success of the migration upon its completion, we recommend that you run the following commands on all appropriate Exchange 2016 or Exchange 2019 servers. This will take snapshots of your current public folder deployment that you can later use to compare with your newly migrated public folders.

   > [!NOTE]
   > Depending on the size of your Exchange organization, it could take some time for these commands to run.

   - Run the following command to take a snapshot of the original source folder structure.

     ```PowerShell
     Get-PublicFolder -Recurse -ResultSize Unlimited | Export-CliXML OnPrem_PFStructure.xml
     ```

   - Run the following command to take a snapshot of public folder statistics such as item count, size, and owner.

     ```PowerShell
     Get-PublicFolderStatistics -ResultSize Unlimited | Export-CliXML OnPrem_PFStatistics.xml
     ```

   - Run the following command to take a snapshot of public folder permissions.

     ```PowerShell
     Get-PublicFolder -Recurse -ResultSize Unlimited | Get-PublicFolderClientPermission | Select-Object Identity,User,AccessRights -ExpandProperty AccessRights | Export-CliXML OnPrem_PFPerms.xml
     ```

   - Run the following command to take a snapshot of your mail-enabled public folders:

     ```PowerShell
     Get-MailPublicFolder -ResultSize Unlimited | Export-CliXML OnPrem_MEPF.xml
     ```

   - Save the files generated from the preceding commands in a safe place in order to make a comparison at the end of the migration.

5. If you're using Microsoft Azure Active Directory Connect (Azure AD Connect) to synchronize your on-premises directories with Azure Active Directory, you need to do the following (if you aren't using Azure AD Connect, you can skip this step):

   1. On an on-premises computer, open Microsoft Azure Active Directory Connect, and then select **Configure**.

   2. On the **Additional tasks** screen, select **Customize synchronization options**, and then click **Next**.

   3. On the **Connect to Azure AD** screen, enter the appropriate credentials, and then click **Next**. Once connected, keep clicking **Next** until you're on the **Optional Features** screen.

   4. Make sure that **Exchange Mail Public Folders** is not selected. If it isn't selected, you can continue to the next section, *Prerequisite steps in Exchange Online*. If it is selected, click to clear the check box, and then click **Next**.

      > [!NOTE]
      > If you don't see **Exchange Mail Public Folders** as an option on the **Optional Features** screen, you can exit Microsoft Azure Active Directory Connect and proceed to the next section, *Prerequisite steps in Exchange Online*.

   5. After you have cleared the **Exchange Mail Public Folders** selection, keep clicking **Next** until you're on the **Ready to configure** screen, and then click **Configure**.

### Prerequisite steps in Exchange Online

In Exchange Online PowerShell, do the following steps:

1. Make sure there are no existing public folder migration requests. If there are, clear them or your own migration request will fail. This step is only required if you think there may be an existing migration request in the pipeline (one that has failed or that you wish to abort).

   The following example will discover any existing batch migration requests:

   ```PowerShell
   Get-MigrationBatch | ?{$_.MigrationType.ToString() -eq "PublicFolder"}
   ```

   The following example removes any existing public folder batch migration requests:

   ```PowerShell
   Remove-MigrationBatch <name of migration batch> -Confirm:$false
   ```

2. Make sure there aren't any existing public folders or public folder mailboxes in Exchange Online. If you do discover public folders in Exchange Online after following the steps below, it's important to determine why they are there and who in your organization started a public folder hierarchy before you begin removing any public folders and public folder mailboxes.

   a. In Exchange Online PowerShell, run the following command to see if any public folders mailboxes exist:

      ```PowerShell
      Get-Mailbox -PublicFolder
      ```

   b. If the command doesn't return any public folder mailboxes, continue to [Step 3: Generate the .csv files](#step-3-generate-the-csv-files). If the command does return any public folders mailboxes, run the following command to see if any public folders exist:

      ```PowerShell
      Get-PublicFolder -Recurse
      ```

3. If you do have any public folders in Office 365 or Exchange Online, run the following PowerShell command to remove them (after confirming that they are not needed). Make sure that you've saved any information within these public folders before deleting them, because all information will be permanently deleted when you remove the public folders.

   ```PowerShell
   Get-MailPublicFolder -ResultSize Unlimited | where {$_.EntryId -ne $null}| Disable-MailPublicFolder -Confirm:$false
   Get-PublicFolder -GetChildren \ -ResultSize Unlimited | Remove-PublicFolder -Recurse -Confirm:$false
   ```

4. After the public folders are removed, run the following commands to remove all public folder mailboxes:

   ```PowerShell
   $hierarchyMailboxGuid = $(Get-OrganizationConfig).RootPublicFolderMailbox.HierarchyMailboxGuid
   Get-Mailbox -PublicFolder | Where-Object {$_.ExchangeGuid -ne $hierarchyMailboxGuid} | Remove-Mailbox -PublicFolder -Confirm:$false -Force
   Get-Mailbox -PublicFolder | Where-Object {$_.ExchangeGuid -eq $hierarchyMailboxGuid} | Remove-Mailbox -PublicFolder -Confirm:$false -Force
   Get-Mailbox -PublicFolder -SoftDeletedMailbox | % {Remove-Mailbox -PublicFolder $_.PrimarySmtpAddress -PermanentlyDelete:$true -force}
   ```

## Step 3: Generate the .csv files

Use the previously downloaded scripts to generate the .csv files that will be used in the migration.

1. From the Exchange Management Shell (on-premises), run the `Export-ModernPublicFolderStatistics.ps1` script to create the folder name-to-folder size mapping file. You must have local administrator permissions to run this script. The resulting file will contain three columns: **FolderName**, **FolderSize**, and **DeletedItemSize**. The values for the **FolderSize** and **DeletedItemSize** columns will be displayed in bytes. For example, **\PublicFolder01,10240, 100** means the public folder in the root of your hierarchy named PublicFolder01 is 10240 bytes (10 KB) in size and there are 100 bytes of recoverable items in it.

   ```PowerShell
   .\Export-ModernPublicFolderStatistics.ps1 <Folder-to-size map path>
   ```

   **Example**:

   ```PowerShell
   .\Export-ModernPublicFolderStatistics.ps1 stats.csv
   ```

2. Run the `ModernPublicFolderToMailboxMapGenerator.ps1` script to create a .csv file that maps source public folders to public folder mailboxes in your Exchange Online destination. This file is used to calculate the correct number of public folder mailboxes in Exchange Online.

Note that the file generated by `ModernPublicFolderToMailboxMapGenerator.ps1` will not contain the name of every public folder in your organization. It will contain references to the parent folders of larger folder trees, or the names of folders which themselves are significantly large. You can think of this file as an "exception" file used to make sure certain folder trees and larger folders get placed into specific public folder mailboxes. It is normal to not see every one of your public folders in this file. Child folders of any folder listed in this mapping file will also be migrated to the same public folder mailbox as their parent folder (unless explicitly mentioned on another line within the mapping file that directs them to a different public folder mailbox).

```PowerShell
.\ModernPublicFolderToMailboxMapGenerator.ps1 <Maximum mailbox size in bytes><Maximum mailbox recoverable item size in bytes><Folder-to-size map path><Folder-to-mailbox map path>
```

- `Maximum mailbox size in bytes` is the maximum amount of data you want to migrate into any single public folder mailbox in Exchange Online. The maximum size of this field is currently 50 GB, but we recommend you use a smaller size, such as 50% of maximum size, to allow for future growth.

- `Maximum mailbox recoverable items size in bytes` is the recoverable items quota on your Exchange Online mailboxes. The maximum size of public folder mailboxes In Exchange Online is currently 50 GB. We recommend setting *RecoverableItemsQuota* to 15 GB or less.

- `Folder-to-size map path` is the file path of the .csv file you created when you ran the `Export-ModernPublicFolderStatistics.ps1` script.

- `Folder-to-mailbox map path` is the file path of the folder-to-mailbox .csv file that you're creating in this step. If you only specify a file name, the file will be generated in the current PowerShell directory on the local computer.

**Example**:

```PowerShell
.\ModernPublicFolderToMailboxMapGenerator.ps1 -MailboxSize 25GB -MailboxRecoverableItemSize 1GB -ImportFile .\stats.csv -ExportFile map.csv
```

> [!NOTE]
> We don't support the migration of public folders to Exchange Online when there are more than 100 unique public folder mailboxes in Exchange Online. During migration, you can have up to 100 public folder mailboxes enabled.

## Step 4: Create the public folder mailboxes in Exchange Online

Next, in Exchange Online PowerShell, create the target public folder mailboxes that will contain your migrated public folders.

Run the following script to create the target public folder mailboxes. The script will create a target mailbox for each mailbox in the .csv file that you generated previously in *Step 3: Generate the .csv files*, when you ran the `ModernPublicFoldertoMailboxMapGenerator.ps1` script.

```PowerShell
$mappings = Import-Csv <Folder-to-mailbox map path>
$primaryMailboxName = ($mappings | Where-Object FolderPath -eq "\" ).TargetMailbox;
New-Mailbox -HoldForMigration:$true -PublicFolder -IsExcludedFromServingHierarchy:$false $primaryMailboxName
($mappings | Where-Object TargetMailbox -ne $primaryMailboxName).TargetMailbox | Sort-Object -unique | ForEach-Object { New-Mailbox -PublicFolder -IsExcludedFromServingHierarchy:$false $_ }
```

- `Folder-to-mailbox map path` is the file path of the folder-to-mailbox.csv file that was generated by the `ModernPublicFoldertoMailboxMapGenerator.ps1` script in *Step 3: Generate the .csv files*.

## Step 5: Start the migration request

A number of commands now need to be run both in your Exchange Server on-premises environment and in Exchange Online.

1. From any of your Exchange 2016 or Exchange 2019 servers hosting public folder mailboxes, execute the following script. This script will synchronize mail-enabled public folders from your local Active Directory to Exchange Online. Make sure that you have downloaded the latest version of this script and that you're running it from Exchange Management Shell.

   ```PowerShell
   .\Sync-ModernMailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile:sync_summary.csv
   ```

   - `Credential` is your Exchange Online administrative username and password.

   - `CsvSummaryFile` is the file path to where you want your log file of synchronization operations and errors located. The log will be in .csv format.

2. In Exchange Online PowerShell, pass the credential of a user who has administrator permissions in the Exchange 2013, Exchange 2016, or Exchange 2019 on-premises environment into the variable `$Source_Credential`. The migration request that you run in Exchange Online will use this credential to gain access to your on-premises Exchange servers to copy the public folder content over to Exchange Online.

   ```PowerShell
   $Source_Credential = Get-Credential <source_domain>\<PublicFolder_Administrator_Account>
   ```

3. In Exchange Online Powershell, pass the Internet routable fully qualified domain name of your Exchange Mailbox Replication Service (MRS) into the variable `$Source_RemoteServer`. The migration request that you run in Exchange Online will use this remote server to copy the public folder content to Exchange Online.

   ```PowerShell
   $Source_RemoteServer = "<MRS proxy endpoint server>"
   ```

4. On your on-premises Exchange server, open the Exchange Management Shell and find the GUID of the primary hierarchy mailbox with the following command:

   ```PowerShell
   (Get-OrganizationConfig).RootPublicFolderMailbox.HierarchyMailboxGuid.GUID
   ```

   Note the output of this command. You will need it in the next step. For example:

   > 91edc6dd-478a-497c-8731-b0b793f5a986

5. In Exchange Online PowerShell, run the following commands to create the public folder migration endpoint and the public folder migration request:

   ```PowerShell
   [byte[]]$bytes = Get-Content -Encoding Byte <folder_mapping.csv>
   $PfEndpoint = New-MigrationEndpoint -PublicFolder -Name PublicFolderEndpoint -RemoteServer $Source_RemoteServer -Credentials $Source_Credential
   New-MigrationBatch -Name PublicFolderMigration -CSVData $bytes -SourceEndpoint $PfEndpoint.Identity -SourcePfPrimaryMailboxGuid <guid you noted from previous step> -AutoStart -NotificationEmails <email addresses for migration notifications>
   ```

   Where `folder_mapping.csv` is the map file that was generated in *Step 3: Generate the .csv files* and `HierarchyMailboxGUID` is the output you noted in the previous step. Be sure to provide the full file path to `folder_mapping.csv`. If the map file was moved for any reason, be sure to use the new location.

   Separate multiple email addresses with commas.

   > [!NOTE]
   > If you receive an error that states, `A parameter cannot be found that matches parameter name 'SourcePfPrimaryMailboxGuid'`, then instead use the following command to create the migration batch:
   >
   > ```PowerShell
   > New-MigrationBatch -Name PublicFolderMigration -CSVData $bytes -SourceEndpoint $PfEndpoint.Identity -AutoStart -NotificationEmails <email addresses for migration notifications>
   > ```

6. Finally, start the migration using the following command in Exchange Online PowerShell:

   ```PowerShell
   Start-MigrationBatch PublicFolderMigration
   ```

While batch migrations need to be created using the `New-MigrationBatch` cmdlet in Exchange Online PowerShell, the progress and completion of the migration can be viewed and managed in the EAC or by running the Get-MigrationBatch cmdlet. The `New-MigrationBatch` cmdlet initiates a mailbox migration request for each public folder mailbox, and you can view the status of these requests using the mailbox migration page.

To go to the mailbox migration page:

1. Log on to Exchange Online and open the EAC.

2. Navigate to **Recipients**, and then select **Migration**.

3. Select the migration request that was just created and then, on the **Details** pane, select **View Details**.

Before moving on to *Step 6: Lock down the public folders on the  Exchange on-premises server*, verify that all data has been copied and that there are no errors in the migration. Once you have confirmed that the batch has moved to the state of **Synced**, run the commands mentioned in *Step 2: Prepare for the migration*, in the final step under **Prerequisite steps in the Exchange Server on-premises environment**, to take a snapshot of the public folders on-premises.

Once these commands have run, you can proceed to the next step. Note that these commands could take a while to complete depending on the number of folders you have. The migration process will synchronize the data from the source (on-premises) environment once every 24 hours.

You can use the following cmdlets to monitor your migration:

- [Get-PublicFolderMailboxMigrationRequest](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxmigrationrequest?view=exchange-ps)

- [Get-PublicFolderMailboxMigrationRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/get-publicfoldermailboxmigrationrequeststatistics?view=exchange-ps)

- [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/get-migrationbatch?view=exchange-ps)

## Step 6: Lock down the public folders on the Exchange on-premises server (public folder downtime required)

Until this point in the migration process, users have been able to access your on-premises public folders. The following steps will now log off users off from Exchange Server public folders and then lock the folders as the migration process completes its final synchronization. Users won't be able to access public folders during this time, and any messages sent to these mail-enabled public folders will be queued and remain undelivered until the public folder migration is complete.

> [!NOTE]
> The final sync might take a substantial amount of time, depending on the changes made to the source environment, the size of the public folder deployment, server capacity, and so on. If the folder hierarchy had many corrupt ACLs that were not cleaned up before the migration, there might be a significant delay in completion. It is recommended that you plan for a minimum of 48 hours of downtime for the final sync to complete.

Ensure the migration batch and individual migration requests have successfully synced.

Run the following command in EXO PowerShell for more information:

`Get-MigrationBatch |?{$_.MigrationType -like "*PublicFolder*"} | ft *last*sync*`

`Get-PublicFolderMailboxMigrationRequest | Get-PublicFolderMailboxMigrationRequestStatistics |ft targetmailbox,*last*sync*`

The LastSyncedDate (on migration batch) and LastSuccessfulSyncTimestamp (on individual jobs) should be within the last 7 days. If the date is too far in the past, such as more than a month ago, you might want to review public folder migration requests and ensure that all the requests were synced recently.

After you have confirmed that the batch and all migration requests have successfully synced, in your on-premises environment, run the following command to lock the Exchange Server public folders for finalization.

```PowerShell
Set-OrganizationConfig -PublicFolderMailboxesLockedForNewConnections $true
```

> [!NOTE]
> If you aren't able to access the `-PublicFolderMailboxesLockedForNewConnections` parameter, it could be because your Active Directory was not prepared during the CU upgrade, as we advised above in *What do you need to know before you begin?* See [Prepare Active Directory and domains](../../plan-and-deploy/prepare-ad-and-domains.md) for more information. Also note that any users who need access to public folders should be migrated first, **before** you migrate the public folders themselves.

If your organization has public folder mailboxes on multiple Exchange servers, you'll need to wait until Active Directory replication is complete. Once complete, you can confirm that all public folder mailboxes have picked up the `PublicFolderMailboxesLockedForNewConnections` flag, and that any pending changes users recently made to their public folders have converged across the organization. All of this could take several hours.

Run the following command in your on-premises environment to ensure that public folders are locked:

```PowerShell
Get-PublicFolder \
```

The expected result if public folders are locked is:

`Couldn't find the public folder mailbox. + CategoryInfo : NotSpecified: (:) [Get-PublicFolder], ObjectNotFoundException`

## Step 7: Finalize the public folder migration (public folder downtime required)

You need to check the following items before you can complete your public folder migration:

1. Confirm that there are no other public folder mailbox moves or public folder moves going on in your on-premises Exchange environment. To do this, use the **Get-MoveRequest** and **Get-PublicFolderMoveRequest** cmdlets to list any existing public folder moves. If there are any moves in progress, or in the **Completed** state, remove them.

2. At this point, we recommend re-running the following script to ensure that any new mail-enabled public folders are synchronized with Exchange Online:

   ```PowerShell
   .\Sync-ModernMailPublicFolders.ps1 -Credential (Get-Credential) -CsvSummaryFile:sync_summary.csv
   ```

3. To complete the public folder migration, run the following command in Exchange Online PowerShell:

   ```PowerShell
   Complete-MigrationBatch PublicFolderMigration
   ```

> [!IMPORTANT]
> After a migration batch is completed, no additional data can be synchornized from the on-premises Exchange servers and Exchange Online.

When you run `Complete-MigrationBatch PublicFolderMigration`, Exchange will perform a final synchronization between your Exchange on-premises organization and Exchange Online. During this period, the status of the migration batch will change from **Synced** to **Completing**, and then finally to **Completed**. If the final synchronization is successful, the public folders in Exchange Online will be unlocked. However, it is strongly recommended that you complete Step 8 and Step 9 of this article before you open up public folders to your users.

It's common for the status of migration batch to remain on **Synced** for a few hours before it switches to **Completing**. For migrations involving a large number of target mailboxes, it's normal to see the status remain in the **Synced** state for more than 24 hours, provided none of the underlying public folder migration requests have failed or were qurantined.

## Step 8: Test and unlock public folders in Exchange Online

Once the public folder migration is complete, take the following steps to test the success of the migration, and to officially verify its completion. These final tasks allow you to test the migrated public folder hierarchy before you permanently switch your organization to Exchange Online public folders.

1. In Exchange Online PowerShell, configure some test user mailboxes to use one of your newly migrated public folder mailboxes as their default public folder mailbox:

   ```PowerShell
   Set-Mailbox -Identity <test user> -DefaultPublicFolderMailbox <public folder mailbox identity>
   ```

   Make sure that your test users have necessary permissions to create public folders.

2. Log on to Outlook with the test user you designated in the previous step, and then perform the following public folder tests. Note that it may take 15 to 30 minutes for changes to take effect. Once Outlook is aware of the changes, it might prompt you to restart a couple of times.

   a. View the hierarchy.

   b. Check permissions.

   c. Create some public folders and then delete them.

   d. Post content to, and delete content from, a public folder.

   If you run into any issues and determine you aren't ready to switch your organization's public folders entirely to Exchange Online, see [Roll back a public folder migration from Exchange Server to Exchange Online](roll-back-exchange-online-migration.md).

3. Run the following command in Exchange Online PowerShell to unlock your public folders in Exchange Online. After you run the command, it may take approximately 15 to 30 minutes for the changes to take effect. Once Outlook is aware of the changes, it might prompt your users to restart Outlook a couple of times.

   ```PowerShell
   Set-OrganizationConfig -RemotePublicFolderMailboxes $Null -PublicFoldersEnabled Local
   ```

## Step 9: Finalize the migration on-premises

To enable emails to mail-enabled public folders on-premises, perform the following steps:

1. Run the following command in your on-premises environment, to take a backup of the emails in the queue that were sent to your mail-enabled public folders. This backup can be used in scenarios where email delivery to mail-enabled public folders failed for any reason:

   ```PowerShell
   $Server=Get-TransportService;ForEach ($t in $server) {Get-Message -Server $t -ResultSize Unlimited| ?{$_.Recipients -like "*PF.InTransit*"} | ForEach-Object {Suspend-Message $_.Identity -Confirm:$False; $Temp="C:\ExportFolder\"+$_.InternetMessageID+".eml"; $Temp=$Temp.Replace("<","_"); $Temp=$Temp.Replace(">","_"); Export-Message $_.Identity | AssembleMessage -Path $Temp;Resume-message $_.Identity -Confirm:$false}}
   ```

2. In your on-premises environment, run the following script to make sure all emails to mail-enabled public folders are correctly routed to Exchange Online. The script will stamp mail-enabled public folders with an `ExternalEmailAddress` that points them to their Exchange Online counterparts:

   ```PowerShell
   .\SetMailPublicFolderExternalAddress.ps1 -ExecutionSummaryFile:mepf_summary.csv
   ```

3. If your testing is successful, in your on-premises environment, run the following command to indicate that the public folder migration is complete:

   ```PowerShell
   Set-OrganizationConfig -PublicFolderMailboxesMigrationComplete:$true -PublicFoldersEnabled Remote
   ```

### How do I know this worked?

In [Step 2: Prepare for the migration](#step-2-prepare-for-the-migration), you took snapshots of your on-premises public folder structure, statistics, and permissions. The following steps will help you verify your public folder migration was successful by taking the same snapshots in Exchange Online post-migration. Compare the data in both files to verify success.

1. In Exchange Online PowerShell, run the following command to take a snapshot of the new folder structure:

   ```PowerShell
   Get-PublicFolder -Recurse -ResultSize Unlimited | Export-CliXML Cloud_PFStructure.xml
   ```

2. In Exchange Online PowerShell, run the following command to take a snapshot of the public folder statistics, including item count, size, and owner:

   ```PowerShell
   Get-PublicFolder -Recurse -ResultSize Unlimited | Get-PublicFolderStatistics | Export-CliXML Cloud_PFStatistics.xml
   ```

3. In Exchange Online PowerShell, run the following command to take a snapshot of the permissions:

   ```PowerShell
   Get-PublicFolder -Recurse -ResultSize Unlimited | Get-PublicFolderClientPermission | Select-Object Identity,User,AccessRights | Export-CliXML Cloud_PFPerms.xml
   ```

4. Exchange Online PowerShell, run the following command to take a snapshot of the mail-enabled public folders:

   ```PowerShell
   Get-MailPublicFolder -ResultSize Unlimited | Export-CliXML Cloud_MEPF.xml
   ```

## Known issues

The following are common public folder migration issues that you may encounter in your organization.

- We don't support the migration of public folders to Exchange Online when there are more than 100 unique public folder mailboxes in Exchange Online.

- Permissions for the root public folder and the EFORMS REGISTRY folder will not be migrated to Exchange Online, and you will have to manually apply them in Exchange Online. To do this, run the following command in your Exchange Online PowerShell. Run the command once for each permission entry that is present on-premises but missing in Exchange Online:

  ```PowerShell
  Add-PublicFolderClientPermission "\" -User <user> -AccessRights <access rights>
  Add-PublicFolderClientPermission "\NON_IPM_SUBTREE\EFORMS REGISTRY" -User <user> -AccessRights <access rights>
  ```

- There is a known issue where some public folder migrations will fail if some public folder mailboxes are not serving the public folder hierarchy. This means the `IsExcludedFromServingHierarchy` parameter on one or more mailboxes is set to `$true`. To avoid this, set all mailboxes in Exchange Online to serve the hierarchy.

- **Send As** and **Send on Behalf** permissions don't get migrated to Exchange Online. If this happens with your migration, use the following commands in your on-premises environment to note who has these permissions.

  To see which public folders have Send As permissions on-premises:

  ```PowerShell
  Get-MailPublicFolder | Get-ADPermission | ?{$_.ExtendedRights -like "*Send-As*"}
  ```

  To see which public folders have Send on Behalf permissions on-premises:

  ```PowerShell
  Get-MailPublicFolder | ?{$_.GrantSendOnBehalfTo -ne "$null"} | Format-Table name,GrantSendOnBehalfTo
  ```

  To add Send As permission to a mail-enabled public folder in Exchange Online, in Exchange Online PowerShell type:

  ```PowerShell
  Add-RecipientPermission -Identity <mail-enabled public folder primary SMTP address> -Trustee <name of user to be assigned permission> -AccessRights SendAs
  ```

  **Example**:

  ```PowerShell
  Add-RecipientPermission -Identity send1 -Trustee Exo1 -AccessRights SendAs
  ```

  To add Send on Behalf permission to a mail-enabled public folder in Exchange Online, in Exchange Online PowerShell type:

  ```PowerShell
  Set-MailPublicFolder -Identity <name of public folder> -GrantSendOnBehalfTo <user or comma-separated list of users>
  ```

  **Example**:

  ```PowerShell
  Set-MailPublicFolder send2 -GrantSendOnBehalfTo exo1,exo2
  ```

- Having more than 10,000 folders under the "\NON_IPM_SUBTREE\DUMPSTER_ROOT" folder can cause the migration to fail. Therefore, check the "\NON_IPM_SUBTREE\DUMPSTER_ROOT" folder to see if there are more than 10,000 folders directly under it (immediate children). You can use the following command to find the number of public folders in this location:

  ```PowerShell
  (Get-PublicFolder -GetChildren "\NON_IPM_SUBTREE\DUMPSTER_ROOT").Count
  ```

  Exchange Online does not support more than 10,000 subfolders, which is why migrations of more than 10,000 folders will fail. We are currently developing a script to unblock such configurations. In the meantime, we suggest waiting to migrate your public folders.

- Migration jobs are not making progress or are stalled. This can happen if there are too many jobs running in parallel, causing jobs to fail with intermittent errors. You can reduce the number of concurrent jobs by modifying `MaxConcurrentMigrations` and  `MaxConcurrentIncrementalSyncs` to a smaller number. Use the following example to set these values:

  ```PowerShell
  Set-MigrationEndpoint <PublicFolderEndpoint> -MaxConcurrentMigrations 30 -MaxConcurrentIncrementalSyncs 20 -SkipVerification
  ```

- Migration jobs fail with the error "Error: Dumpster of the Dumpster folder." If you see this error, it should be resolved if you stop the batch and then restart it.

- Migration jobs fail with the error "Request was quarantined because of the following error: The given key was not present in the dictionary." This happens when a corrupt item is present in a folder which migration jobs cannot copy. To work around this:

  1. Stop the migration batch.

  2. Identify the folder containing the bad item. The migration report should include references to the folder that was being copied when the error occurred.

  3. In your on-premises environment, move the affected folder to the primary public folder mailbox. You can use the `New-PublicFolderMoveRequest` cmdlet to move folders.

  4. Wait for the folder move to complete. After it is complete, remove the move request. Finally, re-start the migration batch.

## Remove public folder mailboxes from your Exchange on-premises environment

After the migration is complete and you have verified that your public folders in Exchange Online are working as expected and contain all expected data, you can remove your on-premises public folder mailboxes.

Be aware that this step is irreversible, because once public folder mailboxes are deleted, they cannot be recovered. Therefore we strongly recommend that, in addition to validating the success of your migration, that you also monitor your Exchange Online public folders for a few weeks before removing the on-premises public folder mailboxes.

## Migrate Public Folders to Office 365 by using Outlook PST export

We recommend that you don't use Outlook's PST export feature to migrate public folders to Office 365 or Exchange Online if your on-premises public folder hierarchy is greater than 30 GB. Office 365 online public folder mailbox growth is managed using an auto-split feature that splits the public folder mailbox when it exceeds size quotas. Auto-split can't handle the sudden growth of public folder mailboxes when you use PST export to migrate your public folders and you may have to wait for up to two weeks for auto-split to move the data from the primary mailbox. In addition, consider the following before using Outlook PST to export public folders to Office 365 or Exchange Online:

- Public folder permissions will be lost during this process. Capture the current permissions before migration and manually add them back once the migration is completed.

- If you use complex permissions or have many folders to migrate, we recommend that you use the cmdlet method for migration.

- Any item and folder changes made to the source public folders during the PST export migration will be lost. Therefore, we recommend that you use the cmdlet method if this export and import process will take a long time to complete.

If you still want to migrate your public folders by using PST files, follow these steps to ensure a successful migration.

1. Use the instructions in [Step 1: Download the migration scripts](#step-1-download-the-migration-scripts) to download the migration scripts. You only need to download the `PublicFolderToMailboxMapGenerator.ps1` file.

2. Follow step number 2 of [Step 3: Generate the .csv files](#step-3-generate-the-csv-files) to create the public folder-to-mailbox mapping file. This file is used to calculate the correct number of public folder mailboxes in Exchange Online.

3. Create the public folder mailboxes that you'll need based on the mapping file. For more information, see [Use the EAC to create a public folder mailbox](create-public-folder-mailboxes.md#use-the-eac-to-create-a-public-folder-mailbox).

4. Use the **New-PublicFolder** cmdlet to create the top-most public folder in each of the public folder mailboxes by using the _Mailbox_ parameter.

5. Export and import the PST files using Outlook.

6. Set the permissions on the public folders using the EAC. For more information, follow [Step 3: Assign permissions to the public folder](new-organizations.md#step-3-assign-permissions-to-the-public-folder) in the [Set up public folders in a new organization](new-organizations.md) article.

> [!CAUTION]
> If you've already started a PST migration and have run into an issue where the primary mailbox is full, you have two options for recovering the PST migration.
The first option is to wait for the auto-split to move the data from the primary mailbox. This may take up to two weeks. However, all the public folders in a completely filled public folder mailbox won't be able to receive new content until the auto-split completes.
Option two is to [create a public folder mailbox in Exchange Server](create-public-folder-mailboxes.md) and then use the **[New-PublicFolder]** cmdlet with the _Mailbox_ parameter to create the remaining public folders in the secondary public folder mailbox.
