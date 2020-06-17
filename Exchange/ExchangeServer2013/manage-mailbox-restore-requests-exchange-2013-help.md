---
title: 'Manage mailbox restore requests: Exchange 2013 Help'
TOCTitle: Manage mailbox restore requests
ms:assetid: 8e668a52-c601-4d96-a51c-ab60267e1321
ms:mtpsurl: https://technet.microsoft.com/library/JJ863437(v=EXCHG.150)
ms:contentKeyID: 50387723
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage mailbox restore requests

_**Applies to:** Exchange Server 2013_

Mailbox restore requests are used to restore disconnected mailboxes. A disconnected mailbox is a mailbox in an Exchange mailbox database that isn't associated with an Active Directory user account. Mailboxes become disconnected when they're disabled, deleted, or moved to another database. For more information, see [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md).

Disconnected mailboxes remain in the mailbox database for the duration specified in the deleted mailbox retention settings for the mailbox database. By default, disconnected mailboxes are retained for 30 days. During this retention period, the contents of a deleted mailbox can be restored (copied) to an existing mailbox. This topic describes how to use the Shell to manage mailbox restore requests.

For additional management tasks related to disconnected mailboxes, see the following topics:

[Disable or delete a mailbox](disable-or-delete-a-mailbox-exchange-2013-help.md)

[Connect a disabled mailbox](connect-a-disabled-mailbox-exchange-2013-help.md)

[Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md)

[Restore a soft-deleted mailbox](restore-a-soft-deleted-mailbox-exchange-2013-help.md)

[Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox restore request" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- The procedures in this topic can only be performed in the Shell. You can't use the EAC to manage mailbox restore requests.

- To display the value of the *Identity* property for all mailbox restore requests, run the following command.

  ```powershell
  Get-MailboxRestoreRequest | Format-Table Identity
  ```

    You can use this identity value to specify a specific mailbox restore request when you're performing the procedures in this topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to view restore request properties

You can view the properties of a mailbox restore request, which provide you with basic information about the status of a mailbox restore request.

To display a list and the value of the *Identity* property for all mailbox restore requests, run the following command.

```powershell
Get-MailboxRestoreRequest | Format-Table Identity
```

You can use the identity to get information about specific mailbox restore requests.

This example returns the status of the restore request "Pilar Pinilla \\MailboxRestore" using the *Identity* parameter.

```powershell
Get-MailboxRestoreRequest -Identity "Pilar Pinilla\MailboxRestore"
```

This example returns all information for the second restore request for the Pilar Pinilla target mailbox.

```powershell
Get-MailboxRestoreRequest -Identity "Pilar Pinilla\MailboxRestore1" | Format-List
```

This example returns the status of restore requests being restored from the source database MBD01.

```powershell
Get-MailboxRestoreRequest -SourceDatabase MBD01
```

This example returns all restore requests that are currently in progress.

```powershell
Get-MailboxRestoreRequest -Status InProgress
```

Other useful status states include `Queued`, `Completed`, `Suspended`, and `Failed`.

This example returns all restore requests that have been suspended.

```powershell
Get-MailboxRestoreRequest -Suspend $true
```

For detailed syntax and parameter information, see [Get-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequest).

## Get-MailboxRestoreRequest Output

By default, the **Get-MailboxRestoreRequest** cmdlet returns the name of the request, the target mailbox to which data is being restored, and the status of the request. The following table lists useful information returned if you pipe the cmdlet to the **Format-List** cmdlet.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>SourceDatabase</code></p></td>
<td><p>Specifies the database that contains the disconnected mailbox that's being restored.</p></td>
</tr>
<tr class="even">
<td><p><code>TargetMailbox</code></p></td>
<td><p>Specifies the mailbox into which data is being restored.</p></td>
</tr>
<tr class="odd">
<td><p><code>Name</code></p></td>
<td><p>Specifies the name of the request.</p></td>
</tr>
<tr class="even">
<td><p><code>RequestQueue</code></p></td>
<td><p>Specifies the database on which the Microsoft Exchange Mailbox Replication service (MRS) stores the detailed status of the request.</p></td>
</tr>
<tr class="odd">
<td><p><code>Status</code></p></td>
<td><p>Specifies the status of the request.</p></td>
</tr>
<tr class="even">
<td><p><code>Suspend</code></p></td>
<td><p>Specifies whether the request is suspended. A mailbox restore can be suspended when it's created using the <strong>New-MailboxRestoreRequest</strong> cmdlet with the <em>Suspend</em> parameter. It can also be suspended if the mailbox restore operation fails or by an administrator using the <strong>Suspend-MailboxRestoreRequest</strong> cmdlet.</p></td>
</tr>
<tr class="odd">
<td><p><code>Identity</code></p></td>
<td><p>Specifies the identity of the request. This identity is a combination of the target mailbox name and the request name.</p></td>
</tr>
</tbody>
</table>

## How do you know this worked?

Run the **Get-MailboxRestoreRequest** cmdlet to verify that you can view properties for mailbox restore requests. If the cmdlet returns an error, verify that you're using the correct syntax and identity. In some cases, the cmdlet may be successful and not return any results. For example, if you've submitted a mailbox restore request and run the command `Get-MailboxRestoreRequest -Status InProgress` and no results are returned, then none of the restore requests are currently running.

## Use the Shell to view restore request statistics

You can view the statistics of a mailbox restore request, which provide you with detailed information that can be used for troubleshooting purposes.

This example returns the default statistics for the restore request danp\\MailboxRestore1. By default, the information returned includes name, mailbox, status, and percentage complete.

```powershell
Get-MailboxRestoreRequestStatistics -Identity danp\MailboxRestore1
```

This example returns the statistics for Dan Park's mailbox and exports the report to a .csv file.

```powershell
Get-MailboxRestoreRequestStatistics -Identity "Dan Park\MailboxRestore" | Export-CSV \\SERVER01\RestoreRequest_Reports\DanPark_Restorestats.csv
```

This example returns additional information about the restore request for Pilar Pinilla's mailbox using the *IncludeReport* parameter and piping the results to the **Format-List** cmdlet.

```powershell
Get-MailboxRestoreRequestStatistics -Identity "Pilar Pinilla\MailboxRestore" -IncludeReport | Format-List
```

This example returns additional information for all restore requests that have a status of `Failed` using the *IncludeReport* parameter, and then saves the information to the file AllRestoreReports.txt in the location where the command is being run.

```powershell
Get-MailboxRestoreRequest -Status Failed | Get-MailboxRestoreRequestStatistics -IncludeReport | Format-List > AllRestoreReports.txt
```

For detailed syntax and parameter information, see [Get-MailboxRestoreRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequestStatistics) and [Get-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequest).

## Get-MailboxRestoreRequestStatistics Output

By default, the [Get-MailboxRestoreRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequestStatistics) cmdlet returns the name of the request, the status of the request, the alias of the target mailbox, and the percentage completed. The following table lists other useful information returned if you pipeline the cmdlet to the **Format-List** cmdlet.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>Name</code></p></td>
<td><p>Specifies the name of the request.</p></td>
</tr>
<tr class="even">
<td><p><code>Status</code></p></td>
<td><p>Specifies the status of the request.</p></td>
</tr>
<tr class="odd">
<td><p><code>StatusDetail</code></p></td>
<td><p>Specifies more details about the request status. For example, if the <code>Status</code> value returns <code>InProgress</code>, the <code>StatusDetail</code> value would return the specific stages for the <code>InProgress</code> status, such as <code>CreatingFolderHierarchy</code> and <code>CopyingMessages</code>.</p></td>
</tr>
<tr class="even">
<td><p><code>SyncStage</code></p></td>
<td><p>Specifies how far along the request is through the restore process.</p></td>
</tr>
<tr class="odd">
<td><p><code>Suspend</code></p></td>
<td><p>Specifies whether the restore request is suspended. This value is <code>true</code> in the following scenarios:</p>
<ul>
<li><p>MRS stopped or is in the process of stopping the request due to a failure.</p></li>
<li><p>An administrator suspended the request.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p><code>SourceExchangeGuid</code></p></td>
<td><p>Specifies the GUID of the source mailbox from which data is being restored.</p></td>
</tr>
<tr class="odd">
<td><p><code>SourceRootFolder</code></p></td>
<td><p>Specifies the name of the root folder in the source mailbox hierarchy from which data is being restored. If this value is blank, data is restored from the folder Top of Information Store.</p></td>
</tr>
<tr class="even">
<td><p><code>SourceDatabase</code></p></td>
<td><p>Specifies the name of the database on which the source mailbox is located.</p></td>
</tr>
<tr class="odd">
<td><p><code>MailboxRestoreFlags</code></p></td>
<td><p>Specifies that the mailbox being restored is either <code>Disabled</code> or <code>Soft-Deleted</code>.</p></td>
</tr>
<tr class="even">
<td><p><code>TargetAlias</code></p></td>
<td><p>Specifies the alias of the target mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><code>TargetIsArchive</code></p></td>
<td><p>Specifies whether the mailbox is being restored into an archive.</p></td>
</tr>
<tr class="even">
<td><p><code>TargetExchangeGuid</code></p></td>
<td><p>Specifies the GUID of the target mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><code>TargetRootFolder</code></p></td>
<td><p>Specifies the name of the root folder in the target mailbox hierarchy to which data is being restored. If this value is blank, data is restored to the folder Top of Information Store.</p></td>
</tr>
<tr class="even">
<td><p><code>TargetDatabase</code></p></td>
<td><p>Specifies the name of the database on which the target mailbox is located.</p></td>
</tr>
<tr class="odd">
<td><p><code>TargetMailboxIdentity</code></p></td>
<td><p>Specifies the identity of the target mailbox.</p></td>
</tr>
<tr class="even">
<td><p><code>IncludeFolders</code></p></td>
<td><p>Specifies the list of folders to include during the restore. If this value is blank, no folders were specified when the request was created, and all folders will be restored to the mailbox (unless the <em>ExcludeFolders</em> parameter is used to exclude specific folders).</p></td>
</tr>
<tr class="odd">
<td><p><code>ExcludeFolders</code></p></td>
<td><p>Specifies the list of folders to exclude during the restore. If this value is blank, no folders were specified when the request was created, and all folders will be restored to the mailbox (unless the <em>IncludeFolders</em> parameter is used to include specific folders).</p></td>
</tr>
<tr class="even">
<td><p><code>ExcludeDumpster</code></p></td>
<td><p>Specifies whether the Recoverable Items folder was excluded when the request was created.</p></td>
</tr>
<tr class="odd">
<td><p><code>ConflictResolutionOption</code></p></td>
<td><p>Specifies the action for MRS to take if there are matching messages in the target and source folders.</p></td>
</tr>
<tr class="even">
<td><p><code>AssociatedMessagesCopyOption</code></p></td>
<td><p>Specifies whether the associated messages are copied when the request is processed. Associated messages are special messages that contain hidden data with information about rules, views, and forms.</p></td>
</tr>
<tr class="odd">
<td><p><code>BadItemLimit</code></p></td>
<td><p>Specifies the number of bad items that MRS will skip if the request encounters corrupted messages.</p></td>
</tr>
<tr class="even">
<td><p><code>BadItemsEncountered</code></p></td>
<td><p>Specifies the number of corrupted messages encountered by the command. If the <em>BadItemsEncountered</em> value is greater than the <em>BadItemLimit</em> value, the request fails.</p></td>
</tr>
<tr class="odd">
<td><p><code>QueuedTimeStamp</code></p></td>
<td><p>Specifies the date and time at which the request was initiated to MRS.</p></td>
</tr>
<tr class="even">
<td><p><code>StartTimeStamp</code></p></td>
<td><p>Specifies the date and time at which MRS started processing the restore request.</p></td>
</tr>
<tr class="odd">
<td><p><code>LastUpdateTimeStamp</code></p></td>
<td><p>Specifies the date and time at which the last change was made to the request. The change may have been made by an administrator or by MRS.</p></td>
</tr>
<tr class="even">
<td><p><code>SuspendTimeStamp</code></p></td>
<td><p>Specifies the date and time at which the request was suspended.</p></td>
</tr>
<tr class="odd">
<td><p><code>OverallDuration</code></p></td>
<td><p>Specifies the amount of time it took to complete the request. If the request is in a <code>Failed</code> state, this value specifies the amount of time between the request being initiated and the request failing. If the request isn't complete, this value specifies the amount of time between the request being initiated and the <strong>Get-MailboxRestoreRequestStatistics</strong> cmdlet being run.</p></td>
</tr>
<tr class="even">
<td><p><code>TotalSuspendedDuration</code></p></td>
<td><p>Specifies the amount of time the request was in the <code>Suspended</code> state.</p></td>
</tr>
<tr class="odd">
<td><p><code>TotalFailedDuration</code></p></td>
<td><p>Specifies the amount of time the request was in the <code>Failed</code> state.</p></td>
</tr>
<tr class="even">
<td><p><code>TotalQueuedDuration</code></p></td>
<td><p>Specifies the amount of time the request was in the <code>Queued</code> state.</p></td>
</tr>
<tr class="odd">
<td><p><code>TotalInProgressDuration</code></p></td>
<td><p>Specifies the amount of time the request was in the <code>In Progress</code> state.</p></td>
</tr>
<tr class="even">
<td><p><code>TotalStalledDueToHADuration</code></p></td>
<td><p>Specifies the amount of time the request was stalled due to high availability.</p></td>
</tr>
<tr class="odd">
<td><p><code>MRSServerName</code></p></td>
<td><p>Specifies the name of the Client Access server that processed the request.</p></td>
</tr>
<tr class="even">
<td><p><code>EstimatedTransferSize</code></p></td>
<td><p>Specifies the total file size that was restored or the file size that MRS expects to restore if the request is in the <code>In Progress</code> state.</p></td>
</tr>
<tr class="odd">
<td><p><code>EstimatedTransferItemCount</code></p></td>
<td><p>Specifies the number of items that were restored or the number of items that MRS expects to restore if the request is in the <code>In Progress</code> state.</p></td>
</tr>
<tr class="even">
<td><p><code>BytesTransferredPerMinute</code></p></td>
<td><p>Specifies the average number of bytes that have been transferred per minute.</p></td>
</tr>
<tr class="odd">
<td><p><code>ItemsTransferred</code></p></td>
<td><p>Specifies the number of items that have been transferred.</p></td>
</tr>
<tr class="even">
<td><p><code>PercentComplete</code></p></td>
<td><p>Specifies the percentage of the request that has been completed.</p></td>
</tr>
<tr class="odd">
<td><p><code>CompletedRequestAgeLimit</code></p></td>
<td><p>Specifies how long a completed restore request will be retained before it's deleted. The default is 30 days.</p></td>
</tr>
<tr class="even">
<td><p><code>PositionInQueue</code></p></td>
<td><p>If the request hasn't started, this value specifies the request's position in the queue.</p></td>
</tr>
<tr class="odd">
<td><p><code>FailureCode</code></p></td>
<td><p>If there is a failure, this value specifies the failure code.</p></td>
</tr>
<tr class="even">
<td><p><code>FailureType</code></p></td>
<td><p>If there is a failure, this value specifies the failure type.</p></td>
</tr>
<tr class="odd">
<td><p><code>FailureSide</code></p></td>
<td><p>If there is a failure, this value specifies whether the failure occurred on the target mailbox or the source mailbox.</p></td>
</tr>
<tr class="even">
<td><p><code>Message</code></p></td>
<td><p>If there is a failure, this value specifies the failure message. This value can also specify the suspend comment.</p></td>
</tr>
<tr class="odd">
<td><p><code>FailureTimestamp</code></p></td>
<td><p>If the request failed, this value specifies the date and time at which the request failed.</p></td>
</tr>
<tr class="even">
<td><p><code>FailureContext</code></p></td>
<td><p>If the request failed, this value specifies information about the action being performed at the time of failure.</p></td>
</tr>
<tr class="odd">
<td><p><code>ValidationMessage</code></p></td>
<td><p>If the request isn't valid, this value specifies the reason.</p></td>
</tr>
<tr class="even">
<td><p><code>RequestQueue</code></p></td>
<td><p>Specifies the database on which MRS stores the detailed status of the request.</p></td>
</tr>
<tr class="odd">
<td><p><code>Identity</code></p></td>
<td><p>Specifies the identity of the request.</p></td>
</tr>
<tr class="even">
<td><p><code>Report</code></p></td>
<td><p>If you used the <em>IncludeReport</em> parameter, this value specifies information that can be used to troubleshoot the request.</p></td>
</tr>
</tbody>
</table>

## How do you know this worked?

Run the **Get-MailboxRestoreRequestStatistics** cmdlet to verify that you can view the statistics for mailbox restore requests. If the cmdlet returns an error, verify that you're using the correct identity for the restore request.

## Use the Shell to change restore request properties

If a mailbox restore request fails, you can use the **Set-MailboxRestoreRequest** cmdlet to change the request's properties to try to recover from the failure.

This example specifies that the restore request MailboxRestore1 for Debra Garcia's mailbox skips 10 corrupted mailbox items.

```powershell
Set-MailboxRestoreRequest -Identity "Debra Garcia\MailboxRestore1" -BadItemLimit 10
```

This example specifies that the restore request MailboxRestore1 for Florence Flipo's mailbox skips 100 corrupted items. Because the *BadItemLimit* value is greater than 50, the *AcceptLargeDataLoss* parameter must be specified.

```powershell
Set-MailboxRestoreRequest -Identity "Florence Flipo\MailboxRestore1" -BadItemLimit 100 -AcceptLargeDataLoss
```

For detailed syntax and parameter information, see [Set-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxRestoreRequest).

## How do you know this worked?

To verify that you've successfully changed the properties of a restore request, run the **Get-MailboxRestoreRequestStatistics** cmdlet to display the revised properties for the restore request. If the restore request was successfully created, the *Status* property will have a value of `Queued`, `InProgress`, or `Completed`. After the restore request is completed, the contents of the soft-deleted mailbox will appear in the target mailbox.

For detailed syntax and parameter information, see [Get-MailboxRestoreRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxRestoreRequestStatistics).

## Use the Shell to suspend a restore request

You can suspend a restore request any time after the request was created but before the request reaches the status of `Completed`. See Use the Shell to resume a restore request later in this topic for the command syntax to resume the restore request using the [Resume-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Resume-MailboxRestoreRequest) cmdlet.

This example suspends the restore request MailboxRestore1 for Pilar Pinilla's mailbox.

```powershell
Suspend-MailboxRestoreRequest -Identity "Pilar Pinilla\MailboxRestore1"
```

This example suspends all restore requests in progress by first retrieving all requests that have a status of `InProgress`, and then piping the output to the **Suspend-MailboxRestoreRequest** cmdlet and including the suspend comment "Resume after FY13Q2 Maintenance."

```powershell
Get-MailboxRestoreRequest -Status InProgress | Suspend-MailboxRestoreRequest -SuspendComment "Resume after FY13Q2 Maintenance"
```

For detailed syntax and parameter information, see [Suspend-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Suspend-MailboxRestoreRequest).

## How do you know this worked?

To verify that you've successfully suspended a mailbox restore request, run the following command.

```powershell
Get-MailboxRestoreRequest <identity> | Format-List Suspend,Status
```

If the value of the *Suspend* property equals `True`, the restore request was successfully suspended. Also, a value of `Suspended` for the *Status* property indicates that the restore request was suspended.

## Use the Shell to resume a restore request

Use the **Resume-MailboxRestoreRequest** cmdlet to resume a restore request that failed or was suspended.

This example resumes the restore request Pilar Pinilla\\MailboxRestore1.

```powershell
Resume-MailboxRestoreRequest -Identity "Pilar Pinilla\MailboxRestore1"
```

This example resumes all restore requests that have a status of Failed.

```powershell
Get-MailboxRestoreRequest -Status Failed | Resume-MailboxRestoreRequest
```

For detailed syntax and parameter information, see [Resume-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Resume-MailboxRestoreRequest).

## How do you know this worked?

To verify that a restore request has resumed, run the following command.

```powershell
Get-MailboxRestoreRequest <identity> | Format-List Suspend,Status
```

If the value of the *Suspend* property equals `False`, the restore request successfully resumed. Also, a value of `InProgress` for the *Status* property indicates that the restore request resumed.

## Use the Shell to remove a restore request

You can use the **Remove-MailboxRestoreRequest** cmdlet to remove mailbox restore requests. If you remove a restore request after mailbox data begins being copied to the target mailbox, the mailbox data that's copied remains in the target mailbox.

> [!NOTE]
> As previously stated, completed restore requests are retained for 30 days by default before they're automatically deleted.

This example removes the restore request Pilar Pinilla\\MailboxRestore1.

```powershell
Remove-MailboxRestoreRequest -Identity "Pilar Pinilla\MailboxRestore1"
```

This example removes all restore requests that have the status of Completed.

```powershell
Get-MailboxRestoreRequest -Status Completed | Remove-MailboxRestoreRequest
```

This example cancels the restore request by using the *RequestGuid* parameter for a request stored on MBXDB01. The parameter set that requires the *RequestGuid* and *RequestQueue* parameters is used only for Microsoft Replication Service debugging purposes. You should use this parameter set only if instructed by Microsoft Customer Service and Support.

```powershell
Remove-MailboxRestoreRequest -RequestQueue MBXDB01 -RequestGuid 25e0eaf2-6cc2-4353-b83e-5cb7b72d441f
```

For detailed syntax and parameter information, see [Remove-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/Remove-MailboxRestoreRequest).

## How do you know this worked?

To verify that you've successfully removed a mailbox restore request, run the following command.

```powershell
Get-MailboxRestoreRequest -Identity <identity of removed restore request>
```

The command will return an error stating that the restore request doesn't exist.

You can also run the **Get-MailboxRestoreRequest** cmdlet. If a restore request was successfully removed, it won't be included in the results.
