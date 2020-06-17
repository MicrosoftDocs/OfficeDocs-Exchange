---
localization_priority: Normal
description: Admins can learn how to remove offline address book (OAB) from Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d69f1e8a-b3cb-4739-90cd-85ea450d06f3
ms.reviewer: 
title: 'Remove an offline address book from Exchange '
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Remove an offline address book

This topic explains how to remove an offline address book (OAB) from Exchange Online. If you remove the default OAB, you must assign a different OAB as the default OAB. For instructions about how to change the default OAB, see [Change the default offline address book](change-default-offline-address-book.md).

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to remove an OAB

This example removes an OAB named My OAB.

```PowerShell
Remove-OfflineAddressBook -Identity "My OAB"
```

For detailed syntax and parameter information, see [Remove-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/remove-offlineaddressbook).


## How do you know this worked?

To verify that you've successfully removed the OAB, run the following command to verify that the OAB is gone.

```PowerShell
Get-OfflineAddressBook
```
