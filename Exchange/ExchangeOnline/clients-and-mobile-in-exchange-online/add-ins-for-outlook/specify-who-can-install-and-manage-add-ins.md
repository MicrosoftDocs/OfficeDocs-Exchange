---
localization_priority: Normal
description: You can specify which administrators in your organization have permissions to install and manage add-ins for Outlook. You can also specify which users in your organization have permission to install and manage add-ins for their own use.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 7ee4302d-b8bb-40a0-9810-10d3a0271bcb
ms.reviewer: 
title: Specify the administrators and users who can install and manage add-ins for Outlook
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Specify the administrators and users who can install and manage add-ins for Outlook

You can specify which administrators in your organization have permissions to install and manage add-ins for Outlook. You can also specify which users in your organization have permission to install and manage add-ins for their own use.

This is done by assigning or removing management roles specific to add-ins. There are five built-in roles you can use.

## Administrative roles

- **Org Marketplace Apps**: Enables an administrator to install and manage add-ins that are available from the Office Store for their organization.

- **Org Custom Apps**: Enables an administrator to install and manage custom add-ins for their organization.

By default, all administrators who are in the **Organization Management** role group have both of the above administrative roles enabled.

## User roles

- **My Marketplace Apps**: Enables a user to install and manage Office Store add-ins for their own use.

- **My Custom Apps**: Enables a user to install and manage custom add-ins for their own use.

- **My ReadWriteMailbox Apps**: Enables a user to install and manage add-ins that request the `ReadWriteMailbox` permission level in their manifest.

 By default, all end users have all of the above user roles enabled.

> [!NOTE]
>
> If you are testing Outlook add-ins and none are showing up, then as a first troubleshooting step, use the **Get-OrganizationConfig** PowerShell cmdlet to query the *AppsForOfficeEnabled* parameter. If the query returns a value of False, set this parameter to True using the **Set-OrganizationConfig** cmdlet and then add-ins should appear as expected.
>
> We do not recommend that the *AppsForOfficeEnabled* parameter be set to False. A value of False will override all of the above Administrative and User role settings and prevent any new apps from being activated by any user in the organization.

For information about add-ins, see [Add-ins for Outlook](add-ins-for-outlook.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can run this cmdlet. Although all parameters for this cmdlet are listed in this topic, you may not have access to some parameters if they're not included in the permissions assigned to you. To see what permissions you need, see the "Role assignments" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Access to the Office Store isn't supported for mailboxes or organizations in specific regions. If you don't see **Add from the Office Store** as an option in the **Exchange admin center** under **Organization** \> **Add-ins** \> **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), you may be able to install an add-in for Outlook from a URL or file location. For more information, contact your service provider.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Assign administrators the permissions required to install and manage add-ins for your organization

### Use the EAC to assign permissions to administrators

You can use the Exchange Admin Center (EAC) to assign administrators the permissions required to install and manage add-ins that are available from the Office Store for your organization.

## Assign users the permissions required to install and manage add-ins for their own use

### Use the EAC to assign permissions to users

You can use the Exchange Admin Center (EAC) to assign users the permissions required to view and modify custom add-ins for their own use. For detailed information about how to do this, see [Manage role groups in Exchange Online](../../permissions-exo/role-groups.md).

## Prevent add-in downloads by turning off the Office Store across Outlook

The above steps will ensure that all end users with the default policy will no longer be able to install or manage Add-ins for Outlook.

1. Log in to the Exchange Admin Console as a global administrator.
2. Navigate to **Permissions**, and then select **User Roles**. 
3. Double-click **Default Role with Add-Ins Management** to open the edit window.
4. Modify **Default Role Assignment Policy** by deselecting **My Custom Apps**, **My MarketPlace Apps**, and **My ReadWriteMailbox Apps**.
5. Click **Save**.


## How do you know this worked?

To verify that you've successfully assigned permissions for a user, replace \<Role  Name\> with the name of the role to verify, and run the following command in Exchange Online PowerShell:

```PowerShell
Get-ManagementRoleAssignment -Role "<Role Name>" -GetEffectiveUsers
```

This example shows you how to verify whom you've assigned permissions to install add-ins from the Office Store for the organization.

```PowerShell
Get-ManagementRoleAssignment -Role "Org Marketplace Apps" -GetEffectiveUsers
```

In the results, review the entries in the **Effective Users** column.

For detailed syntax and parameter information, see [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/get-managementroleassignment).
