---
localization_priority: Normal
description: Use In-Place eDiscovery to search across all mailbox content, including deleted items and original versions of modified items for users placed on In-Place Hold and Litigation Hold.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: feedc0c9-4a44-4bb2-8520-cc29d66d4fc3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create an In-Place eDiscovery search
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create an In-Place eDiscovery search

> [!IMPORTANT]
> As we continue to invest in different ways to search for mailbox content, we're announcing the retirement of In-Place eDiscovery in the Exchange admin center (EAC) in Exchange Online. Starting July 1, 2020, you won't be able to create new In-Place eDiscovery searches. But you'll still be able to manage In-Place eDiscovery searches in the EAC or by using the **Set-MailboxSearch** cmdlet in Exchange Online PowerShell. However, starting October 1, 2020, you won't be able to manage In-Place eDiscovery searches. You'll only be remove them in the EAC or by using the **Remove-MailboxSearch** cmdlet. Using In-Place eDiscovery in Exchange Server and Exchange hybrid deployments will still be supported. For more information about the retirement of In-Place eDiscovery in Exchange Online, see [Retirement of legacy eDiscovery tools](https://docs.microsoft.com/microsoft-365/compliance/legacy-ediscovery-retirement).

Use [In-Place eDiscovery](in-place-ediscovery.md) to search across all mailbox content, including deleted items and original versions of modified items for users placed on [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- To create eDiscovery searches, you have to have an SMTP address in the organization that you're creating the searches in. So in Exchange Online, you must have a licensed Exchange Online (Plan 2) mailbox to create eDiscovery searches. In an Exchange hybrid organization, your on-premises Exchange mailbox must have a corresponding mail user account in your Microsoft 365 or Office 365 organization so that you can search Exchange Online mailboxes. Or, if you sign in with an account that only exists in Office 365, such as the tenant administrator account, that account must be assigned an Exchange Online (Plan 2) license.

- Exchange Server Setup creates a Discovery mailbox called **Discovery Search Mailbox** to copy search results. The Discovery Search Mailbox is also created by default in Exchange Online. You can create additional Discovery mailboxes. For details, see [Create a discovery mailbox](create-a-discovery-mailbox.md).

- When you create an In-Place eDiscovery search, messages returned in search results aren't copied automatically to a discovery mailbox. After you create the search, you can use the Exchange admin center (EAC) to estimate and preview search results or copy them to a discovery mailbox. For details, see [Use the EAC to estimate or preview search results](#use-the-eac-to-estimate-or-preview-search-results) (later in this topic).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create an In-Place eDiscovery search

As previously explained, to create eDiscovery searches, you have to sign in to a user account that has an SMTP address in your organization.

1. Go to **Compliance management** \> **In-place eDiscovery & hold**.

2. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

3. In **In-Place eDiscovery & Hold**, on the **Name and description** page, type a name for the search, add an optional description, and then click **Next**.

4. On the **Mailboxes** page, select the mailboxes to search. You can search across all mailboxes or select specific ones to search. In Exchange Online, you can also select Microsoft 365 groups as a content source for the search.

   > [!IMPORTANT]
   > You can't use the **Search all mailboxes** option to place all mailboxes on hold. To create an In-Place Hold, you must select **Specify mailboxes to search**. For more details, see [Create or remove an In-Place Hold](../../security-and-compliance/create-or-remove-in-place-holds.md).

5. On the **Search query** page, complete the following fields:

   - **Include all user mailbox content**: Select this option to place all content in the selected mailboxes on hold. If you select this option, you can't specify additional search criteria.

   - **Filter based on criteria**: Select this option to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types.

   ![Configure eDiscovery Search Query](../../media/TA_MRM_SearchQuery.png)

   > [!NOTE]
   > The **From:** and **To/Cc/Bcc:** fields are connected by an **OR** operator in the search query that's created when you run the search. That means any message sent or received by any of the specified users (and matches the other search criteria) is included in the search results. > The dates are connected by an **AND** operator.

6. On the **In-place hold settings** page, you can select the **Place content matching the search query in selected mailboxes on hold** check box, and then select one of the following options to place items on In-Place Hold:

   - **Hold indefinitely**: Select this option to place the returned items on an indefinite hold. Items on hold will be preserved until you remove the mailbox from the search or remove the search.

   - **Specify number of days to hold items relative to their received date**: Use this option to hold items for a specific period. For example, you can use this option if your organization requires that all messages be retained for at least seven years. You can use a time-based In-Place Hold along with a retention policy to make sure items are deleted in seven years.

   > [!IMPORTANT]
   > When placing mailboxes or items on In-Place Hold for legal purposes, it is generally recommended to hold items indefinitely and remove the hold when the case or investigation is completed.

7. Click **Finish** to save the search and return an estimate of the total size and number of items that will be returned by the search based on the criteria you specified. Estimates are displayed in the details pane. Click **Refresh** ![Refresh Icon](../../media/ITPro_EAC_RefreshIcon.gif) to update the information displayed in the details pane.

## Use Exchange Online PowerShell to create an In-Place eDiscovery search

This example creates the In-Place eDiscovery search named Discovery-CaseId012 that searches for items containing the keywords Contoso and ProjectA and that also meet the following criteria:

- Start date: 1/1/2009

- End date: 12/31/2011

- Source mailbox: DG-Finance

- Target mailbox: Discovery Search Mailbox

- Message types: Email

- Includes unsearchable items in the search statistics

- Log level: Full

> [!IMPORTANT]
> If you don't specify additional search parameters when running an In-Place eDiscovery search, all items in the specified source mailboxes are returned in the results. If you don't specify mailboxes to search, all mailboxes in your Exchange or Exchange Online organization are searched.

```PowerShell
New-MailboxSearch "Discovery-CaseId012" -StartDate "01/01/2009" -EndDate "12/31/2011" -SourceMailboxes "DG-Finance" -TargetMailbox "Discovery Search Mailbox" -SearchQuery '"Contoso" AND "Project A"' -MessageTypes Email -IncludeUnsearchableItems -LogLevel Full
```

> [!NOTE]
> When using the _StartDate_ and _EndDate_ parameters, you have to use the date format of mm/dd/yyyy, even if your local machine settings are configured to use a different date format, such as dd/mm/yyyy. For example, to search for messages sent between April 1, 2013 and July 1, 2013, you would use **04/01/2013** and **07/01/2013** for the start and end dates.

This example creates an In-Place eDiscovery search named HRCase090116 that searches for email messages sent by Alex Darrow to Sara Davis in 2015.

```PowerShell
New-MailboxSearch "HRCase090116" -StartDate "01/01/2015" -EndDate "12/31/2015" -SourceMailboxes  alexd,sarad -SearchQuery 'From:alexd@contoso.com AND To:sarad@contoso.com' -MessageTypes Email -TargetMailbox "Discovery Search Mailbox" -IncludeUnsearchableItems -LogLevel Full
```

After using Exchange Online PowerShell to create an In-Place eDiscovery search, you have to start the search by using the **Start-MailboxSearch** cmdlet to copy messages to the discovery mailbox specified in the _TargetMailbox_ parameter.

For detailed syntax and parameter information, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch).

## Use the EAC to estimate or preview search results

After you create an In-Place eDiscovery search, you can use the EAC to get an estimate and preview of the search results. If you created a new search using the **New-MailboxSearch** cmdlet, you can use Exchange Online PowerShell to start the search to get an estimate of the search results. You can't use Exchange Online PowerShell to preview messages returned in search results.

1. Navigate to **Compliance management** \> **In-place eDiscovery & hold**.

2. In the list view, select the In-Place eDiscovery search, and then do one of the following:

   - Click **Search** ![Search icon](../../media/ITPro_EAC_.gif) \> **Estimate search results** to return an estimate of the total size and number of items that will be returned by the search based on the criteria you specified. Selecting this option restarts the search and performs an estimate.

     Search Estimates are displayed in the details pane. Click **Refresh** ![Refresh Icon](../../media/ITPro_EAC_RefreshIcon.gif) to update the information displayed in the details pane.

   - Click **Preview search results** in the details pane to preview the results after the search estimate is completed. Selecting this option opens the **eDiscovery search preview** window. All messages returned from the mailboxes that were searched are displayed.

     > [!NOTE]
     > The mailboxes that were searched are listed in the right pane in the **eDiscovery search preview** window. For each mailbox, the number of items returned and the total size of these items is also displayed. All items returned by the search are listed in the right pane, and can be sorted by newest or oldest date. Items from each mailbox can't be displayed in the right pane by clicking a mailbox in the left pane. To view the items returned from a specific mailbox, you can copy the search results and view the items in the discovery mailbox.

![Estimate or Preview Search Results](../../media/TA_Discovery_EstimatePreviewUI.gif)

## Use Exchange Online PowerShell to estimate search results

You can use the _EstimateOnly_ switch to return only get an estimate of the search results and not copy the results to a discovery mailbox. You have to start an estimate-only search with the **Start-MailboxSearch** cmdlet. Then you can retrieve the estimated search results by using the **Get-MailboxSearch** cmdlet.

For example, you would run the following commands to create a new eDiscovery search and then display an estimate of the search results:

```PowerShell
New-MailboxSearch "FY13 Q2 Financial Results" -StartDate "04/01/2013" -EndDate "06/30/2013" -SourceMailboxes "DG-Finance" -SearchQuery '"Financial" AND "Fabrikam"' -EstimateOnly -IncludeKeywordStatistics
```

```PowerShell
Start-MailboxSearch "FY13 Q2 Financial Results"
```

```PowerShell
Get-MailboxSearch "FY13 Q2 Financial Results"
```

To display specific information about the estimated search results from the previous example, you could run the following command:

```PowerShell
Get-MailboxSearch "FY13 Q2 Financial Results" | Format-List Name,Status,LastRunBy,LastStartTime,LastEndTime,Sources,SearchQuery,ResultSizeEstimate,ResultNumberEstimate,Errors,KeywordHits
```

## More information about eDiscovery searches

- After you create a new eDiscovery search, you can copy search results to the discovery mailbox and export those search results to a PST file. For more information, see [Export eDiscovery search results to a PST file](export-search-results.md).

- After you run an eDiscovery search estimate (that includes keywords in the search criteria), you can view keyword statistics by clicking **View keyword statistics** in the details pane for the selected search. These statistics show details about the number of items returned for each keyword used in the search query. However, if more than 100 source mailboxes are included in the search, an error will be returned if you try to view keyword statistics. To view keyword statistics, no more than 100 source mailboxes can be included in the search.

- If you use **Get-MailboxSearch** in Exchange Online to retrieve information about an eDiscovery search, you have to specify the name of a search to return a complete list of the search properties; for example, `Get-MailboxSearch "Contoso Legal Case"`. If you run the **Get-MailboxSearch** cmdlet without using any parameters, the following properties aren't returned:

  - SourceMailboxes

  - Sources

  - SearchQuery

  - ResultsLink

  - PreviewResultsLink

  - Errors

  The reason is that it requires a lot of resources to return these properties for all eDiscovery searches in your organization.
