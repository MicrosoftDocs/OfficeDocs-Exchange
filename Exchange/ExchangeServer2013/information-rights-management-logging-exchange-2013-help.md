---
title: 'Information Rights Management logging: Exchange 2013 Help'
TOCTitle: Information Rights Management logging
ms:assetid: e06f57f9-a9e2-43a2-b88c-288b324d71f0
ms:mtpsurl: https://technet.microsoft.com/library/Ff461940(v=EXCHG.150)
ms:contentKeyID: 49319937
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Information Rights Management logging

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, Information Rights Management (IRM) operations are logged in IRM logs. IRM logs help you monitor and troubleshoot interactions between the Rights Management Services (RMS) client on an Exchange 2013 server and the Active Directory Rights Management Services (AD RMS) cluster in your organization.

To learn about IRM, see [Information Rights Management](information-rights-management-exchange-2013-help.md).

Looking for management tasks related to IRM? See [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## Structure of IRM logs

By default, IRM logs are located in C:\\Program Files\\Microsoft\\Exchange Server\\V14\\Logging\\IRMLogs.

The naming convention for IRM log files is \<*Process*\>\_\<*Process identifier* or *IIS AppPool identifier*\>\_IRMLOG*yyyymmdd*-*nnnn*.log, where:

- \<*Process*\> = process that creates the log file. For example, on Transport service, this will be EdgeTransport.

- \<*Process identifier* or *IIS AppPool identifier\>* = numerical ID of the process.

- *yyyymmdd* = Coordinated Universal Time (UTC) date when the log file was created.

- *nnnn* = instance number, which starts at 1 for each day.

An example IRM log file name is EdgeTransport\_1056\_IRMLOG20101201-1.log.

The following table shows the logs generated on different server roles.

### Logs on server roles

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Server role</th>
<th>IRM log file name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Transport service</p></td>
<td><p>EdgeTransport_&lt;<em>Process identifier</em>&gt;_IRMLOG<em>yyyymmdd</em>-<em>nnnn</em>.log</p></td>
<td><p>This log is used to record all RMS transactions made by the transport pipeline on Transport service (for example, transport protection rules and journal report decryption). The process identifier (PID) of the edgetransport.exe process is used to generate the log file name.</p></td>
</tr>
<tr class="even">
<td><p>Mailbox</p></td>
<td><p>msftefd_&lt;<em>Process identifier</em>&gt;_IRMLOG<em>yyyymmdd</em>-<em>nnnn</em>.log</p></td>
<td><p>This log is used to record all RMS transactions that occur during search and index requests. Exchange 2013 Mailbox servers use the msftefd.exe process for content indexing. The PID of the msftefd.exe process is used to generate the log file name.</p></td>
</tr>
<tr class="odd">
<td><p>Client Access</p></td>
<td><p>w3wp_MSExchangeOWAAppOol_IRMLOG<em>yyyymmdd</em>-<em>nnnn</em>.log</p></td>
<td><p>This log is used to record all transactions for IRM in Microsoft Office Outlook Web App.</p></td>
</tr>
<tr class="even">
<td><p>All Exchange 2013 server roles</p></td>
<td><p>w3wp_MSExchangePowerShellAppPool_IRMLOG<em>yyyymmdd</em>-<em>nnnn</em>.log</p></td>
<td><p>This log is used to record all IRM RMS transactions issued from Windows PowerShell, for example, when issuing the <strong>Test-IRMConfiguration</strong> cmdlet.</p></td>
</tr>
</tbody>
</table>

## Logging process

Information is written to the log file until the file size reaches its maximum specified value. When the maximum size is reached, a log file that has an incremental instance number is created. This process is repeated throughout the day. Circular logging deletes the oldest log files when the IRM log directory reaches its maximum specified size or when a log file reaches the maximum age specified in the IRM logging configuration on each server.

## Information written to IRM logs

IRM log files are text files that contain data in comma-separated value (CSV) format. Each IRM log has a header that contains the following information:

- **\#Software**: Name of the software that created the IRM log file. Typically, the value is `Microsoft Exchange Server`.

- **\#Version**: Version number of the software that created the IRM log file.

- **\#Log-type**: Log type value, which is `Rms Client Manager Log`.

- **\#Date**: The UTC date and time when the log file was created. The UTC date and time is represented in the ISO 8601 date-time format: *yyyy*-*mm*-*dd*T*hh*:*mm*:*ss.fff*Z, where:

  - yyyy = year

  - *mm* = month

  - *dd* = day

  - T = time designator used to show the start of the time component

  - *hh* = hour

  - *mm* = minute

  - *ss* = second

  - *fff* = fractions of a second

  - Z = Zulu, which is another way to denote UTC

- **\#Fields**: Comma-delimited field names used in IRM log files.

    The IRM log stores each RMS transaction event on a single line, organized in comma-separated fields. The following table lists the fields in IRM logs for all server roles that have IRM features enabled.

    <table>
    <colgroup>
    <col style="width: 50%" />
    <col style="width: 50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th>Field</th>
    <th>Description</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td><p><strong>Date-time</strong></p></td>
    <td><p>Lists the UTC timestamp.</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>Feature</strong></p></td>
    <td><p>Lists the RMS client feature used. Valid values include:</p>
    <ul>
    <li><p><code>RacClc</code></p></li>
    <li><p><code>Template</code></p></li>
    <li><p><code>Prelicense</code></p></li>
    <li><p><code>UseLicense</code></p></li>
    <li><p>Signature verification</p></li>
    <li><p><code>ServerInfo</code></p></li>
    </ul></td>
    </tr>
    <tr class="odd">
    <td><p><strong>Event-Type</strong></p></td>
    <td><p>Lists the event type. Valid values include:</p>
    <ul>
    <li><p><code>Acquire</code>   An RMS license or template is requested.</p></li>
    <li><p><code>Success</code>   An RMS license or template is acquired successfully.</p></li>
    <li><p><code>Exception</code>   An error has occurred.</p></li>
    <li><p><code>Queued</code>   A request is pending.</p></li>
    </ul></td>
    </tr>
    <tr class="even">
    <td><p><strong>Tenant-Id</strong></p></td>
    <td><p>Reserved for internal Microsoft use.</p></td>
    </tr>
    <tr class="odd">
    <td><p><strong>Server-url</strong></p></td>
    <td><p>Lists the RMS server URL accessed during the operation.</p></td>
    </tr>
    <tr class="even">
    <td><p><strong>Context</strong></p></td>
    <td><p>Used by the calling process to tie multiple RMS transactions together. Valid values include:</p>
    <ul>
    <li><p><code>MessageID: &lt;Actual message ID&gt;</code></p></li>
    <li><p><code>MailboxGuid: &lt;Mailbox GUID&gt;</code></p></li>
    <li><p><code>AttachmentFileName: &lt;File name&gt;</code></p></li>
    </ul></td>
    </tr>
    <tr class="odd">
    <td><p><strong>Transaction-id</strong></p></td>
    <td><p>Identifies a unique transaction. All events that occur during one transaction have the same transaction ID.</p></td>
    </tr>
    </tbody>
    </table>

## Managing IRM logs

On each server role that has IRM features enabled, IRM logging is enabled by default. For each server role, you can modify the following IRM log configuration by using the server role's corresponding **Set** cmdlet. For example, to configure IRM logging on a Mailbox server, you use the **Set-MailboxServer** cmdlet.

### Configuration parameters for IRM logs

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>IrmLogEnabled</em></p></td>
<td><p>Enables logging of IRM transactions. IRM logging is enabled by default. To disable IRM logging for a server role, set the parameter to <code>$false</code>.</p></td>
</tr>
<tr class="even">
<td><p><em>IrmLogMaxAge</em></p></td>
<td><p>Specifies the maximum age for an IRM log file. Files older than the specified age are deleted. The default value is <code>30.00:00:00</code> (30 days).</p></td>
</tr>
<tr class="odd">
<td><p><em>IrmLogMaxDirectorySize</em></p></td>
<td><p>Specifies the maximum size of all IRM logs in the connectivity log directory. When a directory reaches its maximum file size, the server deletes the oldest log files first. The default value is <code>250 MB</code>.</p></td>
</tr>
<tr class="even">
<td><p><em>IrmLogMaxFileSize</em></p></td>
<td><p>Specifies the maximum file size for a single log file. When a file reaches the specified size, a log file is created, and the instance number is incremented. The default value is <code>10 MB</code>.</p></td>
</tr>
<tr class="odd">
<td><p><em>IrmLogPath</em></p></td>
<td><p>Specifies the IRM log location. The default path is %ExchangeInstallPath%Logging\IRMLogs.</p></td>
</tr>
</tbody>
</table>

For detailed syntax and parameter information, see the following topics:

- [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxServer)

- [Set-ClientAccessServer](https://docs.microsoft.com/powershell/module/exchange/Set-ClientAccessServer)

- [Set-TransportService](https://docs.microsoft.com/powershell/module/exchange/Set-TransportService)
