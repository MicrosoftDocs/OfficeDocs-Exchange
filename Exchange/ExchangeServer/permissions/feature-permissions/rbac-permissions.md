---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage role management in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cb9591c4-fbb3-4199-8007-6bbfdfd5a2e9
ms.reviewer:
title: Role management permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Role management permissions

The permissions required to perform tasks to configure management roles vary depending on the procedure being performed or the cmdlet you want to run. For more information about management roles, see [Understanding Management Roles](https://docs.microsoft.com/exchange/understanding-management-roles-exchange-2013-help).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

## Role management permissions

You can use the features in the following table to manage the management role groups, roles, assignment policies, assignments, scopes that define the permissions you can apply to administrators, and end users. Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Management roles|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Unscoped management roles|[Unscoped Role Management](https://docs.microsoft.com/exchange/unscoped-role-management-role-exchange-2013-help) management role|
|Role groups|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Assignment policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Role assignments|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Management scopes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Management role entries|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Legacy permissions|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Active Directory split permissions|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> **Important**: To run the `setup.exe` command with the _PrepareAD_ and _ActiveDirectorySplitPermissions_ parameters, the account you use must be a member of the Schema Admins and Enterprise Administrators groups.|
