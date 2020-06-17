---
localization_priority: Normal
description: 'Summary: Learn how to create and remove an In-Place Hold in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9d5d8d37-a053-4830-9cb1-6e1ede25e963
ms.reviewer:
title: Create or remove an In-Place Hold
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create or remove an In-Place Hold

An In-Place Hold preserves all mailbox and public folder content, including deleted items and original versions of modified items. All such items can be returned in an In-Place eDiscovery search. When you place an In-Place Hold on a user's mailbox, the contents in the corresponding archive mailbox (if it's enabled) are also placed on hold and returned in a eDiscovery search.

When you create an In-Place Hold, you can place all items in the source mailbox or public folder on hold or you can hold only the items that meet the search criteria specified for the hold. Similarly, you can hold items indefinitely or for a specific amount of time. For more information about In-Place Holds, see [In-Place Hold and Litigation Hold in Exchange Server](holds.md).

You can create In-Place holds in the Exchange admin center (EAC) or in the Exchange Management Shell.

## Before you begin

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place Hold" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

- Depending on your Active Directory topology and replication latency, it may take up to an hour for an In-Place Hold to take effect.

- As previously explained, when you place an In-Place Hold on a user's mailbox, the content in the user's archive mailbox is also placed on hold.

- You can only search or place holds on all public folders in your organization. You can't specify individual public folders.

- See the [More information](#more-information) section for a description of the In-Place Hold workflow process.

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

## Create an In-Place Hold

### Use the EAC to create an In-Place Hold

1. Go to **Compliance management** \> **In-Place eDiscovery & Hold**, and then click **New** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

2. In the **New In-Place eDiscovery & Hold** window, on the **Name and description** page, type a name for the hold and an optional description, and then click **Next**.

3. On the **Mailboxes and Public folders** page, select the content sources to search:

   - To exclude mailboxes from the hold (and place a hold on public folders only), click **Don't search any mailboxes**.

   - To include specific mailboxes in the search, click **Specify mailboxes to search**, and then add the mailboxes that you want to search.

   - To place public folders on hold, click **Search all public folders**.

   ![Use In-Place eDiscovery to search and place a hold on public folders](../../media/TA_MRM_SearchPublicFolders.gif)

   > [!IMPORTANT]
   > You can't select the **Search all mailboxes** option when creating an In-Place Hold. To create an In-Place Hold, you must select the specific mailboxes you want to place on hold.

4. On the **Search query** page, complete the following fields, and then click **Next**.

   - **Include all content**: Select this option to place all content in selected sources on hold.

   - **Filter based on criteria**: Select this option to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types.

   > [!IMPORTANT]
   > If a user is placed on multiple In-Place Holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

5. On the **In-Place Hold settings** page, click the **Place content matching the search query in selected sources on hold** check box and then select one of the following options:

   - **Hold indefinitely**: Place items returned by the search on an indefinite hold. Items on hold will be preserved until you change the hold duration, remove the mailbox (or public folders) from the search, or remove the search.

   - **Specify number of days to hold items relative to their received date**: Hold items for a specific period. For example, you can use this option if your organization requires that all messages be retained for at least seven years. You can use a *time-based* In-Place Hold along with a retention policy to make sure items are permanently deleted in seven years.

6. Click **Finish** to create the In-Place Hold.

### Use the Exchange Management Shell to create an In-Place Hold

This example creates an In-Place Hold named Hold-CaseId012 and adds the mailbox joe@contoso.com to the hold.

> [!IMPORTANT]
> If you don't specify additional search parameters for an In-Place Hold, all items in the specified source mailboxes are placed on hold. If you don't specify the _ItemHoldPeriod_ parameter, items are placed on hold indefinitely or until the mailbox is either removed from hold or the hold is deleted.

```PowerShell
New-MailboxSearch "Hold-CaseId012" -SourceMailboxes "joe@contoso.com" -InPlaceHoldEnabled $true
```

This example places an In-Place Hold on all public folders in the organization, and holds content for 7 years. The hold doesn't include any mailboxes.

```PowerShell
New-MailboxSearch -Name "Hold for Public Folders" -AllPublicFolderSources $true -AllSourceMailboxes $false -ItemHoldPeriod 2555 -InPlaceHoldEnabled $true
```

For detailed syntax and parameter information, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch).

### How do you know this worked?

To verify that you have successfully created the In-Place Hold, do one of the following:

- Use the EAC to verify that the In-Place Hold is listed in the list view of the **In-Place eDiscovery & Hold** page.

- Use the **Get-MailboxSearch** cmdlet to retrieve the mailbox search and check the hold properties. For example, the following command displays the hold properties for the search named Hold-CaseId012:

  ```PowerShell
  Get-MailboxSearch "Hold-CaseId012" | Format-List InPlaceHoldEnabled,ItemHoldPeriod,InPlaceHoldIdentity
  ```

- Use the **Get-Mailbox** cmdlet to display In-Place Hold information for specific user mailboxes or public folder mailboxes. For example, the following command displays the GUID for the In-Place Hold:

  ```PowerShell
  Get-Mailbox "joe@contoso.com" | Format-List InPlaceHolds
  ```

    This example will display the In-Place Hold GUID for all public folder mailboxes in the organization.

  ```PowerShell
  Get-Mailbox -PublicFolder | Format-List Name,InPlaceHolds
  ```

## Remove an In-Place Hold

In Exchange Server, eDiscovery searches are used to hold and search for content in on content sources. You can't remove an In-Place eDiscovery search that's used to place content sources on hold. You must first remove the In-Place Hold by clearing the **Place content matching the search query in selected sources on hold** check box on the **In-Place Hold** page or by setting the _InPlaceHoldEnabled_ parameter to `$false` in the Exchange Management Shell. Alternatively, you can remove mailboxes and public folders from an In-Place Hold by changing the value of the _SourceMailboxes_ or _AllPublicFolderSources_ parameters specified in the search.

### Use the EAC to remove an In-Place Hold

1. Go to **Compliance management** \> **In-Place eDiscovery & Hold**.

2. In the list view, select the In-Place Hold you want to remove, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. In **In-Place eDiscovery & Hold** properties, on the **In-Place Hold** page, clear the **Place content matching the search query in selected sources on hold** check box, and then click **Save**.

4. Select the In-Place Hold again from the list view and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.png).

5. In warning, click **Yes** to remove the search.

### Use the Exchange Management Shell to remove an In-Place Hold

This example first disables In-Place Hold named Hold-CaseId012 and then removes the mailbox search.

```PowerShell
Set-MailboxSearch "Hold-CaseId012" -InPlaceHoldEnabled $false; Remove-MailboxSearch "Hold-CaseId012"
```

For detailed syntax and parameter information, see [Set-Mailboxsearch](https://docs.microsoft.com/powershell/module/exchange/set-mailboxsearch).

### How do you know this worked?

To verify that you have successfully removed an In-Place Hold, do one of the following:

- Use the EAC to verify that the In-Place Hold doesn't appear in the list view of the **In-Place eDiscovery & Hold** tab.

- Use the **Get-MailboxSearch** cmdlet to retrieve all mailbox searches and check that the search you removed is no longer listed.

## More information

 **How does In-Place Hold work?**: If a mailbox or public folder is not on hold, an item is moved to the Deletions subfolder in the Recoverable Items folder when it's permanently deleted (Shift + Delete) or deleted from the Deleted Items folder. A deletion policy (how long items are set to be retained) also moves items to the Deletions subfolder when the retention period expires. When a user purges an item in the Recoverable Items folder or when the deleted item retention period expires for an item, the item is moved to the Purges subfolder and marked for permanent deletion. It will be then be purged from Exchange the next time the mailbox is processed by the Managed Folder Assistant (MFA).

When an In-Place Hold is placed on a mailbox or public folder, purged items are not moved to the Purges subfolder but are instead moved to the DiscoveryHolds subfolder and are preserved for the hold duration specified by the In-Place Hold. The hold duration is calculated from the original date an item was received or created, and defines how long items in the DiscoveryHolds subfolder are held. When the hold duration expires for an item in the DiscoveryHolds subfolder, the item it is marked for permanent deletion and will be purged from Exchange the next time the mailbox or public folder is processed by the MFA. If an indefinite hold is placed on a mailbox or public folder, items will never be purged from the DiscoveryHolds subfolder.

The following illustration shows the subfolders in the Recoverable Items folders and the hold workflow process.

![Recoverable Items folder](../../media/ITPro_RecoverableItems.gif)

> [!NOTE]
> If a mailbox is place on Litigation Hold, purged items are moved to the Purges subfolder and preserved for the hold duration configured for the Litigation Hold.
