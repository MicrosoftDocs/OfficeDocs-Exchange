---
title: 'Remove an address list: Exchange 2013 Help'
TOCTitle: Remove an address list
ms:assetid: 39a313f3-41d4-4c8f-af67-df2316f3687f
ms:mtpsurl: https://technet.microsoft.com/library/Aa997294(v=EXCHG.150)
ms:contentKeyID: 49289233
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an address list

_**Applies to:** Exchange Server 2013_

This topic explains how to remove an address list. You can't remove the default global address list (GAL).

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address list" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't remove a parent address list that contains child address lists. However, you can remove both the child and parent address lists by pressing the CTRL key on the keyboard, and then selecting the parent and child address lists. If you attempt to remove a parent address list without removing the child address lists, you'll receive an error.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to remove an address list

1. Navigate to **Organization** \> **Address lists**.

2. In the list view, select the address list you want to remove, and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. In the warning, click **Yes** to remove the address list.

## Use the Shell to remove an address list

This example removes the address list Sales Department, which doesn't contain child address lists.

```powershell
Remove-AddressList -Identity "Sales Department"
```

Type **Y** to confirm that you want to remove this address list, and then press ENTER.

For detailed syntax and parameter information, see [Remove-AddressList](https://docs.microsoft.com/powershell/module/exchange/Remove-AddressList).

## Use the Shell to remove an address list that contains child address lists

This example removes the parent address list Departments and all its child address lists.

```powershell
Remove-AddressList -Identity Departments -Recursive
```

Type **Y** to confirm that you want to remove the parent address list and its child address lists, and then press ENTER.

For detailed syntax and parameter information, see [Remove-AddressList](https://docs.microsoft.com/powershell/module/exchange/Remove-AddressList).
