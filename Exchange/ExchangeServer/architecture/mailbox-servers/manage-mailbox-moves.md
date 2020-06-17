---
localization_priority: Normal
description: 'Summary: Learn how to move the primary mailbox and the associated archive to the same database or to separate ones in Exchange Server.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 1691b658-f5af-49c6-9170-5c3cb66c7306
ms.reviewer:
title: Manage on-premises mailbox moves in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage on-premises mailbox moves in Exchange Server

In Exchange Server, users' primary mailboxes and archive mailboxes can reside on different databases. A *move request* is the process of moving a mailbox from one mailbox database to another. A *local move request* is a mailbox move that occurs within a single Active Directory forest (as opposed to a remote move request that occurs between Active Directory forests). You use the procedures in this topic for local move requests of primary mailboxes, archive mailboxes, or both in on-premises. Using the move request functionality, you can move the primary mailbox and the associated archive to the same database or to separate ones.

The following two services process your move request to move mailboxes:

- Exchange Mailbox Replication service (MRS)

- Exchange Mailbox Replication Proxy

The procedures in this topic will help you with on-premises mailbox moves. You can use the Exchange Management Shell and the Exchange admin center (EAC) to move mailboxes in your on-premises organization.

For more information about the Mailbox replication service and proxy, see [Learn more about MRS Proxy](https://docs.microsoft.com/previous-versions/exchange-server/exchange-150/jj156451(v=exchg.150)). For more information about mailbox moves, see [Mailbox moves in Exchange Server](../../recipients/mailbox-moves.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 20 minutes

- For more information about accessing and using the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox Move and Migration Permissions " entry in [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create local move requests

You can create local move requests for:

- A single mailbox.

- Multiple mailboxes (also known as a *batch move request*).

- Multiple mailboxes that you specify in a comma-separated value (CSV) file (also known as a _migration batch_).

When you create local move requests in the EAC (for a single mailbox, multiple mailboxes, or multiple mailboxes specified in a CSV file), the request is visible to the **Get-MigrationBatch** cmdlet in the Exchange Management Shell. When the request has been completed (automatically or manually), the results for each individual mailbox are visible to the **Get-MoveRequest** cmdlet.

To create new local move requests in the Exchange Management Shell, you only use the **New-MigrationBatch** cmdlet for migration batches (the mailboxes are specified in a CSV file). To create local move requests that don't use a CSV file (individual mailboxes or batch move requests), you need to use the **New-MoveRequest** cmdlet, and these requests aren't visible to the **Get-MigrationBatch** batch cmdlet (or related **\*-MigrationBatch\*** cmdlets).

### Use the EAC to create a local move request

1. In the EAC, go to **Recipients** \> **Migration** \> click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png), and then select **Move to a different database**.

2. The **New local mailbox move** wizard opens. On the **Select users** page, configure one of these options:

   - **Select the users that you want to move**: Select one or more users:

     **Note**: Even if you're only interested in moving a user's archive mailbox, you select the user's primary mailbox.

     - Click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Select Mailbox** dialog box that appears, select one ore more mailboxes. When you're finished, click **OK**.

     - To remove mailboxes from the list, select the mailbox, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png).

   - **Specify the users with a CSV file**: Click **Browse** and go to the location of the comma-separated value (CSV) file that specifies the mailboxes to move. For more information about the CSV file requirements for local move requests, see [CSV Files for Mailbox Migration](https://docs.microsoft.com/exchange/csv-files-for-mailbox-migration-exchange-2013-help).

   - **Allow unknown columns in the CSV file**:

     - If you leave this check box unselected, move will ignore (silently skip) unknown columns in the CSV file (including optional columns with misspelled column headers). All unknown columns are treated like extra columns that aren't used.

     - If you select this check box, the migration fails if there are any unknown columns in the CSV file. This setting protects against spelling errors in the required and optional column headers, but the CSV file can't contain any unrelated columns.

   When you're finished, click **Next**.

3. On the **Move configuration** page, configure these settings:

   - **New migration batch name**: Enter a descriptive name for the mailbox move operation.

   - **Archive**: Select one of these options:

     - **Move the primary mailbox and the archive mailbox if one exists**

     - **Move primary mailbox only, without moving archive mailbox**

     - **Move archive mailbox only, without moving primary mailbox**

   - **Target database**: This setting affects moves for primary mailboxes.

     - To specify the database for the primary mailbox, click **Browse**. In the **Select Mailbox Database** dialog box that appears, select the database.

     - If you don't specify a database, the automatic distribution logic in Exchange will randomly select a database in the Active Directory site.

   - **Target archive database**: This setting affects moves for archive mailboxes.

     - To specify the database for the archive mailbox, click **Browse**. In the **Select Mailbox Database** dialog box that appears, select the database.

     - If you don't specify a database, the archive mailbox is moved to the same location as the primary mailbox.

   - **Bad item limit**: Specifies the maximum number of corrupted items that are allowed in the mailbox before the request fails. The default value in the EAC is 10. Don't specify a value greater than 50 here. If you want to set the limit to 51 or higher, use the _BadItemLimit_ parameter and the _AcceptLargeDataLoss_ switch in the Exchange Management Shell.

    When you're finished, click **Next**.

4. On the **Start the batch** page, configure these settings:

   - **After the batch is complete, a report will be sent to the following recipients.**: The default value is the account that you're using to move the mailboxes. Click **Browse** to add or remove recipients. When you're finished, click **OK**.

   - **Please select the preferred option to start the batch**: Select one of these options:

   - **Manually start the batch later**

   - **Automatically start the batch**: This is the default value.

   - **Please select the preferred option to complete the batch**: Select one of these options:

   - **Manually complete the batch**

   - **Automatically complete the migration batch**: This is the default value.

   When you're finished, click **New**.

### Use the Exchange Management Shell to create a local move request for individual or multiple mailboxes

A local move request for an individual mailbox uses the **New-MailboxMove** cmdlet. But, a local move request for multiple mailboxes that doesn't specify the mailboxes in a CSV file also uses the **New-MailboxMove** cmdlet. A local move request for multiple mailboxes that doesn't use a CSV file is also known as a *batch move request*.

To create a local move request for an individual mailbox, use this syntax:

```PowerShell
New-MoveRequest "<DescriptiveName>"] -Identity <MailboxIdentity> [<-ArchiveOnly | -PrimaryOnly>] [-TargetDatabase <DatabaseIdentity>] [-ArchiveTargetDatabase<DatabaseIdentity>] [-Priority <PriorityValue>] [-BadItemLimit <Value>] [-AcceptLargeDataLoss]
```

This example creates a new local move request with these settings:

- **Mailbox**: The primary mailbox and archive mailbox (if it exists) for Angela Gruber (agruber@contoso.com). If you only want to move the primary mailbox, use the _PrimaryOnly_ switch. If you only want to move the archive mailbox, use the _ArchiveOnly_ switch.

- **Target database for the primary mailbox**: MBX DB02. If we don't use the _TargetDatabase_ parameter, the automatic distribution logic in Exchange will randomly select a database in the Active Directory site.

- **Target database for the archive mailbox**: MBX DB03. If we don't use the _ArchiveTargetDatabase_ parameter or the _PrimaryOnly_ switch, the archive mailbox database will be moved to the same database as the primary mailbox.

  If we use the _ArchiveOnly_ switch without using the _ArchiveTargetDatabase_ parameter, the automatic distribution logic in Exchange will randomly select a database in the Active Directory site.

- **Priority**: `Normal`, because we aren't using the _Priority_ parameter.

- **Bad item limit**: 10 (the default value in the Exchange Management Shell is 0). Because the value is less than 51, we don't need to use the `AcceptLargeDataLoss` switch.

```PowerShell
New-MoveRequest -Identity agruber@contoso.com -TargetDatabase "MBX 02" -ArchiveTargetDatabase "MBX 03" -BadItemLimit 10
```

This example uses similar settings, but only moves Angela's primary mailbox.

```PowerShell
New-MoveRequest -Identity agruber@contoso.com -PrimaryOnly-TargetDatabase "MBX 02" -BadItemLimit 10
```

This example uses similar settings, but only moves Angela's archive mailbox.

```PowerShell
New-MoveRequest -Identity agruber@contoso.com -ArchiveOnly -ArchiveTargetDatabase "MBX 03" -BadItemLimit 10
```

For detailed syntax and parameter information, see [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/new-moverequest).

A batch move request uses virtually the same syntax as a move request for an individual mailbox. The main differences are:

- You don't use the _Identity_ parameter to specify the mailbox. Instead, you use the **Get-Mailbox** or **Get-User** cmdlets to generate the list of mailboxes that you want to move, and you pipeline the results to the **New-MoveRequest** cmdlet.

- You name the batch move with the _BatchName_ parameter.

This example creates a batch move request with these settings:

- **Mailboxes to move**: All mailbox on the database named MBX DB01.

- **Batch name**: MBX DB01 to MBX DB02.

- **Target database**: MBX DB02. If we didn't use the _TargetDatabase_ parameter, the automatic distribution logic in Exchange would randomly select databases in the Active Directory site.

- **Target database for archive mailboxes**: MBX DB02. Because we aren't using the _ArchiveTargetDatabase_ parameter or the _PrimaryOnly_ switch, the archive mailbox database is moved to the same database as the primary mailbox.

  If we use the _ArchiveOnly_ switch without using the _ArchiveTargetDatabase_ parameter, the automatic distribution logic in Exchange will randomly select databases in the Active Directory site.

- **Priority**: `High`

- **Bad item limit**: 51 (the default value in the Exchange Management Shell is 0), so we also need to use the _AcceptLargeDataLoss_ switch.

```PowerShell
Get-Mailbox -Database "MBX DB01" | New-MoveRequest -BatchName "MBX DB01 to MBX DB02" -TargetDatabase "MBX DB02" -Priority High -BadItemLimit 51 -AcceptLargeDataLoss
```

For detailed syntax and parameter information, see [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/new-moverequest).

### Use the Exchange Management Shell to create a local move request from a CSV file

A local move request for mailboxes that are specified in a CSV file is known as a *migration batch*, and uses the **New-MigrationBatch** cmdlet.

For more information about the CSV file requirements for local move requests, see [CSV Files for Mailbox Migration](https://docs.microsoft.com/exchange/csv-files-for-mailbox-migration-exchange-2013-help).

To create a migration batch, use this syntax:

```PowerShell
New-MigrationBatch -Local [-AutoStart] [-AutoComplete] -Name "<MigrationBatchName>" -CSVData ([Byte[]](Get-Content -Encoding Byte -Path "<PathAndFileName>" -ReadCount 0)) [<-ArchiveOnly | -PrimaryOnly>] [-TargetDatabases "<MailboxDatabase1>","<MailboxDatabase1>"... [-TargetArchiveDatabases "<MailboxDatabase1>","<MailboxDatabase1>"...] [-Priority <PriorityValue>] [-BadItemLimit <Value>] [-AcceptLargeDataLoss]
```

This example creates a migration batch with these settings:

- **CSV file that specifies the mailboxes to move**: C:\Users\Administrator\Desktop\LocalMove 01.csv. If you only want to move the primary mailbox, use the _PrimaryOnly_ switch, or the **MailboxType** value `PrimaryOnly` in the CSV file. If you only want to move the archive mailbox, use the _ArchiveOnly_ switch, or the **MailboxType** value `ArchiveOnly` in the CSV file.

- **Batch name**: LocalMove 01.

- **Target database**: MBX DB02. If we don't use the _TargetDatabase_ parameter, and the primary mailbox databases aren't specified in the CSV file, the automatic distribution logic in Exchange randomly selects databases in the Active Directory site.

- **Target database for archive mailboxes**: MBX DB02. Because we aren't using the _ArchiveTargetDatabase_ parameter (in the command or the CSV file), the archive mailbox database is moved to the same database as the primary mailbox.

  If we use the _ArchiveOnly_ switch (in the command or CSV file) without using the _ArchiveTargetDatabase_ parameter (in the command or CSV file), the automatic distribution logic in Exchange will randomly select databases in the Active Directory site.

- **When to start the migration**: Immediately, because we're using the _AutoStart_ switch. If we don't use this switch, we need to use the **Start-MigrationBatch** cmdlet to start the migration batch after it's created.

- **When to complete the migration**: After the mailboxes complete their initial synchronization, because we're using the _AutoComplete_ switch. If we don't use this switch, we need to use the **Complete-MigrationBatch** cmdlet to start the migration batch after it's created

- **Priority**: `Normal`, because we aren't using the _Priority_ parameter.

- **Bad item limit**: 10 (the default value in the Exchange Management Shell is 0). Because the value is less than 51, we don't need to use the `AcceptLargeDataLoss` switch.

```PowerShell
New-MigrationBatch -Local -AutoStart -AutoComplete -Name "LocalMove 01" -CSVData ([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\LocalMove 01.csv")) -TargetDatabases "MBX DB02" -BadItemLimit 10
```

### How do you know this worked?

To verify that you've successfully created a local move request, do any of these steps:

- In the EAC, go to **Recipients** \> **Migration** and verify the status of the move request (note that you might need to click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png)). You can select the move request, and see more information in the details pane, or by clicking **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

- In the EAC, go to **Recipients** \> **Migration** and click **Status For All Batches**.

- Check the notification message. The sender is Microsoft Outlook. When the move request is complete, you'll get a message with the subject `Migration batch <MigrationBatchName> has completed successfully`.

- In the EAC, click the notification viewer ![Notifications icon](../../media/6f2591b8-d0dc-4665-ab0b-b91a549e5b37.png) to view the status of the request.

- In the Exchange Management Shell, replace _\<MailboxIdentity\>_ with the name, email address, or alias of the mailbox, and run this command to verify the basic property values:

  ```PowerShell
  Get-MoveRequest -Identity <MailboxIdentity> | Format-List DisplayName,Alias,Status,*database*
  ```

- In the Exchange Management Shell, replace _\<BatchName\>_ with the batch name value of the move request, and run this command to verify the basic property values:

  ```PowerShell
  Get-MoveRequest -BatchName <BatchName> | Format-List DisplayName,Alias,Status,*database*
  ```

  **Note**: If you created the move request in the EAC, the batch name value is `MigrationService:<BatchNameValueFromTheEAC>`.

- If you created the move request in the EAC, replace _\<BatchName\>_ with the batch name value you specified, and run this command in the Exchange Management Shell to verify summary information about all mailboxes in the move:

  ```PowerShell
  Get-MigrationUserStatistics -BatchId <BatchName>
  ```

- If you created the move request in the EAC, replace _\<EmailAddress\>_ with the email address of the moved mailbox, and run this command to see detailed information about the specified mailbox:

  ```PowerShell
  Get-MigrationUserStatistics -Identity <EmailAddress> | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/get-migrationuserstatistics).

## Display migration batches

For an example of how to use the Exchange Management Shell to display a migration batch, see Example 2 in [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/get-migrationbatch).

## Create a cross-forest move using a .csv batch file

This example configures the migration endpoint, and then creates a cross-forest batch move from the source forest to the target forest using a .csv file.

```PowerShell
New-MigrationEndpoint -Name Fabrikam -ExchangeRemote -Autodiscover -EmailAddress tonysmith@fabrikam.com -Credentials (Get-Credential fabrikam\tonysmith)
$csvData=[System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\batch.csv")
New-MigrationBatch -CSVData $csvData -Timezone "Pacific Standard Time" -Name FabrikamMerger -SourceEndpoint Fabrikam -TargetDeliveryDomain "mail.contoso.com"
```

For more information about preparing your forest for cross-forest moves, see the following topics:

- [Prepare mailboxes for cross-forest move requests](prep-mailboxes-for-cross-forest-moves.md)

- [Prepare Mailboxes for Cross-Forest Moves Using Sample Code](https://docs.microsoft.com/exchange/prepare-mailboxes-for-cross-forest-moves-using-sample-code-exchange-2013-help)

- [Prepare mailboxes for cross-forest moves using the Exchange Management Shell](prep-mailboxes-for-cross-forest-moves-in-powershell.md)

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/new-migrationbatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/new-moverequest).

### How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- From the Exchange Management Shell, run the following command to retrieve mailbox move information.

  ```PowerShell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/get-migrationuserstatistics).
