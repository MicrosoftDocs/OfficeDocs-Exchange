---
title: 'Reseed the search catalog: Exchange 2013 Help'
TOCTitle: Reseed the search catalog
ms:assetid: 9d873bd4-0422-4975-b5e2-82a347479115
ms:mtpsurl: https://technet.microsoft.com/library/Ee633475(v=EXCHG.150)
ms:contentKeyID: 51407270
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Reseed the search catalog

_**Applies to:** Exchange Server 2013_

If the content index catalog for a mailbox database copy gets corrupted, you may need to reseed the catalog. Corrupted content indexes are indicated in the Application event log by the following event.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Event ID</th>
<th>Level</th>
<th>Source</th>
<th>Details</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>123</p></td>
<td><p>Error</p></td>
<td><p>ExchangeStoreDB</p></td>
<td><p>At &lt;<em>timestamp</em>&gt; the Microsoft Exchange Information Store Database &lt;<em>identity</em>&gt; copy on this server experienced a corrupted search catalog. Consult the event log on the server for other &quot;ExchangeStoreDb&quot; and &quot;MSExchange Search Indexer&quot; events for more specific information about the failure. Reseeding the catalog is recommended via the 'Update-MailboxDatabaseCopy' task.</p></td>
</tr>
</tbody>
</table>

If the mailbox database copy is located on a server that is part of a database availability group (DAG), you can reseed the content index catalog from another DAG member.

If the mailbox database copy is the only copy, you have to manually create a new content index catalog.

For other management tasks related to Exchange Search, see [Exchange Search procedures](exchange-search-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes. Actual reseeding time may vary depending on the size of the content index catalog being reseeded.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange Search" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Reseed the content index catalog if the mailbox database is part of a DAG

Use one of the following procedures if the mailbox database is located on a server that is part of a DAG.

## Reseed the content index catalog from any source

This example reseeds the content index catalog for the database copy DB1 on Mailbox server MBX1 from any source server in the DAG that has a copy of the database.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1 -CatalogOnly
```

For detailed syntax and parameter information, see [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Update-MailboxDatabaseCopy).

## Reseed the content index catalog from a specific source

This example reseeds the content index catalog for the database copy DB1 on Mailbox server MBX1 from Mailbox server MBX2, which also has a copy of the database.

```powershell
Update-MailboxDatabaseCopy -Identity DB1\MBX1 -SourceServer MBX2 -CatalogOnly
```

For detailed syntax and parameter information, see [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Update-MailboxDatabaseCopy).

## Reseed the content index catalog if there is only one copy of the mailbox database

If there is only one copy of the mailbox database, you have to manually reseed the search catalog by recreating the content index catalog.

1. Run the following commands to stop the Microsoft Exchange Search and Microsoft Exchange Search Host Controller services.

    ```powershell
    Stop-Service MSExchangeFastSearch
    ```

    ```powershell
    Stop-Service HostControllerService
    ```

2. Delete, move, or rename the folder that contains the Exchange content index catalog. This folder is named `%ExchangeInstallPath\Mailbox\<name of mailbox database>_Catalog\<GUID>12.1.Single`. For example, you might rename the folder `C:\Program Files\Microsoft\Exchange Server\V15\Mailbox\Mailbox Database 0657134726_Catalog\F0627A72-9F1D-494A-839A-D7C915C279DB12.1.Single_OLD`.

    > [!NOTE]
    > Deleting this folder will make additional disk space available. Alternatively, you might want to rename or move the folder to keep it for troubleshooting purposes.

3. Run the following commands to restart the Microsoft Exchange Search and Microsoft Exchange Search Host Controller services.

    ```powershell
    Start-Service MSExchangeFastSearch
    ```

    ```powershell
    Start-Service HostControllerService
    ```

    After you restart these services, Exchange Search will rebuild the content index catalog.

## How do you know this worked?

It might take a while for Exchange Search to reseed the content index catalog. Run the following command to display the status of the reseeding process.

```powershell
Get-MailboxDatabaseCopyStatus | FL Name,*Index*
```

When the reseeding of the search catalog is in progress, the value of the *ContentIndexState* property is **Crawling**. When the reseeding is complete, this value is changed to **Healthy**.
