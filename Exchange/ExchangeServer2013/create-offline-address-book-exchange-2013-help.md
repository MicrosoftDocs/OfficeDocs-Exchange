---
title: 'Create an offline address book: Exchange 2013 Help'
TOCTitle: Create an offline address book
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.custom:
- 'Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.Mailbox.NewOabWizardForm.OabIntroductionWizardPage'
ms.assetid: b57bb4ce-5b6e-4702-a2f8-04bf3898a861
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
---

# Create an offline address book in Exchange 2013

_**Applies to:** Exchange Server 2013_

An offline address book (OAB) in Exchange Server 2013 is a downloaded copy of an address book that allows an Outlook user to access the information while disconnected from the server. Exchange administrators can decide which address books are made available to users who work offline, and they can also configure the method by which the address books are distributed (web-based distribution or public folder distribution).

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to create an OAB with web-based distribution

This example creates an OAB named OAB_Contoso that uses web-based distribution for Outlook 2007 or later clients by using the default virtual directory.

```powershell
New-OfflineAddressBook -Name "OAB_Contoso" -AddressLists "\Default Global Address List" -VirtualDirectories $Null -GlobalWebDistributionEnabled $True
```

For detailed syntax and parameter information, see [New-OfflineAddressBook](https://docs.microsoft.com/powershell/module/exchange/new-offlineaddressbook).
