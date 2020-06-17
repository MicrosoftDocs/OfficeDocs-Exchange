---
title: 'CSV files for mailbox migration: Exchange 2013 Help'
TOCTitle: CSV files for mailbox migration
ms:assetid: e67b3455-3946-4335-b80c-97823c76ac54
ms:mtpsurl: https://technet.microsoft.com/library/Dn170437(v=EXCHG.150)
ms:contentKeyID: 53890559
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# CSV files for mailbox migration

_**Applies to:** Exchange Server 2013_

You can use a CSV file to bulk migrate a large number of user mailboxes. You can specify a CSV file when you use the Exchange admin center (EAC) or the **New-MigrationBatch** cmdlet in the Exchange Management Shell to create a migration batch. Using a CSV to specify multiple users to migrate in a migration batch is supported in the following migration scenarios:

  - **Moves in on-premises Exchange organizations**

  - **Local move:** A local move is where you move mailboxes from one mailbox database to another. A local move occurs within a single forest.

  - **Cross-forest enterprise move:** In a cross-forest enterprise move, mailboxes are moved to a different forest. Cross-forest moves are initiated either from the target forest, which is the forest that you want to move the mailboxes to, or from the source forest, which is the forest that currently hosts the mailboxes.

  - **Onboarding and offboarding in Exchange Online**

  - **Onboarding remote move migration:** In an Exchange hybrid deployment, you can move mailboxes from an on-premises Exchange organization to Exchange Online. This is also known as an *onboarding* remote move migration because you onboard mailboxes to Exchange Online.

  - **Offboarding remote move migration:** You can also perform an *offboarding* remote move migration, where you migrate Exchange Online mailboxes to your on-premises Exchange organization.

> [!NOTE]
> Both onboarding and offboarding remote move migrations are initiated from your Exchange Online organization.

  - **Staged Exchange migration:** You can also migrate a subset of mailboxes from an on-premises Exchange organization to Exchange Online. This is another type of onboarding migration. You can migrate only Exchange 2003 and Exchange 2007 mailboxes using a staged Exchange migration. Migrating Exchange 2010 and Exchange 2013 mailboxes isn't supported using a staged migration. Prior to running a staged migration, you have to use directory synchronization or some other method to provision mail users in your Exchange Online organization.

  - **IMAP migration:** This onboarding migration type migrates mailbox data from an IMAP server (including Exchange) to Exchange Online. For an IMAP migration, you must provision mailboxes in Exchange Online before you can migrate mailbox data.

> [!NOTE]
> A cutover Exchange migration doesn't support a using a CSV file because all on-premises user mailboxes are migrated to Exchange Online in a single batch.

## Supported attributes for CSV files for bulk moves or migrations

The first row, or header row, of a CSV file used for migrating users lists the names of the attributes, or fields, specified on the rows that follow. Each attribute name is separated by a comma. Each row under the header row represents an individual user and supplies the information required for the migration. The attributes in each individual user row must be in the same order as the attribute names in the header row. Each attribute value is separated by a comma. If the attribute value for a particular record is null, don't type anything for that attribute. However, make sure that you include the comma to separate the null value from the next attribute.

Attribute values in the CSV file override the value of the corresponding parameter when that same parameter is used when creating a migration batch with the EAC or the Exchange Management Shell. For more information and examples, see the section Attribute values in the CSV file override the values for the migration batch.

> [!TIP]
> You can use any text editor to create the CSV file, but using an application like Microsoft&nbsp;Excel will make it easier to import data and configure and organize CSV files. Be sure to save CSV files as a .csv or .txt file.

The following sections describe the supported attributes for the header row of a CSV file for each migration type. Each section includes a table that lists each supported attribute, whether it's required, an example of a value to use for the attribute, and a description.

> [!NOTE]
> In the following sections, <EM>source environment</EM> denotes the current location of a user mailbox or a database. <EM>Target environment</EM> denotes the location that the mailbox will be migrated to or the database that the mailbox will be moved to.

## Local moves

The following table describes the supported attributes for a CSV file for local moves. For more information, see [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Required or optional</th>
<th>Accepted values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EmailAddress</p></td>
<td><p>Required</p></td>
<td><p>SMTP address for the user</p></td>
<td><p>Specifies the user that will be moved.</p></td>
</tr>
<tr class="even">
<td><p>TargetDatabase</p></td>
<td><p>Optional</p></td>
<td><p>Database name</p></td>
<td><p>Specifies the mailbox database that the user's primary mailbox will be moved to. You can specify a different database in the different rows of the CSV file, which lets you move mailboxes in the same migration batch to different databases.</p></td>
</tr>
<tr class="odd">
<td><p>TargetArchiveDatabase</p></td>
<td><p>Optional</p></td>
<td><p>Database name</p></td>
<td><p>Specifies the mailbox database that the user's archive mailbox will be moved to. You can specify a different database in the different rows of the CSV file, which lets you move archive mailboxes in the same migration batch to different databases.</p>

> [!NOTE]
> If you specify a specific archive database, the archive mailbox (if it exists) will be moved to the same database as the primary mailbox.

</td>
</tr>
<tr class="even">
<td><p>BadItemLimit</p></td>
<td><p>Optional</p></td>
<td><p><code>Unlimited</code> or a non-negative integer from <code>0</code> (the default) to a maximum value of <code>2147483647</code></p></td>
<td><p>Specifies the number of bad items to skip if the migration service encounters a corrupted item in the mailbox. If you include this attribute in the CSV file, it will override the default value or a value you specify if you include the <em>BadItemLimit</em> parameter when creating the migration batch using the EAC or the Exchange Management Shell.</p>

> [!TIP]
> We recommend that you use the default value of 0 and only increase the bad item limit for a particular user if the move or migration for that user fails.

<p></p></td>
</tr>
<tr class="odd">
<td><p>MailboxType</p></td>
<td><p>Optional</p></td>
<td><p>Use one of the following values:</p>
<ul>
<li><p><code>PrimaryOnly</code></p></li>
<li><p><code>ArchiveOnly</code></p></li>
<li><p><code>PrimaryAndArchive</code> (the default value)</p></li>
</ul></td>
<td><p>Specifies whether to move the user's primary mailbox, archive mailbox, or both.</p></td>
</tr>
</tbody>
</table>

## Onboarding remote move migrations in a hybrid deployment

In a hybrid deployment, you can move mailboxes from an on-premises Exchange organization to Exchange Online. When onboarding mailboxes, the migration batch is created in the Exchange Online organization and initiated by an Exchange Online administrator. For more information, see [Move mailboxes between on-premises and Exchange Online organizations in hybrid deployments](https://docs.microsoft.com/exchange/hybrid-deployment/move-mailboxes).

The following table describes the supported attributes for a CSV file for onboarding remote move migrations.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Required or optional</th>
<th>Accepted values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EmailAddress</p></td>
<td><p>Required</p></td>
<td><p>SMTP address for the user</p></td>
<td><p>Specifies the email address for the mail-enabled user in the Exchange Online organization that corresponds to the on-premises user mailbox that will be migrated.</p></td>
</tr>
<tr class="even">
<td><p>BadItemLimit</p></td>
<td><p>Optional</p></td>
<td><p><code>Unlimited</code> or a non-negative integer from <code>0</code> (the default) to a maximum value of <code>2147483647</code></p></td>
<td><p>Specifies the number of bad items to skip if the migration service encounters a corrupted item in the mailbox. If you include this attribute in the CSV file, it will override the default value or the value you specify if you include the <em>BadItemLimit</em> parameter when creating the migration batch using the EAC or the Exchange Management Shell.</p>

> [!TIP]
> We recommend that you use the default value of 0 and only increase the bad item limit for a particular user if the move or migration for that user fails.

</td>
</tr>
<tr class="odd">
<td><p>LargeItemLimit</p></td>
<td><p>Optional</p></td>
<td><p><code>Unlimited</code> or a non-negative integer from <code>0</code> (the default) to a maximum value.</p></td>
<td><p>Specifies the number of large items in the user's mailbox that will be skipped. When the number of large items exceeds this value, the migration for the mailbox fails.</p>
<p>The default value is 0, which means that the migration fails if the mailbox contains any large items.</p>
<p>When onboarding mailboxes to Exchange Online, items up to 35 MB are migrated.</p></td>
</tr>
<tr class="even">
<td><p>MailboxType</p></td>
<td><p>Optional</p></td>
<td><p>Use one of the following values:</p>
<ul>
<li><p><code>PrimaryOnly</code></p></li>
<li><p><code>ArchiveOnly</code></p></li>
<li><p><code>PrimaryAndArchive</code> (the default value)</p></li>
</ul></td>
<td><p>Specifies whether to move the user's primary mailbox, archive mailbox, or both.</p></td>
</tr>
</tbody>
</table>

## Cross-forest enterprise moves and offboarding remote move migrations in a hybrid deployment

As previously stated, cross-forest moves are initiated either from the target forest or from the source forest. Offboarding remote move migrations are initiated from your Exchange Online organization. For more information, see:

- [Prepare mailboxes for cross-forest move requests](prepare-mailboxes-for-cross-forest-move-requests-exchange-2013-help.md)

- [Move mailboxes between on-premises and Exchange Online organizations in hybrid deployments](https://docs.microsoft.com/exchange/hybrid-deployment/move-mailboxes)

The following table describes the supported attributes for a CSV file for cross-forest enterprise moves and for offboarding remote move migrations in an Exchange hybrid deployment.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Required or optional</th>
<th>Accepted values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EmailAddress</p></td>
<td><p>Required</p></td>
<td><p>SMTP address for the user</p></td>
<td><p>For cross-forest enterprise moves, this specifies the mailbox or mail-enabled user in the source forest.</p>
<p>For offboarding remote move migrations, it specifies the Exchange Online mailbox.</p></td>
</tr>
<tr class="even">
<td><p>TargetDatabase</p></td>
<td><p>Required for offboarding remote move migrations and for cross-forest enterprise moves that are initiated from the source forest. Alternatively, this attribute can be specified when creating the migration batch in the EAC or using the Exchange Management Shell.</p>
<p>This attribute is optional for cross-forest enterprise moves that are initiated from the target forest.</p></td>
<td><p>Database name</p></td>
<td><p>Specifies the mailbox database in the target forest that the user's primary mailbox will be moved to. You can specify a different database in the different rows of the CSV file, which lets you move mailboxes in the same migration batch to different databases.</p></td>
</tr>
<tr class="odd">
<td><p>TargetArchiveDatabase</p></td>
<td><p>Optional</p></td>
<td><p>Database name</p></td>
<td><p>Specifies the mailbox database in the target forest that the user's archive mailbox will be moved to. You can specify a different database in the different rows of the CSV file, which lets you move archive mailboxes in the same migration batch to different databases.</p></td>
</tr>
<tr class="even">
<td><p>BadItemLimit</p></td>
<td><p>Optional</p></td>
<td><p><code>Unlimited</code> or a non-negative integer from <code>0</code> (the default) to a maximum value of <code>2147483647</code></p></td>
<td><p>Specifies the number of bad items to skip if the migration service encounters a corrupted item in the mailbox. If you include this attribute in the CSV file, it will override the default value or the value you specify if you include the <em>BadItemLimit</em> parameter when creating the migration batch using the EAC or the Exchange Management Shell.</p>

> [!TIP]
> We recommend that you use the default value of 0 and only increase the bad item limit for a particular user if the move or migration for that user fails.

</td>
</tr>
<tr class="odd">
<td><p>LargeItemLimit</p></td>
<td><p>Optional</p></td>
<td><p><code>Unlimited</code> or a non-negative integer from <code>0</code> (the default) to a maximum value.</p></td>
<td><p>Specifies the number of large items in the user's mailbox that will be skipped. When the number of large items exceeds this value, the migration for the mailbox fails.</p>
<p>The default value is 0, which means that the migration fails if the mailbox contains any large items.</p>
<p>When onboarding mailboxes to Exchange Online, items up to 35 MB are migrated.</p></td>
</tr>
<tr class="even">
<td><p>MailboxType</p></td>
<td><p>Optional</p></td>
<td><p>Use one of the following values:</p>
<ul>
<li><p><code>PrimaryOnly</code></p></li>
<li><p><code>ArchiveOnly</code></p></li>
<li><p><code>PrimaryAndArchive</code> (the default value)</p></li>
</ul></td>
<td><p>Specifies whether to move the user's primary mailbox, archive mailbox, or both.</p></td>
</tr>
</tbody>
</table>

## Staged Exchange migrations

You have to use a CSV file to identify the group of users for a migration batch when you want to use a staged Exchange migration to migrate Exchange 2003 and Exchange 2007 on-premises mailboxes to Exchange Online. There isn't a limit for the number of mailboxes that you can migrate to the cloud using a staged Exchange migration. However, the CSV file for a migration batch can contain a maximum of 1,000 rows. To migrate more than 1,000 mailboxes, you have to create additional CSV files, and then use each one to create a new migration batch. For more information about staged Exchange migrations, see [Migrate mailboxes to Exchange Online with a staged migration](https://docs.microsoft.com/exchange/mailbox-migration/perform-a-staged-migration/perform-a-staged-migration).

The following table describes the supported attributes for a CSV file for a staged Exchange migration.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Required or optional</th>
<th>Accepted values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EmailAddress</p></td>
<td><p>Required</p></td>
<td><p>SMTP address for the user</p></td>
<td><p>Specifies the email address for the mail-enabled user (or a mailbox if you're retrying the migration) in Exchange Online that corresponds to the on-premises user mailbox that will be migrated. Mail-enabled users are created in Exchange Online as a result of directory synchronization or another provisioning process. The email address of the mail-enabled user must match the <em>WindowsEmailAddress</em> property for the corresponding on-premises mailbox.</p></td>
</tr>
<tr class="even">
<td><p>Password</p></td>
<td><p>Optional</p></td>
<td><p>A password has to have a minimum length of eight characters, and satisfy any password restrictions that are applied to your Microsoft 365 or Office 365 organization.</p></td>
<td><p>This password is set on the user account when the corresponding mail-enabled user in Exchange Online is converted to a mailbox during the migration.</p></td>
</tr>
<tr class="odd">
<t[CSV files for mailbox migration](csv-files-for-mailbox-migration-exchange-2013-help.md)d><p>ForceChangePassword</p></td>
<td><p>Optional</p></td>
<td><p><code>True</code> or <code>False</code></p></td>
<td><p>Specifies whether a user must change the password the first time they sign in to their Exchange Online mailbox.</p>

> [!NOTE]
> If you've implemented a single sign-on solution by deploying Active Directory Federation Services 2.0 (AD FS 2.0) in your on-premises organization, you must use <CODE>False</CODE> for the value of this attribute.

</td>
</tr>
</tbody>
</table>

## IMAP migrations

A CSV file for an IMAP migration batch can have maximum of 50,000 rows. But it's a good idea to migrate users in several smaller batches. For more information about IMAP migrations, see the following topics:

- [Migrate Email from an IMAP Server to Exchange Online Mailboxes](https://docs.microsoft.com/Exchange/mailbox-migration/migrating-imap-mailboxes/migrating-imap-mailboxes)

- [CSV files for mailbox migration](csv-files-for-mailbox-migration-exchange-2013-help.md)

The following table describes the supported attributes for a CSV file for an IMAP migration.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Attribute</th>
<th>Required or optional</th>
<th>Accepted values</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>EmailAddress</p></td>
<td><p>Required</p></td>
<td><p>SMTP address for the user.</p></td>
<td><p>Specifies the user ID for the user's Exchange Online mailbox</p></td>
</tr>
<tr class="even">
<td><p>UserName</p></td>
<td><p>Required</p></td>
<td><p>String that identifies the user on the IMAP messaging system, in a format supported by the IMAP server.</p></td>
<td><p>Specifies the logon name for the user's account in the IMAP messaging system (the source environment). In addition to the user name, you can use the credentials of an account that has been assigned the necessary permissions to access mailboxes on the IMAP server. For more information, see <a href="https://docs.microsoft.com/Exchange/csv-files-for-mailbox-migration-exchange-2013-help">CSV files for IMAP migration batches</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Password</p></td>
<td><p>Required</p></td>
<td><p>Password string.</p></td>
<td><p>Specifies the password for the user account specified by the UserName attribute.</p></td>
</tr>
</tbody>
</table>

## Attribute values in the CSV file override the values for the migration batch

Attribute values in the CSV file override the value of the corresponding parameter when that same parameter is used when creating a migration batch with the EAC or the Exchange Management Shell. If you want the migration batch value to be applied to a user, you would leave that cell blank in the CSV file. This lets you mix and match certain attribute values for selected users in one migration batch.

For example, let's say you create a batch in the Exchange Management Shell for a cross-forest enterprise move to move users' primary and archive mailboxes to the target forest with the following Exchange Management Shell command.

```powershell
New-MigrationBatch -Name CrossForestBatch1 -SourceEndpoint ForestEndpoint1 -TargetDeliveryDomain forest2.contoso.com -TargetDatabases @(EXCH-MBX-02,EXCH-MBX-03) -TargetArchiveDatabases @(EXCH-MBX-A02,EXCH-MBX-A03) -CSVData ([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\CrossForestBatch1.csv")) -AutoStart
```

> [!NOTE]
> Because the default is to move primary and archive mailboxes, you don't have to explicitly specify it in the Exchange Management Shell command.

A portion of the CrossForestBatch1.csv file for this migration batch looks like this:

```powershell
EmailAddress,TargetDatabase,TargetArchiveDatabase
user1@contoso.com,EXCH-MBX-01,EXCH-MBX-A01
user2@contoso.com,,
user3@contoso.com,EXCH-MBX-01,
...
```

Because the values in the CSV file override the values for the migration batch, the primary and archive mailboxes for user1 are moved to EXCH-MBX-01 and EXCH-MBX-A01, respectively, in the target forest. The primary and archive mailboxes for user2 are moved to either EXCH-MBX-02 or EXCH-MBX-03. The primary mailbox for user3 is moved to EXCH-MBX-01 and the archive mailbox is moved to either EXCH-MBX-A02 or EXCH-MBX-A03.

In another example, let's say you create a batch for an onboarding remote move migration in a hybrid deployment to move archive mailboxes to Exchange Online with the following command.

```powershell
New-MigrationBatch -Name OnBoarding1 -SourceEndpoint RemoteEndpoint1 -TargetDeliveryDomain cloud.contoso.com -CSVData ([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\OnBoarding1.csv")) -MailboxType ArchiveOnly -AutoStart
```

But you also want to move the primary mailboxes for selected users, so a portion of the OnBoarding1.csv file for this migration batch would look like this:

```powershell
EmailAddress,MailboxType
user1@contoso.com,
user2@contoso.com,
user3@cloud.contoso.com,PrimaryAndArchive
user4@cloud.contoso.com,PrimaryAndArchive
...
```

Because the value for mailbox type in the CSV file overrides the values for the *MailboxType* parameter in the command to create the batch, only the archive mailbox for user1 and user2 is migrated to Exchange Online. But the primary and archive mailboxes for user3 and user4 are moved to Exchange Online.
