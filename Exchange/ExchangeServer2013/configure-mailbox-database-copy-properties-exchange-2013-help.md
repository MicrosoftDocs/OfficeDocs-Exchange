---
title: 'Configure mailbox database copy properties: Exchange 2013 Help'
TOCTitle: Configure mailbox database copy properties
ms:assetid: cf186561-ab2c-45c0-90f5-8d3ecfabeeac
ms:mtpsurl: https://technet.microsoft.com/library/Dd351151(v=EXCHG.150)
ms:contentKeyID: 48385566
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure mailbox database copy properties

_**Applies to:** Exchange Server 2013_

Each mailbox database copy has its own properties that you can configure. These include the amount of time, if any, for replay lag and truncation lag, and the activation preference number. For more information about replay lag, truncation lag and the activation preference number, see [Managing mailbox database copies](managing-mailbox-database-copies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to configure mailbox database copy properties

1. In the EAC, go to **Servers** \> **Databases**.

2. Select the database you want to configure.

3. In the Details pane, under **Database Copies**, click **View details** for the desired database copy, and then view or configure the following:

   - **Database**: Displays the name of the selected database.

   - **Mailbox server**: Displays the name of the Mailbox server that hosts the selected database copy.

   - **Content index state**: Displays the current state of the content index for the selected database copy.

   - **Status**: Displays the current status of the selected database copy.

   - **Copy queue length**: Indicates the number of log files waiting to be copied to the selected database copy. This field is relevant only for passive database copies.

   - **Replay queue length**: Indicates the number of log files waiting to be replayed into the selected database copy. This field is relevant only for passive database copies.

   - **Error messages**: Displays any error messages for database copies that have a status of `Failed` or `Failed and Suspended`.

   - **Latest available log time**: Displays the date and time stamp of the most recently generated log file on the active copy of the database. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display **never**.

   - **Last inspected log time**: Displays the date and time stamp of the last log file that was inspected by the LogInspector on the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display **never**.

   - **Last copied log time**: Displays the date and time stamp of the last log file that was copied by the LogCopier on the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display **never**.

   - **Last replayed log time**: Displays the date and time stamp of the last log file that was replayed by the LogReplayer into the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display **never**.

   - **Activation preference number**: Displays the activation preference number. This is used as part of Active Manager's best copy selection process and to balance the DAG by redistributing active mailbox databases throughout the DAG using the RedistributeActiveDatabases.ps1 script. The value for activation preference is a number equal to or greater than 1, where 1 is at the top of the preference order. The number can't be larger than the number of copies of the mailbox database.

   - **Replay lag time (days)**: Displays the amount of time that the Microsoft Exchange Information Store service should wait before replaying log files that have been copied by the Microsoft Exchange Replication service to the passive database copy. Setting this parameter to a value greater than 0 creates a lagged database copy. The default setting for this value is 0 days. The maximum allowable value for this setting is 14 days. The minimum allowable value is 0 days, and setting this value to 0 disables replay lag.

## Use the Shell to configure mailbox database copy properties

This example configures a mailbox database copy with an activation preference number of 3.

```powershell
Set-MailboxDatabaseCopy -Identity DB3\EX3 -ActivationPreference 3
```

This example configures a copy of the database DB1 that's hosted on Server1 with a replay lag time and truncation lag time of 1 day, and an activation preference number of 2.

```powershell
Set-MailboxDatabaseCopy -Identity DB1\Server1 -ReplayLagTime 1.0:0:0 -TruncationLagTime 1.0:0:0 -ActivationPreference 2
```

## How do you know this worked?

To verify that you've successfully configured a mailbox database copy, do one of the following:

- In the EAC, navigate to **Servers** \> **Databases**. Select the appropriate database, and in the Details pane, click **View details** to view the database copy properties.

- In the Shell, run the following command to display configuration information for a database copy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName> | Format-List
  ```

## For more information

[Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxDatabaseCopy)

[Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabaseCopyStatus)

[Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabase)
