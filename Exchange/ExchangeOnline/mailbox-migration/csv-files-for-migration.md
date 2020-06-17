---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b79fb81d-d6f4-4385-867e-7bdd0238366e
ms.reviewer: 
description: 'You can use a comma-separated values (CSV) file to bulk migrate a large number of user mailboxes. You can specify a CSV file when you use the Exchange admin center (EAC) or the New-MigrationBatch cmdlet in Exchange Online PowerShell to create a migration batch. Using a CSV to specify multiple users to migrate in a migration batch is supported in the following migration scenarios:'
title: CSV files for Mailbox migration
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# CSV files for Mailbox migration

You can use a comma-separated values (CSV) file to bulk migrate a large number of user mailboxes. You can specify a CSV file when you use the Exchange admin center (EAC) or the [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) cmdlet in Exchange Online PowerShell to create a migration batch. Using a CSV to specify multiple users to migrate in a migration batch is supported in the following migration scenarios:

- **Onboarding and offboarding in Office 365**

  - **Onboarding remote move migration**: In an Exchange hybrid deployment, you can move mailboxes from an on-premises Exchange organization to Office 365. This is also known as an onboarding remote move migration because you onboard mailboxes to Office 365.

  - **Offboarding remote move migration**: You can also perform an offboarding remote move migration, where you migrate Office 365 mailboxes to your on-premises Exchange organization.

    > [!NOTE]
    > Both onboarding and offboarding remote move migrations are initiated from your Microsoft 365 or Office 365 organization.

  - **Staged Exchange migration**: You can also migrate a subset of mailboxes from an on-premises Exchange organization to Office 365. This is another type of onboarding migration. You can migrate only Exchange 2003 and Exchange 2007 mailboxes using a staged Exchange migration. Migrating Exchange 2010 and Exchange 2013 mailboxes isn't supported using a staged migration. Prior to running a staged migration, you have to use directory synchronization or some other method to provision mail users in your Microsoft 365 or Office 365 organization.

  - **IMAP migration**: This onboarding migration type migrates mailbox data from an IMAP server (including Exchange) to Office 365. For an IMAP migration, you must provision mailboxes in Office 365 before you can migrate mailbox data.

> [!NOTE]
> A cutover Exchange migration doesn't support a using a CSV file because all on-premises user mailboxes are migrated to Office 365 in a single batch.

## Supported attributes for CSV files for bulk moves or migrations

The first row, or header row, of a CSV file used for migrating users lists the names of the attributes, or fields, specified on the rows that follow. Each attribute name is separated by a comma. Each row under the header row represents an individual user and supplies the information required for the migration. The attributes in each individual user row must be in the same order as the attribute names in the header row. Each attribute value is separated by a comma. If the attribute value for a particular record is null, don't type anything for that attribute. However, make sure that you include the comma to separate the null value from the next attribute.

Attribute values in the CSV file override the value of the corresponding parameter when that same parameter is used when creating a migration batch with the EAC or Exchange Online PowerShell. For more information and examples, see the section [Attribute values in the CSV file override the values for the migration batch](csv-files-for-migration.md#BK-Sttributes).

> [!TIP]
> You can use any text editor to create the CSV file, but using an application like Microsoft Excel will make it easier to import data and configure and organize CSV files. Be sure to save CSV files as a .csv or .txt file.

The following sections describe the supported attributes for the header row of a CSV file for each migration type. Each section includes a table that lists each supported attribute, whether it's required, an example of a value to use for the attribute, and a description.

> [!NOTE]
> In the following sections, source environment denotes the current location of a user mailbox or a database. Target environment denotes the location that the mailbox will be migrated to or the database that the mailbox will be moved to.

### Staged Exchange migrations

You have to use a CSV file to identify the group of users for a migration batch when you want to use a staged Exchange migration to migrate Exchange 2003 and Exchange 2007 on-premises mailboxes to Office 365. There isn't a limit for the number of mailboxes that you can migrate to the cloud using a staged Exchange migration. However, the CSV file for a migration batch can contain a maximum of 2,000 rows. To migrate more than 2,000 mailboxes, you have to create additional CSV files and then use each one to create a new migration batch. For more information about staged Exchange migrations, see [What you need to know about a staged email migration to Office 365](what-to-know-about-a-staged-migration.md).

The following table describes the supported attributes for a CSV file for a staged Exchange migration.

|**Attribute**|**Required or optional**|**Accepted values**|**Description**|
|:-----|:-----|:-----|:-----|
|EmailAddress|Required|SMTP address for the user|Specifies the email address for the mail-enabled user (or a mailbox if you're retrying the migration) in Office 365 that corresponds to the on-premises user mailbox that will be migrated. Mail-enabled users are created in Office 365 as a result of directory synchronization or another provisioning process. The email address of the mail-enabled user must match the _WindowsEmailAddress_ property for the corresponding on-premises mailbox.|
|Password|Optional|A password has to have a minimum length of eight characters, and satisfy any password restrictions that are applied to your Microsoft 365 or Office 365 organization.|This password is set on the user account when the corresponding mail-enabled user in Office 365 is converted to a mailbox during the migration.|
|ForceChangePassword|Optional|`True` or `False`|Specifies whether a user must change the password the first time they sign in to their Office 365 mailbox. <br/> **Note**: If you've implemented a single sign-on (SSO) solution by deploying Active Directory Federation Services 2.0 (AD FS 2.0) in your on-premises organization, you must use `False` for the value of this attribute.|

### IMAP migrations

A CSV file for an IMAP migration batch can have maximum of 50,000 rows. But it's a good idea to migrate users in several smaller batches. For more information about IMAP migrations, see the following topics:

- [Migrate your IMAP mailboxes to Office 365](migrating-imap-mailboxes/migrating-imap-mailboxes.md)

- [CSV files for IMAP migration batches](migrating-imap-mailboxes/csv-files-for-imap-migrations.md)

The following table describes the supported attributes for a CSV file for an IMAP migration.

|**Attribute**|**Required or optional**|**Accepted values**|**Description**|
|:-----|:-----|:-----|:-----|
|EmailAddress|Required|SMTP address for the user.|Specifies the user ID for the user's Office 365 mailbox|
|UserName|Required|String that identifies the user on the IMAP messaging system, in a format supported by the IMAP server.|Specifies the logon name for the user's account in the IMAP messaging system (the source environment). In addition to the username, you can use the credentials of an account that has been assigned the necessary permissions to access mailboxes on the IMAP server. For more information, see [CSV files for IMAP migration batches](migrating-imap-mailboxes/csv-files-for-imap-migrations.md).|
|Password|Required|Password string.|Specifies the password for the user account specified by the UserName attribute.|

## Attribute values in the CSV file override the values for the migration batch
<a name="BK-Sttributes"> </a>

Attribute values in the CSV file override the value of the corresponding parameter when that same parameter is used when creating a migration batch with the EAC or Exchange Online PowerShell. If you want the migration batch value to be applied to a user, you would leave that cell blank in the CSV file. This lets you mix and match certain attribute values for selected users in one migration batch.

In this example, let's say you create a batch for an onboarding remote move migration in a hybrid deployment to move archive mailboxes to Office 365 with the following [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) command.

```PowerShell
New-MigrationBatch -Name OnBoarding1 -SourceEndpoint RemoteEndpoint1 -TargetDeliveryDomain cloud.contoso.com -CSVData ([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\OnBoarding1.csv")) -ArchiveOnly:$true -AutoStart
```

But you also want to move the primary mailboxes for selected users, so a portion of the OnBoarding1.csv file for this migration batch would look like this:

```PowerShell
EmailAddress,MailboxType
user1@contoso.com,
user2@contoso.com,
user3@cloud.contoso.com,PrimaryAndArchive
user4@cloud.contoso.com,PrimaryAndArchive
...
```

Because the value for mailbox type in the CSV file overrides the values for the _MailboxType_ parameter in the command to create the batch, only the archive mailbox for user1 and user2 is migrated to Office 365. But the primary and archive mailboxes for user3 and user4 are moved to Office 365.
