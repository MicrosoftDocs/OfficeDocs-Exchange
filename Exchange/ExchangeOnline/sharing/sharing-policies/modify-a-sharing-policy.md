---
localization_priority: Normal
description: Sharing policies control how your users share their calendars with people outside your organization. You may want to change some sharing policy properties, such as changing sharing rules, changing the free/busy access level, temporarily disabling a sharing policy, or removing a sharing policy entirely.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a494c4d2-9316-44ce-9a39-268398378f08
ms.reviewer: 
f1.keywords:
- NOCSH
title: Modify, disable, or remove a sharing policy in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Modify, disable, or remove a sharing policy in Exchange Online

Sharing policies control how your users share their calendars with people outside your organization. You may want to change some sharing policy properties, such as changing sharing rules, changing the free/busy access level, temporarily disabling a sharing policy, or removing a sharing policy entirely.

For details about how to create a sharing policy, see [Create a sharing policy in Exchange Online](create-a-sharing-policy.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) topic.

## Use the Exchange admin center to change a sharing policy
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. Under **Individual Sharing**, select a sharing a policy, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **sharing policy**, click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

5. In **sharing rule**, change the settings such as the domain you want to share information with and the sharing level for calendars. Click **save** to update the rule.

6. In **sharing policy**, click **save** to update the sharing policy.

## Use the Exchange admin center to set a sharing policy as the default sharing policy
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. Under **Individual Sharing**, select a sharing a policy, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **sharing policy**, select the **Make this policy my default sharing policy** check box.

5. Click **save** to update the sharing policy.

## Use the Exchange admin center to disable a sharing policy
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. Under **Individual Sharing**, select a sharing a policy.

4. In the **On** column, clear the check box for the sharing policy you want to disable.

### Use the Exchange admin center to remove a sharing policy
<a name="BKMK_EAC"> </a>

> [!IMPORTANT]
> Before you remove a sharing policy, the sharing policy must be removed from all user mailboxes.

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. Under **Individual Sharing**, select a sharing a policy, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

4. In the warning, click **yes** to delete the sharing policy.

## Use Exchange Online PowerShell to modify, disable or remove a sharing policy
<a name="BKMK_Shell"> </a>

- This example modifies the sharing policy Contoso. This policy allows users in the Contoso domain to see simple free/busy information.

  ```PowerShell
  Set-SharingPolicy -Identity Contoso -Domains 'sales.contoso.com: CalendarSharingFreeBusySimple'
  ```

- This example adds a second domain to the sharing policy Contoso. When you're adding a domain to an existing policy, you must include any previously included domains.

  ```PowerShell
  Set-SharingPolicy -Identity Contoso -Domains 'contoso.com: CalendarSharingFreeBusySimple', 'atlanta.contoso.com: CalendarSharingFreeBusyReviewer', 'beijing.contoso.com: CalendarSharingFreeBusyReviewer'
  ```

- This example sets the sharing policy Contoso as the default sharing policy.

  ```PowerShell
  Set-SharingPolicy -Identity Contoso -Default $True
  ```

- This example disables the sharing policy Contoso.

  ```PowerShell
  Set-SharingPolicy -Identity "Contoso" -Enabled $False
  ```

- The first example removes the sharing policy Contoso. The second example removes the sharing policy Contoso and suppresses the confirmation that you want to remove the policy.

  ```PowerShell
  Remove-SharingPolicy -Identity Contoso
  ```

  ```PowerShell
  Remove-SharingPolicy -Identity Contoso -Confirm

  ```

For detailed syntax and parameter information, see [Set-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/set-sharingpolicy) and [Remove-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-sharingpolicy).
