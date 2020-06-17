---
title: 'Remove a mailbox database copy: Exchange 2013 Help'
TOCTitle: Remove a mailbox database copy
ms:assetid: 99fecdde-b158-4dfc-9ca7-ff7c0ada7819
ms:mtpsurl: https://technet.microsoft.com/library/Dd298164(v=EXCHG.150)
ms:contentKeyID: 48385387
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove a mailbox database copy

_**Applies to:** Exchange Server 2013_

These procedures show you how to remove a copy of a mailbox database. You can't use these procedures to remove the last copy of a mailbox database. For detailed steps about how to remove the last copy of a mailbox database, see [Remove a mailbox database](manage-mailbox-databases-in-exchange-2013-exchange-2013-help.md) or [Remove-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Remove-MailboxDatabase).

Looking for other management tasks related to mailbox database copies? Check out [Managing mailbox database copies](managing-mailbox-database-copies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- Mailbox database copies can only be removed from a healthy database availability group (DAG). If the DAG isn't healthy (for example, the DAG's underlying cluster is down because quorum was lost), you won't be able to remove any mailbox database copies.

- If you're removing the last passive copy of the database, continuous replication circular logging (CRCL) must not be enabled for the specified mailbox database. If CRCL is enabled, you must first disable it. After the mailbox database copy has been removed, circular logging can be enabled. Once enabled for a non-replicated mailbox database, JET circular logging is used instead of CRCL. If you aren't removing the last passive copy of a database, CRCL can remain enabled.

- After removing a database copy, you must manually delete any database and transaction log files from the server from which the database copy is being removed.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to remove a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the mailbox database whose copy you want to remove.

3. In the Details pane, locate the passive copy you want to remove and click **Remove**.

4. Confirm the removal on the warning dialog box by clicking **yes**.

5. Click **ok** to confirm the removal after reviewing any messages.

6. Manually delete any database and transaction log files from the server from which the database copy is being removed.

## Use the Shell to remove a mailbox database copy

This example removes a copy of the mailbox database DB1 from the Mailbox server MBX1.

```powershell
Remove-MailboxDatabaseCopy -Identity DB1\MBX1 -Confirm:$False
```

## How do you know this worked?

To verify that you've successfully removed a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the appropriate database, and in the Details pane, the removed passive copy is no longer listed.

- In the Shell, run the following command to verify removal of the copy.

  ```powershell
  Get-MailboxDatabase <DatabaseName> | Format-List DatabaseCopies
  ```

  The removed passive copy is no longer listed.
