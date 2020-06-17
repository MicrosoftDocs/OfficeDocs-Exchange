---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.service: exchange-online
ms.assetid: b293b7e5-f9f7-4322-8d56-e30c75af6845
ms.reviewer: 
ms.collection: 
- exchange-online
- M365-email-calendar
description: 'Summary: This article describes how to recover a public folder mailbox in Office 365 that was previously soft-deleted, meaning the mailbox retention period has not yet elapsed and the recycle bin has not been purged.'
audience: ITPro
f1.keywords:
- NOCSH
title: Recover a deleted public folder mailbox

---

# Recover a deleted public folder mailbox

 **Summary**: This article describes how to recover a public folder mailbox in Office 365 that was previously soft-deleted, meaning the mailbox retention period has not yet elapsed and the recycle bin has not been purged.

You can delete public folder mailboxes either in the EAC or through the `Remove-Mailbox -PublicFolder` cmdlet. To delete a primary mailbox, all other mailboxes must be deleted first. After a mailbox is deleted it will no longer be visible in the EAC.

Deleted Public Folder mailboxes are recoverable for a period of up to 90 days.

## What do you need to know before you begin?

- Estimated time to complete: 5-10 minutes.

- A public folder mailbox can only be deleted once all folders within that mailbox have been deleted. However, you can bypass this restriction by using the `-Force` switch, as in `Remove-Mailbox -PublicFolder -Force`.

- A deleted public folder mailbox is only recoverable for a period of 90 days after the mailbox is soft-deleted. The retention period for a soft-deleted mailbox is 90 days, after which the mailbox is permanently deleted and you won't be able to restore it.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Public folders" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!NOTE]
> For deleted public folder mailboxes that contain folders, the folders will be automatically recovered along with the mailbox that contains them when you use one of the following procedures to recover the mailbox.

## Restore a primary mailbox

To restore a primary public folder mailbox:

1. Type the following command to find the soft-deleted mailbox:

   ```PowerShell
   Get-Mailbox -PublicFolder -SoftDeletedMailbox
   ```

2. Type the following command to restore the chosen mailbox:

   ```PowerShell
    Undo-SoftDeletedMailbox -PublicFolder
    ```

## Restore a primary mailbox and secondary mailboxes

The **Type** field, part of the information returned by the `Get-Mailbox` cmdlet, identifies public folder mailboxes as either **Primary** or **Secondary**. Primary public folder mailboxes must be restored first.

Perform the following steps to restore both a primary public folder mailbox and any relevant secondary mailboxes.

1. Type the following command to find the soft-deleted mailboxes:

   ```PowerShell
   Get-Mailbox -PublicFolder -SoftDeletedMailbox
   ```

2. Type the following command to restore the primary mailbox:

   ```PowerShell
   Undo-SoftDeletedMailbox -PublicFolder
   ```

3. Type the following for each secondary public folder mailbox that you want to restore (once per mailbox).

   ```PowerShell
   Undo-SoftDeletedMailbox -PublicFolder
   ```

## Restore secondary mailboxes

Use this procedure if you want to restore one or more secondary public folder mailboxes that were soft-deleted, and the primary mailbox still exists within your organization.

1. Type the following command to find the soft-deleted mailboxes:

   ```PowerShell
   Get-Mailbox -PublicFolder -SoftDeletedMailbox
   ```

   You will be able to distinguish primary from secondary public folder mailboxes by the information in the **Type** field.

2. Type the following for each secondary public folder mailbox that you want to restore (once per mailbox).

   ```PowerShell
   Undo-SoftDeletedMailbox -PublicFolder
   ```

> [!NOTE]
> If a primary public folder has been deleted from an organization, any secondary mailbox associated with it can't be restored.
