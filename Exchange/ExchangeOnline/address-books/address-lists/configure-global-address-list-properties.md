---
localization_priority: Normal
description: Admins can learn how to modify the settings of a global address list (GAL) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5fd2c96f-fe93-4b5a-8495-70c450511a37
ms.reviewer:
title: Configure global address list properties in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Configure global address list properties in Exchange Online

The built-in global address list (GAL) that's automatically created by Exchange Online includes every mail-enabled object in the organization. You can create additional GALs to separate users by organization or location, but a user can only see and use one GAL. For more information about address lists, see [Address lists in Exchange Online](address-lists.md).

The same settings to configure a GAL are available as when you created the GAL. For more information, see [Create a global address list in Exchange Online](create-global-address-list.md). For additional GAL management tasks, see [Address list procedures in Exchange Online](address-list-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can't modify the GAL named Default Offline Address Book, the built-in GAL that's available in Exchange Online, and the only GAL that has the **IsDefaultGlobalAddressList** property value `True`.

- You can't replace a custom recipient filter with a precanned recipient filter or vice-versa in an existing GAL.

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For details about recipient filters in the Exchange Online PowerShell, see [Recipient filters for address lists in Exchange Online PowerShell](use-recipient-filters-to-create-an-address-list.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Online PowerShell to modify global address lists

To modify a GAL, use the following syntax:

```PowerShell
Set-GlobalAddressList -Identity <GALIdentity>] [-Name <Name>] [<Precanned recipient filter | Custom recipient filter>]
```

When you modify the precanned _Conditional_ parameter values, you can use the following syntax to add or remove values without affecting other existing values: `@{Add="<Value1>","<Value2>"...; Remove="<Value1>","<Value2>"...}`.

This example modifies the existing GAL named Contoso GAL by adding the **Company** value Fabrikam to the precanned recipient filter.

```PowerShell
Set-GlobalAddressList -Identity "Contoso GAL" -ConditionalCompany @{Add="Fabrikam"}
```

For detailed syntax and parameter information, see [Set-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/set-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully modified a GAL, replace _\<GAL Name\>_ with the name of the GAL and run the following command in Exchange Online PowerShell to verify the property values:

```PowerShell
Get-GlobalAddressList -Identity "<GAL Name>" | Format-List Name,RecipientFilterType,RecipientFilter,IncludedRecipients,Conditional*
```
