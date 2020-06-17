---
title: 'View members of a dynamic distribution group: Exchange 2013 Help'
TOCTitle: View members of a dynamic distribution group
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 40b100c6-864e-4c82-9f98-08dd5c83e378
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# View members of a dynamic distribution group in Exchange 2013

_**Applies to:** Exchange Server 2013_

Dynamic distribution groups are distribution groups whose membership is based on specific recipient filters rather than a defined set of recipients. Microsoft Exchange provides precanned filters to make it easier to create recipient filters for dynamic distribution groups. A precanned filter is a commonly used filter that you can use to meet a variety of recipient-filtering criteria. You can specify the recipient types you want to include in a dynamic distribution group. Additionally, you can also specify a list of conditions that the recipients must meet. You can use the Shell to preview the list of recipients for a dynamic distribution group that uses precanned filters.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Dynamic distribution groups" entry in the  topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to preview the list of members of a dynamic distribution group
<a name="Shell"> </a>

This example returns the list of members for the dynamic distribution group named Full Time Employees. The first command stores the dynamic distribution group object in the variable `$FTE`. The second command uses the **Get-Recipient** cmdlet to list the recipients that match the criteria defined for the dynamic distribution group.

```powershell
$FTE = Get-DynamicDistributionGroup "Full Time Employees"
```

```powershell
Get-Recipient -RecipientPreviewFilter $FTE.RecipientFilter -OrganizationalUnit $FTE.RecipientContainer
```

For detailed syntax and parameter information, see [Get-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-dynamicdistributiongroup) and [Get-Recipient](https://docs.microsoft.com/powershell/module/exchange/get-recipient).

> [!NOTE]
> You cannot view members of a dynamic distribution group by using the EAC.

## How do you know this worked?

To verify that you've successfully viewed the members of a dynamic distribution group, do the following:

- In the Shell, a list of members is returned after you run the previous command to preview a list of dynamic distribution group members. For example, if you created a new user mailbox with properties that match the recipient filter for the dynamic distribution group, this new user should be displayed in the list of group members.
