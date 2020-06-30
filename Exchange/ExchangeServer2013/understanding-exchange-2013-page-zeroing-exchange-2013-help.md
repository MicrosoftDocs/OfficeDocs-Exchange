---
title: 'Understanding Exchange 2013 page zeroing: Exchange 2013 Help'
TOCTitle: Understanding Exchange 2013 page zeroing
ms:assetid: 0ca7b188-efbc-4c0d-bcfe-5138cffc803c
ms:mtpsurl: https://technet.microsoft.com/library/Gg549096(v=EXCHG.150)
ms:contentKeyID: 62279321
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Understanding Exchange 2013 page zeroing

_**Applies to:** Exchange Server 2013_

## Page Zeroing in Exchange 2013

*Zeroing* is a security mechanism that writes either zeros or a binary pattern over deleted data so that the deleted data more difficult to recover. In Exchange Server 2013, an ESE database uses *pages* as its unit of storage, and as a result it implements *page zeroing*. Page zeroing is enabled by default, and it cannot be disabled. Page zeroing operations are recorded in the transaction log files so that all copies of a database are page-zeroed in a similar manner. Zeroing a page on an active database causes the page to get zeroed on a passive copy of the database.

> [!NOTE]
> There is no mechanism for the Extensible Storage Engine (ESE) to prioritize the reutilization of zeroed pages over allocating new space. Tables which have sequential space allocation assigned will intentionally skip fragmented or zeroed pages in favor of using new or free sequential pages. This approach reduces database IOPs.

In Exchange 2013 page zeroing reduces the performance impact on servers when they're performing zeroing functions. This includes:

  - **Optimized storage and network capacity**: ESE writes a page-zeroing record to the transaction log file instead of logging the entire page image. This approach reduces log write I/O, and reduces the bandwidth requirements for shipping log files.

  - **Optimized database disk I/O**: In Exchange 2010 RTM and earlier, page zeroing occurred only during a backup, or during scheduled maintenance, and it caused significant database disk I/O. In Exchange 2010 SP1 and later (including Exchange 2013), page zeroing occurs by default and happens at transaction time. In the majority of cases, zeroing occurs immediately after a hard delete. This design allows the database to leverage the checkpoint depth capability of the engine, which ensures that dirty pages stay in the database cache for a certain amount of time so that additional page updates that occur in close time proximity don't cause additional database write I/Os. Because of this design, page zeroing has no significant database I/O impact, which is why it's enabled by default.

## Implementation of Page Zeroing in the ESE Database

Page zeroing writes a binary pattern over a hard-deleted record. The page-zeroing pattern is specific to the ESE engine operation and it is different for run-time operations versus maintenance operations. The following table lists the fill patterns that correspond to specific run-time operations.

### Fill pattern of page zeroing during ESE run-time

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>ESE run-time operation</th>
<th>Fill pattern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Replace</p></td>
<td><p>R</p></td>
</tr>
<tr class="even">
<td><p>Record/long value delete</p></td>
<td><p>D</p></td>
</tr>
<tr class="odd">
<td><p>Freed page space</p></td>
<td><p>H</p></td>
</tr>
</tbody>
</table>

The following table lists the fill patterns that correspond to specific operations that occur during ESE background database maintenance.

### Fill pattern of page zeroing during ESE background database maintenance

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>ESE background database maintenance operation</th>
<th>Fill pattern</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Record delete</p></td>
<td><p>D</p></td>
</tr>
<tr class="even">
<td><p>Long value delete</p></td>
<td><p>L</p></td>
</tr>
<tr class="odd">
<td><p>Freed page space of partially used page</p></td>
<td><p>Z</p></td>
</tr>
<tr class="even">
<td><p>Freed page space of unused page</p></td>
<td><p>U</p></td>
</tr>
</tbody>
</table>

## Background Database Maintenance

Background database maintenance is a process that continuously checksums and scans each database. Its primary function is to checksum database pages, but it also handles cleaning up space and zeroing out records and pages that were not zeroed out because of a Store crash. Background database maintenance processes approximately 1 MB per second per database. If timely page zeroing is a priority, you can reduce database sizes to ensure page zeroing occurs for the crash recovery cases in a shorter time period (for example, 24 hours).

Background database maintenance is a continuous process, so there are no events associated with its start and completion. You can track the progress of background database maintenance by reading the value of a performance counter:

  - MSExchange Database -\> Instances -\> Database Maintenance Duration

This counter indicates the number of seconds that have passed since maintenance last completed for a given database.

## Process of ESE Database Page Zeroing

The following table discusses database delete scenarios, and when page zeroing functions occur.

### ESE background database maintenance

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Database delete scenario</th>
<th>ESE process and timeframe to zero database data</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ul>
<li><p>Scenario 1: Single item recovery is disabled and user purges item from the Recoverable Items folder.</p></li>
<li><p>Scenario 2: Single item recovery is disabled and the Recoverable Items retention period is set to zero.</p></li>
<li><p>Scenario 3: Single item recovery is enabled and the item expires based on the deleted item retention period.</p></li>
</ul></td>
<td><p>An asynchronous thread writes a binary pattern over the deleted data. This action occurs within milliseconds of the record deletion. If the Store process crashes while the asynchronous zeroing work is still outstanding (or version store cleanup is cancelled due to version store growth), the zeroing is completed when background database maintenance processes that section of the database.</p></td>
</tr>
<tr class="even">
<td><p>View Scenario: Expiration of items from Outlook/Outlook Web App folder view (for example, Conversation view)</p></td>
<td><p>Data zeroing occurs when background database maintenance processes that section of the database.</p></td>
</tr>
<tr class="odd">
<td><p>Move Mailbox/Delete Mailbox Scenario: Source mailbox deleted (expiry of deleted mailbox from dumpster)</p></td>
<td><p>Data zeroing occurs when background database maintenance processes that section of the database.</p></td>
</tr>
</tbody>
</table>

## Monitoring Page Zeroing Behavior

You can measure and monitor page zeroing functionality by viewing two ESE counters:

  - MSExchange Database -\> Database Maintenance Pages Zeroed: Indicates the number of pages zeroed by the database engine since the performance counter was invoked.

  - MSExchange Database -\> Database Maintenance Pages Zeroed/sec: Indicates the rate at which pages are zeroed.

> [!NOTE]
> To learn how to enable these counters, see <A href="https://docs.microsoft.com/previous-versions/tn-archive/aa997018(v=exchg.65)">How to Enable Extended ESE Performance Counters</A>.

Page zeroing is a database maintenance function, so performance information related to both page zeroing for run-time transactions and page zeroing due to background database maintenance is included in these counters.

## Mailbox Data Types without Page Zeroing

The following Mailbox data types have no provisions for page zeroing:

  - Mailbox database transaction logs (.log)

    When transaction logs are deleted (due to truncation via backup or circular logging), there is no process to zero the blocks in the NTFS file system that stored the deleted log file(s). It's likely that NTFS will quickly re-utilize that free space for newly created logs, but there is no guarantee that this will happen.

  - Content index catalog files

    Exchange 2013 uses Search Foundation for search indexing functionality. The search index catalog is comprised of several dozen files stored on the same volume as the mailbox database file. When a message is hard-deleted from the mailbox database, the associated content in the search catalog isn't immediately deleted. Content deletion occurs when Search Foundation does a shadow, or master merge, of many small catalog files in to a single larger file. After the master merge completes, the smaller catalog files are deleted. There is no process to zero the blocks which stored the deleted catalog files.
