---
title: 'Specify the administrators and users who can install and manage add-ins for Outlook in Exchange 2013'
TOCTitle: Specify who can install and manage add-ins for Outlook
ms:assetid: 7ee4302d-b8bb-40a0-9810-10d3a0271bcb
ms:mtpsurl:
ms:contentKeyID:
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Specify the administrators and users who can install and manage add-ins for Outlook in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can specify which administrators in your organization have permissions to install and manage add-ins for Outlook. You can also specify which users in your organization have permission to install and manage add-ins for their own use.

This is done by assigning or removing management roles specific to add-ins. There are five built-in roles you can use.

Administrative roles

- **Org Marketplace Apps**: Enables an administrator to install and manage add-ins that are available from the Office Store for their organization.

- **Org Custom Apps**: Enables an administrator to install and manage custom add-ins for their organization.

User roles

- **My Marketplace Apps**: Enables a user to install and manage Office Store add-ins for their own use.

- **My Custom Apps**: Enables a user to install and manage custom add-ins for their own use.

- **My ReadWriteMailbox Apps**: Enables a user to install and manage add-ins that request the ReadWriteMailbox permission level in their manifest.

By default, all administrators who have the **Organization Management** role group have both of the above administrative roles enabled. Also by default, end users have the above user roles enabled.

For information about each of these roles, see [Org Marketplace Apps role](org-marketplace-apps-role-exchange-2013-help.md), [Org Custom Apps role](org-custom-apps-role-exchange-2013-help.md), [My Marketplace Apps role](my-marketplace-apps-role-exchange-2013-help.md), [My Custom Apps role](my-custom-apps-role-exchange-2013-help.md), and [My ReadWriteMailbox Apps role](my-readwritemailbox-apps-role-exchange-2013-help.md).

For information about add-ins, see [Add-ins for Outlook in Exchange 2013](add-ins-for-outlook-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can run this cmdlet. Although all parameters for this cmdlet are listed in this topic, you may not have access to some parameters if they're not included in the permissions assigned to you. To see what permissions you need, see the "Role assignments" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- Access to the Office Store isn't supported for mailboxes or organizations in specific regions. If you don't see **Add from the Office Store** as an option in the **Exchange admin center** under **Organization** \> **Add-ins** \> **New** ![Add icon](images/ITPro_EAC_AddIcon.gif), you may be able to install an add-in for Outlook from a URL or file location. For more information, contact your service provider.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Assign administrators the permissions required to install and manage add-ins for your organization

### Use the EAC to assign permissions to administrators

You can use the EAC to assign administrators the permissions required to install and manage add-ins that available from the Office Store for your organization. For detailed information about how to do this, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

## Assign users the permissions required to install and manage add-ins for their own use

### Use the EAC to assign permissions to users

You can use the EAC to assign users the permissions required to view and modify custom add-ins for their own use. For detailed information about how to do this, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

## How do you know this worked?

To verify that you've successfully assigned permissions for a user, run a Shell command using the format `Get-ManagementRoleAssignment -Role <Role Name> -GetEffectiveUsers`, where `Role Name` is the role for which you want to verify assigned permissions.

This example shows you how to verify whom you've assigned permissions to install add-ins from the Office Store for the organization.

1. Run `Get-ManagementRoleAssignment -Role "Org Marketplace Apps" -GetEffectiveUsers`.

2. In the results, review the entries in the **Effective Users** column.

For more information about syntax and parameters, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/get-managementroleassignment).
