---
title: 'Permanently delete a mailbox: Exchange 2013 Help'
TOCTitle: Permanently delete a mailbox
ms:assetid: df35765a-0bef-4561-9846-d91d69c0269c
ms:mtpsurl: https://technet.microsoft.com/library/JJ863440(v=EXCHG.150)
ms:contentKeyID: 50387725
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Permanently delete a mailbox

_**Applies to:** Exchange Server 2013_

When you permanently delete active mailboxes and disconnected mailboxes, all mailbox contents are purged from the Exchange mailbox database, and the data loss is permanent. When you permanently delete an active mailbox, the associated Active Directory user account is also deleted.

An alternative to permanently deleting a mailbox is to disconnect it. After you disconnect a mailbox, by default, it's retained in the mailbox database for 30 days. This gives you the opportunity to reconnect or restore a mailbox before it's purged from the database.

To learn more about disconnected mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md)

- [Disable or delete a mailbox](disable-or-delete-a-mailbox-exchange-2013-help.md)

- [Connect a disabled mailbox](connect-a-disabled-mailbox-exchange-2013-help.md)

- [Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md)

> [!NOTE]
> You can't use the EAC to permanently delete an active mailbox or a disconnected mailbox.

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Permanently delete an active mailbox

## Use the Shell to permanently delete an active mailbox

Run the following command to permanently delete an active mailbox and the associated Active Directory user account.

```powershell
Remove-Mailbox -Identity <identity> -Permanent $true
```

> [!NOTE]
> If you don't include the <EM>Permanent</EM> parameter, the deleted mailbox is retained in the mailbox database for 30 days, by default, before it's permanently deleted.

For detailed syntax and parameter information, see [Remove-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Remove-Mailbox).

## How do you know this worked?

To verify that you've permanently deleted an active mailbox, do the following:

1. Verify that the mailbox is no longer listed in the EAC.

2. Verify that the associated user account is no longer listed in Active Directory Users and Computers.

3. Replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands to verify that the mailbox was successfully purged from the Exchange mailbox database.

   ```powershell
   $dbs = Get-MailboxDatabase
   $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"}
   ```

   If you successfully purged the mailbox, the command won't return any results. If the mailbox wasn't purged, the command will return information about the mailbox.

## Permanently delete a disconnected mailbox

## Use the Shell to permanently delete a disconnected mailbox

There are two types of disconnected mailboxes: disabled and soft-deleted. You must specify one of these types when running the cmdlet to permanently delete the mailbox. If the type you specify doesn't match the actual type of the disconnected mailbox, the command fails.

Replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands to determine whether a disconnected mailbox is disabled or soft-deleted.

```powershell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisplayName,MailboxGuid,Database,DisconnectReason
```

The value for the *DisconnectReason* property for disconnected mailboxes will be either `Disabled` or `SoftDeleted`.

You can run the following commands to display the type for all disconnected mailboxes in your organization.

```powershell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -ne $null} | Format-List DisplayName,MailboxGuid,Database,DisconnectReason
```

> [!WARNING]
> When you use the <STRONG>Remove-StoreMailbox</STRONG> cmdlet to permanently delete a disconnected mailbox, all its contents are purged from the mailbox database and the data loss is permanent.

This example permanently deletes the disabled mailbox with the GUID 2ab32ce3-fae1-4402-9489-c67e3ae173d3 from mailbox database MBD01.

```powershell
Remove-StoreMailbox -Database MBD01 -Identity "2ab32ce3-fae1-4402-9489-c67e3ae173d3" -MailboxState Disabled
```

This example permanently deletes the soft-deleted mailbox for Dan Jump from mailbox database MBD01.

```powershell
Remove-StoreMailbox -Database MBD01 -Identity "Dan Jump" -MailboxState SoftDeleted
```

This example permanently deletes all soft-deleted mailboxes from mailbox database MBD01.

```powershell
Get-MailboxStatistics -Database MBD01 | where {$_.DisconnectReason -eq "SoftDeleted"} | ForEach {Remove-StoreMailbox -Database $_.Database -Identity $_.MailboxGuid -MailboxState SoftDeleted}
```

For detailed syntax and parameter information, see [Remove-StoreMailbox](https://docs.microsoft.com/powershell/module/exchange/Remove-StoreMailbox) and [Get-MailboxStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxStatistics).

## How do you know this worked?

To verify that you've permanently deleted a disconnected mailbox and that it was successfully purged from the Exchange mailbox database, replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands.

```powershell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"}
```

If you successfully purged the mailbox, the command won't return any results. If the mailbox wasn't purged, the command will return information about the mailbox.
