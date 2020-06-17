---
title: 'Place a mailbox on retention hold: Exchange 2013 Help'
TOCTitle: Place a mailbox on retention hold
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 2baac4a7-3402-4142-bfb3-1649a950e677
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Place a mailbox on retention hold in Exchange 2013

_**Applies to:** Exchange Server 2013_

Placing a mailbox on retention hold suspends the processing of a retention policy or managed folder mailbox policy for that mailbox. Retention hold is designed for situations such as a user being on vacation or away temporarily.

During retention hold, users can log on to their mailbox and change or delete items. When you perform a mailbox search, deleted items that are past the deleted item retention period aren't returned in search results. To make sure items changed or deleted by users are preserved in legal hold scenarios, you must place a mailbox on legal hold. For more information, see [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md).

You can also include retention comments for mailboxes you place on retention hold. The comments are displayed in supported versions of Microsoft Outlook.

For additional management tasks related to messaging records management (MRM), see [Messaging Records Management Procedures](messaging-records-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to place a mailbox on retention hold. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to place a mailbox on retention hold

This example places Michael Allen's mailbox on retention hold.

```powershell
Set-Mailbox "Michael Allen" -RetentionHoldEnabled $true
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## Use the Shell to remove retention hold for a mailbox

This example removes the retention hold from Michael Allen's mailbox.

```powershell
Set-Mailbox "Michael Allen" -RetentionHoldEnabled $false
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To verify that you have successfully placed a mailbox on retention hold, use the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) cmdlet to retrieve the _RetentionHoldEnabled_ property of the mailbox.

This command retrieves the _RetentionHoldEnabled_ property for Michael Allen's mailbox.

```powershell
Get-Mailbox "Michael Allen" | Select RetentionHoldEnabled
```

This command retrieves all mailboxes in the Exchange organization, filters the mailboxes that are placed on retention hold, and lists them along with the retention policy applied to each.

> [!IMPORTANT]
> Because _RetentionHoldEnabled_ isn't a filterable property in Exchange 2013, you can't use the _Filter_ parameter with the **Get-Mailbox** cmdlet to filter mailboxes that are placed on retention hold on the server-side. This command retrieves a list of all mailboxes and filters on the client running the Shell session. In large environments with thousands of mailboxes, this command may take a long time to complete.

```powershell
Get-Mailbox -ResultSize unlimited | Where-Object {$_.RetentionHoldEnabled -eq $true} | Format-Table Name,RetentionPolicy,RetentionHoldEnabled -Auto
```
