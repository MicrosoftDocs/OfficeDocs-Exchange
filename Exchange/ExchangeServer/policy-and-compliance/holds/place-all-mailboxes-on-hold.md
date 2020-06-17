---
localization_priority: Normal
description: Administrators can learn about the differences between Litigation Hold and In-Place Hold, and how to place mailboxes on In-Place Hold.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4c141604-3210-44cc-b98e-f3e0f15613b8
ms.reviewer:
title: Place all mailboxes on hold
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Place all mailboxes on hold

Your organization may require all mailbox data to be preserved for a specific period. You can use Litigation Hold or In-Place Hold to meet this requirement. After you place a mailbox on Litigation Hold or In-Place Hold, mailbox items that are modified or that are permanently deleted are preserved in the Recoverable Items folder for the duration specified by the hold. For more information, see [In-Place Hold and Litigation Hold in Exchange Server](holds.md).

Before you place all mailboxes in an organization on Litigation Hold or In-Place Hold, consider the following:

- When you place mailboxes on hold, content in a user's archive mailbox (if it's enabled) is also placed on hold.

- Placing all mailboxes in an organization on hold can significantly impact mailbox sizes. In an Exchange Server deployment, plan for adequate storage to meet your organization's preservation requirements.

- Preserving mailbox data for a long duration will result in growth of the Recoverable Items folder in a user's primary mailbox and archive mailbox. The Recoverable Items folder has its own storage limit, so items in the folder don't count towards the mailbox storage limit. In Exchange Server, the default storage limit for the Recoverable Items folder is 30 GB. We recommend that you periodically monitor the size of this folder to ensure it doesn't reach the limit. For more information, see [Recoverable Items folder in Exchange Server](../../policy-and-compliance/recoverable-items-folder/recoverable-items-folder.md).

## Choosing between Litigation Hold and In-Place Hold

Here are some factors to consider when deciding the hold feature you should use to place all mailboxes in your organization on hold.

|**If you want to...**|**Use Litigation Hold**|**Use In-Place Hold**|
|:-----|:-----|:-----|
|Use the EAC|Yes <br/><br/> For setting a Litigation Hold, the EAC is best suited for quick one-off actions on a few mailboxes. We recommend using the Exchange Management Shell for placing a Litigation Hold for all users in your organization. For details, see [Place a mailbox on Litigation Hold](litigation-holds.md).|Yes <br/><br/> However, you can't select more than 500 source mailboxes in the EAC. For details, see [Create or remove an In-Place Hold](in-place-holds.md).|
|Use the Exchange Management Shell|Yes|Yes|
|Place more than 10,000 mailboxes on hold|Yes <br/><br/> Litigation Hold is a mailbox property. You can place all mailboxes in an organization on hold by using the **Set-Mailbox** cmdlet.|Yes; use multiple In-Place Holds <br/><br/> You can use distribution groups to specify a maximum of 10,000 mailboxes in a single In-Place Hold. To place additional mailboxes on hold, you must create additional In-Place Holds. This will result in additional management overhead. Using Litigation Hold placing large numbers of mailboxes on hold is simpler.|
|Place many different mailboxes on hold for different periods.|Yes <br/><br/> Litigation Hold is a mailbox property. You can place each mailbox (or sets of mailboxes) on hold for a different duration. <br/><br/> See the [More information](#more-information) section for examples of using recipient properties in a filter so you can place a Litigation Hold on a subset of mailboxes.|Yes <br/><br/> If you're placing individual holds on thousands of mailboxes, we recommend using Litigation Hold. However, if you're creating holds for specific events that involve multiple users (such as a legal case), use a single in-Place hold for the group of users. <br/><br/> It's not recommended to create separate In-Place Holds for each mailbox as this will create many In-Place Hold queries. This will be more difficult to manage than Litigation Holds. A large number of In-Place Hold objects may also result in slow performance in the EAC when refreshing, creating, or modifying In-Place eDiscovery or In-Place Hold objects.|
|Automatically place new mailboxes on hold|No <br/><br/> You have to place a new mailbox on Litigation Hold after it's created. You can schedule the command or script to run as frequently as required to achieve the same effect.|No <br/><br/> You have to add a new mailbox to an existing In-Place Hold, even if you specified a distribution group when you created the In-Place Hold. You can also schedule the command or script to run as frequently as required to achieve the same effect. We recommend that the script check if an existing In-Place Hold has already reached the 10,000 mailbox limit, and then create a new In-Place Hold if required.|

## Place all mailboxes on Litigation Hold

You can easily and quickly place all mailboxes on hold indefinitely or for a specified hold duration using the Exchange Management Shell. This command places all mailboxes on hold with a hold duration of 2555 days (approximately 7 years).

```PowerShell
Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -LitigationHoldEnabled $true -LitigationHoldDuration 2555
```

The example uses the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) cmdlet and a recipient filter to retrieve all user mailboxes in the organization, and then pipes the list of mailboxes to the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet to enable the Litigation Hold and specify a hold duration. For more information, see [Place a mailbox on Litigation Hold](litigation-holds.md).

## Place all mailboxes on In-Place Hold

You can use the EAC to select up to 500 mailboxes and place them on hold. For details, see [Create or remove an In-Place Hold](in-place-holds.md).

To place more than 500 users on In-Place Hold, use the Exchange Management Shell. For details, see [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch).

> [!TIP]
> In hybrid environments, you can use the inactive mailbox feature in Exchange Online to retain mailboxes without consuming a license or requiring an account for the mailbox. The inactive mailbox feature requires an "Exchange Online Plan 2, Office 365 Enterprise E3 and E5 subscriptions" license. If you have an Exchange Online Plan 1 license, you need to assign a separate "Exchange Online Archiving" license to the mailbox. For more information, see [Inactive mailbox](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365).

## More information

- When you place all mailboxes in your organization on hold, only the mailboxes that exist at the time you run the command are placed on hold. If you create new mailboxes later, run the command again to place them on hold. If you frequently create new mailboxes, you can run the command as a scheduled task as frequently as required.

- Placing mailboxes on hold preserves data by preventing items in the Recoverable Items folder from being deleted until the specified hold duration for an item expires. If a hold is configured to hold items indefinitely, items won't be purged from a mailbox. Also, when a mailbox is on hold the original version of a message is saved before it's modified. Combine Litigation Hold or In-Place Hold with a Retention Policy, which can automatically delete messages (and move them into the Recoverable Items folder) after a specified period, to meet your organization's email retention requirements. See [Retention tags and retention policies in Exchange Server](../mrm/retention-tags-and-retention-policies.md) for details.

- The Exchange Management Shell command used in this topic to place a Litigation Hold on all mailboxes uses a recipient filter that returns all user mailboxes. You can use other recipient properties to return a list of specific mailboxes that you can then pipe to the **Set-Mailbox** cmdlet to place a Litigation Hold on those mailboxes.

   Here are some examples of using the **Get-Mailbox** and **Get-Recipient** cmdlets to return a subset of mailboxes based on common user or mailbox properties. These examples assume that relevant mailbox properties (such as _CustomAttributeN_ or _Department_) have been populated.

   ```PowerShell
   Get-Mailbox -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'CustomAttribute15 -eq "OneYearLitigationHold"'
   ```

   ```PowerShell
   Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'Department -eq "HR"'
   ```

   ```PowerShell
   Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'PostalCode -eq "98052"'
   ```

   ```PowerShell
   Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'StateOrProvince -eq "WA"'
   ```

   ```PowerShell
   Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -ne 'DiscoveryMailbox'"
   ```

   You can use other user mailbox properties in a filter to include or exclude mailboxes. For details, see [Filterable Properties for the -Filter Parameter](https://docs.microsoft.com/powershell/exchange/filter-properties).
