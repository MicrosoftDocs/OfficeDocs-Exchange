---
title: 'Clean up the Recoverable Items folder: Exchange 2013 Help'
TOCTitle: Clean up the Recoverable Items folder
ms:assetid: 82c310f8-de2f-46f2-8e1a-edb6055d6e69
ms:mtpsurl: https://technet.microsoft.com/library/Ff678798(v=EXCHG.150)
ms:contentKeyID: 50470877
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- classified message spillage
- classified message spillage cleanup
- Message spillage
- message spillage cleanup
- classified spillage cleanup
- classified spillage
- Search and destroy
---

# Clean up the Recoverable Items folder

_**Applies to:** Exchange Server 2013_

(This topic is intended for Exchange administrators.)

The Recoverable Items folder (known in earlier versions of Exchange as *the dumpster*) exists to protect from accidental or malicious deletions and to facilitate discovery efforts commonly undertaken before or during litigation or investigations. To learn more about the Recoverable Items folder, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

How you clean up the Recoverable Items folder in a user's mailbox depends on whether the mailbox is placed on In-Place Hold or Litigation Hold, or had single item recovery enabled:

- If a mailbox isn't placed on In-Place Hold or Litigation Hold or doesn't have single item recovery enabled, you can simply delete items from the Recoverable Items folder. After being deleted, you can't use single item recovery to recover the items.

- If the mailbox is placed on In-Place Hold or Litigation Hold or has single item recovery enabled, it's important to preserve the mailbox data until the hold is removed or single item recovery is disabled. In this case, you need to perform more detailed steps to clean up the Recoverable Items folder.

To learn more about In-Place Hold and Litigation Hold, see [In-Place Hold and Litigation Hold](https://docs.microsoft.com/exchange/security-and-compliance/in-place-and-litigation-holds). To learn more about single item recovery, see "Single Item Recovery" in [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

To learn more about the Recoverable Items folder, see [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 30 minutes. This may vary depending on the size of the Recoverable Items folder

- You need to be assigned the following management roles to use the **Search-Mailbox** cmdlet to search for and delete messages in a user's mailbox.

  - **Mailbox Search**: This role allows you to search for messages across multiple mailboxes in your organization. Administrators aren't assigned this role by default. To assign yourself this role so that you can search mailboxes, add yourself as a member of the Discovery Management role group. See [Assign eDiscovery permissions in Exchange](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/assign-ediscovery-permissions).

  - **Mailbox Import Export**: This role allows you to delete messages from a user's mailbox. By default, this role isn't assigned to any role group. To delete messages from users' mailboxes, you can add the Mailbox Import Export role to the Organization Management role group. For more information, see the "Add a role to a role group" section in [Manage role groups](manage-role-groups-exchange-2013-help.md) .

- Because incorrectly cleaning up the Recoverable Items folder can result in data loss, it's important that you're familiar with the Recoverable Items folder and the impact of removing its contents. Before performing this procedure, we recommend that you review the information in [Recoverable Items folder](recoverable-items-folder-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to perform these procedures. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to delete items from the Recoverable Items folder for mailboxes that aren't placed on hold or don't have single item recovery enabled

This example permanently deletes items from Gurinder Singh's Recoverable Items folder and also copies the items to the GurinderSingh-RecoverableItems folder in the Discovery Search Mailbox (a discovery mailbox created by Exchange Setup).

```powershell
Search-Mailbox -Identity "Gurinder Singh" -SearchDumpsterOnly -TargetMailbox "Discovery Search Mailbox" -TargetFolder "GurinderSingh-RecoverableItems" -DeleteContent
```

> [!NOTE]
> To delete items from the mailbox without copying them to another mailbox, use the preceding command without the <EM>TargetMailbox</EM> and <EM>TargetFolder</EM> parameters.

For detailed syntax and parameter information, see [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Search-Mailbox).

## Use the Shell to clean up the Recoverable Items folder for mailboxes that are placed on hold or have single item recovery enabled

If a mailbox reaches its Recoverable Items quota, we recommend that you raise the quota and not delete items from the folder. You can also monitor events in the Application log related to the Recoverable Items warning quota and take necessary actions (such as raising the quota or investigating the growth of the Recoverable Items folder for mailboxes that reach the warning quota).

If storage constraints or similar issues prevent you from raising the Recoverable Items quota, and you need to delete messages from the Recoverable Items folder of a mailbox on In-Place Hold or Litigation Hold or has single item recovery enabled, we recommend that you first copy data from the user's Recoverable Items folder to another mailbox. If you're deleting items due to storage constraints on one volume, you can copy items to a mailbox located on a volume that has adequate storage.

This procedure copies items from Gurinder Singh's Recoverable Items folder to the GurinderSingh-RecoverableItems folder in the Discovery Search Mailbox. Before you copy and delete items from the Recoverable Items folder, you must first perform several steps to make sure items aren't deleted from the Recoverable Items folder. After you copy items to a discovery or backup mailbox and clean up the folder, you can revert to the mailbox's previous settings.

1. Retrieve the following quota settings. Be sure to note the values so you can revert to these settings after cleaning up the Recoverable Items folder:

   - *RecoverableItemsQuota*

   - *RecoverableItemsWarningQuota*

   - *ProhibitSendQuota*

   - *ProhibitSendReceiveQuota*

   - *UseDatabaseQuotaDefaults*

   - *RetainDeletedItemsFor*

   - *UseDatabaseRetentionDefaults*

   > [!NOTE]
   > If the <EM>UseDatabaseQuotaDefaults</EM> parameter is set to <CODE>$true</CODE>, the previous quota settings aren't applied. The corresponding quota settings configured on the mailbox database are applied, even if individual mailbox settings are populated.

   ```powershell
   Get-Mailbox "Gurinder Singh" | Format-List RecoverableItemsQuota, RecoverableItemsWarningQuota, ProhibitSendQuota, ProhibitSendReceiveQuota, UseDatabaseQuotaDefaults, RetainDeletedItemsFor, UseDatabaseRetentionDefaults
   ```

2. Retrieve the mailbox access settings for the mailbox. Be sure to note these settings for later.

   ```powershell
   Get-CASMailbox "Gurinder Singh" | Format-List EwsEnabled, ActiveSyncEnabled, MAPIEnabled, OWAEnabled, ImapEnabled, PopEnabled
   ```

3. Retrieve the current size of the Recoverable Items folder. Note the size so you can raise the quotas in Step 6.

   ```powershell
   Get-MailboxFolderStatistics "Gurinder Singh" -FolderScope RecoverableItems | Format-List Name,FolderAndSubfolderSize
   ```

4. Retrieve the current Managed Folder Assistant work cycle configuration. Be sure to note the setting for later.

   ```powershell
   Get-MailboxServer "My Mailbox Server" | Format-List Name,ManagedFolderWorkCycle
   ```

5. Disable client access to the mailbox to make sure no changes can be made to mailbox data for the duration of this procedure.

   ```powershell
   Set-CASMailbox "Gurinder Singh" -EwsEnabled $false -ActiveSyncEnabled $false -MAPIEnabled $false -OWAEnabled $false -ImapEnabled $false -PopEnabled $false
   ```

6. To make sure no items are deleted from the Recoverable Items folder, increase the Recoverable Items quota, increase the Recoverable Items warning quota, and set the deleted item retention period to a value higher than the current size of the user's Recoverable Items folder. This is particularly important for preserving messages for mailboxes placed on In-Place Hold or Litigation Hold. We recommend raising these settings to twice their current size.

   ```powershell
   Set-Mailbox "Gurinder Singh" -RecoverableItemsQuota 50Gb -RecoverableItemsWarningQuota 50Gb -RetainDeletedItemsFor 3650 -ProhibitSendQuota 50Gb -ProhibitSendRecieveQuota 50Gb -UseDatabaseQuotaDefaults $false -UseDatabaseRetentionDefaults $false
   ```

7. Disable the Managed Folder Assistant on the Mailbox server.

   ```powershell
   Set-MailboxServer MyMailboxServer -ManagedFolderWorkCycle $null
   ```

   > [!IMPORTANT]
   > If the mailbox resides on a mailbox database in a database availability group (DAG), you must disable the Managed Folder Assistant on each DAG member that hosts a copy of the database. If the database fails over to another server, this prevents the Managed Folder Assistant on that server from deleting mailbox data.

8. Disable single item recovery and remove the mailbox from Litigation Hold.

   ```powershell
   Set-Mailbox "Gurinder Singh" -SingleItemRecoveryEnabled $false -LitigationHoldEnabled $false
   ```

    > [!IMPORTANT]
    > After you run this command, it may take up to one hour to disable single item recovery or Litigation Hold. We recommend that you perform the next step only after this period has elapsed.

9. Copy items from the Recoverable Items folder to a folder in the Discovery Search Mailbox and delete the contents from the source mailbox.

   ```powershell
   Search-Mailbox -Identity "Gurinder Singh" -SearchDumpsterOnly -TargetMailbox "Discovery Search Mailbox" -TargetFolder "GurinderSingh-RecoverableItems" -DeleteContent
   ```

   If you need to delete only messages that match specified conditions, use the *SearchQuery* parameter to specify the conditions. This example deletes messages that have the string "Your bank statement" in the **Subject** field.

   ```powershell
   Search-Mailbox -Identity "Gurinder Singh" -SearchQuery "Subject:'Your bank statement'" -SearchDumpsterOnly -TargetMailbox "Discovery Search Mailbox" -TargetFolder "GurinderSingh-RecoverableItems" -DeleteContent
   ```

   > [!NOTE]
   > It isn't required to copy items to the Discovery Search Mailbox. You can copy messages to any mailbox. However, to prevent access to potentially sensitive mailbox data, we recommend copying messages to a mailbox that has access restricted to authorized records managers. By default, access to the default Discovery Search Mailbox is restricted to members of the Discovery Management role group. For details, see <A href="https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery">In-Place eDiscovery</A>.

10. If the mailbox was placed on Litigation Hold or had single item recovery enabled earlier, enable these features again.

    ```powershell
    Set-Mailbox "Gurinder Singh" -SingleItemRecoveryEnabled $true -LitigationHoldEnabled $true
    ```

    > [!IMPORTANT]
    > After you run this command, it may take up to one hour to enable single item recovery or Litigation Hold. We recommend that you enable the Managed Folder Assistant and allow client access (Steps 11 and 12) only after this period has elapsed.

11. Revert the following quotas to the values noted in Step 1:

    - *RecoverableItemsQuota*

    - *RecoverableItemsWarningQuota*

    - *ProhibitSendQuota*

    - *ProhibitSendReceiveQuota*

    - *UseDatabaseQuotaDefaults*

    - *RetainDeletedItemsFor*

    - *UseDatabaseRetentionDefaults*

    In this example, the mailbox is removed from retention hold, the deleted item retention period is reset to the default value of 14 days, and the Recoverable Items quota is configured to use the same value as the mailbox database. If the values you noted in Step 1 are different, you must use the preceding parameters to specify each value and set the *UseDatabaseQuotaDefaults* parameter to `$false`. If the *RetainDeletedItemsFor* *and UseDatabaseRetentionDefaults* parameters were previously set to a different value, you must also revert them to the values noted in Step 1.

    ```powershell
    Set-Mailbox "Gurinder Singh" -RetentionHoldEnabled $false -RetainDeletedItemsFor 14 -RecoverableItemsQuota unlimited -UseDatabaseQuotaDefaults $true
    ```

12. Enable the Managed Folder Assistant by setting the work cycle back to the value you noted in Step 4. This example sets the work cycle to one day.

    ```powershell
    Set-MailboxServer MyMailboxServer -ManagedFolderWorkCycle 1
    ```

13. Enable client access.

    ```powershell
    Set-CASMailbox -ActiveSyncEnabled $true -EwsEnabled $true -MAPIEnabled $true -OWAEnabled $true -ImapEnabled $true -PopEnabled $true
    ```

For detailed syntax and parameter information, see the following topics:

- [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox)

- [Get-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/Get-CASMailbox)

- [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxFolderStatistics)

- [Get-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxServer)

- [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/Set-CASMailbox)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox)

- [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxServer)

- [Search-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Search-Mailbox)

## How do you know this worked?

To verify that you have successfully cleaned up the Recoverable Items folder of a mailbox, use [Get-MailboxFolderStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxFolderStatistics) cmdlet the check the size of the Recoverable Items folder.

This example retrieves the size of the Recoverable Items folder and its subfolders and an item count in the folder and each subfolder.

```powershell
Get-MailboxFolderStatistics -Identity "Gurinder Singh" -FolderScope RecoverableItems | Format-Table Name,FolderAndSubfolderSize,ItemsInFolderAndSubfolders -Auto
```
