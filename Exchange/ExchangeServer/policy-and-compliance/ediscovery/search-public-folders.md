---
localization_priority: Normal
description: You can use In-Place eDiscovery to search for content in public folders and place content in public folders on In-Place Hold. Like content in user mailboxes, content in public folders might be relevant if your organization has to respond to legal requests such as lawsuits or regulatory investigations.
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 1845e557-be01-4921-8aa1-88da2b59c2ba
ms.date: 6/8/2018
ms.reviewer: 
title: Search and place a hold on public folders using In-Place eDiscovery
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Search and place a hold on public folders using In-Place eDiscovery

You can use In-Place eDiscovery to search for content in public folders and place content in public folders on In-Place Hold. Like content in user mailboxes, content in public folders might be relevant if your organization has to respond to legal requests such as lawsuits or regulatory investigations.

## Before you begin

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place eDiscovery" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

- You can include mailboxes and public folders in the same eDiscovery search.

- You can use an In-Place Hold to place content in public folders on hold. But if you select the option to search all mailboxes in your organization, you can't use the search to place a hold on any of the content sources of the search.

## Use the EAC to search and place a hold on public folders

1. Go to **Compliance management** \> **In-place eDiscovery & hold**.

2. Click **New** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

3. On the **Name and description** page, type a name for the search, add an optional description, and then click **Next**.

4. On the **Mailboxes and Public folders** page, under **Public folders**, click **Search all public folders**. Additionally, you can configure whether to include mailboxes in the search:

  - To exclude mailboxes from the search, click **Don't search any mailboxes**.

  - To include specific mailboxes in the search, click **Specify mailboxes to search**, and then add that mailboxes that you want to search.

    > [!NOTE]
    > As previously explained, if you select the **Search all mailboxes**option, you won't be able to enable an In-Place Hold for the search.

![Use In-Place eDiscovery to search and place a hold on public folders](../../media/TA_MRM_SearchPublicFolders.gif)

5. On the **Search query** page, complete the following fields:

  - **Include all content**: Select this option to include all content in the selected sources in the search results. If you select this option, you can't specify additional search criteria.

  - **Filter based on criteria**: Select this option to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types.

6. On the **In-Place Hold settings** page, you can select the **Place content matching the search query in selected mailboxes on hold** to place an In-Place Hold on all public folders in your organization. Leave the check box unselected to not place content on hold. If you place content on hold, select one of the following options for the hold duration:

  - **Hold indefinitely**: Click this button to place items returned by the search on an indefinite hold. Items on hold will be preserved until you remove public folders from the search or remove the search.

  - **Specify number of days to hold items relative to their received date**: Click this button to hold items in public folders for a specific period. For example, you can use this option if your organization requires that public folder content be retained for at least seven years.

7. Click **Finish** to save the search and return an estimate of the total size and number of items that will be returned by the search or placed on hold based on the criteria you specified.

    Estimates are displayed in the details pane on the **In-Place eDiscovery & Hold** page. Select a search and then click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png) to update the information about the search that's displayed in the details pane.

## Use the Exchange Management Shell to search and place a hold on public folders

Here are three examples of using the Exchange Management Shell to search and place a hold on public folders.

### Example 1

This example creates an estimate-only search that searches all public folders in the organization for items sent between January 1, 2015 and June 30, 2015 and that contain the phrase "patent infringement". The search doesn't include any mailboxes. The **Start-MailboxSearch** cmdlet is used to start the estimate-only search.

```
New-MailboxSearch -Name "Northwind Subpoena-All PFs" -AllPublicFolderSources $true -AllSourceMailboxes $false -SearchQuery "patent infringement" -StartDate "01/01/2015" -EndDate "06/30/2015" -TargetMailbox "Discovery Search Mailbox" -EstimateOnly
```

```
Start-MailboxSearch "Northwind Subpoena-All PFs"
```

### Example 2

This example places all content in all public folders on In-Place hold, with an unlimited hold duration. The **Start-MailboxSearch** cmdlet is use to run the search and place the content on hold.

```
New-MailboxSearch -Name "Hold for all PFs" -AllPublicFolderSources $true -AllSourceMailboxes $false -EstimateOnly -InPlaceHoldEnabled $true
```

```
Start-MailboxSearch "Hold for all PFs"
```

### Example 3

This example searches all mailboxes and public folders for any content that contains the words "price list" and "Contoso" and that was sent after January 1, 2015. The **Start-MailboxSearch** cmdlet is use to run the search and copy the search results to the discovery mailbox.

```
New-MailboxSearch -Name "Contoso Litigation" -AllSourceMailboxes $true -AllPublicFolderSources $true -SearchQuery '"price list" AND "contoso"' -StartDate "01/01/2015" -TargetMailbox "Discovery Search Mailbox"
```

```
Start-MailboxSearch "Contoso Litigation"
```

## More information

- You can only search or place holds on all public folders in your organization. You can't select specific public folders to search.

- Moving public folders to a different public folder mailbox doesn't affect searching or placing holds on public folders that have been moved.

- Public folder mailboxes are counted against the source mailbox limit for the eDiscovery search.

- You can't delete public folders that are on In-Place Hold. You will have to remove the hold before you can delete any public folder.

- Mail-enabling a public folder doesn't impact using In-Place eDiscovery to search or place holds on public folders. Mail-enabled and non-mail enabled public folders can be searched and placed on hold.
