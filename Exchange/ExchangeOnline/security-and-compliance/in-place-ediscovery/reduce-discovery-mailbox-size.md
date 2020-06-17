---
localization_priority: Normal
description: Have a discovery mailbox that's exceeded the 50 GB limit? You can fix this issue by creating new discovery mailboxes and copying the search results from the large discovery mailbox to the new ones.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: fa762d14-f942-4728-8813-887d11441a68
ms.reviewer: 
f1.keywords:
- NOCSH
title: Reduce the size of a discovery mailbox in Exchange
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid: MET150
audience: Admin
ms.service: exchange-online
manager: serdars

---

# Reduce the size of a discovery mailbox in Exchange

Have a discovery mailbox that's exceeded the 50 GB limit? You can fix this issue by creating new discovery mailboxes and copying the search results from the large discovery mailbox to the new ones.

## Why would I want to do this?

In Exchange Server and Exchange Online, the maximum size of discovery mailboxes, which are used to store In-Place eDiscovery search results, is 50 GB. Prior to the current size limit, you were able to increase the storage quota to more than 50 GB, which resulted in having discovery mailboxes much larger than 50 GB. There are three issues with discovery mailboxes that are larger than 50 GB:

- They're not supported.

- They can't be migrated to Office 365.

- If they're discovery mailboxes in Exchange Server 2010, they can't be upgraded to later versions.

## The process at a glance

Here's a quick look at what you'll need to do to reduce the size of a discovery mailbox that's exceeded the 50 GB limit:

1. [Step 1: Create discovery mailboxes](#step-1-create-discovery-mailboxes) additional discovery mailboxes to distribute the search results to.

2. [Step 2: Copy search results to a discovery mailbox](#step-2-copy-search-results-to-a-discovery-mailbox) the search results from the existing discovery mailbox to one or more of the new discovery mailboxes.

3. [Step 3: Delete eDiscovery searches](#step-3-delete-ediscovery-searches) eDiscovery searches from the original discovery mailbox to reduce its size.

The strategy presented here groups the search results from the original discovery mailbox into separate eDiscovery searches that are based on date ranges. This is a quick way to copy many search results to a new discovery mailbox. The following graphic illustrates this approach.

![Reducing the size of a discovery mailbox](../../media/TA_MRM_ReduceDiscoveryMailbox.gif)

## What do you need to know before you begin?

- Estimated time to complete this task: Time will vary based on the amount and size of the search results that will be copied to different discovery mailboxes.

- Run the following command to determine the size of the discovery mailboxes in your organization.

  ```PowerShell
  Get-Mailbox -RecipientTypeDetails DiscoveryMailbox | Get-MailboxStatistics | Format-List DisplayName,TotalItemSize
  ```

- Determine if you need to keep some or all of the search results from the discovery mailbox that's exceeded the 50 GB limit. Follow the steps in this topic to retain search results by copying them to a different discovery mailbox. If you don't need to keep the results of a specific eDiscovery search, you can delete the search, as explained in step 3. Deleting a search will delete the search results from the discovery mailbox.

- If you don't need any of the search results from a discovery mailbox that's exceeded the 50 GB limit, you can delete it. If this is the default discovery mailbox that was created when your Exchange organization was provisioned, you can re-create it. For more information, see [Delete and re-create the default discovery mailbox in Exchange](delete-and-re-create-default-discovery-mailbox.md).

- For current legal cases, you might want to export the results of selected eDiscovery searches to .pst files. Doing this keeps the results from a specific search intact. In addition to the .pst files that contain the search results, a search results log (.csv file format) that contains an entry for each message returned in the search results is also exported. Each entry in this file identifies the source mailbox where the message is located. For more information, see [Export eDiscovery search results to a PST file](export-search-results.md).

  After you export search results to .pst files, you'll need to use Outlook if you want to import them to a new discovery mailbox.

## Step 1: Create discovery mailboxes

The first step is to create additional discovery mailboxes so that you can copy the search results from the discovery mailbox that's exceeded the size limit. Based on the 50 GB size limit for discovery mailboxes, determine how many additional discovery mailboxes you'll need and create them. You'll then need to assign users or groups the necessary permissions to open these new discovery mailboxes.

1. Run the following command to create a new discovery mailbox.

   ```PowerShell
   New-Mailbox -Name <discovery mailbox name> -Discovery
   ```

2. Run the following command to assign a user or group permissions to open the discovery mailbox and view search results.

   ```PowerShell
   Add-MailboxPermission <discovery mailbox name> -User <name of user or group> -AccessRights FullAccess -InheritanceType all
   ```

## Step 2: Copy search results to a discovery mailbox

The next step is to use the **New-MailboxSearch** cmdlet to copy the search results from the existing discovery mailbox to a new discovery mailbox that you created in the previous step. This procedure uses the _StartDate_ and _EndDate_ parameters to scope the search results into batches that are no larger than 50 GB. This may require some testing (by estimating the search results) to size the search results appropriately.

1. Run the following command to create a new eDiscovery search.

   ```PowerShell
   New-MailboxSearch -Name "Search results from 2010" -SourceMailboxes "Discovery Search Mailbox" -StartDate "01/01/2010" -EndDate "12/31/2010" -TargetMailbox "Discovery Mailbox Backup 01" -EstimateOnly -StatusMailRecipients admin@contoso.com
   ```

   This example uses the following parameters:

   - _Name_: This parameter specifies the name of the new eDiscovery search. Because the search is scoped by sent and received dates, it's useful that the name of the search includes the date range.

   - _SourceMailboxes_: This parameter specifies the default discovery mailbox. You can also specify the name of another discovery mailbox that's exceeded the size limit.

   - _StartDate_ and _EndDate_: These parameters specify the date range of the search results in the default discovery mailbox to include in the search results.

     > [!NOTE]
     > For dates, use the short date format, mm/dd/yyyy, even if the Regional Options settings on the local computer are configured with a different format, such as dd/mm/yyyy. For example, use **03/01/2014** to specify March 1, 2014.

   - _TargetMailbox_: This parameter specifies that search results should be copied to the discovery mailbox named "Discovery Mailbox Backup 01".

   - _EstimateOnly_: This switch specifies that only an estimate of the number of items that will be returned is provided when the search is started. If you don't include this switch, messages are copied to the target mailbox when the search is started. Using this switch lets you adjust the date ranges if necessary to increase or decrease the number of search results.

   - _StatusMailRecipients_: This parameter specifies that the status message should be sent to the specified recipient.

2. After the search is created, start it by using Exchange Online PowerShell or the Exchange admin center (EAC).

   - **Using Exchange Online PowerShell**: Run the following command to start the search created in the previous step. Because the _EstimateOnly_ switch was included when the search was created, the search results won't be copied to the target discovery mailbox.

   ```PowerShell
   Start-MailboxSearch "Search results from 2010"
   ```

   - **Using the EAC**: Go to **Compliance management** \> **In-Place eDiscovery & hold**. Select the search created in the previous step, click **Search** ![Search icon](../../media/ITPro_EAC_.gif), and then click **Estimate search results**.

3. If necessary, adjust the date range to increase or decrease the amount of search results that are returned. If you change the date range, run the search again to get a new estimate of the results. Consider changing the name of the search to reflect the new date range.

4. When you're finished testing the search, use Exchange Online PowerShell or the EAC to copy the search results to the target discovery mailbox.

   - **Using Exchange Online PowerShell**: Run the following commands to copy the search results. You have to remove the _EstimateOnly_ switch before you can copy the search results.

   ```PowerShell
   Set-MailboxSearch "Search results from 2010" -EstimateOnly $false
   ```

   ```PowerShell
   Start-MailboxSearch "Search results from 2010"
   ```

   - **Using the EAC**: Go to **Compliance management** \> **In-Place eDiscovery & hold**. Select the search, click **Search** ![Search icon](../../media/ITPro_EAC_.gif), and then click **Copy search results**.

5. Repeat steps 1 through 4 to create new searches for additional date ranges. Include the date range in the name of the new search to indicate the range of the results. To make sure none of the discovery mailboxes exceeds the 50 GB limit, use different discovery mailboxes as the target mailbox.

## Step 3: Delete eDiscovery searches

After you've copied search results from the original discovery mailbox to another discovery mailbox, you can delete the original eDiscovery searches. Deleting an eDiscovery search will delete the search results from the discovery mailbox where those search results are stored.

Before deleting a search, you can run the following command to identify the size of the search results that have been copied to a discovery mailbox for all searches in your organization.

```PowerShell
Get-MailboxSearch | Format-List Name,TargetMailbox,ResultSizeCopied
```

You can use Exchange Online PowerShell or the EAC to delete an eDiscovery search.

- **Using Exchange Online PowerShell**: Run the following command.

  ```PowerShell
  Remove-MailboxSearch -Identity <name of search>
  ```

- **Using the EAC**: Go to **Compliance management** \> **In-Place eDiscovery & hold**. Select the search that you want to delete, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

## How do you know this worked?

After you've deleted the eDiscovery searches to remove the results from the discovery mailbox where they were stored, run the following command to display the size of a selected discovery mailbox.

```PowerShell
Get-Mailbox <name of discovery mailbox> | Get-MailboxStatistics | Format-List TotalItemSize
```
