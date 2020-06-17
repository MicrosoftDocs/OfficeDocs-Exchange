---
localization_priority: Normal
description: 'Summary: Learn about moving and migrating mailboxes in Exchange Server 2016 and Exchange Server 2019, and the enhanced capabilities in the batch move architecture.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9c0a0bc9-2a39-4cf0-aa6e-6e5ef3fd38b5
ms.reviewer:
title: Mailbox moves in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mailbox moves in Exchange Server

You use mailbox moves to move mailboxes to, from, and within your Exchange organization. These are the basic types of mailbox moves that are available:

- **Local mailbox moves**: You move mailboxes from one mailbox database to another on Exchange servers within a single Active Directory forest. For instructions, see [Manage on-premises mailbox moves in Exchange Server](../architecture/mailbox-servers/manage-mailbox-moves.md).

- **Cross-forest mailbox moves**: You move mailboxes to Exchange servers in a different Active Directory forest. You can initiate the move from the target forest where you want to move the mailboxes (known as a *pull* move type), or from the source forest that currently hosts the mailboxes (known as a *push* move type). For more information, see [Prepare mailboxes for cross-forest move requests](../architecture/mailbox-servers/prep-mailboxes-for-cross-forest-moves.md).

- **Remote mailbox moves in hybrid deployments**: In hybrid deployments between on-premises Exchange and Microsoft Office 365, you can move mailboxes from Exchange to Office 365 (known as *onboarding remote move migrations*) and from Office 365 to Exchange (know as *offboarding remote move migrations*). For more information, see [Move mailboxes between on-premises and Exchange Online organizations in hybrid deployments](https://docs.microsoft.com/exchange/hybrid-deployment/move-mailboxes).

    **Note**: For more information about migrating on-premises Exchange organizations to Office 365, see [Ways to migrate multiple email accounts to Office 365](https://docs.microsoft.com/Exchange/mailbox-migration/mailbox-migration).

Mailbox moves in Exchange 2016 and Exchange 2019 use the batch move architecture that was introduced in Exchange 2013. The batch move architecture gives you the ability to move mailboxes in large batches. The enhanced management capabilities in the batch move architecture includes:

- Email notification during move with reporting.

- Automatic retry and automatic prioritization of moves.

- Move primary and personal archive mailboxes together or separately.

- Option for manual move request finalization to let you review your move before completion.

- Periodic incremental syncs to update migration changes.

You can move mailboxes in the Exchange admin center (EAC), or by using the [New-MoveRequest](https://docs.microsoft.com/powershell/module/exchange/new-moverequest) or [New-MigrationBatch](https://docs.microsoft.com/powershell/module/exchange/new-migrationbatch) cmdlets in the Exchange Management Shell.

## Scenarios for local and cross-forest mailbox moves

These are some scenarios for local mailbox moves:

- **Upgrade**: When you upgrade from an earlier version of Exchange, you move mailboxes from the existing Exchange servers to an Exchange Mailbox server.

- **Realignment**: For example, you might want to move a mailbox to a database that has a larger mailbox size limit.

- **Investigate an issue**: If you need to investigate an issue with a mailbox, you can move that mailbox to a different server. For example, you can move all mailboxes that have high activity to another server.

- **Corrupted mailboxes**: If you encounter corrupted mailboxes, you can move the mailboxes to a different server or database. The corrupted messages won't be moved.

- **Physical location changes**: You can move mailboxes to a server in a different Active Directory site. For example, if a user moves to a different physical location, you can move that user's mailbox to a server closer to the new location.

These are some scenarios for cross-forest mailbox moves:

- **Separation of administrative roles**: You might want to separate Exchange administration from Active Directory user account administration. To do this, you can move mailboxes from a single forest into a resource forest scenario. In this scenario, the Exchange mailboxes reside in one forest and their associated Active Directory user accounts reside in a different forest.

- **Outsourced email administration**: You might want to outsource the administration of email and retain the administration of Active Directory user accounts. To do this, you can move mailboxes from a single forest into a resource forest scenario.

- **Integrate email and user account administration**: You might want to change from a separated or outsourced email administration model to a model where email and user accounts can be managed from within the same forest. To do this, you can move mailboxes from a resource forest scenario to a single forest. In this scenario, the Exchange mailboxes and Active Directory user accounts reside in the same forest.

## CSV files for mailbox moves

One of the major benefits of the batch move architecture is the ability to use a comma-separated value (CSV) to specify the mailboxes to move. The information that's required in the CSV file depends on the type of move. For more information, see [CSV Files for Mailbox Migration](https://docs.microsoft.com/exchange/csv-files-for-mailbox-migration-exchange-2013-help).

## Migration endpoints for cross-forest and remote mailbox moves

You use migration endpoints for cross-forest mailbox moves, and remote mailbox moves between Exchange and Office 365 in hybrid deployments. You don't use migration endpoints for local mailbox moves.

Migration endpoints specify the remote server information, source throttling settings, and the required credentials for migrating the mailboxes.

- Cross-forest mailbox moves require an ExchangeRemoteMove migration endpoint.

- Onboarding mailbox move migrations in hybrid organizations (from Exchange to Office 365) require an ExchangeRemoteMove migration endpoint as the *source* of the migration batch.

- Offboarding mailbox move migrations in hybrid organizations (from Office 365 to Exchange) require an ExchangeRemoteMove migration endpoint as the *target* of the migration batch.

You can create migration endpoints in the EAC or by using the [New-MigrationEndpoint](https://docs.microsoft.com/powershell/module/exchange/new-migrationendpoint) cmdlet in the Exchange Management Shell.

## MRS Proxy endpoints for cross-forest and remote mailbox moves

The Mailbox Replication Service Proxy (MRS Proxy) facilitates cross-forest mailbox moves and remote move migrations. By default, the Client Access services on Mailbox servers aren't configured to accept incoming move requests, so you'll need to enable the MRS Proxy endpoint.

- For cross-forest moves from the target forest (pull moves), you need to enable the MRS Proxy endpoint in the Client Access services on Mailbox servers in the source forest.

- For cross-forest moves from the source forest (push moves), you need to enable the MRS Proxy endpoint in the Client Access services on Mailbox servers in the target forest.

- For both onboarding and offboarding remote move migrations in hybrid deployments, you need to enable the MRS Proxy endpoint in the Client Access services on Mailbox servers in the on-premises Exchange organization.

For more information, see [Enable the MRS Proxy endpoint for remote moves](../architecture/mailbox-servers/mrs-proxy-endpoint.md).
