---
localization_priority: Normal
description: 'Summary: How to remove a copy of a mailbox database in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 99fecdde-b158-4dfc-9ca7-ff7c0ada7819
ms.date: 7/9/2018
ms.reviewer: 
title: Remove a mailbox database copy
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Remove a mailbox database copy

You can use these procedures to remove a copy of a mailbox database, but you can't use them to remove the **last** copy of a mailbox database. For detailed steps about how to remove the last copy of a mailbox database, see [Remove a mailbox database](../../architecture/mailbox-servers/manage-databases.md#BKMK_Remove) or [Remove-MailboxDatabase](http://technet.microsoft.com/library/4d07d736-1dd7-43af-9f54-37d7c648572e.aspx).

Looking for other management tasks related to mailbox database copies? Check out [Manage mailbox database copies](manage-database-copies.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- Mailbox database copies can only be removed from a healthy database availability group (DAG). If the DAG isn't healthy (for example, the DAG's underlying cluster is down because quorum was lost), you won't be able to remove any mailbox database copies.

- If you're removing the last passive copy of the database, continuous replication circular logging (CRCL) must not be enabled for the specified mailbox database. If CRCL is enabled, you must first disable it. After the mailbox database copy has been removed, circular logging can be enabled. Once enabled for a non-replicated mailbox database, JET circular logging is used instead of CRCL. If you aren't removing the last passive copy of a database, CRCL can remain enabled.

- After removing a database copy, you must manually delete any database and transaction log files from the server from which the database copy is being removed.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](http://technet.microsoft.com/library/63976059-25f8-4b4f-b597-633e78b803c0.aspx).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to remove a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the mailbox database whose copy you want to remove.

3. In the Details pane, locate the passive copy you want to remove and click **Remove**.

4. Confirm the removal on the warning dialog box by clicking **yes**.

5. Click **ok** to confirm the removal after reviewing any messages.

6. Manually delete any database and transaction log files from the server from which the database copy is being removed.

## Use the Exchange Management Shell to remove a mailbox database copy

This example removes a copy of the mailbox database DB1 from the Mailbox server MBX1.

```
Remove-MailboxDatabaseCopy -Identity DB1\MBX1 -Confirm:$False
```

For detailed syntax and parameter information, see [Remove-MailboxDatabaseCopy](http://technet.microsoft.com/library/18a41719-99dd-4bf7-97af-2e9b0e39ba2d.aspx).

## How do you know this worked?

To verify that you've successfully removed a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the appropriate database, and in the Details pane, the removed passive copy is no longer listed.

- In the Exchange Management Shell, run the following command to verify removal of the copy.

  ```
  Get-MailboxDatabase <DatabaseName> | Format-List DatabaseCopies
  ```

    The removed passive copy is no longer listed.
