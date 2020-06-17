---
title: 'Place all mailboxes on hold: Exchange 2013 Help'
TOCTitle: Place all mailboxes on hold
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 4c141604-3210-44cc-b98e-f3e0f15613b8
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Place all mailboxes on hold in Exchange 2013

_**Applies to:** Exchange Server 2013_

Your organization may require all mailbox data to be preserved for a specific period. You can use Litigation Hold or In-Place Hold to meet this requirement. After you place a mailbox on Litigation Hold or In-Place Hold, mailbox items that are modified or that are permanently deleted are preserved in the Recoverable Items folder. For more information, see [In-Place Hold and Litigation Hold](in-place-and-litigation-holds-exchange-2013-help.md).

Before you place all mailboxes in an organization on Litigation Hold or In-Place Hold, consider the following:

- When you place mailboxes on hold, content in a user's archive mailbox (if it's enabled) is also placed on hold.

- Placing all mailboxes in an organization on hold can significantly impact mailbox sizes. In an Exchange Server 2013 deployment, plan for adequate storage to meet your organization's preservation requirements.

- Preserving mailbox data for a long duration will result in growth of the Recoverable Items folder in a user's primary mailbox and archive mailbox. The Recoverable Items folder has its own storage limit, so items in the folder don't count towards the mailbox storage limit. In Exchange Server 2013, the default storage limit for the Recoverable Items folder is 30 GB. We recommend that you periodically monitor the size of this folder to ensure it doesn't reach the limit. For more information, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

## Choosing between Litigation Hold and In-Place Hold

Here are some factors to consider when deciding the hold feature you should use to place all mailboxes in your organization on hold.

|**You want to...**|**Use Litigation Hold**|**Use In-Place Hold**|
|:-----|:-----|:-----|
|Use the EAC|Yes  <br/> For setting Litigation Hold, EAC is best suited for quick one-off actions on a few mailboxes. We recommend using the Shell for setting Litigation Hold for all users in your organization.|Yes  <br/> However, you can't select more than 500 source mailboxes in the EAC.|
|Use the Shell|Yes|Yes|
|Place more than 10,000 mailboxes on hold|Yes  <br/> Litigation Hold is a mailbox property. You can place all mailboxes in an organization on hold by using the **Set-Mailbox** cmdlet.|Yes; use multiple In-Place Holds  <br/> You can use distribution groups to specify a maximum of 10,000 mailboxes in a single In-Place Hold. To place additional mailboxes on hold, you must create additional In-Place Holds. This will result in additional management overhead. Using Litigation Hold placing large numbers of mailboxes on hold is simpler.|
|Place many different mailboxes on hold for different periods.|Yes  <br/> Litigation Hold is a mailbox property. You can place each mailbox (or sets of mailboxes) on hold for a different duration.  <br/> See the [More information](#more-information) section for examples of using recipient properties in a filter so you can place a Litigation Hold on a subset of mailboxes.|Yes  <br/> If you're placing individual holds on thousands of mailboxes, we recommend using Litigation Hold. If you're creating holds for specific events that involve multiple users, use a single in-Place hold for the group of users.  <br/> It's not recommended to create separate In-place Holds for each mailbox as this will create many In-Place Hold queries that will be more difficult to manage than Litigation Holds. A large number of In-Place Hold objects may also result in slow performance in the EAC when refreshing, creating, or modifying hold objects.|
|Automatically place new mailboxes on hold|No  <br/> You have to place a new mailbox on Litigation Hold after it's created. You can schedule the command or script to run as frequently as required to achieve the same effect.|No  <br/> You have to add a new mailbox to an In-Place Hold, even if you specified a distribution group when you created the In-Place Hold. You can also schedule the command or script to run as frequently as required to achieve the same effect. We recommend that the script check if an existing In-Place Hold has already reached the 10,000 mailbox limit, and then create a new In-Place Hold if required.|
|Place all public folders on hold|No|Yes|

## Place all mailboxes on Litigation Hold

You can easily and quickly place all mailboxes on hold indefinitely or for a specified duration using the Shell. This command places all mailboxes on hold for 2555 days (approximately 7 years).

```powershell
Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -LitigationHoldEnabled $true -LitigationHoldDuration 2555
```

The example uses the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) cmdlet and a recipient filter to retrieve all user mailboxes in the organization, and then pipes the list of mailboxes to the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet to enable the Litigation Hold and specify a hold duration. For more information, see [Place a mailbox on Litigation Hold](place-a-mailbox-on-litigation-hold-exchange-2013-help.md).

## Place all mailboxes on In-Place Hold

You can use the EAC to select up to 500 mailboxes and place them on hold. For details, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md).

> [!TIP]
> To place more than 500 users on In-Place Hold, use the Shell. See [New-MailboxSearch](https://docs.microsoft.com/powershell/module/exchange/new-mailboxsearch).

## More information

- When you place all mailboxes in your organization on hold, only the mailboxes that exist at the time you run the command are placed on hold. If you create new mailboxes later, run the command again to place them on hold. If you frequently create new mailboxes, you can run the command as a scheduled task as frequently as required.

- Placing mailboxes on hold preserves data by preventing deletion before the specified period and saving the original version of a message before it's modified. It doesn't automatically delete messages after the specified period. Combine Litigation Hold or In-Place Hold with a Retention Policy, which can automatically delete messages after the specified period, to meet your organization's email retention requirements. See [Retention tags and retention policies](retention-tags-and-policies-exchange-2013-help.md) for details.

- The PowerShell command used in this topic to place a Litigation Hold on all mailboxes uses a recipient filter that returns all user mailboxes. You can use other recipient properties to return a list of specific mailboxes that you can then pipe to the **Set-Mailbox** cmdlet to place a Litigation Hold on those mailboxes.

    Here are some examples of using the **Get-Mailbox** and **Get-Recipient** cmdlets to return a subset of mailboxes based on common user or mailbox properties. These examples assume that relevant mailbox properties (such as _CustomAttributeN_ or _Department_) have been populated.

  ```powershell
  Get-Mailbox -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'CustomAttribute15 -eq "OneYearLitigationHold"'
  ```

  ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'Department -eq "HR"'
  ```

  ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'PostalCode -eq "98052"'
  ```

  ```powershell
  Get-Recipient -RecipientTypeDetails UserMailbox -ResultSize unlimited -Filter 'StateOrProvince -eq "WA"'
  ```

  ```powershell
  Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -ne 'DiscoveryMailbox'"
  ```

  You can use other user mailbox properties in a filter to include or exclude mailboxes. For details, see [Filterable Properties for the -Filter Parameter](https://docs.microsoft.com/powershell/exchange/filter-properties).
