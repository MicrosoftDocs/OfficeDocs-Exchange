---
title: 'Enable or disable mailbox audit logging for a mailbox: Exchange 2013 Help'
TOCTitle: Enable or disable mailbox audit logging for a mailbox
ms:assetid: c4bbfd52-6196-49c7-8c31-777fbbee11f2
ms:mtpsurl: https://technet.microsoft.com/library/Ff461937(v=EXCHG.150)
ms:contentKeyID: 49300697
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable mailbox audit logging for a mailbox

_**Applies to:** Exchange Server 2013_

With mailbox audit logging, you can track logons to a mailbox as well as what actions are taken while the user is logged on. When you enable mailbox audit logging for a mailbox, some actions performed by administrators and delegates are logged by default. None of the actions performed by the mailbox owner are logged. To learn more about mailbox audit logging, see [Mailbox audit logging](mailbox-audit-logging-exchange-2013-help.md).

> [!WARNING]
> Auditing of mailbox owner actions can generate a large number of mailbox audit log entries and is therefore disabled by default. We recommend that you only enable auditing of specific owner actions needed to meet business or security requirements.

For additional tasks related to mailbox audit logging, see [Mailbox audit logging procedures](mailbox-audit-logging-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox audit logging" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to enable or disable mailbox audit logging. You must use the Shell.

- An administrator who has been assigned the Full Access permission to a user's mailbox is considered a delegate user.

- Mailboxes are considered to be accessed by an administrator only in the following scenarios:

  - In-Place eDiscovery is used to search a mailbox.

  - The **New-MailboxExportRequest** cmdlet is used to export a mailbox.

  - Microsoft Exchange Server MAPI Editor is used to access the mailbox.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable mailbox audit logging

You can use the Shell to enable or disable mailbox audit logging for a mailbox. This enables or disables logging of all operations specified for administrator, delegates, and the mailbox owner.

This example enables mailbox audit logging for Ben Smith's mailbox.

```powershell
Set-Mailbox -Identity "Ben Smith" -AuditEnabled $true
```

This example disables mailbox audit logging for Ben Smith's mailbox.

```powershell
Set-Mailbox -Identity "Ben Smith" -AuditEnabled $false
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## Use the Shell to configure mailbox audit logging settings for administrator, delegate, and owner access

When mailbox audit logging is enabled for a mailbox, only the administrator, delegate, and owner actions specified in the audit logging configuration for the mailbox are logged.

This example specifies that the `SendAs` or `SendOnBehalf` actions performed by delegate users will be logged for Ben Smith's mailbox.

```powershell
Set-Mailbox -Identity "Ben Smith" -AuditDelegate SendAs,SendOnBehalf -AuditEnabled $true
```

This example specifies that the `MessageBind` and `FolderBind` actions performed by administrators will be logged for Ben Smith's mailbox.

```powershell
Set-Mailbox -Identity "Ben Smith" -AuditAdmin MessageBind,FolderBind -AuditEnabled $true
```

This example specifies that the `HardDelete` action performed by the mailbox owner will be logged for Ben Smith's mailbox.

```powershell
Set-Mailbox -Identity "Ben Smith" -AuditOwner HardDelete -AuditEnabled $true
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox).

## How do you know this worked?

To verify that you have successfully enabled mailbox audit logging for a mailbox and specified the correct logging settings for administrator, delegate, or owner access, use the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Get-Mailbox) cmdlet to retrieve the mailbox audit logging settings for that mailbox.

This example retrieves Ben Smith's mailbox settings and pipes the specified audit settings, including the audit log age limit, to the **Format-List** cmdlet.

```powershell
Get-Mailbox "Ben Smith" | Format-List *audit*
```
