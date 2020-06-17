---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage Unified Messaging services and features in Exchange Server 2016.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d326c3bc-8f33-434a-bf02-a83cc26a5498
ms.reviewer:
title: Unified Messaging permissions in Exchange 2016
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Unified Messaging permissions

The permissions required to manage Unified Messaging services and features on Exchange 2016 Mailbox servers vary depending on the procedure being performed or the cmdlet you want to run.

> [!NOTE]
> Unified Messaging is not available in Exchange 2019.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

## UM component permissions

You can configure settings for the UM components and features in the following table.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|UM auto attendants|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM call answering rules|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM call data and summary reports|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM Call Router service (front-end)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM dial plans|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM hunt groups|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM IP gateways|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM mailbox policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM prompts|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Unified Messaging Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|UM service (back-end)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
