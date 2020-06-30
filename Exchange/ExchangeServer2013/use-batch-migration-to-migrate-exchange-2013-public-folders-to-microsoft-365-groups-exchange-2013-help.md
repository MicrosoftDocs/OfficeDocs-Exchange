---
title: 'Use batch migration to migrate Exchange 2013 public folders to Microsoft 365 Groups'
TOCTitle: Use batch migration to migrate Exchange 2013 public folders to Microsoft 365 Groups
ms:assetid: 1d800576-957d-4916-ae2a-55c08ca75be1
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
ms:mtpsurl: https://technet.microsoft.com/library/Mt843873(v=EXCHG.150)
ms:contentKeyID: 74468675
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Use batch migration to migrate Exchange 2013 public folders to Microsoft 365 Groups

**Summary:** How to move your Exchange 2013 public folders to Microsoft 365 Groups.

Through a process known as *batch migration*, you can move some or all of your Exchange 2013 public folders to Microsoft 365 Groups. Groups is a new collaboration offering from Microsoft that offers certain advantages over public folders. See [Migrate your public folders to Microsoft 365 Groups](https://docs.microsoft.com/exchange/collaboration-exo/public-folders/migrate-your-public-folders-to-office-365-groups) for an overview of the differences between public folders and Groups, and reasons why your organization may or may not benefit from switching to Groups.

This article contains the step-by-step procedures for performing the actual batch migration of your Exchange 2013 public folders.

## What do you need to know before you begin?

Ensure that all of the following conditions are met before you begin preparing your migration.

- The Exchange 2013 server needs to be running **Exchange 2013 CU15** or later.

- In Exchange Online, you need to be a member of the Organization Management role group. This role group is different from the permissions assigned to you when you subscribe to Microsoft 365, Office 365, or Exchange Online. For details about how to enable the Organization Management role group, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

- In Exchange 2013, you need to be a member of the Organization Management or Server Management RBAC role groups. For details, see [Add Members to a Role Group](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/dd638143(v=exchg.141)).

- Before you migrate your public folders to Microsoft 365 Groups, we recommend that you first move user mailboxes to Microsoft 365 or Office 365 for those users who need access to Microsoft 365 Groups after migration. For more information, see [Ways to migrate multiple email accounts to Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/mailbox-migration).

- MRS Proxy needs to be enabled on at least one Exchange server, and that server must also be hosting public folder mailboxes. See [Enable the MRS Proxy endpoint for remote moves](enable-the-mrs-proxy-endpoint-for-remote-moves-exchange-2013-help.md) for details.

- You can't use the Exchange admin center (EAC) or the Exchange Management Console (EMC) to perform this procedure. On the Exchange 2013 servers, you need to use the Exchange Management Shell. For Exchange Online, you need to use Exchange Online PowerShell. For more information, see [Connect to Exchange Online using remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- Only public folders of type calendar and mail can be migrated to Microsoft 365 Groups at this time; migration of other types of public folders is not supported. Also, the target groups in Microsoft 365 and Office 365 are expected to be created prior to the migration.

- The batch migration process only copies messages and calendar items from public folders for migration to Microsoft 365 Groups. It doesn't copy other entities of public folders like policies, rules, and permissions, since those are not supported in Microsoft 365 Groups.

- Microsoft 365 Groups comes with a 50GB mailbox. Ensure that the sum of public folder data that you are migrating totals less than 50GB. In addition, leave storage space for additional content to be added by your users in the future, post-migration. We recommend migrating public folders no bigger than 25GB in total size.

- This is not an "all or nothing" migration. You can pick and choose specific public folders to migrate, and only those public folders will be migrated. If the public folder being migrated has sub-folders, those sub-folders will not be automatically included in the migration. If you need to migrate them, you need to explicitly include them.

- The public folders will not be affected in any manner by this migration. However, once you use our lock-down script to make the migrated public folders read-only, your users will be forced to use Microsoft 365 Groups instead of public folders.

- You must use a single migration batch to migrate all of your public folder data. Exchange allows creating only one migration batch at a time. If you attempt to create more than one migration batch simultaneously, the result will be an error.

- Before you begin, we recommend that you read this article in its entirety, as downtime is required for some steps.

## Step 1: Get the scripts

The batch migration to Microsoft 365 Groups requires running a number of scripts at different points in the migration, as described below in this article. Download the scripts and their supporting files [from this location](https://www.microsoft.com/download/details.aspx?id=55985). After all the scripts and files are downloaded, save them to the same location, such as `c:\PFtoGroups\Scripts`.

Before proceeding, verify you have downloaded and saved all of the following scripts and files:

> [!NOTE]
> Make sure to save all scripts and files to the same location.

- **AddMembersToGroups.ps1**: This script adds members and owners to Microsoft 365 Groups based on permission entries in the source public folders.

- **AddMembersToGroups.strings.psd1**: This support file is used by the script `AddMembersToGroups.ps1`.

- **LockAndSavePublicFolderProperties.ps1**: This script makes public folders read-only to prevent any modifications, and it transfers the mail-related public folder properties (provided the public folders are mail-enabled) to the target groups, which will re-route emails from the public folders to the target groups. This script also backs up the permission entries and the mail properties before modifying them.

- **LockAndSavePublicFolderProperties.strings.psd1**: This support file is used by the script `LockAndSavePublicFolderProperties.ps1`.

- **UnlockAndRestorePublicFolderProperties.ps1**: This script restores access rights and mail properties of the public folders using backup files created by `LockandSavePublicFolderProperties.ps1`.

- **UnlockAndRestorePublicFolderProperties.strings.psd1**: This support file is used by the script `UnlockAndRestorePublicFolderProperties.ps1`.

- **WriteLog.ps1**: This script enables the preceding three scripts to write logs.

- **RetryScriptBlock.ps1**: This script enables the `AddMembersToGroups`, `LockAndSavePublicFolderProperties`, and `UnlockAndRestorePublicFolderProperties` scripts to retry certain actions in the event of transient errors.

For details about `AddMembersToGroups.ps1`, `LockAndSavePublicFolderProperties.ps1`, and `UnlockAndRestorePublicFolderProperties.ps1`, and the tasks they execute in your environment, see Migration scripts later in this article.

## Step 2: Prepare for the migration

The following steps are necessary to prepare your organization for the migration:

1. Compile a list of public folders (mail and calendar types) that you want to migrate to Microsoft 365 Groups.

2. Have a list of corresponding target groups for each public folder being migrated. You can either create a new group in Microsoft 365 or Office 365 for each public folder or use an existing group. If you're creating a new group, see [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2) to understand the settings a group must have. If a public folder that you are migrating has the default permission set to **Author** or above, you should create the corresponding group in Microsoft 365 or Office 365 with the **Public** privacy setting. However, for users to see the public group under the **Groups** node in Outlook, they will still have to join the group.

3. Rename any public folders that contain a backslash (**\\**) in their name. Otherwise, those public folders may not get migrated correctly.

4. You need to have the migration feature **PAW** enabled for your Microsoft 365 or Office 365 tenant. To verify this, run the following command in Exchange Online PowerShell:

    ```powershell
    Get-MigrationConfig
    ```

    If the output under **Features** lists **PAW**, then the feature is enabled and you can continue to *Step 3: Crete the .csv file*.

    If PAW is not yet enabled for your tenant, it could be because you have some existing migration batches, either public folder batches or user batches. These batches could be in any state, including Completed. If this is the case, please complete and remove any existing migration batches until no records are returned when you run `Get-MigrationBatch`. Once all existing batches are removed, PAW should get enabled automatically. Note that the change may not reflect in `Get-MigrationConfig` immediately, which is okay. Once this step is completed, you can continue creating new batches of user migrations.

## Step 3: Create the .csv file

Create a .csv file, which will provide input for one of the migration scripts.

The .csv file needs to contain the following columns:

- **FolderPath**. Path of the public folder to be migrated.

- **TargetGroupMailbox**. SMTP address of the target group in Microsoft 365 or Office 365. You can run the following command to see the primary SMTP address.

  ```powershell
  Get-UnifiedGroup <alias of the group> | Format-Table PrimarySmtpAddress
  ```

An example .csv:

```powershell
"FolderPath","TargetGroupMailbox"
"\Sales","sales@contoso.onmicrosoft.com"
"\Sales\EMEA","emeasales@contoso.onmicrosoft.com"
```

Note that a mail folder and a calendar folder can be merged into a single group in Microsoft 365 or Office 365. However, any other scenario of multiple public folders merging into one group isn't supported within a single migration batch. If you do need to map multiple public folders to the same Microsoft 365 group, you can accomplish this by running different migration batches, which should be executed consecutively, one after another. You can have up to 500 entries in each migration batch.

One public folder should be migrated to only one group in one migration batch.

## Step 4: Start the migration request

In this step, you gather information from your Exchange environment, and then you use that information in Exchange Online PowerShell to create a migration batch. After that, you start the migration.

1. On the Exchange 2013 server, find the MRS proxy endpoint server and make note of it. You will need this information later when you run the migration request.

2. In Exchange Online PowerShell, use the information that was returned above in step 1 to run the following commands. The variables in these commands will be the values from step 1.

    1. Pass the credential of a user with administrator permissions in the Exchange 2013 environment into the variable `$Source_Credential`. When you eventually run the migration request in Exchange Online, you will use this credential to gain access to your Exchange 2013 servers in order to copy the content over to Exchange Online.

       ```powershell
       $Source_Credential = Get-Credential
       <source_domain>\<PublicFolder_Administrator_Account>
       ```

    2. Use the MRS proxy server information from your Exchange 2013 environment that you noted in Step 1 above and pass that value into the variable `$Source_RemoteServer`.

       ```powershell
       $Source_RemoteServer = "<MRS proxy endpoint>"
       ```

3. In Exchange Online PowerShell, run the following command to create a migration endpoint:

   ```powershell
   $PfEndpoint = New-MigrationEndpoint -PublicFolderToUnifiedGroup -Name PFToGroupEndpoint -RemoteServer $Source_RemoteServer -Credentials $Source_Credential
   ```

4. Run the following command to create a new public folder-to-Microsoft 365 group migration batch. In this command:

   - **CSVData** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

   - **NotificationEmails** is an optional parameter that can be used to set email addresses that will receive notifications about the status and progress of the migration.

   - **AutoStart** is an optional parameter which, when used, starts the migration batch as soon as it is created.

   - **PublicFolderToUnifiedGroup** is the parameter to indicate that it is a public folder to Microsoft 365 Groups migration batch.

   ```powershell
   New-MigrationBatch -Name PublicFolderToGroupMigration -CSVData (Get-Content <path to .csv file> -Encoding Byte) -PublicFolderToUnifiedGroup -SourceEndpoint $PfEndpoint.Identity [-NotificationEmails <email addresses for migration notifications>] [-AutoStart]
   ```

5. Start the migration by running the following command in Exchange Online PowerShell. Note that this step is necessary only if the `-AutoStart` parameter was not used while creating the batch above in step 4.

   ```powershell
   Start-MigrationBatch PublicFolderToGroupMigration
   ```

While batch migrations need to be created using the `New-MigrationBatch` cmdlet in Exchange Online PowerShell, the progress of the migration can be viewed and managed in Exchange admin center. You can also view the progress of the migration by running the [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationBatch) and [Get-MigrationUser](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUser) cmdlets. The `New-MigrationBatch` cmdlet initiates a migration user for each Microsoft 365 group mailbox, and you can view the status of these requests using the mailbox migration page.

To view the mailbox migration page:

1. In Exchange Online, open Exchange admin center.

2. Navigate to **Recipients**, and then select **Migration**.

3. Select the migration request that was just created and then, on the **Details** pane, select **View Details**.

When the batch status is **Completed**, you can move on to the next step.

## Step 5: Add members to Microsoft 365 groups from public folders

You can add members to the target group in Microsoft 365 or Office 365 manually as required. However, if you want to add members to the group based on the permission entries in public folders, you need to do that by running the script `AddMembersToGroups.ps1` on the Exchange 2013 server as shown in the following command. User mailboxes must be synced to Exchange Online in order to be added as members of a Microsoft 365 group. To know which public folder permissions are eligible to be added as members of a group in Microsoft 365 or Office 365, see Migration scripts later in this article.

In the following command:

- **MappingCsv** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **BackupDir** is the directory where the migration log files will be stored.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

- **Credential** is the Exchange Online username and password.

```powershell
.\AddMembersToGroups.ps1 -MappingCsv <path to .csv file> -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $true -Credential (Get-Credential)
```

Once users have been added to a group in Microsoft 365 or Office 365, they can begin using it.

## Step 6: Lock down the public folders (public folder downtime required)

When the majority of the data in your public folders has migrated to Microsoft 365 Groups, you can run the script `LockAndSavePublicFolderProperties.ps1` on the Exchange 2013 server to make the public folders read-only. This step ensures that no new data is added to public folders before the migration completes.

> [!NOTE]
> If there are mail-enabled public folders (MEPFs) among the public folders being migrated, this step will copy some properties of MEPFs, such as SMTP addresses, to the corresponding group in Microsoft 365 or Office 365 and then mail-disable the public folder. Because the migrating MEPFs will be mail-disabled after the execution of this script, you will start seeing emails sent to MEPFs instead being received in the corresponding groups. For more details, see Migration scripts later in this article.

In the following command:

- **MappingCsv** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **BackupDir** is the directory where the backup files for permission entries, MEPF properties, and migration log files will be stored. This backup will be useful in case you need to roll back to public folders.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

- **Credential** is the Exchange Online username and password.

```powershell
.\LockAndSavePublicFolderProperties.ps1 -MappingCsv <path to .csv file> -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $true -Credential (Get-Credential)
```

## Step 7: Finalize the public folder to Microsoft 365 Groups migration

After you've made your public folders read-only, you'll need to perform the migration again. This is necessary for a final incremental copy of your data. Before you can run the migration again, you'll have to remove the existing batch, which you can do by running the following command:

```powershell
Remove-MigrationBatch <name of migration batch>
```

Next, create a new batch with the same .csv file by running the following command. In this command:

- **CsvData** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **NotificationEmails** is an optional parameter that can be used to set email addresses that will receive notifications about the status and progress of the migration.

- **AutoStart** is an optional parameter which, when used, starts the migration batch as soon as it is created.

```powershell
New-MigrationBatch -Name PublicFolderToGroupMigration -CSVData (Get-Content <path to .csv file> -Encoding Byte) -PublicFolderToUnifiedGroup -SourceEndpoint $PfEndpoint.Identity [-NotificationEmails <email addresses for migration notifications>] [-AutoStart]
```

After the new batch is created, start the migration by running the following command in Exchange Online PowerShell. Note that this step is only necessary if the `-AutoStart` parameter was not used in the preceding command.

```powershell
Start-MigrationBatch PublicFolderToGroupMigration
```

After you have finished this step (the batch status is **Completed**), verify that all data has been copied to Microsoft 365 Groups. At that point, provided you are satisfied with the Groups experience, you can begin deleting the migrated public folders from your Exchange 2013 environment.

> [!IMPORTANT]
> While there are supported procedures for rolling back your migration and returning to public folders, this isn't possible after the source public folders have been deleted. See How do I roll back to public folders from Microsoft 365 Groups? for more information.

## Known issues

The following known issues can occur during a typical public folders to Microsoft 365 Groups migration.

- The script that transfers SMTP address from mail-enabled public folders to Microsoft 365 Group only adds the addresses as secondary email addresses in Exchange Online. Because of this, if you have Exchange Online Protection (EOP) or Centralized Mail Flow setup in your environment, will have issues sending email to the groups (to the secondary email addresses) post-migration.

- If the .csv mapping file has an entry with invalid public folder path, the migration batch displays as **Completed** without throwing an error, and no further data is copied.

## Migration scripts

For your reference, this section provides in-depth descriptions for three of the migration scripts and the tasks they execute in your Exchange environment. All scripts and supporting files can be [downloaded from this location](https://www.microsoft.com/download/details.aspx?id=55985).

### AddMembersToGroups.ps1

This script will read the permissions of the public folders being migrated and then add members and owners to Microsoft 365 Groups as follows:

- Users with the following permission roles will be added as members to a group in Microsoft 365 or Office 365. **Permission roles:** Owner, PublishingEditor, Editor, PublishingAuthor, Author.

- In addition to the above, users with the following minimum access rights will also be added as members to a group in Microsoft 365 or Office 365. **Access rights:** ReadItems, CreateItems, FolderVisible, EditOwnedItems, DeleteOwnedItems.

- Users with access right "Owner" will be added as owners to a group and users with other eligible access rights will be added as members.

- Security groups cannot be added as members to groups in Microsoft 365 or Office 365. Therefore they will be expanded, and then the individual users will be added as members or owners to the groups based on the access rights of the security group.

- When users in security groups that have access rights over a public folder have themselves explicit permissions over the same public folder, explicit permissions will be given preference. For example, consider a case in which a security group called "SG1" has members User1 and User2. Permission entries for the public folder "PF1" are as follows:

  SG1: Author in PF1

  User1: Owner in PF1

  In this case, User1 will be added as an owner to the group in Microsoft 365 or Office 365.

- When the default permission of a public folder being migrated is 'Author' or above, the script will suggest setting the corresponding group's privacy setting as 'Public'.

This script can be run even after the lock-down of public folders, with parameter the `ArePublicFoldersLocked` set to` $true`. In this scenario, the script will read permissions from the back up file created during lock-down.

### LockAndSavePublicFolderProperties.ps1

This script makes the public folders being migrated read-only. When mail-enabled public folders are migrated, they will first be mail-disabled and their SMTP addresses will be added to the respective groups in Microsoft 365 or Office 365. Then the permission entries will be modified to make them read-only. A back up of the mail properties of mail-enabled public folders, as well as the permission entries of all the public folders, will be copied, before performing any modification on them.

If there are multiple migration batches, a separate backup directory should be used with each mapping .csv file.

The following mail properties will be stored, along with respective mail-enabled public folders and Microsoft 365 groups:

- PrimarySMTPAddress

- EmailAddresses

- ExternalEmailAddress

- EmailAddressPolicyEnabled

- GrantSendOnBehalfTo

- SendAs Trustee list

The above mail properties will be stored in a .csv file, which can be used in the roll back process (if you want to return to using public folders, see How do I roll back to public folders from Microsoft 365 Groups? for more information). A snapshot of the mail-enabled public folders' properties will also be stored in a file called PfMailProperties.csv. This file is not necessary for the roll back process, but can still be used for your reference.

The following mail properties will be migrated to target group as part of lock down:

- PrimarySMTPAddress

- EmailAddresses

- SendAs Trustee list

- GrantSendOnBehalfTo

The script ensures that the PrimarySMTPAddress and EmailAddresses of migrating mail-enabled public folders will be added as secondary SMTP addresses of the corresponding groups in Microsoft 365 or Office 365. Also, SendAs and SendOnBehalfTo permissions of users on mail-enabled public folders will be given equivalent permission in the corresponding target groups.

#### Access rights allowed

Only the following access rights will be allowed for users to ensure that the public folders are made read-only for all users. These are stored in **ListOfAccessRightsAllowed**.

- ReadItems

- CreateSubfolders

- FolderContact

- FolderVisible

The permission entries will be modified as follows:

1. ###  

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Before lock down</th>
    <th>After lock down</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p>None</p></td>
    <td><p>None</p></td>
    </tr>
    <tr class="even">
    <td><p>AvailabilityOnly</p></td>
    <td><p>AvailabilityOnly</p></td>
    </tr>
    <tr class="odd">
    <td><p>LimitedDetails</p></td>
    <td><p>LimitedDetails</p></td>
    </tr>
    <tr class="even">
    <td><p>Contributor</p></td>
    <td><p>FolderVisible</p></td>
    </tr>
    <tr class="odd">
    <td><p>Reviewer</p></td>
    <td><p>ReadItems, FolderVisible</p></td>
    </tr>
    <tr class="even">
    <td><p>NonEditingAuthor</p></td>
    <td><p>ReadItems, FolderVisible</p></td>
    </tr>
    <tr class="odd">
    <td><p>Aughor</p></td>
    <td><p>ReadItems, FolderVisible</p></td>
    </tr>
    <tr class="even">
    <td><p>Editor</p></td>
    <td><p>ReadItems, FolderVisible</p></td>
    </tr>
    <tr class="odd">
    <td><p>PublishingAuthor</p></td>
    <td><p>ReadItems, CreateSubfolders, FolderVisible</p></td>
    </tr>
    <tr class="even">
    <td><p>PublishingEditor</p></td>
    <td><p>ReadItems, CreateSubfolders, FolderVisible</p></td>
    </tr>
    <tr class="odd">
    <td><p>Owner</p></td>
    <td><p>ReadItems, CreateSubfolders, FolderContact, FolderVisible</p></td>
    </tr>
    </tbody>
    </table>

2. Access rights for users without read permissions will be left untouched, and they will continue to be blocked from read rights.

3. For users with custom roles, all the access rights that are not in **ListOfAccessRightsAllowed** will be removed. In the event that the users don't have any access rights from the allowed list after filtering, these users' access right will be set to 'None'.

There might be an interruption in sending emails to mail-enabled public folders during the time between when the folders are mail-disabled and their SMTP addresses are added to Microsoft 365 Groups.

### UnlockAndRestorePublicFolderProperties.ps1

This script will re-assign permissions back to public folders, based on the back up file taken during public folder lock-down. This script will also mail-enable public folders that had been mail-disabled, after it removes the folders' SMTP addresses from their respective groups in Microsoft 365 or Office 365. There might be slight downtime during this process.

## How do I roll back to public folders from Microsoft 365 Groups?

In the event that you change your mind and want to return to using public folders after using Microsoft 365 Groups, the command listed below will restore your environment to the state it was pre-migration. A roll back can be performed as long as the backup files exist and as long as you didn't delete the public folders post-migration.

On your Exchange 2013 server, run the following command. In this command:

- **BackupDir** is the directory where the backup files for permission entries, MEPF properties, and migration log files will be stored. Make sure you use the same location you specified in *Step 6: Lock down the public folders to cut-over (public folder downtime required)*.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

- **Credential** is the Exchange Online username and password.

```powershell
.\UnlockAndRestorePublicFolderProperties.ps1 -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $true -Credential (Get-Credential)
```

Be aware that any items added to the Microsoft 365 group, or any edit operations performed in the groups, are not copied back to your public folders. Therefore there will be data loss, assuming new data was added while the public folder was a group.

Note also that it's not possible to restore a subset of public folders, which means all of the public folders there were migrated should be restored.

The corresponding groups in Microsoft 365 or Office 365 won't be deleted as part of the roll back process. You'll have to clean or delete those groups manually.
