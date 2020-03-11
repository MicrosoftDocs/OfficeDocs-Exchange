---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage sharing and collaboration features in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b7fa4b7c-1266-45bd-a14b-f66be0459cc5
ms.reviewer:
title: Sharing and collaboration permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Sharing and collaboration permissions

The permissions required to configure sharing and collaboration features vary depending on the procedure being performed or the cmdlet you want to run. For more information about sharing and collaboration, see [Collaboration](https://docs.microsoft.com/Exchange/collaboration/collaboration) and [Sharing](https://docs.microsoft.com/exchange/sharing-exchange-2013-help).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://technet.microsoft.com/library/dd298183.aspx).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

## Sharing and collaboration feature permissions

You can use the features in the following table to configure sharing and collaboration features. The role groups that are required to configure each feature are listed.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://technet.microsoft.com/library/dd351130.aspx).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Partner applications - configure|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Public folders, mail-enabled|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://technet.microsoft.com/library/669d602e-68e3-41f9-a455-b942d212d130.aspx)|
|Public folders|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Public Folder Management](https://technet.microsoft.com/library/e167d95e-bb39-43fd-b960-204ab0de27da.aspx)|
|Site mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://technet.microsoft.com/library/669d602e-68e3-41f9-a455-b942d212d130.aspx)|
|Site mailbox provisioning policy|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://technet.microsoft.com/library/669d602e-68e3-41f9-a455-b942d212d130.aspx)|
