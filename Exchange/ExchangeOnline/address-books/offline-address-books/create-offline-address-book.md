---
localization_priority: Normal
description: Admins can learn how to create offline address books (OABs) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b57bb4ce-5b6e-4702-a2f8-04bf3898a861
ms.reviewer: 
title: Create an offline address book
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.Mailbox.NewOabWizardForm.OabIntroductionWizardPage
manager: serdars

---

# Create an offline address book

An offline address book (OAB) is a downloadable address list collection that Outlook users can access while disconnected from Exchange Online. An OAB allows Outlook users to access the information within the specified address lists while disconnected from Exchange Online. Admins can decide which address lists are made available to users who work offline.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- By default, the Address List role isn't assigned to any role groups in Exchange Online. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see [Modify role groups](../../permissions-exo/role-groups.md#modify-role-groups).

- You can only use Exchange Online PowerShell to perform the procedures in this topic. To connect to Exchange Online PowerShell, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to create an OAB with web-based distribution

This example creates an OAB named OAB_Contoso that contains the default global address list.

```PowerShell
New-OfflineAddressBook -Name "OAB_Contoso" -AddressLists "\Default Global Address List"
```

For detailed syntax and parameter information, see [New-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/new-offlineaddressbook).
