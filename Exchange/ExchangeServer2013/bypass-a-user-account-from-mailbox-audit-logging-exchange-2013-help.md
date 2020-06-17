---
title: 'Bypass a user account from mailbox audit logging: Exchange 2013 Help'
TOCTitle: Bypass a user account from mailbox audit logging
ms:assetid: 98a87071-fe31-4b67-beb8-a73799e54df2
ms:mtpsurl: https://technet.microsoft.com/library/Ff461934(v=EXCHG.150)
ms:contentKeyID: 49300631
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Bypass a user account from mailbox audit logging

_**Applies to:** Exchange Server 2013_

When you enable mailbox audit logging for a mailbox, specified mailbox access events (for example, accessing a folder or a message, or permanently deleting a message) are logged. However, access by some authorized accounts, such as accounts used by third-party tools or accounts used for lawful monitoring, can create a large number of mailbox audit log entries and may not be of interest to your organization.

You can configure a user or computer account to bypass mailbox audit logging, so actions taken by that user or account for any mailbox aren't logged. By bypassing trusted user or computer accounts that need frequent access to mailboxes, you can reduce the noise in mailbox audit logs.

> [!WARNING]
> If you use mailbox audit logging to audit mailbox access and actions, you must monitor mailbox audit bypass associations at regular intervals. If a mailbox audit bypass association is added for an account, the account can access any mailbox in the organization to which it has been assigned permissions, without any mailbox audit logging entries being generated for such access or any actions taken (such as message deletions).

For additional management tasks related to mailbox audit logging, see [Mailbox audit logging procedures](mailbox-audit-logging-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox audit logging" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- When an account is configured to bypass mailbox audit logging, access to any mailbox by that account won't be logged. You can't configure an account to bypass the logging of access to a specific mailbox.

- You can't use the Exchange admin center (EAC) to enable or disable mailbox audit logging bypass for an account. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable mailbox audit logging bypass for an account

For an example of how to enable mailbox audit logging bypass for an account, see [Example 1](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxAuditBypassAssociation#examples) in [Set-MailboxAuditBypassAssociation](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxAuditBypassAssociation).

For an example of how to disable mailbox audit logging bypass for an account, see [Example 2](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxAuditBypassAssociation#examples) in [Set-MailboxAuditBypassAssociation](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxAuditBypassAssociation).

## How do you know this worked?

After you have bypassed a user account from mailbox audit logging, you can check the bypass settings by running the [Get-MailboxAuditBypassAssociation](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxAuditBypassAssociation) cmdlet.

For examples of how to check mailbox audit bypass associations, see the [Examples](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxAuditBypassAssociation#examples) section in that topic.