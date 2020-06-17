---
title: 'Mailbox moves in Exchange 2013: Exchange 2013 Help'
TOCTitle: Mailbox moves in Exchange 2013
ms:assetid: 9c0a0bc9-2a39-4cf0-aa6e-6e5ef3fd38b5
ms:mtpsurl: https://technet.microsoft.com/library/JJ150543(v=EXCHG.150)
ms:contentKeyID: 47560063
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mailbox moves in Exchange 2013

_**Applies to:** Exchange Server 2013_

When you move a mailbox, you're moving it from a *source mailbox database* to a *target mailbox database*. The target mailbox database can be on the same server, on a different server, in a different domain, in a different Active Directory site, or in another forest.

## Reasons for moving mailboxes

You may need to move mailboxes in the following scenarios:

  - **Upgrade**: When you upgrade from an existing Microsoft Exchange Server 2007 or Exchange Server 2010 organization to Exchange Server 2013, you move mailboxes from the existing Exchange servers to an Exchange 2013 Mailbox server.

  - **Realignment**: You can move mailboxes for realignment purposes. For example, you may want to move a mailbox from one database to a database that has a larger mailbox size limit.

  - **Investigate an issue**: If you need to investigate an issue with a mailbox, you can move that mailbox to a different server. For example, you can move all mailboxes that have high activity to another server.

  - **Corrupted mailboxes**: If you encounter corrupted mailboxes, you can move the mailboxes to a different server or database. The corrupted messages won't be moved.

  - **Physical location changes**: You can move mailboxes to a server in a different Active Directory site. For example, if a user moves to a different physical location, you can move that user's mailbox to a server closer to the new location.

  - **Separation of administrative roles**: You may want to separate Exchange administration from Windows operating system account administration. To do this, you can move mailboxes from a single forest into a resource forest scenario. In this scenario, the Exchange mailboxes reside in one forest and their associated Windows user accounts reside in a separate forest.

  - **Outsource email administration**: You may want to outsource the administration of email and retain the administration of Windows user accounts. To do this, you can move mailboxes from a single forest into a resource forest scenario.

  - **Integrate email and user account administration**: You may want to change from a separated or outsourced email administration model to a model in which email and user accounts can be managed from within the same forest. To do this, you can move mailboxes from a resource forest scenario to a single forest. In this scenario, the Exchange mailboxes and Windows user accounts reside in the same forest.

## Exchange 2013 moves

Microsoft Exchange Server 2013 introduces the concept of *batch moves* and *migration endpoints*. Migration endpoints are management objects that describe the remote server and the connections that can be associated with one or more batches. And, the new batch move architecture improves on Mailbox Replication service (MRS) moves with enhanced management capability. Batch moves architecture in Exchange 2013 features the following capabilities:

  - Ability to move multiple mailboxes in large batches.

  - Email notification during move with reporting.

  - Automatic retry and automatic prioritization of moves.

  - Primary and personal archive mailboxes can be moved together or separately.

  - Option for manual move request finalization, which allows you to review your move before you complete it.

  - Periodic incremental syncs to update migration changes.

In Exchange 2013 you must move mailboxes between Exchange 2007 and Exchange 2010 and Exchange 2013 from the Exchange 2013 admin center (EAC) and the Exchange Management Shell.

> [!NOTE]
> You can still perform single mailbox moves in Exchange 2013 similar to Exchange Server 2010 by using either the EAC or the move request or migration batch cmdlets.

> [!NOTE]
> You can't move on-premises mailboxes from Exchange Server 2003 to Exchange 2013.

For more information about managing new and existing moves, see [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).

## Migration endpoints

Migration endpoints capture the remote server information and persist the required credentials for migrating the data and the source throttling settings. You can use migration endpoints to configure settings for remote and cross-forest moves. Local moves don't require the use of an endpoint. (Local moves are moves that move mailboxes between two different on-premises Exchange databases.)

The following list shows the types of moves that support migration endpoints:

  - **Cross-forest move**: Move mailboxes between two different on-premises Exchange forests. Cross-forest moves require the use of a Exchange RemoteMove endpoint.

  - **Remote move**: In a hybrid deployment, a remote move involves *onboarding* or *offboarding* migrations. Remote moves require the use of a RemoteMove endpoint. Onboarding moves mailboxes from an on-premises Exchange organization to Exchange Online in Microsoft Office 365, and uses a RemoteMove endpoint as the source endpoint of the migration batch. Offboarding moves mailboxes from Exchange Online in Office 365 to an on-premises Exchange organization and uses a Exchange RemoteMove endpoint as the target endpoint of the migration batch.

The following table shows the migration endpoint types and values that you can manage in Exchange 2013.

### Values of migration endpoint types

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Exchange RemoteMove</th>
<th>Exchange LocalMove</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>MaxConcurrentMigrations</p></td>
<td><p>MaxConcurrentMigrations</p></td>
</tr>
<tr class="even">
<td><p>RemoteServer</p></td>
<td><p>Site</p></td>
</tr>
<tr class="odd">
<td><p>ArchiveDomain</p></td>
<td><p>Database</p></td>
</tr>
<tr class="even">
<td><p>BadItemLimit</p></td>
<td><p>ArchiveDatabase</p></td>
</tr>
<tr class="odd">
<td><p>LargeItemLimit</p></td>
<td><p>BadItemLimit</p></td>
</tr>
<tr class="even">
<td><p>Databases</p></td>
<td><p>LargeItemLimit</p></td>
</tr>
<tr class="odd">
<td><p>TargetDeliveryDomain</p></td>
<td><p>PrimaryOnly</p></td>
</tr>
<tr class="even">
<td><p>PrimaryOnly</p></td>
<td><p>ArchiveDatabase</p></td>
</tr>
<tr class="odd">
<td><p>ArchiveDatabase</p></td>
<td><p>DAG</p></td>
</tr>
<tr class="even">
<td><p>ArchiveOnly</p></td>
<td><p>Forest</p></td>
</tr>
</tbody>
</table>

For more information about migration endpoints, see the **Migration** user interface in the EAC and [New-MigrationEndpoint](https://docs.microsoft.com/powershell/module/exchange/New-MigrationEndpoint).

For more information about managing new and existing moves, see [Manage on-premises moves](manage-on-premises-moves-exchange-2013-help.md).
