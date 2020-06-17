---
title: 'Create an address book policy: Exchange 2013 Help'
TOCTitle: Create an address book policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 6359abaf-e6f6-4667-8c2b-3860728b39a9
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an address book policy in Exchange 2013

_**Applies to:** Exchange Server 2013_

Address book policies (ABPs) allow you to segment users into specific groups to provide customized views of your organization's global address list (GAL). When creating an ABP, you assign a GAL, an offline address book (OAB), a room list, and one or more address lists to the policy. You can then assign the ABP to mailbox users, providing them with access to a customized GAL in Outlook and Outlook Web App. The goal is to provide a simpler mechanism to accomplish GAL segmentation for on-premises organizations that require multiple GALs. To learn more about ABPs, see [Address book policies](address-book-policies-exchange-2013-help.md).

Interested in scenarios that use this procedure? See [Scenario: Deploying address book policies](scenario-deploying-address-book-policies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address book policies" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- Creating an ABP for an organization is a multi-step process that requires planning. For more information, see [Scenario: Deploying address book policies](scenario-deploying-address-book-policies-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to create ABPs. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to create an ABP

This example creates an ABP with the following settings:

- **Name**: All Fabrikam ABP

- **GAL**: All Fabrikam

- **OAB**: Fabrikam-All-OAB

- **Room list**: All Fabrikam Rooms

- **Address lists**: All Fabrikam, All Fabrikam Mailboxes, All Fabrikam DLs, and All Fabrikam Contacts

```powershell
New-AddressBookPolicy -Name "All Fabrikam ABP" -AddressLists "\All Fabrikam","\All Fabrikam Mailboxes","\All Fabrikam DLs","\All Fabrikam Contacts" -OfflineAddressBook \Fabrikam-All-OAB -GlobalAddressList "\All Fabrikam" -RoomList "\All Fabrikam Rooms"
```

For detailed syntax and parameter information, see [New-AddressBookPolicy](https://docs.microsoft.com/powershell/module/exchange/new-addressbookpolicy).

## For more information

[Assign an address book policy to mail users](assign-an-address-book-policy-to-mail-users-exchange-2013-help.md)
