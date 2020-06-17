---
title: 'Install or remove add-ins for Outlook for your Exchange 2013 organization'
TOCTitle: Install or remove add-ins for Outlook for your organization
ms:assetid: 112f3ef7-9943-4a1e-8a42-e08e8e9f67f4
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

# Install or remove add-ins for Outlook for your Exchange 2013 organization

_**Applies to:** Exchange Server 2013_

You can install or remove add-ins for Outlook for your organization by using the EAC or the Exchange Management Shell.

> [!NOTE]
> By default, after you install an add-in for your organization, the add-in is available for all users in your organization. After installation, you can use the EAC or the Exchange Management Shell to make the add-in optional or required for your users, and to specify whether you want the add-in to be enabled or disabled. For information about how to change the default settings for an add-in, see [Manage user access to add-ins for Outlook](manage-user-access-to-add-ins-2013-help.md). To limit availability of add-ins to specific users in your organization, you must use the Exchange Management Shell. For more information, see [Manage user access to add-ins for Outlook](manage-user-access-to-add-ins-2013-help.md).

For additional management tasks, see [Add-ins for Outlook](add-ins-for-outlook-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Apps for Outlook" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For more information about the EAC, see [Exchange admin center in Exchange 2013](exchange-admin-center-in-exchange-2013-exchange-2013-help.md).

- To learn how to connect to the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You can assign administrators permission to install and manage add-ins for your organization. You can also assign users permission to install and manage add-ins for their own use. For more information, see [Specify the administrators and users who can install and manage add-ins for Outlook](specify-who-can-install-and-manage-add-ins-2013-help.md).

- Access to the Office Store isn't supported for mailboxes or organizations in specific regions. If you don't see **Add from the Office Store** as an option in the **Exchange admin center** under **Organization** \> **Add-ins** \> **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif), you may be able to install an add-in for Outlook from a URL or file location. For more information, contact your service provider.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Install an add-in for Outlook

### Use the EAC to add an add-in

1. In the EAC, navigate to **Organization** \> **Add-ins**.

2. Click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif), and then choose the location that you want to install the add-in from.

   - **Add from the Office Store**: At the Office Store, select the app you want to install, and then click **Add**. Apps that work with Outlook Web App are listed under **Add-ins for Office and SharePoint** \> **Outlook**.

   > [!NOTE]
   > Access to the Office Store isn't supported for mailboxes or organizations in specific regions. If you don't see **Add from the Office Store** as an option in the **Exchange admin center** under **Organization** \> **Add-ins** \> **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif), you may be able to install an add-in for Outlook from a URL or file location. For more information, contact your service provider.

   - **Add from URL**: In **URL**, enter the full URL for the add-in manifest file that you want to install.

   - **Add from file**: Select **Browse**, and then navigate to the location of the add-in manifest file that you want to install.

3. Click **Save**.

### Use the Exchange Management Shell to add an add-in

This example shows you how to add an add-in from a URL.

```PowerShell
New-App -OrganizationApp -Url <URL location for add-in manifest file>
```

This example shows you how to add an add-in from a file.

```PowerShell
New-App -OrganizationApp -FileData <File location for add-in manifest file>
```

> [!TIP]
> When you use the Exchange Management Shell to install an add-in for your organization, you can install the add-in and configure settings for it at the same time.

For syntax and parameters, see [New-App](https://docs.microsoft.com/powershell/module/exchange/new-app).

## Remove an add-in for Outlook

### Use the EAC to remove an add-in

1. In the EAC, navigate to **Organization** \> **Add-ins**.

2. In the list view, select the app that you want to remove, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

### Use the Exchange Management Shell to remove an add-in

You can use the Exchange Management Shell to remove an add-in from your organization.

> [!NOTE]
> Run the following command to look up the display names and application IDs for all the add-ins for Outlook installed for your organization.

```PowerShell
Get-App -OrganizationApp |Format-List DisplayName,AppID
```

Run the following command to remove the custom add-in Finance Test Add-in from the organization.

```PowerShell
Remove-App -OrganizationApp -Identity <GUID for Finance Test Add-in>
```

For syntax and parameters, see [Remove-App](https://docs.microsoft.com/powershell/module/exchange/remove-app).

## How do you know this worked?

To view the add-ins that are installed in your organization, do one the following:

- In the EAC, navigate to **Organization** \> **Add-ins**, and then review the list of installed add-ins.

- From the Exchange Management Shell, run `Get-App`, and then review the list of installed add-ins.
