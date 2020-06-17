---
description: 'Summary: Learn how to disable or delete a mailbox in Exchange Server 2016 or Exchange Server 2019.'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: 31ad25d6-2942-4fd9-aecb-cdf9654163d2
ms.collection: exchange-server
ms.reviewer:
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
title: Disable or delete a mailbox in Exchange Server

---

# Disable or delete a mailbox in Exchange Server

In Exchange Server, you can use the Exchange admin center (EAC) or the Exchange Management Shell to disable or delete mailboxes. Disabled or deleted mailboxes are also known as *disconnected mailboxes*. For more information about disconnected mailboxes, see [Disconnected mailboxes](disconnected-mailboxes.md).

 **Note**: If you need to delete a mailbox in Office 365, see [Delete or Restore User Mailboxes in Exchange Online](https://docs.microsoft.com/exchange/recipients-in-exchange-online/delete-or-restore-mailboxes).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- For more information about accessing and using the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Disable mailboxes

When you disable a mailbox, all Exchange attributes are removed from the associated user account in Active Directory. The disconnected mailbox is hidden and marked for removal. The disconnected mailbox is permanently deleted (purged) based on the **MailboxRetention** property value for the mailbox database (the default value is 30 days). Before the mailbox is purged, you can reconnect it to a new or existing user account that doesn't already have an associated mailbox. For more information, see [Connect a disabled mailbox](connect-disabled-mailboxes.md).

 **Note**: Disabling a mailbox that has an associated archive marks both the primary and archive mailboxes for removal. To only mark the archive mailbox for removal without affecting the primary mailbox, see [Disable an archive mailbox](../../policy-and-compliance/in-place-archiving/manage-archives.md#disable-an-archive-mailbox).

### Use the EAC to disable a mailbox

1. In the EAC, go to **Recipients**, and click the tab for the type of mailbox that you want to disable:

   - **Mailboxes** for user mailboxes and linked mailboxes.

   - **Shared** for shared mailboxes.

2. Find and select the mailbox that you want to disable. For example:

   - Scroll through the list. You can also click the column headers to sort the mailboxes.

   - Click **Search** and enter the text to filter the list of mailboxes.

   - Select multiple mailboxes by selecting a mailbox, holding the Shift key, and selecting a mailbox farther down in the list, or by holding down the CTRL key as you select each mailbox.

3. After you've selected the mailbox or mailboxes that you want to disable, click **More** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png), select **Disable**, and then click **Yes** in the warning message that appears.

### Use the Exchange Management Shell to disable a mailbox

To disable a mailbox, use this syntax:

```PowerShell
Disable-Mailbox <MailboxIdentity> [-Arbitration] [-Archive] [-PublicFolder] [-RemoteArchive]
```

This example disables the user mailbox that has the alias value danj.

```PowerShell
Disable-Mailbox danj
```

This example disables the room mailbox named Conf Room 31/1234 (12).

```PowerShell
Disable-Mailbox "Conf Room 31/1234 (12)"
```

This example disables the shared mailbox that has the email address sharedmbx@contoso.com.

```PowerShell
Disable-Mailbox sharedmbx@contoso.com
```

For detailed syntax and parameter information, see [Disable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/disable-mailbox).

### How do you know this worked?

To verify that you've successfully disabled a mailbox, do any of these steps:

- In the EAC, click **Recipients**, go to the appropriate tab for the type of mailbox that you disabled, and verify that the mailbox is no longer listed. Note that you might need to click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png).

- In Active Directory Users and Computers, right-click the user account whose mailbox you disabled, and then click **Properties**. On the **General** tab, verify that the **E-mail** field is blank.

- In the Exchange Management Shell, replace _\<DisplayName\>_ with the user's display name, and run the following commands to verify the **DisconnectReason** property value is `Disabled` (which indicates the mailbox has been marked for removal):

  ```PowerShell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisconnectReason,DisconnectDate
  ```

  **Notes**:

  - The **DisconnectReason** property doesn't distinguish between disabled and deleted mailboxes (the value for both is `Disabled`). The presence of the associated user account indicates whether the mailbox was disabled.

    When you delete a mailbox, the value of the **DisconnectReason** property is also `Disabled`, but the corresponding Active Directory user account is also deleted.

  - If the command returns no results, replace _\<DatabaseName\>_ with the name of the mailbox database where the disconnected mailbox resides, and run this command to synchronize the mailbox state for all disconnected mailboxes on the database:

  ```PowerShell
  Get-MailboxStatistics -Database "<DatabaseName>" | foreach {Update-StoreMailboxState -Database $_.Database -Identity $_.MailboxGuid -Confirm:$false}
  ```

    Then, run the previous command, which should now return results.

- In the Exchange Management Shell, replace _\<UserIdentity\>_ with the name or user principal name of the user (for example, user@contoso.com), and run this command to verify that the **RecipientType** property value is `User`, not `UserMailbox`.

  ```PowerShell
  Get-User -Identity <UserIdentity>
  ```

## Delete mailboxes

When you delete a mailbox, the mailbox is disconnected from the associated user account, and the account is removed from Active Directory. The disconnected mailbox is hidden and marked for removal. The disconnected mailbox is permanently deleted (purged) based on the **MailboxRetention** property value for the mailbox database (the default value is 30 days). Before the mailbox is purged, you can reconnect it to a new or existing user account that doesn't already have an associated mailbox. For more information, see [Connect or restore a deleted mailbox](restore-deleted-mailboxes.md).

**Note**: Deleting a mailbox that has an associated archive marks both the primary and archive mailboxes for removal. To only mark the archive mailbox for removal without affecting the primary mailbox, see [Disable an archive mailbox](../../policy-and-compliance/in-place-archiving/manage-archives.md#disable-an-archive-mailbox).

### Use the EAC to delete a mailbox

1. In the EAC, go to the location for the type of mailbox that you want to delete:

   - **Recipients** \> **Mailboxes** for user mailboxes and linked mailboxes.

   - **Recipients** \> **Resources** for room and equipment mailboxes.

   - **Recipients** \> **Shared** for shared mailboxes.

   - **Public folders** \> **Public folder mailboxes** for public folder mailboxes.

2. Find and select the mailbox that you want to disable. For example:

   - Scroll through the list. You can also click the column headers to sort the mailboxes.

   - Click **Search** and enter the text to filter the list of mailboxes.

   - Select multiple mailboxes by selecting a mailbox, holding the Shift key, and selecting a mailbox farther down in the list, or by holding down the CTRL key as you select each mailbox.

3. After you've selected the mailbox or mailboxes that you want to delete, click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.png), and then click **Yes** in the warning message that appears.

### Use the Exchange Management Shell to delete a mailbox

To delete a mailbox, use this syntax:

```PowerShell
Remove-Mailbox <MailboxIdentity> [-Arbitration] [-PublicFolder]
```

This example deletes the mailbox that has the email address pilarp@contoso.com.

```PowerShell
Remove-Mailbox pilarp@contoso.com
```

This example deletes the equipment mailbox named Fleet Van (16).

```PowerShell
Remove-Mailbox "Fleet Van (16)"
```

This example deletes the mailbox that has the alias value corpprint.

```PowerShell
Remove-Mailbox corpprint
```

For detailed syntax and parameter information, see [Remove-Mailbox](https://docs.microsoft.com/powershell/module/exchange/remove-mailbox).

**Note**: If you use the **Remove-Mailbox** cmdlet with the _Purge_ switch, the mailbox is immediately purged and isn't recoverable. For more information, see [Permanently delete a mailbox](permanently-delete-mailboxes.md).

### How do you know this worked?

To verify that you've successfully deleted a mailbox, do any of these steps:

- In the EAC, click **Recipients**, go to the appropriate tab for the type of mailbox that you deleted, and verify that the mailbox is no longer listed. Note that you might need to click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png).

- In Active Directory Users and Computers, verify that the associated account is no longer listed. Note that mailbox types other than user and linked mailboxes also have associated user accounts that are disabled (for example, room, equipment, arbitration, shared, and public folder mailboxes).

- In the Exchange Management Shell replace _\<DisplayName\>_ with the user's display name, and run the following commands to verify the **DisconnectReason** property value is `Disabled` (which indicates the mailbox has been marked for removal):

  ```PowerShell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisconnectReason,DisconnectDate
  ```

  **Notes**:

  - The **DisconnectReason** property doesn't distinguish between disabled and deleted mailboxes (the value for both is `Disabled`). The absence of the associated user account indicates whether the mailbox was deleted.

  - If the command returns no results, replace _\<DatabaseName\>_ with the name of the mailbox database where the disconnected mailbox resides, and run the following command to synchronize the mailbox state for all disconnected mailboxes on the database:

    ```PowerShell
    Get-MailboxStatistics -Database "<DatabaseName>" | foreach {Update-StoreMailboxState -Database $_.Database -Identity $_.MailboxGuid -Confirm:$false}
    ```

    Then, run the previous command, which should now return results.

- In the Exchange Management Shell, replace _\<UserIdentity\>_ with the name or user principal name of the user (for example, user@contoso.com), and run this command to verify that the user can't be found.

  ```PowerShell
  Get-User <UserIdentity>
  ```

## More information

When you delete the Active Directory user account that's associated with a mailbox, Exchange will detect that the mailbox is no longer connected to a user account, and will mark the mailbox for removal, even if the mailbox has been placed on Litigation Hold or In-Place Hold. To retain the mailbox, do these steps:

- Instead of deleting the user account, disable the user account.

- Change the properties of the mailbox to restrict its use and who has access to the mailbox. For example, set send and receive quotas equal to 1, block who can send messages to the mailbox, and restrict who has access the mailbox.

- Retain the mailbox until all data has been expunged, or until preserving the data is no longer required.

For more information, see [In-Place Hold and Litigation Hold in Exchange Server](../../policy-and-compliance/holds/holds.md).
