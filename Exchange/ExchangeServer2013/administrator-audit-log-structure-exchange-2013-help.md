---
title: 'Administrator audit log structure: Exchange 2013 Help'
TOCTitle: Administrator audit log structure
ms:assetid: 87e259c9-c884-4d53-bd78-d13f2300d73e
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Ff459251(v=EXCHG.150)
ms:contentKeyID: 50117644
ms.date: 05/13/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Administrator audit log structure

_**Applies to:** Exchange Server 2013_

Administrator audit logs contain a record of all the cmdlets and parameters that have been run in the Exchange Management Shell and by the Exchange Administration Center (EAC). They're created on-demand when you run the Administrator audit log report in the EAC, or when you run the **New-AdminAuditLogSearch** cmdlet in the Shell. For more information about audit logs, see [Administrator audit logging](administrator-audit-logging-exchange-2013-help.md).

The audit logs are XML files and can contain multiple audit log entries. The following table describes each XML tag and its associated attributes.

Looking for management tasks related to Administrator audit logs? See [Manage administrator audit logging](manage-administrator-audit-logging-exchange-2013-help.md).

### Audit log XML tags and attributes

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Element</th>
<th>Attribute</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;</code></p>
<p></p></td>
<td><p><code>N/A</code></p></td>
<td><p>This is the XML document declaration tag. It's included in every audit log XML file and contains the XML version number and the character encoding value.</p></td>
</tr>
<tr class="even">
<td><p><code>SearchResults</code></p></td>
<td><p><code>N/A</code></p></td>
<td><p>This tag contains all the audit log entries in the XML file. The <code>Event</code> tag is a child of this tag.</p>
<p>There is only one <code>SearchResults</code> tag per XML file.</p></td>
</tr>
<tr class="odd">
<td><p><code>Event</code></p></td>
<td><p></p></td>
<td><p>This tag contains the audit log entry for an individual cmdlet. This tag contains the <code>Caller</code>, <code>Cmdlet</code>, <code>ObjectModified</code>, <code>RunDate</code>, <code>Succeeded</code>, <code>Error</code>, and <code>OriginatingServer</code> attributes. The <code>CmdletParameters</code> and <code>ModifiedProperties</code> tags are children of this tag.</p>
<p>There is one <code>Event</code> tag per audit log entry.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>Caller</code></p></td>
<td><p>This attribute contains the user account of the user who ran the cmdlet in the <code>Cmdlet</code> attribute.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>Cmdlet</code></p></td>
<td><p>This attribute contains the name of the cmdlet that was run by the user in the <code>Caller</code> attribute.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>ObjectModified</code></p></td>
<td><p>This attribute contains the object that was modified by the cmdlet specified in the <code>Cmdlet</code> attribute. The <code>ModifiedProperties</code> tag shows which properties were modified on this object.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>RunDate</code></p></td>
<td><p>This attribute contains the date and time when the cmdlet in the <code>Cmdlet</code> attribute was run.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>Succeeded</code></p></td>
<td><p>This attribute specifies whether the cmdlet in the <code>Cmdlet</code> attribute ran successfully. The value is either <code>True</code> or <code>False</code>.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>Error</code></p></td>
<td><p>This attribute contains the error message generated if the cmdlet in the <code>Cmdlet</code> attribute failed to complete successfully. If no error was encountered, the value is set to <code>None</code>.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>OriginatingServer</code></p></td>
<td><p>This attribute contains the server on which the cmdlet specified in the <code>Cmdlet</code> attribute was run.</p></td>
</tr>
<tr class="odd">
<td><p><code>CmdletParameters</code></p></td>
<td><p><code>N/A</code></p></td>
<td><p>This tag contains all of the parameters specified when the cmdlet was run. The <code>Parameter</code> tag is a child of this tag.</p>
<p>There is one <code>CmdletParameters</code> tag per <code>Event</code> tag.</p></td>
</tr>
<tr class="even">
<td><p><code>Parameter</code></p></td>
<td><p></p></td>
<td><p>This tag contains an individual parameter that was specified when the cmdlet was run. This tag contains the <code>Name</code> and <code>Value</code> attributes.</p>
<p>There can be multiple <code>Parameter</code> tags per <code>CmdletParameters</code> tag.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>Name</code></p></td>
<td><p>This attribute contains the name of the parameter that was specified on the cmdlet that was run.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>Value</code></p></td>
<td><p>This attribute contains the value that was provided on the parameter specified in the <code>Name</code> attribute.</p></td>
</tr>
<tr class="odd">
<td><p><code>ModifiedProperties</code></p></td>
<td><p><code>N/A</code></p></td>
<td><p>This tag contains all of the properties that were modified by the cmdlet that was run. The <code>Property</code> tag is a child of this tag.</p>
<p>There is one <code>ModifiedProperties</code> tag per <code>Event</code> tag.</p>

> [!IMPORTANT]
> This tag is only populated if the <EM>LogLevel</EM> parameter on the <STRONG>Set-AdminAuditLogConfig</STRONG> cmdlet is set to <CODE>Verbose</CODE>.

</td>
</tr>
<tr class="even">
<td><p><code>Property</code></p></td>
<td><p></p></td>
<td><p>This tag contains an individual property that was specified when the cmdlet was run. This tag contains the <code>Name</code>, <code>OldValue</code>, and <code>NewValue</code> attributes.</p>
<p>There can be multiple <code>Property</code> tags per <code>ModifiedProperties</code> tag.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>Name</code></p></td>
<td><p>This attribute contains the name of the property that was modified when the cmdlet was run.</p></td>
</tr>
<tr class="even">
<td><p></p></td>
<td><p><code>OldValue</code></p></td>
<td><p>This attribute contains the value that was contained in the property specified in the <code>Name</code> attribute before it was changed.</p></td>
</tr>
<tr class="odd">
<td><p></p></td>
<td><p><code>NewValue</code></p></td>
<td><p>This attribute contains the value that the property in the <code>Name</code> attribute was changed to.</p></td>
</tr>
</tbody>
</table>

## Example audit log entry

The following is an example of a typical audit log entry. Based on the information in log entry, we know the following occurred:

- On 10/18/2012 at 3:48 P.M. Pacific Daylight Time (UTC-7), the user `Administrator` ran the cmdlet **Set-Mailbox**.

- The two following parameters were provided when the **Set-Mailbox** cmdlet was run:

  - *Identity* with a value of `david`

  - *ProhibitSendReceiveQuota* with a value of `10GB`

- The two following properties on the object `david` were modified:

  > [!NOTE]
  > The modified properties are saved to the audit log because the <EM>LogLevel</EM> parameter on the <CODE>Set-AdminAuditLogConfig</CODE> cmdlet was set to <CODE>Verbose</CODE> in this example.

  - *ProhibitSendReceiveQuota* with a new value of `10GB`, which replaced the old value of `35GB`

- The operation completed successfully without any errors.

```XML
<?xml version="1.0" encoding="utf-8"?>
<SearchResults>

  <Event Caller="corp.e15a.contoso.com/Users/Administrator" Cmdlet="Set-Mailbox" ObjectModified="corp.e15a.contoso.com/Users/david" RunDate="2012-10-18T15:48:15-07:00" Succeeded="true" Error="None" OriginatingServer="WIN8MBX (15.00.0516.032)">
    <CmdletParameters>
      <Parameter Name="Identity" Value="david" />
      <Parameter Name="ProhibitSendReceiveQuota" Value="10 GB (10,737,418,240 bytes)" />
    </CmdletParameters>
    <ModifiedProperties>
      <Property Name="ProhibitSendReceiveQuota" OldValue="35 GB (37,580,963,840 bytes)" NewValue="10 GB (10,737,418,240 bytes)" />
    </ModifiedProperties>
  </Event>
</SearchResults>
```
