---
localization_priority: Normal
description: Learn how to remove address book policies (ABPs) from Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c20c6f82-2f75-4116-9be1-c5af10113f71
ms.reviewer:
title: Remove an address book policy in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Remove an address book policy

Address book policies (ABPs) allow you to segment users into specific groups to give them customized global address lists (GALs) in Outlook and Outlook on the web (formerly known as Outlook Web App). For more information about ABPs, see [Address book policies in Exchange Online](address-book-policies.md).

You can only remove ABPs from your Exchange Online organization using Exchange Online PowerShell, and only if the ABP isn't assigned to a mailbox (active mailboxes or soft-deleted mailboxes that are still recoverable).

## What do you need to know before you begin?

- Estimated time to complete: Less than 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets or features that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to remove an ABP

### Step 1: Verify the ABP isn't assigned to a mailbox

1. Replace \<ABPName\> with the name of the ABP, and run the following command to get the **DistinguishedName** (DN) value of the ABP that you want to remove:

   ```PowerShell
   Get-AddressBookPolicy -Identity "<ABPName>" | Format-List DistinguishedName
   ```

2. To see if the ABP is assigned to an active mailbox, replace \<ABPDistinguishedName\> with the DN of the ABP and run the following command:

   ```PowerShell
   Get-Mailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<ABPDistinguishedName>'"
   ```

   To remove the ABP assignment from any active mailboxes that you find, replace \<ABPDistinguishedName\> with the DN of the ABP and run the following commands:

   ```PowerShell
   $a = Get-Mailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<ABPDistinguishedName>'"
   ```

   ```PowerShell
   $a | foreach {Set-Mailbox -Identity $_.MicrosoftOnlineServicesID -AddressBookPolicy $null}
   ```

3. To see if the ABP is assigned to a soft-deleted (recoverable) mailbox, replace \<ABPDistinguishedName\> with the DN of the ABP and run the following command:

   ```PowerShell
   Get-Mailbox -SoftDeletedMailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<ABPDistinguishedName>'"
   ```

   To remove the ABP assignment from any soft-deleted mailboxes that you find, replace \<ABPDistinguishedName\> with the DN of the ABP and run the following commands:

   ```PowerShell
   $s = Get-Mailbox -SoftDeletedMailbox -ResultSize unlimited -Filter "AddressBookPolicy -eq '<ABPDistinguishedName>'"
   ```

   ```PowerShell
   $s | foreach {Set-Mailbox -Identity $_.MicrosoftOnlineServicesID -AddressBookPolicy $null}
   ```

**Note**: If you don't assign an ABP to a mailbox, the GAL for your entire organization will be visible to the user in Outlook and Outlook on the web. Instead of using the value `$null`, you can specify the name of a different ABP (enclosed in quotation marks if the name contains spaces).

### Step 2: Remove the ABP

To remove an ABP, use this syntax:

```PowerShell
Remove-AddressBookPolicy -Identity <ABPIdentity>
```

This example removes the ABP named ABP TailspinToys.

```PowerShell
Remove-AddressBookPolicy -Identity "ABP TailspinToys"
```

For detailed syntax and parameter information, see [Remove-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-addressbookpolicy).

## How do you know this worked?

To verify that you've successfully removed an ABP, use either of these procedures in Exchange Online PowerShell:

- Run the following command to verify that the ABP isn't listed:

  ```PowerShell
  Get-AddressBookPolicy
  ```

- Replace _\<ABPName\>_ with the name of the ABP, and run the following command to confirm that an error is returned:

  ```PowerShell
  Get-AddressBookPolicy -Identity "<ABPName>"
  ```
