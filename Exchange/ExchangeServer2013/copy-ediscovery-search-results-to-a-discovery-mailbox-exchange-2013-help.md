---
title: 'Copy eDiscovery search results to a discovery mailbox: Exchange 2013 Help'
TOCTitle: Copy eDiscovery search results to a discovery mailbox
ms:assetid: bff2ce89-9e6f-494a-bd6a-2f2011507845
ms:mtpsurl: https://technet.microsoft.com/library/Dn624163(v=EXCHG.150)
ms:contentKeyID: 61200239
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Copy eDiscovery search results to a discovery mailbox

_**Applies to:** Exchange Server 2013_

After you create an In-Place eDiscovery search, you can use the EAC to copy the results to a discovery mailbox. You can also use the Shell to start an eDiscovery search that was created using the **New-MailboxSearch** cmdlet, which will copy the results to the discovery mailbox that was specified when you created the search.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes or longer depending on the number of mailbox items returned in the search results

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- An eDiscovery search has to be created, by using the EAC or the Shell, before you can copy the search results. For details, see [Create an In-Place eDiscovery search](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search).

- Exchange 2013 Setup creates a discovery mailbox called **Discovery Search Mailbox** to copy search results. You can create additional discovery mailboxes. For details, see [Create a discovery mailbox](https://docs.microsoft.com/Office365/SecurityCompliance/eop/exchange-online-protection-overview).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to copy search results

1. In the EAC, go to **Compliance management** \> **In-place eDiscovery & hold**.

2. In the list view, select an eDiscovery search.

3. Click **Search** ![Search icon](images/Dn624163.773574d0-9b92-4cab-9f6b-81532c7418b9(EXCHG.150).gif "Search icon"), and then click **Copy search results** from the drop-down list.

4. In **Copy Search Results**, select from the following options:

   - **Include unsearchable items**: Select this check box to include mailbox items that couldn't be searched (for example, messages with attachments of file types that couldn't be indexed by Exchange Search). For more information, see [Unsearchable items in Exchange eDiscovery](unsearchable-items-in-exchange-ediscovery-exchange-2013-help.md).

   - **Enable de-duplication**: Select this check box to exclude duplicate messages. Only a single instance of a message will be copied to the discovery mailbox.

   - **Enable full logging**: Select this check box to include a full log in search results.

   - **Send me mail when the copy is completed**: Select this check box to get an email notification when the search is completed.

   - **Copy results to this discovery mailbox**: Click **Browse** to select the discovery mailbox where you want the search results copied to.

        ![Copy Search Results](images/Dn624163.875e25ed-8308-408c-92c4-8c76fc9d9bfc(EXCHG.150).gif "Copy Search Results")

5. Click **Copy** to start the process to copy the search results to the specified discovery mailbox.

6. Click **Refresh** ![Refresh Icon](images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif "Refresh Icon") to update the information about the copying status that is displayed in the details pane.

7. When copying is complete, click **Open** to open the discovery mailbox to view the search results.

## Use the Shell to copy search results

After using the **New-MailboxSearch** cmdlet to create an In-Place eDiscovery search, you must start the search to copy messages to the discovery mailbox you specified in the *TargetMailbox* parameter. For information about creating eDiscovery searches using the Shell, see:

- [Use the Shell to create an In-Place eDiscovery search](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search)

- [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/New-MailboxSearch)

For example, you would run the following command to start an eDiscovery search named *Fabrikam Investigation* to copy the search results to the specified discovery mailbox.

```powershell
Start-MailboxSearch "Fabrikam Investigation"
```

If you used the *EstimateOnly* switch to get an estimate of the search results, you have to remove the switch before you can copy the search results. You also have to specify a discovery mailbox to copy to search results to. For example, say you created an estimate-only search by using the following command:

```powershell
New-MailboxSearch "FY13 Q2 Financial Results" -StartDate "04/01/2013" -EndDate "06/30/2013" -SourceMailboxes "DG-Finance" -SearchQuery '"Financial" AND "Fabrikam"' -EstimateOnly -IncludeUnsearchableItems
```

To copy the results of this search to a discovery mailbox, you would run the following commands:

```powershell
Set-MailboxSearch "FY13 Q2 Financial Results" -EstimateOnly $false -TargetMailbox "Discovery Search Mailbox"
```

```powershell
Start-MailboxSearch "FY13 Q2 Financial Results"
```

## More information about copying search results

- After you copy search results to the discovery mailbox, you can export those search results to a PST file. For more information, see [Export eDiscovery search results to a PST file](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/export-search-results).

- For more information about unsearchable items, see [Unsearchable items in Exchange eDiscovery](unsearchable-items-in-exchange-ediscovery-exchange-2013-help.md).

- If you're copying all mailbox content within a specific date range (by not specifying any keywords in the search criteria), then all unsearchable items within that date range will be automatically included in the search results. Therefore, don't select the **Include unsearchable items** checkbox when copying search results. Otherwise, a duplicate copy of all unsearchable items will be copied to the discovery mailbox.

- In addition to copying the search results to a discovery mailbox, you can also estimate or preview the search results for a selected search.

  - **Estimate search results**: This option returns an estimate of the total size and number of items that will be returned by the search based on the criteria you specified. Estimates are displayed in the details pane in the EAC.

  - **Preview search results**: This option lets you preview the search results returned by the search instead of having to copy them to a discovery mailbox to view. This lets you quickly determine whether the search results are relevant. After you preview the results, you can revise your search query to narrow the search results and rerun the search. Items in the preview page are read-only versions of the actual search results, so you can't move, edit, delete or forward on the preview page.

    For more information, see [Estimate or preview search results](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/create-in-place-ediscovery-search).
