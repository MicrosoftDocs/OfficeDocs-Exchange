---
title: 'Enable or disable single item recovery for a mailbox: Exchange 2013 Help'
TOCTitle: Enable or disable single item recovery for a mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 2e7f1bcd-8395-45ad-86ce-22868bd46af0
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable single item recovery for a mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Shell to enable or disable single item recovery on a mailbox. In Exchange Server, single item recovery is disabled when a mailbox is created. If single item recovery is enabled, messages that are permanently deleted (purged) by the user are retained in the Recoverable Items folder of the mailbox until the deleted item retention period expires. This lets an administrator recover messages purged by the user before the deleted item retention period expires. Also, if a message is changed by a user or a process, copies of the original item are also retained when single item recovery is enabled.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Retention and legal holds" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to enable or disable single item recovery.

- In Exchange Server, the mailbox uses the deleted item retention settings of the mailbox database, by default. The deleted item retention period for a mailbox database is set to 14 days, but you can override the default by configuring this setting on a per-mailbox basis. For details, see [Configure Deleted Item retention and Recoverable Items quotas](configure-deleted-item-retention-and-recoverable-items-quotas-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to enable single item recovery

This example enables single item recovery for the mailbox of April Summers.

```powershell
Set-Mailbox -Identity "April Summers" -SingleItemRecoveryEnabled $true
```

This example enables single item recovery for the mailbox of Pilar Pinilla and sets the number of days that deleted items are retained to 30 days.

```powershell
Set-Mailbox -Identity "Pilar Pinilla" -SingleItemRecoveryEnabled $true -RetainDeletedItemsFor 30
```

This example enables single item recovery for all user mailboxes in the organization.

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -SingleItemRecoveryEnabled $true
```

This example enables single item recovery for all user mailboxes in the organization and sets the number of days that deleted items are retained to 30 days

```powershell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Set-Mailbox -SingleItemRecoveryEnabled $true -RetainDeletedItemsFor 30
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## Use the Shell to disable single item recovery

You might need to disable single item recovery for a user's mailbox. For example, before you can use **Search-Mailbox -DeleteContent** to permanently delete content from a mailbox, you have to disable single item recovery. For more information, see [Search for and delete messages in Exchange 2013](search-for-and-delete-messages-exchange-2013-help.md).

This example disables single item recovery for the mailbox of Ayla Kol.

```powershell
Set-Mailbox -Identity "Ayla Kol" -SingleItemRecoveryEnabled $false
```

## How do you know this worked?

To verify that you've enabled single item recovery for a mailbox and display the value for how long deleted items will be retained (in days), run the following command.

```powershell
Get-Mailbox <Name> | Format-List SingleItemRecoveryEnabled,RetainDeletedItemsFor
```

You can use this same command to verify that single item recovery is disabled for a mailbox.

## More information

- To learn more about single item recovery, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md). To recover messages purged by the user before the deleted item retention period expires, see [Recover deleted messages in a user's mailbox](recover-deleted-messages-exchange-2013-help.md).

- If a mailbox is placed on In-Place Hold or Litigation Hold, messages in the Recoverable Items folder are retained until the hold duration expires. If the hold duration is unlimited, then items are retained until the hold is removed or the hold duration is changed.
