---
title: 'Disable or delete a mailbox: Exchange 2013 Help'
TOCTitle: Disable or delete a mailbox
ms:assetid: 31ad25d6-2942-4fd9-aecb-cdf9654163d2
ms:mtpsurl: https://technet.microsoft.com/library/JJ863434(v=EXCHG.150)
ms:contentKeyID: 50387714
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable or delete a mailbox

_**Applies to:** Exchange Server 2013 SP1_

You can use the EAC or the Shell to disable or delete a mailbox in Exchange 2013. When a mailbox is disabled or deleted, Exchange retains the mailbox in the mailbox database and switches the mailbox to a disabled state. Disabled and deleted mailboxes are retained in the mailbox database until the deleted mailbox retention period expires, which is 30 days by default. After the retention period expires, the mailbox is permanently deleted or *purged*.

If you need to delete a mailbox in Exchange Online, see [Delete or restore user mailboxes in Exchange Online](https://docs.microsoft.com/exchange/recipients-in-exchange-online/delete-or-restore-mailboxes).

> [!NOTE]
> Disabled or deleted mailboxes are referred to as <EM>disconnected mailboxes</EM>.

The primary difference between deleting and disabling a mailbox is that when you disable a mailbox, the Exchange attributes are removed from the corresponding Active Directory user account, but the user account is retained. When you delete a mailbox, both the Exchange attributes and the Active Directory user account are deleted. This difference also determines your options to reconnect or restore disabled and deleted mailboxes.

The following table shows which types of Exchange mailboxes you can disable and delete.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Mailbox type</th>
<th>Disable?</th>
<th>Delete?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Archive mailbox</p></td>
<td><p>Yes</p></td>
<td><p>No *</p></td>
</tr>
<tr class="even">
<td><p>Linked mailbox</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>Resource mailbox (Room or Equipment)</p></td>
<td><p>No</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="even">
<td><p>Shared mailbox</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
<tr class="odd">
<td><p>User mailbox</p></td>
<td><p>Yes</p></td>
<td><p>Yes</p></td>
</tr>
</tbody>
</table>

\* If an archive mailbox is enabled, it will be deleted when the primary mailbox is deleted. For information about disabling archive mailboxes, see [Manage In-Place Archives in Exchange 2013](manage-in-place-archives-in-exchange-2013-exchange-2013-help.md).

If an administrator deletes a user account that has a mailbox, the Exchange Information store will eventually detect that the mailbox is no longer connected to a user account and mark that mailbox for deletion, even if the mailbox is on hold. If you want to retain the mailbox you must do the following:

1. Instead of deleting the user account, disable the user account.

2. Change the properties of the mailbox to restrict its use and access to the mailbox. For example, set send and receive quotas equal to 1, block who can send messages to the mailbox, and restrict who can access the mailbox.

3. Retain the mailbox until all data has been expunged, or until hold is no longer required.

For additional management tasks related to disconnected mailboxes, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md)

- [Connect a disabled mailbox](connect-a-disabled-mailbox-exchange-2013-help.md)

- [Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md)

- [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Disable a mailbox

As previously stated, when you disable a mailbox, the Exchange attributes are removed from the corresponding Active Directory user account, but the user account is retained.

## Use the EAC to disable a mailbox

The following procedure shows how to disable a user mailbox. Use the same procedure to disable other mailbox types after navigating to the appropriate page in the EAC.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to disable.

3. Click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon") and then click **Disable**.

4. A warning appears asking if you're sure you want to disable the mailbox. Click **Yes** to disable the mailbox.

The mailbox is removed from the mailbox list.

## Use the Shell to disable a mailbox

Use the following command to disable user mailboxes, linked mailboxes, resource mailboxes, and shared mailboxes.

```powershell
Disable-Mailbox <identity>
```

When you run this command, a message is displayed that asks you to confirm that you want to disable the mailbox.

Here are some examples of commands for disabling mailboxes.

```powershell
Disable-Mailbox danj
```

```powershell
Disable-Mailbox "Conf Room 31/1234 (12)"
```

```powershell
Disable-Mailbox sharedmbx@contoso.com
```

## How do you know this worked?

To verify that you've successfully disabled a mailbox, do one of the following:

- In the EAC, click **Recipients**, navigate to the appropriate page for the mailbox type that you disabled, and then verify that the mailbox is no longer listed.

- In Active Directory Users and Computers, right-click the user account whose mailbox you disabled, and then click **Properties**. On the **General** tab, notice that the **E-mail** field is blank. This verifies that the mailbox is disabled, but the user account still exists.

- In the Shell, replace _\<DisplayName\>_ with the display name of the mailbox and run the following commands.

  ```powershell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisconnectReason,DisconnectDate
  ```

  The `Disabled` value in the *DisconnectReason* property indicates that the mailbox is disabled.

  > [!NOTE]
  > When you delete a mailbox, the value in the <EM>DisconnectReason</EM> property is also <CODE>Disabled</CODE>. However, the corresponding Active Directory user account is deleted.

- In the Shell, run the following command.

  ```powershell
  Get-User <identity>
  ```

    Note that the value for the *RecipientType* property is `User`, instead of `UserMailbox`, which is the value for users with enabled mailboxes. This also verifies that the mailbox is disabled, but the user account is retained.

## Delete a mailbox

As previously stated, when you delete a mailbox, both the Exchange attributes and the Active Directory user account are deleted. The mailbox (and the archive mailbox, if it's enabled) will be permanently deleted from the mailbox database after the mailbox retention period expires.

## Use the EAC to delete a mailbox

The following procedure shows how to delete a user mailbox. Use the same procedure to delete other mailbox types after navigating to the appropriate page in the EAC.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to delete, and then click **Delete** ![Delete icon](images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif "Delete icon").

3. A warning appears asking if you're sure you want to delete the mailbox. Click **Yes** to delete the mailbox.

The mailbox is removed from the mailbox list.

## Use the Shell to delete a mailbox

Use the following command to delete user mailboxes, linked mailboxes, resource mailboxes, and shared mailboxes.

```powershell
Remove-Mailbox <identity>
```

When you run this command, a message is displayed that asks you to confirm that you want to remove the mailbox and the corresponding Active Directory user account.

Here are some examples of commands for deleting mailboxes.

```powershell
Remove-Mailbox pilarp@contoso.com
```

```powershell
Remove-Mailbox "Fleet Van (16)"
```

```powershell
Remove-Mailbox corpprint
```

## How do you know this worked?

To verify that you've successfully deleted a mailbox, do one of the following sets of verification procedures.

1. In the EAC, click **Recipients** and then navigate to the appropriate page for the mailbox type that you deleted, and verify that the mailbox is no longer listed.

2. In Active Directory Users and Computers, verify that the corresponding user account is no longer listed.

Or

1. In the Shell, replace _\<DisplayName\>_ with the display name of the mailbox and run the following commands to verify that the mailbox has been deleted.

   ```powershell
   $dbs = Get-MailboxDatabase
   $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisconnectReason,DisconnectDate
   ```

   The `Disabled` value in the *DisconnectReason* property indicates that the mailbox has been deleted.

   > [!NOTE]
   > When you disable a mailbox, the value in the <EM>DisconnectReason</EM> property is also <CODE>Disabled</CODE>. However, the corresponding Active Directory user account is retained.

2. Run the following command to verify that Active Directory user account has been deleted.

   ```powershell
   Get-User <identity>
   ```

   The command will return an error stating that user couldn't be found, verifying that the account was deleted.
