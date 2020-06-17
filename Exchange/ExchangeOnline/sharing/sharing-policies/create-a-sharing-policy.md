---
localization_priority: Normal
description: Create a new Sharing Policy to change how people in your organization share calendars with individual business associates, friends, or family members. Sharing policies control how your users share their calendars with people outside your organization. By default, all users can invite anyone with an email address to view their calendar. After you create a new sharing policy, you have to apply that policy to mailboxes before it takes effect. To apply a specific sharing policy to users, see Apply a sharing policy to mailboxes in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f412ce6c-74dc-4d77-93ab-362c37414015
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create a sharing policy in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create a sharing policy in Exchange Online

Create a new Sharing Policy to change how people in your organization share calendars with individual business associates, friends, or family members. Sharing policies control how your users share their calendars with people outside your organization. By default, all users can invite anyone with an email address to view their calendar. After you create a new sharing policy, you have to apply that policy to mailboxes before it takes effect. To apply a specific sharing policy to users, see [Apply a sharing policy to mailboxes in Exchange Online](apply-a-sharing-policy.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) topic.

- Only Outlook 2010 or later and Outlook on the web (formerly known as Outlook Web App) users can create sharing invitations.

## Use the wizard to create a sharing policy

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. In the list view, under **Individual Sharing**, click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. In **new sharing policy**, type a friendly name for the sharing policy in the **Policy name** box.

5. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to define the sharing rules for the policy.

6. In **sharing rule**, select one of the following options to specify the domains you want to share with:

   - **Sharing with all domains**

   - **Sharing with a specific domain**

7. If you select **Sharing with a specific domain**, type the name of the domain you want to share with. If you need to enter more than one domain for this sharing policy, save the settings for the first domain, then edit the sharing rules to add more domains.

8. To specify the information that can be shared, select the **Share your calendar folder** check box, and then select one of the following options:

   - **Calendar free/busy information with time only**

   - **Calendar free/busy information with time, subject, and location**

   - **All calendar appointment information, including time, subject, location and title**

9. Click **save** to set the rules for the sharing policy.

10. If you want to set this sharing policy as the new default sharing policy for all users in your Microsoft 365 or Office 365 organization, select the **Make this policy my default sharing policy** check box.

11. Click **save** to create the sharing policy.

## Use Exchange Online PowerShell to create a sharing policy

- This example creates the sharing policy Contoso. This policy allows users in the contoso.com domain to see your user's detailed calendar availability (free/busy) information. By default, this policy is enabled.

  ```PowerShell
  New-SharingPolicy -Name "Contoso" -Domains contoso.com: CalendarSharingFreeBusyDetail
  ```

- This example creates the sharing policy ContosoWoodgrove for two different domains (contoso.com and woodgrovebank.com) with different sharing settings configured for each domain. The policy is disabled.

  ```PowerShell
  New-SharingPolicy -Name "ContosoWoodgrove" -Domains 'contoso.com: CalendarSharingFreeBusySimple', 'woodgrovebank.com: CalendarSharingFreeBusyDetail' -Enabled $false
  ```

For detailed syntax and parameter information, see [New-SharingPolicy](https://docs.microsoft.com/powershell/module/exchange/new-sharingpolicy).

## How do you know this worked?

To verify that you have successfully created the sharing policy, run the following command to view the sharing policy information.

```PowerShell
Get-SharingPolicy <policy name> | format-list
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
