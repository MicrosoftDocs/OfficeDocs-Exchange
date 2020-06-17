---
localization_priority: Normal
description: 'Summary: Learn how to create an In-Place eDiscovery search in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: feedc0c9-4a44-4bb2-8520-cc29d66d4fc3
ms.reviewer:
title: Create an In-Place eDiscovery search in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create an In-Place eDiscovery search in Exchange Server

 Use an In-Place eDiscovery search to search for content across all mailboxes and public folders in your Exchange Server organization. This includes searching permanently deleted items and original versions of modified items (in the Recoverable Items folder) for users placed on Litigation Hold or In-Place Hold. For more information about these searches, see [In-Place eDiscovery in Exchange Server](ediscovery.md).

## Before you begin

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

- To create eDiscovery searches, you have to have an SMTP address in the organization that you're creating the searches in. In an Exchange hybrid organization, your on-premises Exchange mailbox must have a corresponding mail user account in your Microsoft 365 or Office 365 organization so that you can search cloud-based mailboxes. Or, if you sign in with an account that only exists in Office 365, such as the tenant administrator account, that account must be assigned an Exchange Online license. For more information about the Office 365 licensing requirements for In-Place eDiscovery searches, see [Exchange Online Service Description](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-service-description).

- Exchange Server Setup creates a Discovery mailbox called **Discovery Search Mailbox** to copy search results. You can create additional Discovery mailboxes. For details, see [Create a discovery mailbox](https://docs.microsoft.com/Exchange/create-a-discovery-mailbox-exchange-2013-help).

- When you create a search, messages returned in search results aren't copied automatically to a discovery mailbox. After you create the search, you can use the Exchange admin center (EAC) to estimate and preview search results or copy them to a discovery mailbox. You can also export the search results to a .pst file. For details, see:

  - [Use the EAC to estimate or preview search results](#use-the-eac-to-estimate-or-preview-search-results) (later in this topic)

  - [Copy eDiscovery search results to a discovery mailbox](copy-results-to-discovery-mailboxes.md)

  - [Export eDiscovery search results to a PST file](export-results-to-pst.md)

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

## Use the EAC to create a search

As previously explained, to create eDiscovery searches, you have to sign in to a user account that has an SMTP address in your organization.

1. Go to **Compliance management** \> **In-place eDiscovery & Hold**, and then click **New** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. In the **New In-Place eDiscovery & Hold** window, on the **Name and description** page, type a name for the search, add an optional description, and then click **Next**.

3. On the **Mailboxes and Public folders** page, select the content sources to search:

   - To include all mailboxes in the search, click **Search all mailboxes**. If you select this option, you won't be able to enable an In-Place Hold for the search.

   - To exclude mailboxes from the search (and search only public folders), click **Don't search any mailboxes**.

   - To include specific mailboxes in the search, click **Specify mailboxes to search**, and then add that mailboxes that you want to search.

   - To include public folders in the search (or to place public folders on hold), click **Search all public folders**. For more information about searching public folders, see [Search and place a hold on public folders using In-Place eDiscovery](search-public-folders.md).

   ![Use In-Place eDiscovery to search and place a hold on public folders](../../media/TA_MRM_SearchPublicFolders.gif)

4. On the **Search query** page, complete the following fields:

   - **Include all content**: Select this option to include all content in the search results. If you select this option, you can't specify additional search criteria.

   - **Filter based on criteria**: Select this option to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types. For more information about search queries, see [Message properties and search operators for In-Place eDiscovery in Exchange Server](message-properties-and-search-operators.md).

     ![Configure an eDiscovery search query](../../media/TA_MRM_SearchQuery.png)

     > [!NOTE]
     > The **From:** and **To/Cc/Bcc:** fields are connected by an **OR** operator in the search query that's created when you run the search. That means any message sent or received by any of the specified users (and matches the other search criteria) is included in the search results. The dates are connected by an **AND** operator.

5. On the **In-Place Hold settings** page, you can select the **Place content matching the search query in selected sources on hold** check box, and then select one of the following options to place items on In-Place Hold:

   - **Hold indefinitely**: Select this option to place the returned items on an indefinite hold. Items on hold will be preserved until you remove the content source from the search or if you delete the search.

   - **Specify number of days to hold items relative to their received date** Use this option to hold items for a specific period. For example, you can use this option if your organization requires that all messages be retained for at least seven years. You can use a *time-based* In-Place Hold along with a retention policy to make sure items are deleted in seven years.

     > [!IMPORTANT]
     > When placing content sources or specific items on In-Place Hold for legal purposes, it's generally recommended to hold items indefinitely and remove the hold when the case or investigation is completed.

6. Click **Finish** to save the search and return an estimate of the total size and number of items that will be returned by the search based on the criteria you specified. Estimates are displayed in the details pane. Click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png) to update the information displayed in the details pane.

## Use the Exchange Management Shell to create a search

Here are four examples of using the Exchange Management Shell to search and place a hold on content in mailboxes and public folders. For detailed syntax and parameter information about using the Exchange Management Shell to create eDiscovery searches, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch)

### Example 1

This example creates the search Discovery-CaseId012 for items containing the keywords **Contoso** and **ProjectA**. The search results are place on In-Place hold, with an unlimited hold duration. The search also includes the following criteria:

- Start date: 1/1/2013

- End date: 12/31/2015

- Source mailbox: DG-Finance

- Target mailbox: Discovery Search Mailbox

- Message types: Email

- Log level: Full

> [!IMPORTANT]
> If you don't specify a search query, a date range, or a message type, all items in the source mailboxes or public folders are returned in the results. The results would be similar to selecting **Include all content** on the **Search query** page in the EAC.

```PowerShell
New-MailboxSearch "Discovery-CaseId012" -StartDate "01/01/2013" -EndDate "12/31/2015" -SourceMailboxes "DG-Finance" -TargetMailbox "Discovery Search Mailbox" -SearchQuery '"Contoso" AND "Project A"' -MessageTypes Email -IncludeUnsearchableItems -LogLevel Full -InPlaceHoldEnabled $true
```

```PowerShell
Start-MailboxSearch "Discovery-CaseId012"
```

After using the Exchange Management Shell to create an In-Place eDiscovery search, you have to start the search by using the **Start-MailboxSearch** cmdlet to copy messages to the discovery mailbox specified in the _TargetMailbox_ parameter. For details, see [Copy eDiscovery search results to a discovery mailbox](copy-results-to-discovery-mailboxes.md).

> [!NOTE]
> When using the _StartDate_ and _EndDate_ parameters, you have to use the date format of mm/dd/yyyy, even if your local machine settings are configured to use a different date format, such as dd/mm/yyyy. For example, to search for messages sent between April 1, 2015 and July 1, 2015, you would use **04/01/2015** and **07/01/2015** for the start and end dates.

### Example 2

This example creates an In-Place eDiscovery search named HRCase090116 that searches for email messages sent by Alex Darrow to Sara Davis in 2015.

```PowerShell
New-MailboxSearch "HRCase090116" -StartDate "01/01/2015" -EndDate "12/31/2015" -SourceMailboxes alexd,sarad -SearchQuery 'From:alexd@contoso.com AND To:sarad@contoso.com' -MessageTypes Email -TargetMailbox "Discovery Search Mailbox" -IncludeUnsearchableItems -LogLevel Full
```

```PowerShell
Start-MailboxSearch "HRCase090116"
```

### Example 3

This example creates an estimate-only search that searches all public folders in the organization for items sent between January 1, 2015 and June 30, 2015, and that contain the phrase "patent infringement". The search doesn't include any mailboxes. The **Start-MailboxSearch** cmdlet is used to start the estimate-only search.

```PowerShell
New-MailboxSearch -Name "Northwind Subpoena-All PFs" -AllPublicFolderSources $true -AllSourceMailboxes $false -SearchQuery "patent infringement" -StartDate "01/01/2015" -EndDate "06/30/2015" -TargetMailbox "Discovery Search Mailbox" -EstimateOnly
```

```PowerShell
Start-MailboxSearch "Northwind Subpoena-All PFs"
```

### Example 4

This example searches all mailboxes and public folders for any content that contains the words "price list" and "Contoso" and that was sent after January 1, 2015. The **Start-MailboxSearch** cmdlet is use to run the search and copy the search results to the discovery mailbox.

```PowerShell
New-MailboxSearch -Name "Contoso Litigation" -AllSourceMailboxes $true -AllPublicFolderSources $true -SearchQuery '"price list" AND "contoso"' -StartDate "01/01/2015" -TargetMailbox "Discovery Search Mailbox"
```

```PowerShell
Start-MailboxSearch "Contoso Litigation"
```

## Use the EAC to estimate or preview search results

After you create an eDiscovery search, you can use the EAC to get an estimate and preview of the search results. If you created a new search using the **New-MailboxSearch** cmdlet, you can use the Exchange Management Shell to start the search to get an estimate of the search results.

1. Go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the search, and then do one of the following:

   - Click **Search** ![Search icon](../../media/ITPro_EAC_.png) \> **Estimate search results** to return an estimate of the total size and number of items that will be returned by the search based on the criteria you specified. Selecting this option restarts the search and performs an estimate.

     Search estimates are displayed in the details pane. Click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png) to update the information displayed in the details pane.

   - Click **Preview search results** in the details pane to preview the results after the search estimate is completed. Selecting this option opens the **eDiscovery search preview** window. All messages returned from the mailboxes or public folders that were searched are displayed.

     > [!NOTE]
     > The mailboxes or public folders that were searched are listed in the right pane in the **eDiscovery search preview** window. For each source, the number of items returned and the total size of these items is also displayed. All items returned by the search are listed in the right pane, and can be sorted by newest or oldest date. Items from each mailbox or public folder can't be displayed in the right pane by clicking a source in the left pane. To view the items returned from a specific mailbox or public folder, you can copy the search results and view the items in the discovery mailbox.

   ![Estimate or Preview Search Results](../../media/TA_Discovery_EstimatePreviewUI.gif)

## Use the Exchange Management Shell to estimate search results

You can use the _EstimateOnly_ switch to get an estimate of the search results and not copy the results to a discovery mailbox. You have to start an estimate-only search with the **Start-MailboxSearch** cmdlet. Then you can retrieve the estimated search results by using the **Get-MailboxSearch** cmdlet. You can't use the Exchange Management Shell to preview messages returned in search results.

For example, you would run the following commands to create a new search and then display an estimate of the search results:

```PowerShell
New-MailboxSearch "FY15 Q2 Financial Results" -StartDate "04/01/2015" -EndDate "06/30/2015" -SourceMailboxes "DG-Finance" -SearchQuery '"Financial" AND "Fabrikam"' -EstimateOnly -IncludeKeywordStatistics

```

```PowerShell
Start-MailboxSearch "FY15 Q2 Financial Results"
```

```PowerShell
Get-MailboxSearch "FY15 Q2 Financial Results"
```

To display specific information about the estimated search results from the previous example, you could run the following command:

```PowerShell
Get-MailboxSearch "FY15 Q2 Financial Results" | Format-List Name,Status,LastRunBy,LastStartTime,LastEndTime,Sources,SearchQuery,ResultSizeEstimate,ResultNumberEstimate,Errors,KeywordHits
```

## More information

- After you create a new eDiscovery search, you can copy search results to the discovery mailbox and export those search results to a PST file. For more information, see:

  - [Copy eDiscovery search results to a discovery mailbox](copy-results-to-discovery-mailboxes.md)

  - [Export eDiscovery search results to a PST file](export-results-to-pst.md)

- After you run an eDiscovery search estimate (that includes keywords in the search criteria), you can view keyword statistics by clicking **View keyword statistics** in the details pane for the selected search. These statistics show details about the number of items returned for each keyword used in the search query. However, if more than 100 source mailboxes are included in the search, an error will be returned if you try to view keyword statistics. To view keyword statistics, no more than 100 source mailboxes can be included in the search.

- If you use **Get-MailboxSearch** in Exchange Online to retrieve information about an eDiscovery search, you have to specify the name of a search to return a complete list of the search properties; for example, `Get-MailboxSearch "Contoso Legal Case"`. If you run the **Get-MailboxSearch** cmdlet without using any parameters, the following properties aren't returned:

  - `SourceMailboxes`

  - `Sources`

  - `PublicFolderSources`

  - `SearchQuery`

  - `ResultsLink`

  - `PreviewResultsLink`

  - `Errors`

    The reason is that it requires a lot of resources to return these properties for all eDiscovery searches in your organization.
