---
title: 'Add an address list to or remove an address list from an offline address book: Exchange 2013 Help'
TOCTitle: Add an address list to or remove an address list from an offline address book
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 86bd5651-ad41-4516-bf23-6579f4e4da03
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add an address list to or remove an address list from an offline address book in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Shell to add or remove an address list from an offline address book (OAB). By default, there is an OAB named the Default Offline Address Book that contains the global address list (GAL). OABs are generated based on the address lists that they contain. To create custom OABs that users can download, you can add or remove address lists from OABs.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- Changes to the address list aren't available for client download until after the OAB in which the address list resides has been generated. For more information, see [Update offline address book](update-offline-address-book-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to add an address list to an OAB

When using the _AddressLists_ parameter, any address lists that currently exist will be overwritten. You must include existing address lists when you use the _AddressLists_ parameter to continue to generate those address lists in your OAB. This example, in which you have AddressList1 and AddressList2, adds AddressList3.

```powershell
Set-OfflineAddressBook -Identity "My OAB" -AddressLists AddressList1,AddressList2,AddressList3
```

For detailed syntax and parameter information, see [Set-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/set-offlineaddressbook).

## Use the Shell to remove an address list from an OAB

To remove an address list from an OAB, simply omit that address list from the list of address lists. This example, in which you have AddressList1, AddressList2, and AddressList3, removes AddressList3.

```powershell
Set-OfflineAddressBook -Identity "My OAB" -AddressLists AddressList1,AddressList2
```

For detailed syntax and parameter information, see [Set-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/set-offlineaddressbook).
