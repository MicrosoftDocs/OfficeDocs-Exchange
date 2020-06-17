---
title: 'Modify an In-Place eDiscovery search: Exchange 2013 Help'
TOCTitle: Modify an In-Place eDiscovery search
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 3162743c-cc12-4997-91e0-bcbfea8bcb17
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Modify an In-Place eDiscovery search in Exchange 2013

_**Applies to:** Exchange Server 2013_

After you create an In-Place eDiscovery search, you can modify it to change the search parameters. For example, you can change the mailboxes to be searched, date ranges, key words, logging options, or you can specify a different Discovery mailbox to store search results. Any changes you make to the search properties will be used when you restart the search.

> [!CAUTION]
> If an In-Place eDiscovery search is running, you must stop it before modifying it. When you restart the search, the results from the last time the search was run are removed from the Discovery mailbox. However, the logs from previous searches are saved.

## What do you need to know before you begin?

- Estimated time to complete: 2-5 minutes.

- An In-Place eDiscovery search has been created and isn't running.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to modify an In-Place eDiscovery search

1. Navigate to **Compliance management** \> **In-place eDiscovery & hold**.

2. In the list view, select the In-Place eDiscovery search you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. In **In-Place eDiscovery & Hold**, you can modify the following settings:

   - On **Name** page, modify the name for the search and the optional description.

   - On the **Mailboxes** page, modify the mailboxes to search. You can search across all mailboxes or select specific ones to search.

     > [!IMPORTANT]
     > You can't use the **Search all mailboxes** option to place all mailboxes on Exchange 2013 Mailbox servers on hold. To create an In-Place Hold, you must select **Specify mailboxes to search**. For more details, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md).

   - On the **Search query** page, modify the following fields:

   - **Include all user mailbox content** Select this option to place all content in the selected mailboxes on hold.

   - **Filter based on criteria** Select this option to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types.

   - On the **In-Place Hold** page, select the **Place content matching the search query in selected mailboxes on hold** check box, and then select one of the following options to place items on In-Place Hold:

   - **Hold indefinitely** Select this option to place the returned items on an indefinite hold. Items on hold will be preserved until you remove the mailbox from the search or remove the search.

   - **Specify number of days to hold items relative to their received date** Use this option to hold items for a specific period. For example, you can use this option if your organization requires that all messages be retained for at least seven years. You can use a time-based In-Place Hold along with a retention policy to make sure items are deleted in seven years.

     > [!IMPORTANT]
     > When placing mailboxes or items on In-Place Hold for legal purposes, it is generally recommended to hold items indefinitely and remove the hold when the case or investigation is completed.

4. Click **Save**.

## Use the Shell to modify an In-Place eDiscovery search

This example modifies the In-Place eDiscovery search Search-Project Contoso to search mailboxes belonging to members of the DG-ProjectManagers distribution group.

```powershell
Set-MailboxSearch -Identity "Search-Project Contoso" -SourceMailboxes "DG-ProjectManagers"
```

## How do you know this worked?

To verify that you have successfully modified an In-Place eDiscovery search, do one of the following:

- Use the EAC to check properties of the search.

- Use the **Get-MailboxSearch** cmdlet from the Shell to check the properties of the search. For examples of how to check the properties of a mailbox search, see the "Examples" section in [Get-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/get-mailboxsearch).
