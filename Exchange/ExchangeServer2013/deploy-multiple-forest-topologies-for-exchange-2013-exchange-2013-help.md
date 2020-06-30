---
title: 'Deploy multiple forest topologies for Exchange 2013: Exchange 2013 Help'
TOCTitle: Deploy multiple forest topologies for Exchange 2013
ms:assetid: d51f2b7d-9045-40cf-8b9f-43787a6fff6d
ms:mtpsurl: https://technet.microsoft.com/library/Bb124734(v=EXCHG.150)
ms:contentKeyID: 50406267
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deploy multiple forest topologies for Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic provides an overview of deploying Microsoft Exchange Server 2013 in multiple forest topologies. You'll find information about the following subjects:

  - **Supported multiple forest topologies**: Exchange 2013 supports two types of multiple forest topologies: cross-forest and resource forest.

  - **GAL synchronization**: If you have a cross-forest environment, you need to ensure that the GAL in any given forest contains mail recipients from other forests.

  - **Moving mailboxes across forests**:  The **New-MoveRequest** and **New-MigrationBatch** cmdlets in the Exchange Management Shell can help move mailboxes from one forest to another.

  - **Understanding multiple forest administration**: Learn about the permissions model to configure and manage the permissions between your forests.

## Supported multiple forest topologies

Exchange 2013 supports the following types of multiple forest topologies:

  - **Cross-forest**: A cross-forest topology is one with multiple Exchange forests. Here is an overview of what you need to do to deploy Exchange 2013 in a topology with a multiple forest:

    1. You must first install Exchange 2013 in each forest. For more information, see [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md).

    2. Next, you must synchronize the recipients in each of the forests, so that the Global Address List (GAL) in each forest contains users from all the synchronized forests. See the "GAL Synchronization" section below for more details.

    3. Finally, you must configure the Availability service so that users in one forest can view availability data for users in another forest. For more information, see [Configure the Availability service for cross-forest topologies](configure-the-availability-service-for-cross-forest-topologies-exchange-2013-help.md).

    For details about deploying Exchange 2013 in a cross-forest topology, see [Deploy Exchange 2013 in a cross-forest topology](deploy-exchange-2013-in-a-cross-forest-topology-exchange-2013-help.md).

  - **Resource forest**: A resource forest topology is one with an Exchange forest and one or more user accounts forests. Here is an overview of what you need to do to deploy Exchange 2013 in a topology with a resource forest:

    1. You must have a forest with Exchange installed. In the Exchange forest, you must have disabled the user accounts that have Exchange mailboxes.

    2. You must have at least one forest that contains user accounts. This forest should *not* have Exchange installed.

    3. Then, you must associate the disabled user accounts in the Exchange forest with the user accounts in the accounts forest.

    For details about deploying Exchange 2013 in a resource forest topology, see [Deploy Exchange 2013 in an Exchange resource forest topology](deploy-exchange-2013-in-an-exchange-resource-forest-topology-exchange-2013-help.md).

## GAL synchronization

By default, a GAL contains mail recipients from a single forest. If you have a cross-forest environment, we recommend using Microsoft Identity Lifecycle Manager (ILM) 2007 Feature Pack 1 (FP1) to ensure that the GAL in any given forest contains mail recipients from other forests. ILM 2007 FP1 creates mail users that represent recipients from other forests, thereby allowing users to view them in the GAL and send mail. For example, users in Forest A appear as a mail user in Forest B and vice versa. Users in the target forest can then select the mail user object that represents a recipient in another forest to send mail.

To enable GAL synchronization, you create management agents that import mail-enabled users, contacts, and groups from designated Active Directory services into a centralized metadirectory. In the metadirectory, mail-enabled objects are represented as mail users. Groups are represented as contacts without any associated membership. The management agents then export these mail users to an organizational unit in the specified target forest.

For more information about Forefront Identity Manager (FIM), see [Forefront Identity Manager 2010](https://docs.microsoft.com/previous-versions/tn-archive/ff630889(v=technet.10)).

## Moving mailboxes across forests

In a cross-forest topology, you may want to move mailboxes from one forest to another. To do this you must use the **New-MoveRequest** or **New-MigrationBatch** cmdlet in the Exchange Management Shell. For more information about moving mailboxes across forests, see the following topics:

  - [Prepare mailboxes for cross-forest move requests](prepare-mailboxes-for-cross-forest-move-requests-exchange-2013-help.md)

  - [Prepare mailboxes for cross-forest moves using the Prepare-MoveRequest.ps1 script in the Shell](prepare-mailboxes-for-cross-forest-moves-using-the-prepare-moverequest-ps1-script-in-the-shell-exchange-2013-help.md)

  - [Prepare mailboxes for cross-forest moves using sample code](prepare-mailboxes-for-cross-forest-moves-using-sample-code-exchange-2013-help.md)

## Understanding multiple forest administration

Exchange 2013 uses new permissions functionality to manage your multiple forest environments.

Exchange 2013 uses a Role Based Access Control (RBAC) permissions model. The management role groups that administrators are members of, and the management role assignment policies that end-users are assigned, determine what each administrator and end-user can do. To understand multiple forest permissions, you need to be familiar with RBAC. For more information about RBAC and role groups and role assignment policies in particular, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

You can use the RBAC permissions model to configure and manage the permissions between your forests. For more information about multiple forest permissions, see the following topics:

  - [Understanding multiple-forest permissions](understanding-multiple-forest-permissions-exchange-2013-help.md)

  - [Manage linked role groups](manage-linked-role-groups-exchange-2013-help.md)

  - [Create linked role groups that mirror built-in role groups](create-linked-role-groups-that-mirror-built-in-role-groups-exchange-2013-help.md)
