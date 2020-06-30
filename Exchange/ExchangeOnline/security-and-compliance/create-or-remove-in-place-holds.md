---
localization_priority: Normal
description: An In-Place Hold preserves all mailbox content, including deleted items and original versions of modified items. All such mailbox items are returned in an In-Place eDiscovery search. When you place an In-Place Hold on a user's mailbox on, the contents in the corresponding archive mailbox (if it's enabled) are also placed on hold, and returned in a eDiscovery search.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9d5d8d37-a053-4830-9cb1-6e1ede25e963
ms.reviewer: 
f1.keywords:
- NOCSH
title: Create or remove an In-Place Hold
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Create or remove an In-Place Hold

> [!IMPORTANT]
>  As we continue to invest in different ways to preserve mailbox content, we're announcing the retirement of In-Place Holds in the Exchange admin center (EAC) in Exchange Online. Starting July 1, 2020, you won't be able to create new In-Place Holds. But you'll still be able to manage In-Place Holds in the EAC or by using the **Set-MailboxSearch** cmdlet in Exchange Online PowerShell. However, starting October 1, 2020, you won't be able to manage In-Place Holds. You'll only be remove them in the EAC or by using the **Remove-MailboxSearch** cmdlet. Using In-Place Holds in Exchange Server and Exchange hybrid deployments will still be supported. For more information about the retirement of In-Place Holds in Exchange Online, see [Retirement of legacy eDiscovery tools](https://docs.microsoft.com/microsoft-365/compliance/legacy-ediscovery-retirement).

An In-Place Hold preserves all mailbox content, including deleted items and original versions of modified items. All such mailbox items are returned in an [In-Place eDiscovery](in-place-ediscovery/in-place-ediscovery.md) search. When you place an In-Place Hold on a user's mailbox on, the contents in the corresponding archive mailbox (if it's enabled) are also placed on hold, and returned in a eDiscovery search.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place Hold" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

- To place an Exchange Online mailbox on In-Place Hold, it must be assigned an Exchange Online (Plan 2) license. If a mailbox is assigned an Exchange Online (Plan 1) license, you would have to assign it a separate Exchange Online Archiving license to place it on hold.

- Depending on your Active Directory topology and replication latency, it may take up to an hour for an In-Place Hold to take effect.

- As previously explained, when you place an In-Place Hold on a user's mailbox, content in the user's archive mailbox is also placed on hold. If you place an In-Place Hold on an on-premises primary mailbox in an Exchange hybrid deployment, the cloud-based archive mailbox (if enabled) is also placed on hold.

- If a user is placed on multiple In-Place Holds, the search queries from any query-based hold are combined (with **OR** operators). In this case, the maximum number of keywords in all query-based holds placed on a mailbox is 500. If there are more than 500 keywords, then all content in the mailbox is placed on hold (not just that content that matches the search criteria). All content is held until the total number of keywords is reduced to 500 or less.

- In Exchange Online, the quota for the Recoverable Items folder is automatically increased to 100 GB when you place an In-Place Hold on a mailbox. The default size of the Recoverable Items folder is 30 GB.

- In Exchange Online, you can place an In-Place hold on Microsoft 365 groups. When you place a Microsoft 365 group on hold, the group mailbox is placed on hold; the mailboxes of the group members aren't placed on hold. For information about Microsoft 365 groups, see [Learn about Microsoft 365 groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa).

## Create an In-Place Hold

### Use the EAC to create an In-Place Hold

1. Navigate to **Compliance management** \> **In-place eDiscovery & hold**.

2. Click **New** ![Add Icon](../media/ITPro_EAC_AddIcon.gif).

3. In **In-Place eDiscovery & Hold**, on the **Name and description** page, type a name for the search and an optional description, and then click **Next**.

4. On the **Mailboxes and Public folders** page, choose the content locations that you want to place on hold and then click **Next**.

   ![Choose the content locations to place on hold](../media/bbe76c50-a93b-4e5e-acd2-78e0d747ea19.png)

   1. **Search all mailboxes**: You can't select this option to create an In-Place Hold. You can select this option for In-Place eDiscovery searches, but to create an In-Place Hold, you must select the specific mailboxes that you want to place on hold.

   2. **Don't search any mailboxes**: Select this option when you're creating an In-Place Hold exclusively for public folders.

   3. **Specify mailboxes to search**: Select this option and then click **Add** ![Add Icon](../media/ITPro_EAC_AddIcon.gif) to select the mailboxes or distribution groups that you want to place on hold. In Exchange Online, you can also select Microsoft 365 groups to place on hold.

   4. **Search all public folders**: In Exchange Online, you can select this checkbox to place all public folders in your organization on hold. As previously explained, to create an In-Place Hold only for public folders, be sure to select the **Don't search any mailboxes** option.

5. On the **Search query** page, complete the following fields, and then click **Next**:

   - **Include all user mailbox content**: Click this button to place all content in selected mailboxes on hold.

   - **Filter based on criteria**: Click this button to specify search criteria, including keywords, start and end dates, sender and recipient addresses, and message types. When you create a query-based hold, only items that match the search criteria are preserved.

     > [!TIP]
    > When you place public folders on In-Place Hold, email messages related to the public folder hierarchy synchronization process are also preserved. This might result in thousands of hierarchy synchronization related email items being preserved. These messages can fill up the storage quota for the Recoverable Items folder on public folder mailboxes. To prevent this, you can create a query-based In-Place Hold and add the following `property:value` pair to the search query: > `NOT(subject:HierarchySync*)`> The result is that any message (related to the synchronization of the public folder hierarchy) that contains the phrase "HierarchySync" in the subject line is not placed on hold.

6. On the **In-Place Hold settings** page, select the **Place content matching the search query in selected mailboxes on hold** check box and then select one of the following options:

   - **Hold indefinitely**: Click this button to place items returned by the search on an indefinite hold. Items on hold will be preserved until you remove the mailbox from the search or remove the search.

   - **Specify number of days to hold items relative to their received date**: Click this button to hold items for a specific period. For example, you can use this option if your organization requires that all messages be retained for at least seven years. You can use a time-based In-Place Hold along with a retention policy to make sure items are deleted in seven years. To learn more about retention polices, see [Retention tags and retention policies](messaging-records-management/retention-tags-and-policies.md).

### Use Exchange Online PowerShell to create an In-Place Hold

This example creates an In-Place Hold named Hold-CaseId012 and adds the mailbox joe@contoso.com to the hold.

> [!IMPORTANT]
> If you don't specify additional search parameters for an In-Place Hold, all items in the specified source mailboxes are placed on hold. If you don't specify the _ItemHoldPeriod_ parameter, items are placed on hold indefinitely or until the mailbox is either removed from hold or the hold is deleted.

```PowerShell
New-MailboxSearch "Hold-CaseId012"-SourceMailboxes "joe@contoso.com" -InPlaceHoldEnabled $true
```

For detailed syntax and parameter information, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch).

### How do you know this worked?

To verify that you have successfully created the In-Place Hold, do one of the following:

- Use the EAC to verify that the In-Place Hold is listed in the list view of the **In-place eDiscovery & hold** tab.

- Use the **Get-MailboxSearch** cmdlet to retrieve the mailbox search and check the search parameters. For an example of how to retrieve a mailbox search, see the examples in [Get-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/get-mailboxsearch).

## Remove an In-Place Hold

> [!IMPORTANT]
> In Exchange Server, mailbox searches can be used for an In-Place Hold and In-Place eDiscovery. You can't remove a mailbox search that's used for In-Place Hold. You must first disable the In-Place Hold by clearing the **Place content matching the search query in selected mailboxes on hold** check box on the **In-Place Hold settings** page or by setting the _InPlaceHoldEnabled_ parameter to `$false` in Exchange Online PowerShell. You can also remove a mailbox by using the _SourceMailboxes_ parameter specified in the search.

### Use the EAC to remove an In-Place Hold

1. Navigate to **Compliance management** \> **In-Place eDiscovery & hold**.

2. In the list view, select the In-Place Hold you want to remove and then click **Edit** ![Edit icon](../media/ITPro_EAC_EditIcon.gif).

3. In **In-Place eDiscovery & Hold** properties, on the **In-Place Hold** page, clear the **Place content matching the search query in selected mailboxes on hold**, and then click **Save**.

4. Select the In-Place Hold again from the list view, and then click **Delete** ![Delete icon](../media/ITPro_EAC_DeleteIcon.gif).

5. In warning, click **Yes** to remove the search.

### Use Exchange Online PowerShell to remove an In-Place Hold

This example first disables In-Place Hold named Hold-CaseId012 and then removes the mailbox search.

```PowerShell
Set-MailboxSearch "Hold-CaseId012" -InPlaceHoldEnabled $false
Remove-MailboxSearch "Hold-CaseId012"
```

For detailed syntax and parameter information, see [Set-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/set-mailboxsearch).

### How do you know this worked?

To verify that you have successfully removed an In-Place Hold, do one of the following:

- Use the EAC to verify that the In-Place Hold doesn't appear in the list view of the **In-place eDiscovery & hold** tab.

- Use the **Get-MailboxSearch** cmdlet to retrieve all mailbox searches and check that the search you removed is no longer listed. For an example of how to retrieve a mailbox search, see the examples in [Get-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/get-mailboxsearch).
