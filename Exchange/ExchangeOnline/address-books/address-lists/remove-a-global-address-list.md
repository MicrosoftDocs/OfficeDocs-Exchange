---
localization_priority: Normal
description: Admins can learn how to remove custom global address lists (GALs) from Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 65d75b69-641b-4a37-a63c-47cf018f5f22
ms.reviewer:
title: Remove a global address list in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Remove a global address list in Exchange Online

The built-in global address list (GAL) that's automatically created by Exchange Online includes every mail-enabled object in the organization. You can create additional GALs to separate users by organization or location, but a user can only see and use one GAL. For more information about address lists, see [Address lists in Exchange Online](address-lists.md).

You can use the procedures in this topic to remove any custom GALs that you've created. You can't remove:

- The GAL named Default Offline Address Book, which is the built-in GAL that's available in Exchange Online, and the only GAL that has the **IsDefaultGlobalAddressList** property value `True`.

- A GAL that's defined in an offline address book (OAB). For OAB procedures, see [Offline address book procedures](../offline-address-books/offline-address-book-procedures.md).

For additional GAL management tasks, see [Address list procedures in Exchange Online](address-list-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to remove a GAL

To remove a GAL, use the following syntax:

```PowerShell
Remove-GlobalAddressList -Identity <GALIdentity>
```

This example removes the address list named Agency A GAL.

```PowerShell
Remove-GlobalAddressList -Identity "Agency A GAL"
```

For detailed syntax and parameter information, see [Remove-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/remove-globaladdresslist).

#### How do you know this worked?

To verify that you've successfully removed a GAL, run the following command in Exchange Online PowerShell to verify that the GAL isn't listed:

```PowerShell
Get-GlobalAddressList
```
