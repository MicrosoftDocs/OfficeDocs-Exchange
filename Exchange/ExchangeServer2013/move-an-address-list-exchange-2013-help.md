---
title: 'Move an address list: Exchange 2013 Help'
TOCTitle: Move an address list
ms:assetid: c843bbd5-6c0e-41e1-b749-7ae87c1beb25
ms:mtpsurl: https://technet.microsoft.com/library/Bb124534(v=EXCHG.150)
ms:contentKeyID: 49289405
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Move an address list

_**Applies to:** Exchange Server 2013_

This topic explains how to move an existing address list to a new container under the root address list.

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address Lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to move an address list

This example uses the address list's GUID to move the address list to the Building 4 container, which is located in the All Users\\Sales container.

```powershell
Move-AddressList -Identity c3fffd8e-026b-41b9-88c4-8c21697ac8ac -Target "\All Users\Sales\Building4"
```

Type **Y** to confirm that you want to move this address list, and then press ENTER.

For detailed syntax and parameter information, see [Move-AddressList](https://docs.microsoft.com/powershell/module/exchange/Move-AddressList).
