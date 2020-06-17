---
title: 'Remove an address book policy: Exchange 2013 Help'
TOCTitle: Remove an address book policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c20c6f82-2f75-4116-9be1-c5af10113f71
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an address book policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

Use this procedure to remove an address book policy (ABP).

## What do you need to know before you begin?

- Estimated time to complete: Less than 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address book policies" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't remove an ABP if it's assigned to a user's mailbox or to a soft-deleted mailbox. To determine if an ABP is assigned to a user, run the following Shell command:

  ```powershell
  Get-Mailbox | Where $._AddressBookPolicy -eq <AddressBookPolicyName>
  ```

  To determine if an ABP is assigned to a soft-deleted mailbox, run the following command:

  ```powershell
  Get-Mailbox -SoftDeletedMailbox | Where $._AddressBookPolicy -eq <AddressBookPolicyName>
  ```

- To remove an ABP from a user's mailbox, you can use the **Mailbox features** page of the mailbox's properties or the **Set-Mailbox** cmdlet.

- You can't use the Exchange admin center (EAC) to remove an ABP. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to remove an ABP

This example removes the ABP ABP_TailspinToys.

```powershell
Remove-AddressBookPolicy -Identity "ABP_TailspinToys"
```

For detailed syntax and parameter information, see [Remove-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-addressbookpolicy).
