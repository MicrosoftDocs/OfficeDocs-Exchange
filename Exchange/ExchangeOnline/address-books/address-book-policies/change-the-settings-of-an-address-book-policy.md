---
localization_priority: Normal
description: Admins can learn how to modify address book policies (ABPs) in Exchange Online
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ba1ca350-71c2-4c60-a612-33bfa9320b5e
ms.reviewer:
title: Change the settings of an address book policy in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Change the settings of an address book policy

Address book policies (ABPs) allow you to segment users into specific groups to give them customized global address lists (GALs) in Outlook and Outlook on the web (formerly known as Outlook Web App). For more information about ABPs, see [Address book policies in Exchange Online](address-book-policies.md).

After you create an ABP, you can view or modify the name and the assigned address lists: the global address list (GAL), offline address book (OAB), room list, and other address lists.

In Exchange Online, you can only modify ABPs in Exchange Online PowerShell.

For additional management tasks related to ABPs, see [Address book policy procedures in Exchange Online](address-book-policy-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets or features that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to modify address book policies

To modify an ABP, use this syntax:

```PowerShell
Set-AddressBookPolicy -Identity "<ABPName>" [-Name "<Unique Name>"] [-GlobalAddressList "<GAL>"] [-OfflineAddressBook "<OAB>"] [-RoomList "<RoomList>"] [-AddressLists <AddressLists>]
```

- The _Name_, _GlobalAddressList_, _OfflineAddressBook_, and _RoomList_ parameters all take single values, so the value you specify replaces the existing value.

   This example modifies the ABP named "All Fabrikam ABP" by replacing the OAB with the specified OAB.

    ```PowerShell
    Set-AddressBookPolicy -Identity "All Fabrikam ABP" -OfflineAddressBook \Fabrikam-OAB-2
    ```

- The _AddressLists_ parameter takes multiple values, so you need to decide whether you want to *replace* the existing address lists in the ABP, or *add and remove* address lists without affecting the other address lists in the ABP.

   This example replaces the existing address lists in the ABP named Government Agency A with the specified address lists.

   ```PowerShell
   Set-AddressBookPolicy -Identity "Government Agency A" -AddressLists "GovernmentAgencyA-Atlanta","GovernmentAgencyA-Moscow"
   ```

   To add address lists to an ABP, you need to specify the new address lists *and* any existing address lists that you want to keep.

   This example adds the address list named Contoso-Chicago to the ABP named ABP Contoso, which is already configured to use the address list named Contoso-Seattle.

   ```PowerShell
   Set-AddressBookPolicy -Identity "ABP Contoso" -AddressLists "Contoso-Chicago","Contoso-Seattle"
   ```

   To remove address lists from an ABP, you need to specify the existing address lists that you want to keep, and omit the address lists that you want to remove.

   For example, the ABP named ABP Fabrikam uses the address lists named Fabrikam-HR and Fabrikam-Finance. To remove the Fabrikam-HR address list, specify only the Fabrikam-Finance address list.

   ```PowerShell
   Set-AddressBookPolicy -Identity "ABP Fabrikam" -AddressLists Fabrikam-Finance
   ```

For detailed syntax and parameter information, see [Set-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/set-addressbookpolicy).

### How do you know this worked?

To verify that you've successfully modify an ABP, replace _\<ABPName\>_ with the name of the ABP, and run the following command in Exchange Online PowerShell to verify the property values:

```PowerShell
Get-AddressBookPolicy -Identity "<ABPName>" | Format-List
```
