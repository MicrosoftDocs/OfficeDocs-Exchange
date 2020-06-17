---
title: 'Search the mailbox audit log for a mailbox: Exchange 2013 Help'
TOCTitle: Search the mailbox audit log for a mailbox
ms:assetid: 5b518a08-3b51-4ba3-bfbd-0e35cc5ff374
ms:mtpsurl: https://technet.microsoft.com/library/Ff461930(v=EXCHG.150)
ms:contentKeyID: 49300503
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Search the mailbox audit log for a mailbox

_**Applies to:** Exchange Server 2013_

You can synchronously search mailbox audit log entries for a mailbox and have the search results displayed in the Shell.

If you want to search mailbox audit logs for multiple mailboxes and have the results emailed to a specified address, you must create a mailbox audit log search instead. For details, see [Create a mailbox audit log search](create-a-mailbox-audit-log-search-exchange-2013-help.md).

For additional management tasks related to mailbox audit logging, see [Mailbox audit logging procedures](mailbox-audit-logging-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox audit logging" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- By default, mailbox audit logging is disabled for all mailboxes. For each mailbox you want to audit, you must enable audit logging and specify the mailbox owner, delegate, or administrator actions you want to audit. For details, see [Enable or disable mailbox audit logging for a mailbox](enable-or-disable-mailbox-audit-logging-for-a-mailbox-exchange-2013-help.md).

- You can't use the EAC to search the mailbox audit log for a mailbox. However, you can use the EAC to run or search for and export a non-owner mailbox access report.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to search the mailbox audit log for a mailbox

For examples of how to use the Shell to search the mailbox audit log for a mailbox, see [Examples](https://docs.microsoft.com/powershell/module/exchange/Search-MailboxAuditLog#examples) in [Search-MailboxAuditLog](https://docs.microsoft.com/powershell/module/exchange/Search-MailboxAuditLog).
