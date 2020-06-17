---
localization_priority: Normal
description: 
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8df985e9-75ba-47ea-9cc3-aa98a5d8acf4
ms.reviewer: 
title: Configure offline address book distribution properties
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Servers.ClientAccess.OabDistributionGeneralPage
manager: serdars
ROBOTS: NOINDEX, NOFOLLOW

---

# Configure offline address book distribution properties

For each offline address book (OAB) distribution point in Exchange, you can configure two URLs: an internal URL that can be accessed only from your internal corporate network and an external URL that can be accessed from the internet.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure. In Exchange Online, this feature is available only in the Address Lists role, and by default, the role isn't assigned to any role groups. To use this cmdlet, you need to add the Address Lists role to a role group (for example, to the Organization Management role group). For more information, see [Modify a role group in Exchange Online](https://docs.microsoft.com/exchange/permissions-exo/role-groups#modify-role-groups).

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use Exchange Online PowerShell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to configure OAB distribution properties

This example sets the polling interval for OAB distribution on the OAB virtual directory OAB (Default Web Site) to six hours.

```PowerShell
Set-OABVirtualDirectory "OAB (Default Web Site)" -PollInterval 360
```

This example sets the external distribution point to https://contoso.com/OAB for the default OAB virtual directory OAB (Default Web Site).

```PowerShell
Set-OABVirtualDirectory "OAB (Default Web Site)" -ExternalUrl https://contoso.com/OAB
```

For detailed syntax and parameter information, see [set-OabVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/set-oabvirtualdirectory).

## For More Information

[Offline address books in Exchange Online](offline-address-books.md)
