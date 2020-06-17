---
localization_priority: Normal
description: Sharing policies control how your users share their calendars with people outside your organization. The sharing policy that an admin applies to the user's mailbox determines what level of access a user can share and with whom. If you don't change anything, then all users can invite anyone with an email address to view their calendar. If you create a new sharing policy, you have to apply that policy to mailboxes before it takes effect. Sharing policies are applied to individual user's mailboxes. An admin can also disable a user's sharing policy to prevent external access to calendars.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a075b1ec-5a96-4d0e-a609-ab64e50cdb9e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Apply a sharing policy to mailboxes in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Apply a sharing policy to mailboxes in Exchange Online

Sharing policies control how your users share their calendars with people outside your organization. The sharing policy that an admin applies to the user's mailbox determines what level of access a user can share and with whom. If you don't change anything, then all users can invite anyone with an email address to view their calendar. If you create a new sharing policy, you have to apply that policy to mailboxes before it takes effect. Sharing policies are applied to individual user's mailboxes. An admin can also disable a user's sharing policy to prevent external access to calendars.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) topic.

- A sharing policy must exist. For details, see [Create a sharing policy in Exchange Online](create-a-sharing-policy.md).

## Use the Exchange admin center to apply a sharing policy to one mailbox
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **recipients** \> **mailboxes**.

3. In the list view, select the mailbox you want, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **User Mailbox**, click **mailbox features**.

5. In the **Sharing policy** list, select the sharing policy you want to apply to this mailbox.

6. Click **save** to apply the sharing policy.

## Use the Exchange admin center to apply a sharing policy to multiple mailboxes
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center dashboard, go to **Admin** \> **Exchange**.

2. Go to **recipients** \> **mailboxes**.

3. In the list view, hold the Ctrl key while you select multiple mailboxes.

4. In the details pane, the mailbox properties will be configured for bulk edit. Scroll down to click **More options**.

5. Under **Sharing Policy**, click **Update**.

6. In **bulk assign sharing policy**, **select the sharing policy** from the list.

7. Click **save** to apply the sharing policy to the selected mailboxes.

## Use Exchange Online PowerShell to apply a sharing policy to one or more mailboxes
<a name="BKMK_Shell"> </a>

This example applies the sharing policy Contoso to Barbara's mailbox.

```PowerShell
Set-Mailbox -Identity Barbara -SharingPolicy "Contoso"
```

This example finds all user mailboxes in the Marketing department and then applies the sharing policy Contoso Marketing.

```PowerShell
Get-Mailbox -Filter "Department -eq 'Marketing'" | Set-Mailbox -SharingPolicy "Contoso Marketing"
```

This example shows all mailboxes that have the sharing policy Contoso applied, and it sorts the users into a table that displays only their aliases and email addresses.

```PowerShell
Get-Mailbox -ResultSize unlimited | Where {$_.SharingPolicy -eq "Contoso"} | format-table Alias,EmailAddresses
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) and [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox).

## How do you know this worked?

To verify that you have successfully applied the sharing policy to a user mailbox, do one of the following:

- In the Exchange admin center, go to **recipients** \> **mailboxes**, and then select the mailbox to which you applied the sharing policy. Click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif), click **mailbox features**, and then confirm that the correct sharing policy displays in the **Sharing policy**.

- Run the following command to verify the sharing policy was assigned to a user mailbox. Verify that the correct sharing policy is listed for the _SharingPolicy_ parameter.

  ```PowerShell
  Get-Mailbox <username> | format-list
  ```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
