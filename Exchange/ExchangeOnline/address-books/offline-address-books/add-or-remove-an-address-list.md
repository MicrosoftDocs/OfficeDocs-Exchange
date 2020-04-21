---
localization_priority: Normal
description: Admins can learn how to add or remove address lists in offline address books (OABs) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 86bd5651-ad41-4516-bf23-6579f4e4da03
ms.reviewer: 
title: Add an address list to or remove an address list from an offline address book in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Add an address list to or remove an address list from an offline address book in Exchange Online

You can use Exchange Online PowerShell to add or remove an address list from an offline address book (OAB). By default, there is an OAB named the Default Offline Address Book that contains the global address list (GAL). OABs are generated based on the address lists that they contain. To create custom OABs that users can download, you can add or remove address lists from OABs.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- Changes to the address list aren't available for client download until after the OAB in which the address list resides has been generated.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/exchange-online/connect-to-exchange-online-powershell/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to add and remove address lists from offline address books

When you modify the address lists that are configured in an OAB, the values that you specify will *replace* any address lists in the OAB. To add address lists to the OAB, specify the current address lists plus the ones you want to add. To remove address lists from the OAB, specify the current address lists minus the ones you want to remove.

In this example, the OAB named Marketing OAB is already configured with Address List 1 and Address List 2. To keeps those address lists and add Address List 3, run the following command:

```PowerShell
Set-OfflineAddressBook -Identity "Marketing OAB" -AddressLists "Address List1","Address List 2","Address List 3"
```

Similarly, to keep the OAB configured with Address List 1 and Address 2, but remove Address List 3, run the following command:

```PowerShell
Set-OfflineAddressBook -Identity "Marketing OAB" -AddressLists "Address List 1","Address List 2"
```

For detailed syntax and parameter information, see [Set-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/email-addresses-and-address-books/set-offlineaddressbook).

## How do you know this worked?

To verify that you've successfully added or removed address lists from an OAB, run the following command to verify the property `AddressLists` property values:

```PowerShell
Get-OfflineAddressBook | Format-List Name,AddressLists
```
