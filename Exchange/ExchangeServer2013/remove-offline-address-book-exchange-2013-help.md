---
title: 'Remove an offline address book: Exchange 2013 Help'
TOCTitle: Remove an offline address book
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: d69f1e8a-b3cb-4739-90cd-85ea450d06f3
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an offline address book in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic explains how to remove an offline address book (OAB).

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- After you remove an OAB that's linked to a user or to a mailbox database, the recipient will download the default OAB until you assign a new OAB for that user. If you remove the default OAB, you must assign a different OAB as the default OAB. For instructions about how to change the default OAB, see [Change the default offline address book](change-default-offline-address-book-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to remove an OAB

This example removes an OAB named My OAB.

```powershell
Remove-OfflineAddressBook -Identity "My OAB"
```

Type Y to confirm that you want to remove the OAB, and then press ENTER.

For detailed syntax and parameter information, see [Remove-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/remove-offlineaddressbook).
