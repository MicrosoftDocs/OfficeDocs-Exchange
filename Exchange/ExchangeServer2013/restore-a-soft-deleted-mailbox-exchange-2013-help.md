---
title: 'Restore a soft-deleted mailbox: Exchange 2013 Help'
TOCTitle: Restore a soft-deleted mailbox
ms:assetid: 4f3f5ce4-9d12-4ed8-9f70-d8a6aa8a1b2e
ms:mtpsurl: https://technet.microsoft.com/library/JJ863435(v=EXCHG.150)
ms:contentKeyID: 50387715
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Restore a soft-deleted mailbox

_**Applies to:** Exchange Server 2013_

Use the Shell to connect a soft-deleted mailbox to an Active Directory user account. A mailbox becomes *soft-deleted* in the source mailbox database when it's moved to a different mailbox database. Exchange doesn't fully delete the mailbox from the source mailbox database when the move is complete. Instead, the mailbox in the source mailbox database is switched to a soft-deleted state. This lets you restore the source mailbox in case errors occur during the move that cause a failure or corruption of the mailbox on the destination database. If this happens, you can restore the source mailbox and try the move again.

A soft-deleted mailbox is retained in the source database until the deleted mailbox retention period expires or until the **Remove-StoreMailbox** cmdlet is used to purge the soft-deleted mailbox. Until a soft-deleted mailbox is permanently deleted from the Exchange mailbox database, you can use the Shell to restore the contents of the soft-deleted mailbox to an existing mailbox or an archive mailbox.

To learn more about soft-deleted mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md)

- [Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md)

- [Manage mailbox restore requests](manage-mailbox-restore-requests-exchange-2013-help.md)

- [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- The procedures in this topic can only be performed in the Shell. You can't use the EAC to restore soft-deleted mailboxes.

- Replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands to verify that the soft-deleted mailbox that you want to connect a user account still exists in the mailbox database and is not a disabled mailbox.

  ```powershell
  $dbs = Get-MailboxDatabase
  dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisplayName,DisconnectReason,DisconnectDate
  ```

  The soft-deleted mailbox has to exist in the mailbox database and the value for the *DisconnectReason* property has to be `SoftDeleted`. If the mailbox has been purged from the database, the command won't return any results.

  Alternatively, run the following command to display all soft-deleted mailboxes in your organization.

  ```powershell
  $dbs = Get-MailboxDatabase
  dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "SoftDeleted"} | Format-List DisplayName,DisconnectReason,DisconnectDate
  ```

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to restore a soft-deleted mailbox

You can use the Shell to restore a soft-deleted mailbox to an existing mailbox by using the **New-MailboxRestoreRequest** cmdlet. When you restore a soft-deleted mailbox, its contents are copied to an existing mailbox, which is called the *target mailbox*. After a mailbox restore request is successfully completed, the request is retained for 30 days, by default, before it's removed. You can remove it sooner by using the **Remove-MailboxRestoreRequest** cmdlet.

After a soft-deleted mailbox is restored, the mailbox is retained in the mailbox database until it's permanently deleted by an administrator or purged when the deleted mailbox retention period expires.

To create a mailbox restore request, you have to use the display name, mailbox GUID, or legacy distinguished name (DN) of the soft-deleted mailbox. Use the **Get-MailboxStatistics** cmdlet to display the values of the **DisplayName**, **MailboxGuid**, and **LegacyDN** properties for the soft-deleted mailbox that you want to restore. For example, run the following commands to return this information for all disabled and soft-deleted mailboxes in your organization.

```powershell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "SoftDeleted"} | Format-List DisplayName,MailboxGuid,LegacyDN,Database
```

This example restores a soft-deleted mailbox, which is identified by the display name in the *SourceStoreMailbox* parameter and is located on the MBXDB01 mailbox database, to the target mailbox named Debra Garcia. The *AllowLegacyDNMismatch* parameter is used so the source mailbox can be restored to a mailbox that doesn't have the same legacy DN value as the soft-deleted mailbox.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox "Debra Garcia" -SourceDatabase MBXDB01 -TargetMailbox "Debra Garcia" -AllowLegacyDNMismatch
```

This example restores Pilar Pinilla's soft-deleted archive mailbox, which is identified by the mailbox GUID, to her current archive mailbox. The *AllowLegacyDNMismatch* parameter isn't necessary because a primary mailbox and its corresponding archive mailbox have the same legacy DN.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox dc35895a-a628-4bba-9aa9-650f5cdb9ae7 -SourceDatabase MBXDB02 -TargetMailbox pilarp@contoso.com -TargetIsArchive
```

For detailed syntax and parameter information, see [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest).

## How do you know this worked?

To verify that you've successfully restored a soft-deleted mailbox to the target mailbox, run the **Get-MailboxRestoreRequest** cmdlet or the **Get-MailboxRestoreRequestStatistics** cmdlet to display information about the restore request. If the restore request was successfully created, the *Status* property will have a value of **Queued**, **InProgress**, or **Completed**. After the restore request is completed, the contents from the soft-deleted mailbox will appear in the target mailbox.

For more information, see:

- [Manage mailbox restore requests](manage-mailbox-restore-requests-exchange-2013-help.md)

- [Get-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequest)

- [Get-MailboxRestoreRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequestStatistics)
