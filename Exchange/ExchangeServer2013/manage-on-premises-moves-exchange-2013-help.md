---
title: 'Manage on-premises moves: Exchange 2013 Help'
TOCTitle: Manage on-premises moves
ms:assetid: 1691b658-f5af-49c6-9170-5c3cb66c7306
ms:mtpsurl: https://technet.microsoft.com/library/JJ150487(v=EXCHG.150)
ms:contentKeyID: 47559947
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage on-premises moves

_**Applies to:** Exchange Server 2013_

A move request is the process of moving a mailbox from one mailbox database to another. A local move request is a mailbox move that occurs within a single forest. In Microsoft Exchange Server 2013, mailboxes and personal archive mailboxes can reside on separate databases. Using the move request functionality, you can move the primary mailbox and the associated archive to the same database or to separate ones. The procedures in this topic will help you with on-premises mailbox moves.

Use the following procedures to move mailboxes in your on-premises organization. These procedures use the Exchange Management Shell and the Exchange Center (EAC).

When you use move request to move mailboxes, the move requests are processed by the following two services:

- Microsoft Exchange Mailbox Replication service

- Microsoft Exchange Mailbox Replication Proxy

For more information about the Mailbox replication server and proxy, see [Learn more about MRS Proxy](https://docs.microsoft.com/previous-versions/exchange-server/exchange-150/jj156451(v=exchg.150)).

For more information about mailbox moves, see [Mailbox moves in Exchange 2013](mailbox-moves-in-exchange-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 20 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox Move and Migration Permissions " entry in [Recipients Permissions](recipients-permissions-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Test whether a mailbox is ready to move

This example uses the *WhatIf* switch to test whether Tony Smith's mailbox is ready to move to the new database DB01 and if there are any errors within the command. When you use the *WhatIf* switch, the system performs checks on the mailbox. If the mailbox isn't ready to move, you receive an error.

```powershell
New-MoveRequest -Identity 'tony@alpineskihouse.com' -TargetDatabase DB01 -WhatIf
```

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## Create a local move request

## Use the EAC to create a local move request

To create a local move request, log in to the EAC and perform the following steps:

1. In the EAC, navigate to **Recipients** \> **Migration**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New local mailbox move** wizard, select the user you want to move click **OK** and then click **Next**.

3. On the **Move configuration** page, specify a name for the new batch. Select which options you want for the archive mailbox, and mailbox database location and click **New**.

## Use the Shell to create a local move request

For an example of how to create a local move request, see Example 2 in [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- In the EAC, navigate to **Recipients** \> **Migration**.

- Verify that your move was successful in the EAC by clicking **Status For All Batches**.

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Create a batch move request

## Use the EAC to create a batch move request

log in to the EAC and perform the following steps:

1. In the EAC, navigate to **Recipients** \> **Migration**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New local mailbox move** wizard, select the users you want to move, click **OK** and then click **Next**.

3. On the **Move configuration** page, specify a name for the new batch. Select which options you want for the archive mailbox, and mailbox database location and click **New**.

> [!WARNING]
> Make sure that you don't set the Bad Item Limit to over 50 items. If you do, the move may fail. If you want to set the Bad Item Limit over 50 items, you must use the Exchange Management Shell and set the -<EM>AcceptLargeDataLoss</EM> parameter to true.

## Use the Shell to create a batch move request

This example creates a migration batch for a local move, where the mailboxes in the specified .csv file are moved to a different mailbox database. This .csv file contains a single column that contains the email address for each of the mailboxes that will be moved. The header for this column must be named **EmailAddress**. The migration batch in this example must be started manually by using the **Start-MigrationBatch** cmdlet or the Exchange admin center (EAC). Alternatively, you can use the *AutoStart* parameter to start the migration batch automatically.

```powershell
New-MigrationBatch -Local -Name LocalMove1 -CSVData ([System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\LocalMove1.csv")) -TargetDatabases MBXDB2 -TimeZone "Pacific Standard Time"
```

```powershell
Start-MigrationBatch -Identity LocalMove1
```

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [Start-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/Start-MigrationBatch).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- Verify that your move was successful in the EAC by clicking **Status For All Batches**.

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Display migration batches

For an example of how to use the Shell to display a migration batch, see Example 2 in [Get-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationBatch).

## Move only a user's primary mailbox

## Use the EAC to move only a user's primary mailbox

1. In the EAC, navigate to **Recipients** \> **Migration**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New local mailbox move** wizard, select the user whose primary mailbox you want to move, click **OK** and then click **next**.

3. On the **Move configuration** page, specify a name for the new batch. Select **Move primary mailbox only**, select which options you want for the mailbox database location, and then click **New**.

## Use the Shell to move only a user's primary mailbox

This example moves only Tony Smith's primary mailbox to DB01. The archive isn't moved.

```powershell
New-MoveRequest -Identity 'tony@alpineskihouse.com' -PrimaryOnly -TargetDatabase "DB01"
```

For detailed syntax and parameter information, see [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- In the EAC, click **Status For All Batches**.

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Create a cross-forest move using a .csv batch file

This example configures the migration endpoint, and then creates a cross-forest batch move from the source forest to the target forest using a .csv file.

```powershell
New-MigrationEndpoint -Name Fabrikam -ExchangeRemote -Autodiscover -EmailAddress tonysmith@fabrikam.com -Credentials (Get-Credential fabrikam\tonysmith)

$csvData=[System.IO.File]::ReadAllBytes("C:\Users\Administrator\Desktop\batch.csv")
New-MigrationBatch -CSVData $csvData -Timezone "Pacific Standard Time" -Name FabrikamMerger -SourceEndpoint Fabrikam -TargetDeliveryDomain "mail.contoso.com"
```

For more information about preparing your forest for cross-forest moves, see the following topics:

- [Prepare mailboxes for cross-forest move requests](prepare-mailboxes-for-cross-forest-move-requests-exchange-2013-help.md)

- [Prepare mailboxes for cross-forest moves using sample code](prepare-mailboxes-for-cross-forest-moves-using-sample-code-exchange-2013-help.md)

- [Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell](prepare-mailboxes-for-cross-forest-moves-using-the-prepare-moverequest-ps1-script-in-the-shell-exchange-2013-help.md)

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Move only an archive mailbox

## Use the EAC to move only an archive mailbox

1. In the EAC, navigate to **Recipients** \> **Migration**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New local mailbox move** wizard, select the user whose archive mailbox you want to move, click **OK** and then click **Next**.

3. On the **Move configuration** page, specify a name for the new batch. Select **Move archive mailbox only**, select which options you want for the mailbox database location, and then click **New**.

## Use the Shell to move only an archive mailbox

This example moves only Tony Smith's archive mailbox to DB03. The primary mailbox isn't moved.

```powershell
New-MoveRequest -Identity 'tony@alpineskihouse.com' -ArchiveOnly -ArchiveTargetDatabase "DB03"
```

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Move a user's primary mailbox and archive mailbox to separate databases

This example moves Ayla's primary mailbox and archive mailbox to separate databases. The primary database is moved to DB01, and the archive is moved to DB03.

```powershell
New-MoveRequest -Identity 'ayla@humongousinsurance.com' -TargetDatabase DB01 -ArchiveTargetDatabase -DB03
```

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).

## Move a user's primary mailbox and allow a large bad item limit

## Use the EAC to move a user's primary mailbox and allow a large bad item limit

1. In the EAC, navigate to **Recipients** \> **Migration**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New local mailbox move** wizard, select the user whose primary mailbox you want to move, click **OK**, and then click **Next**.

3. On the **Move configuration** page, specify a name for the new batch. Select **Move primary mailbox only**, and then select which options you want for the mailbox database location.

4. Click **More Options** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon"), enter the bad item limit, and then click **OK**.

## Use the Shell to move a user's primary mailbox and allow a large bad item limit

This example moves Lisa's primary mailbox to mailbox database DB01 and sets the bad item limit to `100`. To set a large bad item limit, you must use the *AcceptLargeDataLoss* parameter.

```powershell
New-MoveRequest -Identity 'Lisa' -PrimaryOnly -TargetDatabase "DB01" -BadItemLimit 100 -AcceptLargeDataLoss
```

For detailed syntax and parameter information, see [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/New-MigrationBatch) and [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/New-MoveRequest).

## How do you know this worked?

To verify that you have successfully completed your migration, do the following:

- From the Shell, run the following command to retrieve mailbox move information.

  ```powershell
  Get-MigrationUserStatistics -Identity BatchName -Status | Format-List
  ```

For more information, see [Get-MigrationUserStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MigrationUserStatistics).
