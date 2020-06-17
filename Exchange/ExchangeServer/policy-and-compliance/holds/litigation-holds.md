---
localization_priority: Normal
description: Place a mailbox on Litigation Hold to preserve all mailbox content, including deleted items and original versions of modified items.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: adee4621-3626-4aec-aa53-00b35ff0d0b0
ms.reviewer:
title: Place a mailbox on Litigation Hold
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Place a mailbox on Litigation Hold

Place a mailbox on Litigation Hold to preserve all mailbox content, including deleted items and original versions of modified items. When you place a mailbox on Litigation Hold, the user's archive mailbox (if it's enabled) is also placed on hold. Deleted and modified items are preserved for a specified period or until you remove the mailbox from Litigation Hold. All such mailbox items are returned in an [In-Place eDiscovery in Exchange Server](../../policy-and-compliance/ediscovery/ediscovery.md) search.

## Before you begin

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "In-Place Hold" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

- The Litigation Hold setting may take up to 60 minutes to take effect.

- Litigation Hold preserves items in the Recoverable Items folder in the user's mailbox. The default size for this folder is 30 GB. Depending on number and size of items deleted or modified, the size of the Recoverable Items folder of the mailbox may increase quickly. The Recoverable Items folder is configured with a high quota by default. We recommend that you monitor mailboxes that are placed on Litigation Hold on a weekly basis to ensure they don't reach the limits of the Recoverable Items quotas.

- As previously explained, when you place a user's mailbox on Litigation Hold, the user's archive mailbox is also placed on hold.

- Litigation Hold preserves deleted items and also preserves original versions of modified items until the hold is removed. You can optionally specify a hold duration, which preserves a mailbox item for the specified duration period. If you specify a hold duration period, it's calculated from the date a message is received or a mailbox item is created.

- See the [More information](#more-information) section for a description of the Litigation Hold workflow process.

## Use the EAC to place a mailbox on Litigation Hold

1. Go to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to place on Litigation Hold, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox features**.

4. Under **Litigation hold: Disabled**, click **Enable** to place the mailbox on Litigation Hold.

5. On the **Litigation Hold** page, enter the following optional information:

   - **Litigation hold duration (days)**: Use this box to specify how long mailbox items are held when the mailbox is placed on Litigation Hold. The duration is calculated from the date a mailbox item is received or created. If you leave this box blank, items are held indefinitely or until the hold is removed. Use days to specify the duration.

   - **Note**<sup>*</sup>: Use this box to inform the user their mailbox is on Litigation Hold. The note will appear on the **File** tab in Outlook 2010 or later.

   - **URL**<sup>*</sup>: Use this box to direct the user to a website for more information about Litigation Hold. This URL appears on the **File** tab Outlook 2010 or later.

   <sup>*</sup>If you leave the **Note** and **URL** values blank, the user isn't notified that you placed a litigation hold on their mailbox.

6. Click **Save** on the **Litigation Hold** page, and then click **Save** on the mailbox properties page.

## Use the Exchange Management Shell to place a mailbox on Litigation Hold indefinitely

This example places the mailbox bsuneja@contoso.com on Litigation Hold. Items in the mailbox are held indefinitely or until the hold is removed.

```PowerShell
Set-Mailbox bsuneja@contoso.com -LitigationHoldEnabled $true
```

> [!NOTE]
> When you place a mailbox on Litigation Hold indefinitely (by not specifying a duration period), the value for the _LitigationHoldDuration_ property mailbox is set to `Unlimited`.

## Use the Exchange Management Shell to place a mailbox on Litigation Hold and preserve items for a specified duration

This example places the mailbox bsuneja@contoso.com on Litigation Hold and preserves items for 2555 days (approximately 7 years).

```PowerShell
Set-Mailbox bsuneja@contoso.com -LitigationHoldEnabled $true -LitigationHoldDuration 2555
```

## Use the Exchange Management Shell to place all mailboxes on Litigation Hold

Your organization may require that all mailbox data be preserved.

This example places all user mailboxes in the organization on Litigation Hold and sets the hold duration for one year (365 days).

```PowerShell
Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -LitigationHoldEnabled $true -LitigationHoldDuration 365
```

The example uses the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) cmdlet to retrieve all mailboxes in the organization, specifies a recipient filter to include all user mailboxes, and then pipes the list of mailboxes to the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) cmdlet to enable the Litigation Hold and set the hold duration.

To place all user mailboxes on an indefinite hold, run the previous command but don't include the _LitigationHoldDuration_ parameter.

See the [More information](#more-information) section for examples of using other recipient properties in a filter to include or exclude one or more mailboxes.

## Use the Exchange Management Shell to remove a mailbox from Litigation Hold

This example removes the mailbox bsuneja@contoso.com from Litigation Hold.

```PowerShell
Set-Mailbox bsuneja@contoso.com -LitigationHoldEnabled $false
```

## How do you know this worked?

To verify that you have successfully placed a mailbox on Litigation Hold, do the one of the following:

- In the EAC:

  1. Go to **Recipients** \> **Mailboxes**.

  2. In the list of user mailboxes, click the mailbox that you want to verify Litigation Hold settings for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

  3. On the mailbox properties page, click **Mailbox features**.

  4. Under **Litigation hold**, verify that hold is enabled.

  5. Click **View details** to verify when the mailbox was placed on Litigation Hold and by whom. You can also verify or change the values in the optional **Litigation hold duration (days)**, **Note**, and **URL** boxes.

- In the Exchange Management Shell, run one of the following commands:

  ```PowerShell
  Get-Mailbox <name of mailbox> | Format-List LitigationHold*
  ```

    or

  ```PowerShell
  Get-Mailbox -ResultSize Unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,LitigationHold*
  ```

    If a mailbox is placed on Litigation Hold indefinitely, the value for the _LitigationHoldDuration_ property mailbox is set to `Unlimited`.

## More information

- **How does Litigation Hold work?** In the normal deleted item workflow, a mailbox item is moved to the Deletions subfolder in the Recoverable Items folder when a user permanently deletes it (Shift + Delete) or deletes it from the Deleted Items folder. A deletion policy (which is a retention tag configured with a Delete retention action) also moves items to the Deletions subfolder when the retention period expires. When a user purges an item in the Recoverable Items folder or when the deleted item retention period expires for an item, it's moved to the Purges subfolder in the Recoverable Items folder and marked for permanent deletion. It will be purged from Exchange the next time the mailbox is processed by the Managed Folder Assistant (MFA).

  When a mailbox is placed on Litigation Hold, items in the Purges subfolder are preserved for the hold duration specified by the Litigation Hold. The hold duration is calculated from the original date an item was received or created, and defines how long items in the Purges subfolder are held. When the hold duration expires for an item in the Purges subfolder, the item is marked for permanent deletion and will be purged from Exchange the next time the mailbox is processed by the MFA. If an indefinite hold is placed on a mailbox, items will never be purged from the Purges subfolder.

  The following illustration shows the subfolders in the Recoverable Items folders and the hold workflow process.

  ![Recoverable Items folder](../../media/ITPro_RecoverableItems.gif)

  > [!NOTE]
  > If an In-Place Hold is placed on a mailbox, purged items are moved from the Deletions subfolder to the DiscoveryHolds subfolder and are preserved for the hold duration for the In-Place Hold.

- If your organization requires that all mailbox data has to preserved for a specific period of time, consider the following before you place all mailboxes in an organization on Litigation Hold.

  - When you use the previous command to place a hold on all mailboxes in an organization (or a subset of mailboxes matching a specified recipient filter) only mailboxes that exist at the time that you run the command are placed on hold. If you create new mailboxes later, you have to run the command again to place the new mailboxes on hold. If you create new mailboxes often, you can run the command as a scheduled task as frequently as required.

  - Placing all mailboxes on Litigation Hold can significantly impact mailbox sizes. In an Exchange 2016 or Exchange 2019 organization, plan for adequate storage to meet your organization's preservation requirements.

  - The Recoverable Items folder has its own storage limit, so items in the folder don't count towards the mailbox storage limit. As previously explained, preserving mailbox data for a long period of time will result in growth of the Recoverable Items folder in a user's mailbox and archive. We recommend that you periodically monitor the size of this folder by using the **Get-MailboxFolderStatistics** cmdlet to ensure it doesn't reach the limit. For more information, see:

    - [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/get-mailboxfolderstatistics)

    - [Clean up or delete items from the Recoverable Items folder](../recoverable-items-folder/clean-up-deleted-items.md).

- The previous command to place a hold on all mailboxes uses a recipient filter that returns all user mailboxes. You can use other recipient properties to return a list of specific mailboxes that you can then pipe to the **Set-Mailbox** cmdlet to place a Litigation Hold on those mailboxes.

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
