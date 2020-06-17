---
title: 'Create an address list by using recipient filters: Exchange 2013 Help'
TOCTitle: Create an address list by using recipient filters
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 8eabea64-97c6-40af-b61c-9b6a125cbdf1
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an address list by using recipient filters in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic explains how to create an address list by using recipient filters. To learn more about address lists, see [Address lists](address-lists-exchange-2013-help.md).

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- To use the _RecipientFilter_ parameter to create a custom filter, you must specify a string for the filter. The Shell uses OPATH for the filtering syntax. OPATH is a querying language designed to query object data sources.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to create an address list by using recipient filters

This example creates an address list for all users with Exchange mailboxes who reside in Washington or Oregon.

```powershell
New-AddressList -Name "Pacific Northwest Mailboxes" -RecipientFilter "((RecipientType -eq 'UserMailbox') -and ((StateOrProvince -eq 'Washington') -or (StateOrProvince -eq 'Oregon')))"
```

This example creates an address list for all users with Exchange mailboxes who have `AgencyB` as the value for the _CustomAttribute15_ parameter.

```powershell
New-AddressList -Name "AgencyB" -RecipientFilter "(RecipientType -eq 'UserMailbox') -and (CustomAttribute15 -like *AgencyB*)"
```

For detailed syntax and parameter information, see [New-AddressList](https://docs.microsoft.com/powershell/module/exchange/new-addresslist).
