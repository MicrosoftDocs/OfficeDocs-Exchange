---
title: 'Update a global address list: Exchange 2013 Help'
TOCTitle: Update a global address list
ms:assetid: 236e8530-62dd-4c43-8a5d-8465623252e6
ms:mtpsurl: https://technet.microsoft.com/library/Bb266966(v=EXCHG.150)
ms:contentKeyID: 49289195
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Update a global address list

_**Applies to:** Exchange Server 2013_

You can use the Shell to update a global address list (GAL). A GAL is a directory that contains entries for every group, user, and contact within an organization's implementation of Microsoft Exchange.

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- By default in Exchange Online, the Address List role isn't assigned to any role groups. To use any cmdlets that require the Address List role, you need to add the role to a role group. For more information, see the "Add a role to a role group" section in the topic, [Manage role groups](manage-role-groups-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to update a GAL

This example updates a GAL for the Fourth Coffee company.

> [!NOTE]
> Running this command only starts the update process. It may take several hours for the GAL to be updated.

```powershell
Update-GlobalAddressList -Identity "Fourth Coffee"
```

For detailed syntax and parameter information, see [Update-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/Update-GlobalAddressList).
