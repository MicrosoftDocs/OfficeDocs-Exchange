---
title: 'Configure Deleted Item retention and Recoverable Items quotas'
TOCTitle: Configure Deleted Item retention and Recoverable Items quotas
ms:assetid: de7d667a-1c93-4364-a4f9-2aa5e3678b12
ms:mtpsurl: https://technet.microsoft.com/library/Ee364752(v=EXCHG.150)
ms:contentKeyID: 50470878
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Deleted Item retention and Recoverable Items quotas

_**Applies to:** Exchange Server 2013_

When a user deletes items from the Deleted Items default folder by using the Delete, Shift+Delete, or **Empty Deleted Items Folder** actions, the items are moved to the **Recoverable Items\\Deletions** folder. The duration that deleted items remain in this folder is based on the deleted item retention settings configured for the mailbox database or the mailbox. By default, a mailbox database is configured to retain deleted items for 14 days, and the recoverable items warning quota and recoverable items quota are set to 20 gigabytes (GB) and 30 GB respectively.

> [!NOTE]
> Before the retention time for deleted items elapses, Microsoft Outlook and Microsoft Office&nbsp;Outlook Web App users can recover deleted items by using the Recover Deleted Items feature. To learn more about these features, see the "Recover deleted items" topic for <A href="https://go.microsoft.com/fwlink/p/?linkid=198206">Outlook</A> or <A href="https://go.microsoft.com/fwlink/p/?linkid=198207">Outlook Web App</A>.

You can use the Shell to configure deleted item retention settings and recoverable items quotas for a mailbox or mailbox database. Deleted item retention settings are ignored when a mailbox is placed on In-Place Hold or litigation hold.

To learn more about deleted item retention, the Recoverable Items folder, In-Place Hold, and litigation hold, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Configure deleted item retention for a mailbox

### Use the EAC to configure deleted item retention for a mailbox

1. Navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select a mailbox, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the mailbox property page, click **Mailbox usage**, click **More options**, and then select one of the following

   - **Use the default retention settings from the mailbox database**: Use this setting to use the deleted item retention setting that's configured for the mailbox database.

   - **Customize the settings for this mailbox**: Use this setting to configure deleted item retention settings for the mailbox.

     **Keep deleted items for (days)**: This box displays the length of time that deleted items are retained before they're permanently deleted and can't be recovered by the user. When the mailbox is created, this value is based on the deleted item retention settings configured for the mailbox database. By default, a mailbox database is configured to retain deleted items for 14 days. The value range for this property is from 0 through 24,855 days.

     **Don't permanently delete items until the database is backed up**: Select this check box to prevent mailboxes and email messages from being deleted until after the mailbox database on which the mailbox is located has been backed up.

### Use the Shell to configure deleted item retention for a mailbox

This example configures April Stewart's mailbox to retain deleted items for 30 days.

```powershell
Set-Mailbox -Identity - "April Stewart" -RetainDeletedItemsFor 30
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## Use the Shell to configure recoverable items quotas for a mailbox

> [!NOTE]
> You can't use the EAC to configure recoverable items quotas for a mailbox.

This example configures a recoverable items warning quota of 12 GB and a recoverable items quota of 15 GB for April Stewart's mailbox.

```powershell
Set-Mailbox -Identity "April Stewart" -RecoverableItemsWarningQuota 12GB -RecoverableItemsQuota 15GB -UseDatabaseQuotaDefaults $false
```

> [!NOTE]
> To configure a mailbox to use different recoverable items quotas than the mailbox database in which it resides, you must set the <EM>UseDatabaseQuotaDefaults</EM> parameter to <CODE>$false</CODE>.

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## Use the Shell to configure deleted item retention for a mailbox database

> [!NOTE]
> You can't use the EAC to configure deleted item retention for a mailbox database.

This example configures a deleted item retention period of 10 days for the mailbox database MDB2.

```powershell
Set-MailboxDatabase -Identity MDB2 -DeletedItemRetention 10
```

For detailed syntax and parameter information, see [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxDatabase).

## Use the Shell to configure recoverable items quotas for a mailbox database

> [!NOTE]
> You can't use the EAC to configure recoverable items quotas for a mailbox database

This example configures a recoverable items warning quota of 15 GB and a recoverable items quota of 20 GB on mailbox database MDB2.

```powershell
Set-MailboxDatabase -Identity MDB2 -RecoverableItemsWarningQuota 15GB -RecoverableItemsQuota 20GB
```

For detailed syntax and parameter information, see [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxDatabase).
