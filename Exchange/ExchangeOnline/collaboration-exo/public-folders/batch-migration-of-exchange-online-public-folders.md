---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: e0be0727-f27f-4673-8a6f-af6ab5dbdace
ms.reviewer: 
ms.collection:
- Strat_EX_EXOBlocker
- exchange-online
- M365-email-calendar
description: 'Summary: How to move your Exchange Online public folders to Microsoft 365 Groups.'
audience: ITPro
f1.keywords:
- NOCSH
title: Use batch migration to migrate Exchange Online public folders to Microsoft 365 Groups

---

# Use batch migration to migrate Exchange Online public folders to Microsoft 365 Groups

 **Summary**: How to move your Exchange Online public folders to Microsoft 365 Groups.

Through a process known as batch migration, you can move some or all of your Exchange Online public folders to Microsoft 365 Groups. Groups is a new collaboration offering from Microsoft that offers certain advantages over public folders. See [Migrate your public folders to Microsoft 365 Groups](migrate-your-public-folders-to-microsoft-365-groups.md) for an overview of the differences between public folders and Groups, and reasons why your organization may or may not benefit from switching to Groups.

This article contains the step-by-step procedures for performing the actual batch migration of your Exchange Online public folders.

## What do you need to know before you begin?

Ensure that all of the following conditions are met before you begin preparing your migration.

- Only public folders of type calendar and mail can be migrated to Microsoft 365 Groups at this time; migration of other types of public folders is not supported. Also, the target Microsoft 365 groups are expected to be created prior to the migration.

- Microsoft 365 Groups doesn't support the permission roles and access rights that are available in public folders. In Microsoft 365 Groups, the users are designated as either **members** or **owners**.

- The batch migration process only copies messages and calendar items from public folders for migration to Microsoft 365 Groups. It doesn't copy other types of public folder content like rules and permissions since those are not supported in Microsoft 365 Groups.

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

- **AddMembersToGroups.ps1**. This script adds members and owners to Microsoft 365 Groups based on permission entries in the source public folders.

- **AddMembersToGroups.strings.psd1**. This support file is used by the script `AddMembersToGroups.ps1`.

- **LockAndSavePublicFolderProperties.ps1**. This script makes public folders read-only to prevent any modifications, and it transfers the mail-related public folder properties (provided the public folders are mail-enabled) to the target groups, which will re-route emails from the public folders to the target groups. This script also backs up the permission entries and the mail properties before modifying them.

- **LockAndSavePublicFolderProperties.strings.psd1**: This support file is used by the script `LockAndSavePublicFolderProperties.ps1`.

- **UnlockAndRestorePublicFolderProperties.ps1**. This script restores access rights and mail properties of the public folders using backup files created by `LockandSavePublicFolderProperties.ps1`.

- **UnlockAndRestorePublicFolderProperties.strings.psd1**. This support file is used by the script `UnlockAndRestorePublicFolderProperties.ps1`.

- **WriteLog.ps1**. This script enables the preceding three scripts to write logs.

- **RetryScriptBlock.ps1**. This script enables the `AddMembersToGroups`, `LockAndSavePublicFolderProperties`, and `UnlockAndRestorePublicFolderProperties` scripts to retry certain actions in the event of transient errors.

For details about `AddMembersToGroups.ps1`, `LockAndSavePublicFolderProperties.ps1`, and `UnlockAndRestorePublicFolderProperties.ps1`, and the tasks they execute in your environment, see [Migration scripts](#migration-scripts) later in this article.

## Step 2: Prepare for the migration

The following steps are necessary to prepare your organization for the migration:

1. Compile a list of public folders (mail and calendar types) that you want to migrate to Microsoft 365 Groups.

2. Have a list of corresponding target groups for each public folder being migrated. You can either create a new group in Office 365 for each public folder or use an existing group. If you're creating a new group, see [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2) to understand the settings a group must have. If a public folder that you are migrating has the default permission set to **Author** or above, you should create the corresponding group in Office 365 with the **Public** privacy setting. However, for users to see the public group under the **Groups** node in Outlook, they will still have to join the group.

3. Rename any public folders that contain a backslash ( **\\**) in their name. Otherwise, those public folders may not get migrated correctly.

4. You need to have the migration feature **PAW** enabled for your Microsoft 365 or Office 365 organization. To verify this, run the following command in Exchange Online PowerShell:

   ```PowerShell
   Get-MigrationConfig
   ```

   If the output under **Features** lists **PAW**, then the feature is enabled and you can continue to *Step 3: Create the .csv file*.

   If PAW is not yet enabled for your tenant, it could be because you have some existing migration batches, either public folder batches or user batches. These batches could be in any state, including Completed. If this is the case, please complete and remove any existing migration batches until no records are returned when you run `Get-MigrationBatch`. Once all existing batches are removed, PAW should get enabled automatically. Note that the change may not reflect in `Get-MigrationConfig` immediately, which is okay. Once this step is completed, you can continue creating new batches of user migrations.

## Step 3: Create the .csv file

Create a .csv file, which will provide input for one of the migration scripts.

The .csv file needs to contain the following columns:

- **FolderPath**. Path of the public folder to be migrated.

- **TargetGroupMailbox**. SMTP address of the target Microsoft 365 group. You can run the following command to see the primary SMTP address.

  ```PowerShell
  Get-UnifiedGroup <alias of the group> | Format-Table PrimarySmtpAddress
  ```

An example .csv:

```csv
"FolderPath","TargetGroupMailbox"
"\Sales","sales@contoso.onmicrosoft.com"
"\Sales\EMEA","emeasales@contoso.onmicrosoft.com"
```

Note that a mail folder and a calendar folder can be merged into a single Microsoft 365 group. However, any other scenario of multiple public folders merging into one group isn't supported within a single migration batch. If you do need to map multiple public folders to the same Microsoft 365 group, you can accomplish this by running different migration batches, which should be executed consecutively, one after another. You can have up to 500 entries in each migration batch.

One public folder should be migrated to only one group in one migration batch.

## Step 4: Start the migration request

In this step, you gather information from your Exchange environment, and then you use that information in Exchange Online PowerShell to create a migration batch. After that, you start the migration.

1. In Exchange Online PowerShell, run the following command to create a new public folder-to-Microsoft 365 group migration batch. In this command:

   - **CSVData** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

   - **AutoStart** is an optional parameter which, when used, starts the migration batch as soon as it is created.

   - **PublicFolderToUnifiedGroup** is the parameter to indicate that it is a public folder to Microsoft 365 Groups migration batch.

     ```PowerShell
     New-MigrationBatch -Name PublicFolderToGroupMigration -CSVData (Get-Content <path to .csv file> -Encoding Byte) -PublicFolderToUnifiedGroup [-AutoStart]
     ```

2. Start the migration by running the following command in Exchange Online PowerShell. Note that this step is necessary only if the `-AutoStart` parameter was not used while creating the batch above in step 1.

    ```PowerShell
    Start-MigrationBatch PublicFolderToGroupMigration
    ```

While batch migrations need to be created using the `New-MigrationBatch` cmdlet in Exchange Online PowerShell, the progress of the migration can be viewed and managed in Exchange admin center. You can also view the progress of the migration by running the [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/get-migrationbatch) and [Get-MigrationUser](https://docs.microsoft.com/powershell/module/exchange/get-migrationuser) cmdlets. The `New-MigrationBatch` cmdlet initiates a migration user for each Microsoft 365 group mailbox, and you can view the status of these requests using the mailbox migration page.

To view the mailbox migration page:

1. In Exchange Online, open Exchange admin center.

2. Navigate to **Recipients**, and then select **Migration**.

3. Select the migration request that was just created and then, on the **Details** pane, select **View Details**.

When the batch status is **Completed**, you can move on to *Step 5: Add members to Microsoft 365 groups from public folders*.

## Step 5: Add members to Microsoft 365 groups from public folders

You can add members to the target Microsoft 365 group manually as required. However, if you want to add members to the group based on the permission entries in public folders, you need to do that by running the script `AddMembersToGroups.ps1` as shown in the following command. To know which public folder permissions are eligible to be added as members of a Microsoft 365 group, see [Migration scripts](#migration-scripts) later in this article.

In the following command:

- **MappingCsv** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **BackupDir** is the directory where the migration log files will be stored.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

```PowerShell
.\AddMembersToGroups.ps1 -MappingCsv <path to .csv file> -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $false
```

Once users have been added to a Microsoft 365 group, they can begin using it.

## Step 6: Lock down the public folders (public folder downtime required)

When the majority of the data in your public folders has migrated to Microsoft 365 Groups, you can run the script `LockAndSavePublicFolderProperties.ps1` to make the public folders read-only. This step ensures that no new data is added to public folders before the migration completes.

> [!NOTE]
> If there are mail-enabled public folders (MEPFs) among the public folders being migrated, this step will copy some properties of MEPFs, such as SMTP addresses, to the corresponding Microsoft 365 group and then mail-disable the public folder. Because the migrating MEPFs will be mail-disabled after the execution of this script, you will start seeing emails sent to MEPFs instead being received in the corresponding groups. For more details, see [Migration scripts](#migration-scripts) later in this article.

In the following command:

- **MappingCsv** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **BackupDir** is the directory where the backup files for permission entries, MEPF properties, and migration log files will be stored. This backup will be useful in case you need to roll back to public folders.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

```PowerShell
.\LockAndSavePublicFolderProperties.ps1 -MappingCsv <path to .csv file> -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $false
```

## Step 7: Finalize the public folder to Microsoft 365 Groups migration

After you've made your public folders read-only, you'll need to perform the migration again. This is necessary for a final incremental copy of your data. Before you can run the migration again, you'll have to remove the existing batch, which you can do by running the following command:

```PowerShell
Remove-MigrationBatch <name of migration batch>
```

Next, create a new batch with the same .csv file by running the following command. In this command:

- **CSVData** is the .csv file created above in *Step 3: Create the .csv file*. Be sure to provide the full path to this file. If the file was moved for any reason, be sure to verify and use the new location.

- **NotificationEmails** is an optional parameter that can be used to set email addresses that will receive notifications about the status and progress of the migration.

- **AutoStart** is an optional parameter which, when used, starts the migration batch as soon as it is created.

```PowerShell
New-MigrationBatch -Name PublicFolderToGroupMigration -CSVData (Get-Content <path to .csv file> -Encoding Byte) -PublicFolderToUnifiedGroup [-NotificationEmails <email addresses for migration notifications>] [-AutoStart]
```

After the new batch is created, start the migration by running the following command in Exchange Online PowerShell. Note that this step is only necessary if the `-AutoStart` parameter was not used in the preceding command.

```PowerShell
Start-MigrationBatch PublicFolderToGroupMigration
```

After you have finished this step (the batch status is **Completed**), verify that all data has been copied to Microsoft 365 Groups. At that point, provided you are satisfied with the Groups experience, you can begin deleting the migrated public folders from your Exchange Online environment.

> [!IMPORTANT]
> While there are supported procedures for rolling back your migration and returning to public folders, this isn't possible after the source public folders have been deleted. See [How do I roll back to public folders from Microsoft 365 Groups?](#how-do-i-roll-back-to-public-folders-from-microsoft-365-groups) for more information.

## Known issues

The following known issues can occur during a typical public folders to Microsoft 365 Groups migration.

- The script that transfers SMTP address from mail-enabled public folders to Microsoft 365 groups only adds the addresses as secondary email addresses in Exchange Online. Because of this, if you have Exchange Online Protection (EOP) or Centralized Mail Flow setup in your environment, will have issues sending email to the groups (to the secondary email addresses) post-migration.

- If the .csv mapping file has an entry with invalid public folder path, the migration batch displays as **Completed** without throwing an error, and no further data is copied.

## Migration scripts

For your reference, this section provides in-depth descriptions for three of the migration scripts and the tasks they execute in your Exchange environment. You can download all of the scripts and supporting files [from this location](https://www.microsoft.com/download/details.aspx?id=55985).

### AddMembersToGroups.ps1

This script will read the permissions of the public folders being migrated and then add members and owners to Microsoft 365 Groups as follows:

- Users with the following permission roles will be added as members to a Microsoft 365 group. **Permission roles**: Owner, PublishingEditor, Editor, PublishingAuthor, Author

- In addition to the above, users with the following minimum access rights will also be added as members to a Microsoft 365 group. **Access rights**: ReadItems, CreateItems, FolderVisible, EditOwnedItems, DeleteOwnedItems

- Users with access right "Owner" will be added as owners to a group and users with other eligible access rights will be added as members.

- Security groups cannot be added as members to Microsoft 365 groups. Therefore they will be expanded, and then the individual users will be added as members or owners to the groups based on the access rights of the security group.

- When users in security groups that have access rights over a public folder have themselves explicit permissions over the same public folder, explicit permissions will be given preference. For example, consider a case in which a security group called "SG1" has members User1 and User2. Permission entries for the public folder "PF1" are as follows:

    SG1: Author in PF1

    User1: Owner in PF1

    In this case, User1 will be added as an owner to the Microsoft 365 group.

- When the default permission of a public folder being migrated is 'Author' or above, the script will suggest setting the corresponding group's privacy setting as 'Public'.

This script can be run even after the lock-down of public folders, with parameter `ArePublicFoldersLocked` set to ` $true `. In this scenario, the script will read permissions from the back up file created during lock-down.

### LockAndSavePublicFolderProperties.ps1

This script makes the public folders being migrated read-only. When mail-enabled public folders are migrated, they will first be mail-disabled and their SMTP addresses will be added to the respective Microsoft 365 groups. Then the permission entries will be modified to make them read-only. A back up of the mail properties of mail-enabled public folders, as well as the permission entries of all the public folders, will be copied, before performing any modification on them.

 If there are multiple migration batches, a separate backup directory should be used with each mapping .csv file.

The following mail properties will be stored, along with respective mail-enabled public folders and Microsoft 365 groups:

- PrimarySMTPAddress

- EmailAddresses

- ExternalEmailAddress

- EmailAddressPolicyEnabled

- GrantSendOnBehalfTo

- SendAs Trustee list

The above mail properties will be stored in a .csv file, which can be used in the roll back process (if you want to return to using public folders, see [How do I roll back to public folders from Microsoft 365 Groups?](#how-do-i-roll-back-to-public-folders-from-microsoft-365-groups) for more information). A snapshot of the mail-enabled public folders' properties will also be stored in a file called PfMailProperties.csv. This file is not necessary for the roll back process, but can still be used for your reference.

The following mail properties will be migrated to target group as part of the lock down:

- PrimarySMTPAddress

- EmailAddresses

- SendAs Trustee list

- GrantSendOnBehalfTo

The script ensures that the PrimarySMTPAddress and EmailAddresses of migrating mail-enabled public folders will be added as secondary SMTP addresses of the corresponding Microsoft 365 groups. Also, SendAs and SendOnBehalfTo permissions of users on mail-enabled public folders will be given equivalent permission in the corresponding target groups.

 **Access rights allowed**

Only the following access rights will be allowed for users to ensure that the public folders are made read-only for all users. These are stored in **ListOfAccessRightsAllowed**.

- ReadItems

- CreateSubfolders

- FolderContact

- FolderVisible

The permission entries will be modified as follows:

1. 

   |**Before lock down**|**After lock down**|
   |:-----|:-----|
   |None|None|
   |AvailabilityOnly|AvailabilityOnly|
   |LimitedDetails|LimitedDetails|
   |Contributor|FolderVisible|
   |Reviewer|ReadItems, FolderVisible|
   |NonEditingAuthor|ReadItems, FolderVisible|
   |Aughor|ReadItems, FolderVisible|
   |Editor|ReadItems, FolderVisible|
   |PublishingAuthor|ReadItems, CreateSubfolders, FolderVisible|
   |PublishingEditor|ReadItems, CreateSubfolders, FolderVisible|
   |Owner|ReadItems, CreateSubfolders, FolderContact, FolderVisible|

2. Access rights for users without read permissions will be left untouched, and they will continue to be blocked from read rights.

3. For users with custom roles, all the access rights that are not in **ListOfAccessRightsAllowed** will be removed. In the event that the users don't have any access rights from the allowed list after filtering, these users' access right will be set to 'None'.

There might be an interruption in sending emails to mail-enabled public folders during the time between when the folders are mail-disabled and their SMTP addresses are added to Microsoft 365 Groups.

### UnlockAndRestorePublicFolderProperties.ps1

This script will re-assign permissions back to public folders, based on the back up file taken during public folder lock-down. This script will also mail-enable public folders that had been mail-disabled, after it removes the folders' SMTP addresses from their respective Microsoft 365 groups. There might be slight downtime during this process.

## How do I roll back to public folders from Microsoft 365 Groups?

In the event that you change your mind and want to return to using public folders after using Microsoft 365 Groups, the command listed below will restore your environment to the state it was pre-migration. A roll back can be performed as long as the backup files exist and as long as you didn't delete the public folders post-migration.

Run the following command. In this command:

- **BackupDir** is the directory where the backup files for permission entries, MEPF properties, and migration log files will be stored. Make sure you use the same location you specified in *Step 6: Lock down the public folders to cut-over (public folder downtime required)*.

- **ArePublicFoldersOnPremises** is a parameter to indicate whether public folders are located on-premises or in Exchange Online.

```PowerShell
.\UnlockAndRestorePublicFolderProperties.ps1 -BackupDir <path to backup directory> -ArePublicFoldersOnPremises $false
```

Be aware that any items added to the Microsoft 365 groups, or any edit operations performed in the groups, are not copied back to your public folders. Therefore there will be data loss, assuming new data was added while the public folder was a group.

Note also that it's not possible to restore a subset of public folders, which means all of the public folders there were migrated should be restored.

The corresponding Microsoft 365 groups won't be deleted as part of the roll back process. You'll have to clean or delete those groups manually.
