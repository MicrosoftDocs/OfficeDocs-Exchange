---
title: 'Configure activation policy for a mailbox database copy: Exchange 2013 Help'
TOCTitle: Configure activation policy for a mailbox database copy
ms:assetid: 6b37ed6e-2e36-4688-b485-8fdbb8193ec8
ms:mtpsurl: https://technet.microsoft.com/library/Dd298046(v=EXCHG.150)
ms:contentKeyID: 48385199
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure activation policy for a mailbox database copy

_**Applies to:** Exchange Server 2013_

*Activation* is the process of changing a mailbox database copy from a passive copy to an active copy. Activation occurs automatically by the system as part of a database or server failover operation, and it can be performed manually by an administrator as part of a database or server switchover operation. Blocking a database for activation prevents it from becoming the active copy during a database or server failover.

Looking for other management tasks related to mailbox database copies? Check out [Managing mailbox database copies](managing-mailbox-database-copies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure the activation policy for a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the database that you want to configure.

3. In the Details pane, under **Database Copies**, locate the database copy you want to configure and click **Suspend**.

4. Optionally, add a comment, and select the check box that says **This copy can only be activated by manual intervention**.

5. Click **Save** to save the configuration changes for the mailbox database copy.

## Use the Shell to suspend or resume a database copy for activation

This example blocks the copy of the database DB1 on the server MBX2 for activation.

```powershell
Suspend-MailboxDatabaseCopy -Identity DB1\MBX2 -ActivationOnly
```

This example resumes the copy of the database DB1 on the server MBX2 for activation.

```powershell
Resume-MailboxDatabaseCopy -Identity DB1\MBX2
```

For detailed syntax and parameter information, see [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Suspend-MailboxDatabaseCopy) or [Resume-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Resume-MailboxDatabaseCopy).

## Use the Shell to configure the activation policy for a server

This example configures the database copies on server MBX2 as blocked for activation.

```powershell
Set-MailboxServer -Identity MBX2 -DatabaseCopyAutoActivationPolicy Blocked
```

This example configures the database copies on server MBX3 as blocked for out-of-site activation.

```powershell
Set-MailboxServer -Identity MBX3 -DatabaseCopyAutoActivationPolicy IntrasiteOnly
```

This example configures the database copies on server MBX4 as unblocked for activation.

```powershell
Set-MailboxServer -Identity MBX4 -DatabaseCopyAutoActivationPolicy Unrestricted
```

For detailed syntax and parameter information, see [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Suspend-MailboxDatabaseCopy), [Resume-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Resume-MailboxDatabaseCopy), or [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxServer).

## How do you know this worked?

To verify that you've successfully configured the activation policy, do one of the following:

- In the Shell, run the following command to verify activation settings for a database copy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName> | Format-List ActivationSuspended
  ```

- In the Shell, run the following command to verify activation settings for a DAG member.

  ```powershell
  Get-MailboxServer <ServerName> | Format-List DatabaseCopyAutoActivationPolicy
  ```
