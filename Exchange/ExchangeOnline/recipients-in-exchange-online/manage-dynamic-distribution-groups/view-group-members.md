---
localization_priority: Normal
description: Dynamic distribution groups are distribution groups whose membership is based on specific recipient filters rather than a defined set of recipients. Microsoft Exchange provides precanned filters to make it easier to create recipient filters for dynamic distribution groups. A precanned filter is a commonly used filter that you can use to meet a variety of recipient-filtering criteria. You can specify the recipient types you want to include in a dynamic distribution group. Additionally, you can also specify a list of conditions that the recipients must meet. You can use Exchange Online PowerShell to preview the list of recipients for a dynamic distribution group that uses precanned filters.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 40b100c6-864e-4c82-9f98-08dd5c83e378
ms.reviewer: 
f1.keywords:
- NOCSH
title: View members of a dynamic distribution group
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# View members of a dynamic distribution group

Dynamic distribution groups are distribution groups whose membership is based on specific recipient filters rather than a defined set of recipients. Microsoft Exchange provides precanned filters to make it easier to create recipient filters for dynamic distribution groups. A precanned filter is a commonly used filter that you can use to meet a variety of recipient-filtering criteria. You can specify the recipient types you want to include in a dynamic distribution group. Additionally, you can also specify a list of conditions that the recipients must meet. You can use Exchange Online PowerShell to preview the list of recipients for a dynamic distribution group that uses precanned filters.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to preview the list of members of a dynamic distribution group
<a name="Shell"> </a>

This example returns the list of members for the dynamic distribution group named Full Time Employees. The first command stores the dynamic distribution group object in the variable `$FTE`. The second command uses the **Get-Recipient** cmdlet to list the recipients that match the criteria defined for the dynamic distribution group.

```PowerShell
$FTE = Get-DynamicDistributionGroup "Full Time Employees"
```

```PowerShell
Get-Recipient -RecipientPreviewFilter $FTE.RecipientFilter -OrganizationalUnit $FTE.RecipientContainer
```

This example displays the list of users and email addresses (more than 1000 mailboxes).

```PowerShell
Get-Recipient -ResultSize Unlimited -RecipientPreviewFilter $FTE.RecipientFilter -OrganizationalUnit $FTE.RecipientContainer | Format-Table Name,Primary*
```
For detailed syntax and parameter information, see [Get-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-dynamicdistributiongroup) and [Get-Recipient](https://docs.microsoft.com/powershell/module/exchange/get-recipient).

> [!NOTE]
> You can't view members of a dynamic distribution group by using the EAC.

## How do you know this worked?

To verify that you've successfully viewed the members of a dynamic distribution group, do the following:

- In Exchange Online PowerShell, a list of members is returned after you run the previous command to preview a list of dynamic distribution group members. For example, if you created a new user mailbox with properties that match the recipient filter for the dynamic distribution group, this new user should be displayed in the list of group members.
