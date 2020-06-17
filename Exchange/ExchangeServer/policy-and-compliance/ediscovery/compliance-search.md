---
localization_priority: Normal
description: 'Summary: Learn how to run a script to create an In-Place eDiscovery search that uses the list of source mailboxes and search query from a Compliance Search.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9f0f4a3d-de9a-4d8a-9172-2edf4288d766
ms.reviewer:
title: Use Compliance Search to search all mailboxes in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Use Compliance Search to search all mailboxes in Exchange Server

The Compliance Search feature in Exchange Server allows you to search all mailboxes in your organization. Unlike In-Place eDiscovery where you can search up to 10,000 mailboxes, there are no limits for the number of target mailboxes in a single search. For scenarios that require you to perform organization-wide searches, you can use the **New-ComplianceSearch** cmdlet to search all mailboxes. Then you can use the workflow features of In-Place eDiscovery to perform other eDiscovery-related tasks, such as placing mailboxes on hold and exporting search results. For example, let's say you have to search all mailboxes to identify specific custodians that are responsive to a legal case. You can use the **New-ComplianceSearch** cmdlet to search all mailboxes in your organization to identify those that are responsive to the case. Then you can use that list of custodian mailboxes as the source mailboxes for an In-Place eDiscovery. Using In-Place eDiscovery also allows you to put a hold on those source mailboxes, copy search results to a discovery mailbox, and export the search results.

This topic includes a script that you can run to create an In-Place eDiscovery search by using the list of source mailboxes and search query from a compliance search that is created by running the **New-ComplianceSearch** cmdlet.

## Step 1: Run the New-ComplianceSearch cmdlet to search all mailboxes

The first step is to use the Exchange Management Shell to create a compliance search that searches all mailboxes in your organization. There's no limit for the number of mailboxes for a single compliance search. Specify an appropriate keyword query (or a query for sensitive information types) so that the search returns only those source mailboxes that are relevant to your investigation. If necessary, refine the search query to narrow the scope of search results and source mailboxes that are returned.

> [!NOTE]
> If the source compliance search doesn't return any results, an In-Place eDiscovery won't be created when you run the script in Step 3. You may have to revise the search query and then rerun the compliance search to return search results.

Here's an example of using the **New-ComplianceSearch** cmdlet to search all mailboxes in your organization. The search query returns all messages sent between October 1, 2015 and October 31, 2015 and that contain the phrase "financial report" in the subject line. The first command creates the search, and the second command runs the search.

```PowerShell
New-ComplianceSearch -Name "Search All-Financial Report" -ExchangeLocation all -ContentMatchQuery 'sent>=01/01/2015 AND sent<=06/30/2015 AND subject:"financial report"'
```

```PowerShell
Start-ComplianceSearch -Identity "Search All-Financial Report"
```

For more information, see [New-ComplianceSearch](https://docs.microsoft.com/powershell/module/exchange/new-compliancesearch).

> [!IMPORTANT]
> When you create a compliance search by using the **New-ComplianceSearch** cmdlet, a shadow In-Place eDiscovery search is created (but not started) and displayed on the **In-Place eDiscovery & Hold** page in the Exchange admin center (EAC). It's also returned by using the **Get-MailboxSearch** cmdlet. This mailbox search is named **ComplianceSearchName -shadow**. We recommend that you delete the shadow In-Place eDiscovery search, and use the script in Step 3 to create the In-Place eDiscovery search. The functionality of creating a shadow search will be removed in a cumulative update for Exchange 2016.

## (Optional) Step 2: Verify the number of source mailboxes in the compliance search
<a name="step2"> </a>

A compliance search will return a maximum of 500 source mailboxes that contain search results. If there are more than 500 mailboxes that contain content that matches the search query, only the top 500 mailboxes with the most search results are included in the compliance search that you created in the previous step. So if more than 500 mailboxes contain search results, some of those mailboxes won't be included in the list of source mailboxes copied to the new In-Place eDiscovery search created in Step 3.

To help you create a compliance search with no more than 500 source mailboxes, follow these steps to run a script that displays the number of source mailboxes (that contain search results) returned by the compliance search you created in Step 1.

1. Save the following text to a Windows PowerShell script file by using a filename suffix of .ps1. For example, you could save it to a file named SourceMailboxes.ps1.

   ```PowerShell
   [CmdletBinding()]
   Param(
        [Parameter(Mandatory=$True,Position=1)]
        [string]$SearchName
   )
   $search = Get-ComplianceSearch $SearchName
   if ($search.Status -ne "Completed")
   {
                   "Please wait until the search finishes.";
                   break;
   }
   $results = $search.SuccessResults;
   if (($search.Items -le 0) -or ([string]::IsNullOrWhiteSpace($results)))
   {
                   "The compliance search " + $SearchName + " didn't return any useful results.";
                   break;
   }
   $mailboxes = @();
   $lines = $results -split '[\r\n]+';
   foreach ($line in $lines)
   {
       if ($line -match 'Location: (\S+),.+Item count: (\d+)' -and $matches[2] -gt 0)
       {
           $mailboxes += $matches[1];
       }
   }
   "Number of mailboxes that have search hits: " + $mailboxes.Count
   ```

2. In the Exchange Management Shell, go to the folder where the script you created in the previous step is located, and then run the script; for example:

   ```PowerShell
   .\SourceMailboxes.ps1
   ```

3. When prompted by the script, type the name of the compliance search that you created in Step 1.

   The script displays the number of source mailboxes that contain search results.

If there are more than 500 source mailboxes, try creating two (or more) compliance searches. For example, search half of your organization's mailboxes in one compliance search and the other half in another compliance search. You could also change the search criteria to reduce the number of mailboxes that contain search results. For example, you could specify a date range or refine the keyword query.

## Step 3: Run the script to create an In-Place eDiscovery search from the Compliance Search

The next step is to run a script that will convert an existing compliance search to an In-Place eDiscovery search. Here's what the script does:

- Prompts you for the name of the compliance search to convert.

- Verifies that the compliance search has completed running. If the compliance search doesn't return any results, and In-Place eDiscovery won't be created.

- Saves a list of the source mailboxes from the compliance search that contain search results to a variable.

- Creates a new In-Place eDiscovery search, with the following properties. Note that the new search isn't started. You'll start it in step 4.

  - **Name**: The name of the new search uses this format: *\<Name of compliance search\>* _MBSearch1. If you run the script again and use the same source compliance search, the search will be named *\<Name of compliance search\>* _MBSearch2.

  - **Source mailboxes**: All mailboxes from the compliance search that contain search results.

  - **Search query**: The new search uses the search query from the compliance search. If the compliance search includes all content (where the search query is blank) the new search will also have a blank search query and will include all content found in the source mailboxes.

  - **Estimate only search**: The new search is marked as an estimate-only search. It won't copy search results to a discovery mailbox after you start it.

1. Save the following text to a Windows PowerShell script file by using a filename suffix of ps1. For example, you could save it to a file named MBSearchFromComplianceSearch.ps1.

   ```PowerShell
   [CmdletBinding()]
   Param(
       [Parameter(Mandatory=$True,Position=1)]
       [string]$SearchName,
       [switch]$original,
       [switch]$restoreOriginal
   )
   $search = Get-ComplianceSearch $SearchName
   if ($search.Status -ne "Completed")
   {
   	"Please wait until the search finishes";
   	break;
   }
   $results = $search.SuccessResults;
   if (($search.Items -le 0) -or ([string]::IsNullOrWhiteSpace($results)))
   {
   	"The compliance search " + $SearchName + " didn't return any useful results";
   	"A mailbox search object wasn't created";
   	break;
   }
   $mailboxes = @();
   $lines = $results -split '[\r\n]+';
   foreach ($line in $lines)
   {
       if ($line -match 'Location: (\S+),.+Item count: (\d+)' -and $matches[2] -gt 0)
       {
           $mailboxes += $matches[1];
       }
   }
   $msPrefix = $SearchName + "_MBSearch";
   $I = 1;
   $mbSearches = Get-MailboxSearch;
   while ($true)
   {
       $found = $false;
       $mbsName = "$msPrefix$I";
       foreach ($mbs in $mbSearches)
       {
           if ($mbs.Name -eq $mbsName)
           {
               $found = $true;
               break;
           }
       }
       if (!$found)
       {
           break;
       }
       $I++;
   }
   $query = $search.KeywordQuery;
   if ([string]::IsNullOrWhiteSpace($query))
   {
       $query = $search.ContentMatchQuery;
   }
   if ([string]::IsNullOrWhiteSpace($query))
   {
   	New-MailboxSearch "$msPrefix$i" -SourceMailboxes $mailboxes -EstimateOnly;
   }
   else
   {
   	New-MailboxSearch "$msPrefix$i" -SourceMailboxes $mailboxes -SearchQuery $query -EstimateOnly;
   }
   ```

2. In the Exchange Management Shell, go to the folder where the script that you created in the previous step is located, and then run the script; for example:

   ```PowerShell
   .\MBSearchFromComplianceSearch.ps1
   ```

3. When prompted by the script, type the name of the compliance search that you want to covert to an In-Place eDiscovery search (for example, the search that you created in Step 1) , and then press **Enter**.

   If the script is successful, a new In-Place eDiscovery search is created with a status of **NotStarted**. Run the command `Get-MailboxSearch <Name of compliance search>_MBSearch1 | FL` to display the properties of the new search.

## Step 4: Start the In-Place eDiscovery search


The script that you run in Step 3 creates a new In-Place eDiscovery search, but doesn't start it. The next step is to start the search so you can get an estimate of the search results.

1. In the Exchange admin center (EAC), go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the In-Place eDiscovery search that you created in Step 3.

3. Click **Search** (![Search icon](../../media/ITPro_EAC_.png)) \> **Estimate search results** to start the search and return an estimate of the total size and number of items returned by the search.

   The estimates are displayed in the details pane. Click **Refresh** (![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png)) to update the information displayed in the details pane.

4. To preview the results after the search is completed, click **Preview search results** in the details pane.

> [!TIP]
> Alternatively, you can use the Exchange Management Shell to start the In-Place eDiscovery search; for example `Start-MailboxSearch -Identity <Name of compliance search>_MBSearch1`.

## Next steps after creating and running the In-Place eDiscovery search

After you create and start the In-Place eDiscovery search that was created by the script in Step 3, you can use the normal In-Place eDiscovery workflow to perform different eDiscovery actions on the search results.

### Create an In-Place Hold

1. In the EAC, go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the In-Place eDiscovery search that you created in Step 3, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

3. On the **In-Place Hold** page, select the **Place content matching the search query in selected mailboxes on hold** check box and then select one of the following options:

   - **Hold indefinitely**: Choose this option to place items returned by the search on an indefinite hold. Items on hold will be preserved until you remove the mailbox from the search or remove the search.

   - **Specify number of days to hold items relative to their received date**: Choose this option to hold items for a specific period. The duration is calculated from the date a mailbox item is received or created.

4. Click **Save** to create the In-Place Hold and restart the search.

### Copy the search results

1. In the EAC, go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the In-Place eDiscovery search that you created in Step 3.

3. Click **Search** (![Search icon](../../media/ITPro_EAC_.png)), and then click **Copy search results** from the drop-down list.

4. In **Copy Search Results**, select from the following options:

   - **Include unsearchable items**: Select this check box to include mailbox items that couldn't be searched (for example, messages with attachments of file types that couldn't be indexed by Exchange Search).

   - **Enable de-duplication**: Select this check box to exclude duplicate messages. Only a single instance of a message will be copied to the discovery mailbox.

   - **Enable full logging**: Select this check box to include a full log in search results.

   - **Send me mail when the copy is completed**: Select this check box to get an email notification when the search is completed.

   - **Copy results to this discovery mailbox**: Click **Browse** to select the discovery mailbox where you want the search results copied to.

5. Click **Copy** to start the process to copy the search results to the specified discovery mailbox.

6. Click **Refresh** (![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png)) to update the information about the copying status that is displayed in the details pane.

7. When copying is complete, click **Open** to open the discovery mailbox to view the search results.

### Export the search results

1. In the EAC, go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the In-Place eDiscovery search that you created in Step 3, and then click **Export to a PST file**.

3. In the list view, select the In-Place eDiscovery search you want to export the results of, and then click **Export to a PST file**.

4. In the **eDiscovery PST Export Tool** window, do the following:

   - Click **Browse** to specify the location where you want to download the PST file.

   - Click the **Enable deduplication** checkbox to exclude duplicate messages. Only a single instance of a message will be included in the PST file.

   - Click the **Include unsearchable items** checkbox to include mailbox items that couldn't be searched (for example, messages with attachments of file types that couldn't be indexed by Exchange Search). Unsearchable items are exported to a separate PST file.

5. Click **Start** to export the search results to a PST file.

   A window is displayed that contains status information about the export process.
