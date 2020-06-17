---
title: 'Configure global address list properties: Exchange 2013 Help'
TOCTitle: Configure global address list properties
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 5fd2c96f-fe93-4b5a-8495-70c450511a37
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure global address list properties in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic explains how to modify the settings of a global address list (GAL).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't edit the settings of the default GAL.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to configure GAL properties

This example assigns a new name, FourthCoffee, to the GAL that has the GUID 96d0c505-eba8-4103-ad4f-577a1bf4ad7b.

```powershell
Set-GlobalAddressList -Identity 96d0c505-eba8-4103-ad4f-577a1bf4ad7b -Name FourthCoffee
```

> [!NOTE]
> If you're using precanned conditional filter properties, the value for the _IncludedRecipients_ parameter can't be blank.

This example changes the recipients who will be included in the Fourth Coffee global GAL to those whose company is set to Fourth Coffee.

```powershell
Set-GlobalAddressList -Identity Fourth Coffee -RecipientFilter "Company -eq 'Fourth Coffee'"
```

For detailed syntax and parameter information, see [Set-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/set-globaladdresslist).
