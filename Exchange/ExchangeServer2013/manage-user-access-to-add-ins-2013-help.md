---
title: 'Manage user access to add-ins for Outlook in Exchange 2013'
TOCTitle: Manage user access to add-ins for Outlook
ms:assetid: e5833dec-a23a-439e-ac03-92671817bff8
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

# Manage user access to add-ins for Outlook in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Exchange admin center (EAC) or the Exchange Management Shell to manage user access to add-ins for Outlook.

- Using the EAC, you can manage basic add-in access settings for your users at an organizational level. For example, you can configure whether an add-in is enabled or disabled for your users. You can also specify whether an add-in is required or optional for your users.

- With the Exchange Management Shell, you can manage all the settings that you can with the EAC, as well as other settings. For example, you can limit availability to specific users in your organization.

For additional management tasks, see [Add-ins for Outlook](add-ins-for-outlook-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- For more information about the EAC, see [Exchange admin center in Exchange 2013](exchange-admin-center-in-exchange-2013-exchange-2013-help.md).

- To learn how to connect to the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Add-ins for Outlook" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Specify whether an add-in is available, enabled, or disabled

### Use the EAC to specify whether an add-in is available, enabled, or disabled

1. In the EAC, navigate to **Organization** \> **Add-ins**.

2. In the list view, select the add-in that you want to change settings for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. If you don't want your users to use the add-in, clear the **Make this add-in available to users in your organization** check box, and then click **Save**.

4. If you want your users to be able to use the add-in, select **Make this add-in available to users in your organization**, and then select the option you want.

   - **Optional, enabled by default**: Use this setting if you want to allow your users to turn off the add-in.

   - **Optional, disabled by default**: Use this setting if you want to allow your users to turn on the add-in.

   - **Mandatory, always enabled. Users can't disable this add-in**: Use this setting if you don't want your users to turn off the add-in.

5. Click **Save**.

### Use the Exchange Management Shell to specify whether an add-in is available, enabled, or disabled

First, run the following command to find the display names and add-in IDs for all the add-ins for Outlook installed for your organization.

```powershell
Get-App -OrganizationApp | Format-List DisplayName,AppId
```

The **AppId** value is a GUID that uniquely identifies the add-in (for example, `fe93bfe1-7947-460a-a5e0-7a5906b51360`). You use the **AppId** value to identify and change the settings of the add-in.

To disable and hide an add-in from all your users, replace _\<AppId\>_ with the real **AppId** value and run the following command:

```powershell
Set-App -Identity <AppId> -OrganizationApp -Enabled $false
```

To enable an add-in by default, but allow your users to turn it off, replace _\<AppId\>_ with the real **AppId** value and run the following command:

```powershell
Set-App -Identity <AppId> -OrganizationApp -Enabled $true -DefaultStateForUser Enabled
```

To disable an add-in by default, but allow your users to turn it on, replace _\<AppId\>_ with the real **AppId** value and run the following command:

```powershell
Set-App -Identity <AppId> -OrganizationApp -Enabled $true -DefaultStateForUser Disabled
```

If you want the add-in to be required for your users, replace _\<AppId\>_ with the real **AppId** value and run the following command:

```powershell
Set-App -Identity <AppId> -OrganizationApp -Enabled $true -DefaultStateForUser AlwaysEnabled
```

For detailed syntax and parameters, see [Set-App](https://docs.microsoft.com/powershell/module/exchange/set-app).

### How do you know this worked?

To verify that you've successfully configured an add-in, use either of the following steps:

- In the EAC, navigate to **Organization** \> **Add-ins** and review the values in the **User Default** and **Provided To** columns.

- In the Exchange Management Shell, run the following command and verify the values of the **DefaultStateForUser** and **Enabled** properties:

  ```powershell
  Get-App -OrganizationApp | Format-List DisplayName,AppId,Enabled,DefaultStateForUser
  ```

## Use the Exchange Management Shell to limit add-in availability to specific users

To limit the availability of an add-in to specific users, you can't use the EAC. You can only use the Exchange Management Shell.

This example limits the LinkedIn add-in with the hypothetical **AppId** value `ac83a9d5-5af2-446f-956a-c583adc94d5e` to the members of the distribution group named Marketing.

```powershell
$a = Get-DistributionGroupMember Marketing
```

```powershell
Set-App -Identity ac83a9d5-5af2-446f-956a-c583adc94d5e -OrganizationApp -ProvidedTo SpecificUsers -UserList $a.Identity -DefaultStateForUser Enabled
```

For detailed syntax and parameters, see [Set-App](https://docs.microsoft.com/powershell/module/exchange/set-app).

### How do you know this worked?

To verify that you've successfully limited add-in availability to specific users, run the following command in the Exchange Management Shell and verify the value of the **ProvidedTo** and **UserList** properties:

```powershell
Get-App -OrganizationApp | Format-List DisplayName,AppId,Enabled,DefaultStateForUser,ProvidedTo,UserList
```
