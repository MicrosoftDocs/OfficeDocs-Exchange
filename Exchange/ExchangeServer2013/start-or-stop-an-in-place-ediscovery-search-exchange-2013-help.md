---
title: 'Start or stop an In-Place eDiscovery search: Exchange 2013 Help'
TOCTitle: Start or stop an In-Place eDiscovery search
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 0d546763-4bf5-4523-91f4-d181b7ee4ac2
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Start or stop an In-Place eDiscovery search in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can stop or restart an In-Place eDiscovery search at any time. For example, if you want to modify search properties such as keywords or mailboxes searched, you must first stop a search. You can then restart the search after making the required changes.

> [!CAUTION]
> If you restart an In-Place eDiscovery search, search results copied to the Discovery mailbox specified in the search are removed.

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to start or stop an In-Place eDiscovery search

1. Navigate to **Compliance management** \> **In-place eDiscovery & hold**.

2. To stop a search that's in progress, select the search, and then click **Stop search**.

3. To start a search that was stopped, select the search, and then click **Resume search**.

## Use the Shell to start or stop an In-Place eDiscovery search

For an example of how to start an In-Place eDiscovery search, see "Example 1" in [Start-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/start-mailboxsearch).

For an example of how to stop an In-Place eDiscovery search, see "Example 1" in [Stop-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/stop-mailboxsearch).
