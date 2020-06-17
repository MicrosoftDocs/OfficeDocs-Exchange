---
localization_priority: Normal
description: Learn about permissions that are required to manage email address and address book features in Exchange Server 2016 or Exchange Server 2019
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 1c1de09d-16ef-4424-9bfb-eb7edffbc8c2
ms.reviewer:
title: Email address and address book permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Email address and address book permissions

The permissions required to configure email address and address book features vary depending on the procedure being performed or the cmdlet you want to run. For more information about email addresses and address books, see [Email addresses and address books in Exchange Server](../../email-addresses-and-address-books/email-addresses-and-address-books.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

## Email address and address book permissions

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Address book policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Address lists|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Email address policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Details templates|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Global address lists|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Offline address books|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Offline address book connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
