---
localization_priority: Normal
description: 'Summary: How to suspend or resume a mailbox database copy in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 96aa1b82-3e15-4215-843e-3d583af9504b
ms.reviewer:
title: Suspend or resume a mailbox database copy
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Suspend or resume a mailbox database copy

You may need to suspend or resume a database copy for a variety of reasons, such as maintenance on the disk that contains the database copy. Or you may need to suspend an individual database copy from activation for disaster recovery purposes.

Looking for other management tasks related to mailbox database copies? Check out [Manage mailbox database copies](manage-database-copies.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Suspend a mailbox database copy

### Use the EAC to suspend a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the database whose copy you want to suspend.

3. In the Details pane, under **Database Copies**, click **Suspend** under the database copy you want to suspend.

4. In the **Comments** field, add an optional comment of up to 512 characters specifying the reason for the suspension.

5. To suspend the database copy from automatic activation, select the **This copy can only be activated by manual intervention** check box.

6. Click **save** to suspend the database copy.

### Use the Exchange Management Shell to suspend a mailbox database copy

This example suspends continuous replication for a copy of the database DB1 hosted on the server MBX1. An optional comment has also been specified.

```powershell
Suspend-MailboxDatabaseCopy -Identity DB1\MBX1 -SuspendComment "Maintenance on MBX1" -Confirm:$False
```

This example suspends activation for a copy of the database DB2 hosted on the server MBX2.

```powershell
Suspend-MailboxDatabaseCopy -Identity DB2\MBX2 -ActivationOnly -Confirm:$False
```

For detailed syntax and parameter information, see [Suspend-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/suspend-mailboxdatabasecopy).

## Resume a mailbox database copy

### Use the EAC to resume a mailbox database copy

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the database whose copy you want to resume.

3. In the Details pane, under **Database Copies**, click **Resume** under the database copy you want to resume.

4. Click **yes** to resume the database copy.

### Use the Exchange Management Shell to resume a mailbox database copy
<a name="UseShellResume"> </a>

This example resumes a copy of the database DB1 on the server MBX1.

```powershell
Resume-MailboxDatabaseCopy -Identity DB1\MBX1
```

This example resumes a copy of the database DB2 on the server MBX2 for replication only.

```powershell
Resume-MailboxDatabaseCopy -Identity DB2\MBX2 -ReplicationOnly
```

For detailed syntax and parameter information, see [Resume-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/resume-mailboxdatabasecopy).

## How do you know this worked?

To verify that you have successfully suspended or resumed a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the appropriate database, and in the Details pane, click **View details** to view the database copy properties:

- In the Exchange Management Shell, run the following command to display status information for a database copy:

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName> | Format-List
  ```
