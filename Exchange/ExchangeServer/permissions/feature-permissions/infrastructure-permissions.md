---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to perform tasks to configure various components of Exchange Server 2016 or Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 3646a4e8-36b2-41fb-89a4-79b0963fcb11
ms.reviewer:
title: Exchange infrastructure and PowerShell permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange infrastructure and PowerShell permissions

The permissions required to perform tasks to configure various components of Exchange Server depend on the procedure being performed or the cmdlet you want to run. See each of the sections in this topic for more information about their respective features.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

> [!NOTE]
> Some features may require that you have local administrator permissions on the server you want to manage. To manage these features, you must be a member of the Local Administrators group on that server.

## Exchange infrastructure permissions

The following table lists the permissions required to perform tasks that configure general Exchange settings.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Administrator audit logging|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Records Management](https://docs.microsoft.com/exchange/records-management-exchange-2013-help)|
|Exchange admin center configuration settings|[View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Exchange admin center connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange server configuration settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange Help settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Message categories|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Hygiene Management](https://docs.microsoft.com/exchange/hygiene-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Product key|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Test system health|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|View-only administrator audit logging|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Records Management](https://docs.microsoft.com/exchange/records-management-exchange-2013-help) <br/> **Note**: You can also manually assign the View-Only Audit Logs management role to a management role group. For more information, see [View-Only Audit Logs](https://docs.microsoft.com/exchange/view-only-audit-logs-role-exchange-2013-help).|
|Write to audit log|Users that are members of any role group or assigned any management role can write to the administrator audit log.|

## Exchange PowerShell infrastructure permissions

The following table lists the permissions required to perform tasks that configure features that control how the Exchange Management Shell runs.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Active Directory Domain Services server settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [UM Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|Cmdlet extension agents|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|PowerShell virtual directories|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|PowerShell and WinRM installation|Local Server Administrator|
|Remote PowerShell|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Federation and certificates permissions

The following table lists permissions required for performing tasks related to federation trusts, OAuth configuration, certificate management, and hybrid deployment configuration.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Certificate management|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Federation trusts, OAuth|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Test federation trusts, OAuth|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Hybrid deployment configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Intra-Organization connectors|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Records Management](https://docs.microsoft.com/exchange/records-management-exchange-2013-help)|
