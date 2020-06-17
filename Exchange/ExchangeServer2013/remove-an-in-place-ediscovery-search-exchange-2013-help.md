---
title: 'Remove an In-Place eDiscovery search: Exchange 2013 Help'
TOCTitle: Remove an In-Place eDiscovery search
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 78461a78-1255-4a26-9d36-c6b8eb82a4f9
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an In-Place eDiscovery search in Exchange 2013

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can use In-Place eDiscovery to search mailbox content. You can remove an In-Place eDiscovery search at any time. When you remove an In-Place eDiscovery search, search results are removed from the Discovery mailbox.

> [!CAUTION]
> Deleting an In-Place eDiscovery search will remove any search results copied to a Discovery mailbox.

## What do you need to know before you begin?

- Estimated time to completion: 2-5 minutes.

- To remove an In-Place eDiscovery search that has In-Place Hold enabled, you must first remove the In-Place Hold from the search. For details, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to remove an In-Place eDiscovery search

1. Navigate to **Compliance management** \> **In-place eDiscovery & hold**.

2. In the list view, select the In-Place eDiscovery search you want to remove, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

## Use the Shell to remove an In-Place eDiscovery search

For an example of how to remove an In-Place eDiscovery search, see the "Examples" section in [Remove-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxsearch).

## How do you know this worked?

To verify that you have successfully removed an In-Place eDiscovery search, do one of the following:

- Use the EAC to verify that the search is no longer displayed in the list view of the **In-place eDiscovery & hold** tab.

- Use the **Get-MailboxSearch** cmdlet to retrieve In-Place eDiscovery searches. For an example of how to retrieve In-Place eDiscovery searches, see the "Examples" section in [Get-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/get-mailboxsearch).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
