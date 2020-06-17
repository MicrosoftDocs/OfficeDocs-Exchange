---
title: 'Change the default offline address book: Exchange 2013 Help'
TOCTitle: Change the default offline address book
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 61abf78e-2543-4431-acc8-839e3c7a4548
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change the default offline address book in Exchange 2013

_**Applies to:** Exchange Server 2013_

By default, when you install the Mailbox server role, a Web-based default offline address book (OAB) named Default Offline Address Book is created. You can set any OAB in your Exchange organization as the default OAB. This new default OAB is associated with all newly created mailbox databases. You can have only one default OAB in your organization. If you delete the default OAB, Microsoft Exchange doesn't automatically assign another OAB as the default. You must manually designate another OAB as the default.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to change the default OAB

This example sets the OAB named My OAB as the default OAB.

```powershell
Set-OfflineAddressBook -Identity "My OAB" -IsDefault $true
```

For detailed syntax and parameter information, see [Set-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/set-offlineaddressbook).
