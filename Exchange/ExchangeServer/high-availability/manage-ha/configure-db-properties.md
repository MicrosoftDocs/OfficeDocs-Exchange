---
localization_priority: Normal
description: 'Summary: How to configure the properties of a mailbox database copy in Exchange Server, and what those properties are.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: cf186561-ab2c-45c0-90f5-8d3ecfabeeac
ms.reviewer:
title: Configure mailbox database copy properties
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure mailbox database copy properties

Each mailbox database copy has its own properties, which you can configure. These properties include the amount of time, if any, for replay lag and truncation lag, and the activation preference number. For more information about replay lag, truncation lag and the activation preference number, see [Manage mailbox database copies](manage-database-copies.md).

## What do you need to know before you begin?

- Estimated time to complete this task: 1 minute

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

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

   - **Latest available log time**: Displays the date and time stamp of the most recently generated log file on the active copy of the database. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display never.

   - **Last inspected log time**: Displays the date and time stamp of the last log file that was inspected by the LogInspector on the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display never.

   - **Last copied log time**: Displays the date and time stamp of the last log file that was copied by the LogCopier on the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display never.

   - **Last replayed log time**: Displays the date and time stamp of the last log file that was replayed by the LogReplayer into the selected database copy. This field is relevant only for passive database copies. On active database copies (replicated and stand-alone), this field will display never.

   - **Activation preference number**: Displays the activation preference number. This is used as part of Active Manager's best copy selection process, and it is used to balance the DAG by redistributing active mailbox databases throughout the DAG via the DAG's `PreferenceMoveFrequency` property. This property defines the frequency (measured in time) when the Microsoft Exchange Replication service rebalances database copies by performing a lossless switchover that activates the copy with an activation preference number of 1. The value for activation preference is a number equal to or greater than 1, where 1 is at the top of the preference order. The number can't be larger than the number of copies of the mailbox database.

   - **Replay lag time (days)**: Displays the amount of time that the Microsoft Exchange Information Store service should wait before replaying log files that have been copied by the Microsoft Exchange Replication service to the passive database copy. Setting this parameter to a value greater than 0 creates a lagged database copy. The default setting for this value is 0 days. The maximum allowable value for this setting is 14 days. The minimum allowable value is 0 days, and setting this value to 0 disables replay lag.

## Use the Exchange Management Shell to configure mailbox database copy properties

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

- In the Exchange Management Shell, run the following command to display configuration information for a database copy.

  ```powershell
  Get-MailboxDatabaseCopyStatus <DatabaseCopyName> | Format-List
  ```

## For more information

[Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabasecopy)

[Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabasecopystatus)

[Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase)
