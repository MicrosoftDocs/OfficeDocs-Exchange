---
localization_priority: Normal
description: Admins can learn how to specify the default offline address book (OAB) in Exchange Online
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 61abf78e-2543-4431-acc8-839e3c7a4548
ms.reviewer: 
title: Change the default offline address book in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Change the default offline address book in Exchange Online

By default, the automatically-created OAB named Default Offline Address Book is the default OAB. You can set any OAB in your Exchange Online organization as the default OAB. The default OAB is used by:

- Mailboxes without an address book policy (ABP) assigned, or where the assigned ABP policy has no OAB defined (by default, there are no ABPs).

- Mailboxes without an OAB assigned (by default, all mailboxes).

If you delete the default OAB, Exchange Online doesn't automatically assign another OAB as the default. You need to manually designate another OAB as the default.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to change the default OAB

This example sets the OAB named My OAB as the default OAB.

```PowerShell
Set-OfflineAddressBook -Identity "My OAB" -IsDefault $true
```

For detailed syntax and parameter information, see [Set-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/set-offlineaddressbook).

## How do you know this worked?

To verify that you've successfully changed the default OAB, run the following command to verify the `IsDefault` property value:

```PowerShell
Get-OfflineAddressBook | Format-List Name,IsDefault
```
