---
title: 'Remove a global address list: Exchange 2013 Help'
TOCTitle: Remove a global address list
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 65d75b69-641b-4a37-a63c-47cf018f5f22
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a global address list in Exchange 2013

_**Applies to:** Exchange Server 2013_

The global address list (GAL) is a directory that contains entries for every group, user, and contact within an Exchange organization.

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't remove the default GAL.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to remove a GAL

This example removes the GAL Fourth Coffee from the domain controller ad-server.fourthcoffee.com.

```powershell
Remove-GlobalAddressList -Identity "Fourth Coffee" -DomainController ad-server.fourthcoffee.com
```

To confirm that you want to remove the GAL, type Y, and then press ENTER.

For detailed syntax and parameter information, see [Remove-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/remove-globaladdresslist).
