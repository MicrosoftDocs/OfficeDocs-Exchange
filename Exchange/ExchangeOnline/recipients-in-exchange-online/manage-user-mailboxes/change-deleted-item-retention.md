---
localization_priority: Normal
description: Learn how to use Exchange Online PowerShell to change the deleted item retention period for Exchange Online mailboxes.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: ce17f1ec-b96c-4c9e-b20a-507fe0afc684
ms.reviewer: 
f1.keywords:
- NOCSH
title: Change how long permanently deleted items are kept for an Exchange Online mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Change how long permanently deleted items are kept for an Exchange Online mailbox

If you've *permanently* deleted an item in Microsoft Outlook or Outlook on the web (formerly known as Outlook Web App), the item is moved to a folder ( **Recoverable Items** \> **Deletions**) and kept there for 14 days, by default. You can change how long items are kept, up to a maximum of 30 days.

> [!NOTE]
> You must use the new Exchange admin center (EAC) or Exchange Online PowerShell to make the change. Unfortunately, you can't currently do this directly in the Outlook or Outlook on the web.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- If you want to place a mailbox on [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md) so the retention limit is ignored, make sure the mailbox has an Exchange Online (Plan 2) user license.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" section in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- You can use either the new EAC or Exchange Online PowerShell to perform this procedure. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Change how long permanently deleted items are kept using the new Exchange admin center

1. In the new Exchange admin center (EAC), navigate to **Recipients > Mailboxes**.

   The **Mailboxes** page is displayed.
   
2. Select the mailbox for which you want to edit the duration of the deleted item's retention. Click on the display name.

   The properties page is displayed.

3. Under **More actions**, click  **Recover deleted items** and perform the following actions:
    1. To Choose the predefined time durations, select any of options from the **Time** drop-down list, except **Custom date range**
    1. To customize the duration, select the **Custom date range**.
    1. Select the time period from the **Start date** and **End date** fields.
    1. Enter the subject of the deleted message in the **Subject line search**.
  
  >[!NOTE]
  >This option is used if you want to retrieve a message through its subject.

## Change how long permanently deleted items are kept

In these examples, we increase the retention period to 30 days, the maximum for Exchange Online mailboxes. But you can set the number to whatever you like, up to that limit.

**Example 1:** Set Emily Maier's mailbox to keep deleted items for 30 days. In Exchange Online PowerShell, run the following command.

```PowerShell
Set-Mailbox -Identity "Emily Maier" -RetainDeletedItemsFor 30
```

**Example 2:** Set all user mailboxes in the organization to keep deleted items for 30 days. In Exchange Online PowerShell, run the following command.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -RetainDeletedItemsFor 30
```

Need more details about using these commands? See Exchange Online PowerShell Help topic [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

> [!NOTE]
> These commands only apply to existing mailboxes and will not affect new mailboxes that you create in the future. To change this setting on all new mailboxes, use a mailbox plan that has a new retention policy that applies to new mailboxes. See [Mailbox plans](mailbox-plans.md) and [Set-MailboxPlan](https://docs.microsoft.com/powershell/module/exchange/set-mailboxplan) for more information.

> [!TIP]
> To keep deleted items for longer than 30 days, place the mailbox on In-Place Hold or Litigation Hold. This works because when a mailbox is placed on hold, deleted items are kept and retention settings for deleted items are ignored. See [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md).

## Check to be sure the value is changed

To check for one mailbox, run the following command:

```PowerShell
Get-Mailbox <Name> | Format-List RetainDeletedItemsFor
```

Or to check for all mailboxes, run the following command:

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Format-List Name,RetainDeletedItemsFor
```

## More about deleted items and retention time

When a user permanently deletes a mailbox item (such as an email message, a contact, a calendar appointment, or a task) in Microsoft Outlook and Outlook on the web, the item is moved to the **Recoverable Items** folder, and into a subfolder named **Deletions**.

A mailbox item is deleted and moved to the **Recoverable Items** folder when a user does one of the following:

- Deletes an item from the Deleted Items folder

- Empties the Deleted Items folder

- Permanently deletes an item by selecting it and pressing **Shift+Delete**

 How long deleted items are kept in the **Deletions** folder depends on the deleted item retention period that is set for the mailbox. An Exchange Online mailbox keeps deleted items for 14 days, by default. Use Exchange Online PowerShell, as shown above, to change this setting, to increase the period up to a maximum of 30 days.

Users can recover, or purge, deleted items before the retention time for a deleted item expires. To do so, they use the **Recover Deleted Items** feature in Outlook or Outlook on the web. See the following topics for [Outlook for Windows](https://support.microsoft.com/office/49e81f3c-c8f4-4426-a0b9-c0fd751d48ce) or for [Outlook on the web](https://support.microsoft.com/office/98b5a90d-4e38-415d-a030-f09a4cd28207).

Additional help:

- If a user purges a deleted item, you can recover it before the deleted item retention period expires. For details, see [Recover deleted messages in a user's mailbox](recover-deleted-messages.md).

- To learn more about deleted item retention, the Recoverable Items folder, In-Place Hold, and Litigation Hold, see [Recoverable Items folder in Exchange Online](../../security-and-compliance/recoverable-items-folder/recoverable-items-folder.md).
