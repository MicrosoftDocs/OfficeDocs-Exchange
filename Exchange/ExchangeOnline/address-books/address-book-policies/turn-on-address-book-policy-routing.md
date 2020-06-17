---
localization_priority: Normal
description: Admins can learn how to turn on address book policy routing in Exchange Online to enable virtual organizations within an organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5627b8ac-0551-4558-b3b6-25c402698426
ms.reviewer: 
title: Turn on address book policy routing in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Turn on address book policy routing in Exchange Online

Address book policies (ABPs) allow you to segment users into specific groups to give them customized global address lists (GALs) in Outlook and Outlook on the web (formerly known as Outlook Web App). For more information about ABPs, see [Address book policies in Exchange Online](address-book-policies.md).

ABP routing creates the virtual organizations within a single Exchange Online organization. Your virtual organization is determined by the global address list (GAL) you reside in. When ABP routing is turned on, users that are assigned to different GALs appear as external recipients and won't be able to view each other's contact cards.

In Exchange Online, you can only turn on ABP routing in Exchange Online PowerShell.

Looking for the Exchange Server version of this topic? See [Use the Exchange Management Shell to install and configure the Address Book Policy Routing Agent](https://docs.microsoft.com/Exchange/email-addresses-and-address-books/address-book-policies/abp-procedures#use-the-exchange-management-shell-to-install-and-configure-the-address-book-policy-routing-agent).

## What do you need to know before you begin?

- You need to be a member of the Organization Management role group in Exchange Online (or a global administrator) before you can perform the procedure in this topic.

- To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to turn on ABP routing

To enable ABP routing in the Exchange Online organization, run the following command:

```PowerShell
Set-TransportConfig -AddressBookPolicyRoutingEnabled $true
```

For detailed syntax and parameter information, see [Set-TransportConfig](https://docs.microsoft.com/powershell/module/exchange/set-transportconfig).

### How do you know this worked?

To verify that you've successfully turned on ABP routing, use any of the following steps:

- In Exchange Online PowerShell, run the following command to verify that ABP routing is enabled for the organization:

   ```PowerShell
   Get-TransportConfig | Format-List AddressBookPolicyRoutingEnabled
   ```

- Have a user that's assigned an ABP send an email message to an user that's assigned a different ABP, and verify that the sender's email address doesn't resolve to their display name.
