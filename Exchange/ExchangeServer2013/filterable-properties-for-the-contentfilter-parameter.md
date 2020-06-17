---
title: Filterable properties for the -ContentFilter parameter
TOCTitle: Filterable properties for the -ContentFilter parameter
ms:assetid: cf504a59-1938-489c-bb48-b27b2ac3234e
ms:mtpsurl: https://technet.microsoft.com/library/Ff601762(v=EXCHG.150)
ms:contentKeyID: 49895015
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Filterable properties for the -ContentFilter parameter

_**Applies to:** Exchange Server 2013_

This topic lists the filterable properties for the *ContentFilter* parameter. The *ContentFilter* parameter is used to export messages to a .pst file that match the filter. The *ContentFilter* parameter is used in the [New-MailboxExportRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxExportRequest) cmdlet.

## Filterable properties

Many of the properties for the *ContentFilter* parameter accept wildcard characters. If you use a wildcard character, use the **-like** operator instead of the **-eq** operator. The **-like** operator is used to find pattern matches in rich types, such as strings, whereas the **-eq** operator is used to find an exact match.

The following table contains a list of the filterable properties for the *ContentFilter* parameter. This table lists the name of the property, a description, the acceptable values, and a syntax example. For more information about OPATH filters, see [Recipient filters in Exchange PowerShell commands - Additional OPATH syntax information](https://docs.microsoft.com/powershell/exchange/recipient-filters#additional-opath-syntax-information).

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Description</th>
<th>Values</th>
<th>Example syntax</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>All</p></td>
<td><p>This property returns all messages that have a particular string in any of the indexed properties. For example, use this property if you want to export all messages that have &quot;Ayla&quot; as the recipient, the sender, or have the name mentioned in the message body.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "All -like '*Ayla*'"
```

</td>
</tr>
<tr class="even">
<td><p>Attachment</p></td>
<td><p>This property returns messages that have the specified string in the content of an attachment or in the attachment's file name.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "Attachment -like '*.jpg'"
```

</td>
</tr>
<tr class="odd">
<td><p>BCC</p></td>
<td><p>This property returns sent messages that have the specified recipient in the Bcc field.</p></td>
<td><p>Display name</p>
<p>Alias</p>
<p>SMTP address</p>
<p>LegacyDN</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "(BCC -eq 'ayla@contoso.com') -or (BCC -eq 'tony@contoso.com')"
```

</td>
</tr>
<tr class="even">
<td><p>Body</p></td>
<td><p>This property returns messages that have the specified string within the message body.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "Body -like '*prospectus*'"
```

</td>
</tr>
<tr class="odd">
<td><p>Category</p></td>
<td><p>This property returns messages that have a matching category. Categories are set by users or Inbox rules.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "Category -like '*Blue*'"
```

</td>
</tr>
<tr class="even">
<td><p>CC</p></td>
<td><p>This property returns sent messages that have the specified recipient in the Cc field.</p></td>
<td><p>Display name</p>
<p>Alias</p>
<p>SMTP address</p>
<p>LegacyDN</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "(CC -eq 'ayla@contoso.com') -or (CC -eq 'tony@contoso.com')"
```

</td>
</tr>
<tr class="odd">
<td><p>Expires</p></td>
<td><p>This property returns messages that have a specified expiration time stamp.</p></td>
<td><p>Date-Time stamp</p></td>
<td>

```powershell
-ContentFilter "Expires -lt '01/01/2013'"
```

</td>
</tr>
<tr class="even">
<td><p>HasAttachment</p></td>
<td><p>This property returns messages with or without attachments.</p></td>
<td><p>Boolean</p>
<p><code>$true</code> or <code>$false</code></p></td>
<td>

```powershell
-ContentFilter "HasAttachment -eq `$true"
```

</td>
</tr>
<tr class="odd">
<td><p>Importance</p></td>
<td><p>This property returns messages that have a specified importance level.</p></td>
<td><p>0 or &quot;Low&quot;</p>
<p>1 or &quot;Normal&quot;</p>
<p>2 or &quot;High&quot;</p></td>
<td>

```powershell
-ContentFilter "Importance -eq 'high'"
```

```powershell
-ContentFilter "Importance -eq 2"
```

</td>
</tr>
<tr class="even">
<td><p>IsFlagged</p></td>
<td><p>This property returns messages that have been flagged by the user or Inbox rule.</p></td>
<td><p>Boolean</p>
<p><code>$true</code> or <code>$false</code></p></td>
<td>

```powershell
-ContentFilter "IsFlagged -eq `$true"
```

</td>
</tr>
<tr class="odd">
<td><p>IsRead</p></td>
<td><p>This property returns messages that have been read or not read by the user.</p></td>
<td><p>Boolean</p>
<p><code>$true</code> or <code>$false</code></p></td>
<td>

```powershell
-ContentFilter "IsRead -eq `$true"
```

</td>
</tr>
<tr class="even">
<td><p>MessageKind</p></td>
<td><p>This property returns messages that are of the specified type.</p></td>
<td><p>Calendar</p>
<p>Contact</p>
<p>Doc</p>
<p>Email</p>
<p>Fax</p>
<p>InstantMessage</p>
<p>Journal</p>
<p>Note</p>
<p>Post</p>
<p>RSSFeed</p>
<p>Task</p>
<p>Voicemail</p></td>
<td>

```powershell
-ContentFilter "MessageKind -eq 'Calendar'"
```

```powershell
-ContentFilter "MessageKind -ne 'Email'"
```

</td>
</tr>
<tr class="odd">
<td><p>MessageLocalee</p></td>
<td><p>This property returns messages that are of the specified locale.</p></td>
<td><p>CultureInfo</p></td>
<td>

```powershell
-ContentFilter "MessageLocale -ne 'en-US'"
```

```powershell
-ContentFilter "MessageLocale -eq 'tr-TR'"
```

</td>
</tr>
<tr class="even">
<td><p>Participants</p></td>
<td><p>This property returns messages that have the specified recipient in the To, Bcc, or Cc fields.</p></td>
<td><p>Display name</p>
<p>Alias</p>
<p>SMTP address</p>
<p>LegacyDN</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "(Participants -eq 'ayla@contoso.com') -or (Participants -eq 'tony@contoso.com')"
```

</td>
</tr>
<tr class="odd">
<td><p>PolicyTag</p></td>
<td><p>This property returns messages that have a policy tag. The Exchange store persists policy tags as GUIDs. Therefore, the string can contain either an explicit GUID value, which is then searched by the PR_POLICY_TAG, or a wildcard string.</p>
<p>If the supplied value isn't a GUID, the command uses Active Directory information to resolve names to GUIDs.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "PolicyTag -ne '00000000-0000-0000-0000-000000000000'"
```

</td>
</tr>
<tr class="even">
<td><p>Received</p></td>
<td><p>This property returns messages that were received with the specified Received time stamp.</p></td>
<td><p>Date-Time stamp</p></td>
<td>

```powershell
-ContentFilter "Received -lt '01/01/2013 9:00'"
```

```powershell
-ContentFilter "(Received -lt '01/01/2013') -and (Received -gt '01/01/2012')"
```

</td>
</tr>
<tr class="odd">
<td><p>Sender</p></td>
<td><p>This property returns messages that were received from the specified sender.</p></td>
<td><p>Display name</p>
<p>Alias</p>
<p>SMTP address</p>
<p>LegacyDN</p>
<p>Wildcard</p></td>
<td>

```powershell
ContentFilter "Sender -eq 'tony'"
```

</td>
</tr>
<tr class="even">
<td><p>Sent</p></td>
<td><p>This property returns messages that were sent with the specified Sent time stamp.</p></td>
<td><p>Date-Time stamp</p></td>
<td>

```powershell
-ContentFilter "Sent -lt '01/01/2013 9:00'"
```

```powershell
-ContentFilter "(Sent -lt '01/01/2013') -and (Sent -gt '01/01/2012')"
```

</td>
</tr>
<tr class="odd">
<td><p>Size</p></td>
<td><p>This property returns messages that are of a specific size.</p></td>
<td><p>B (bytes)</p>
<p>KB (kilobytes)</p>
<p>MB (megabytes)</p></td>
<td>

```powershell
-ContentFilter "Size -gt '10KB'"
```

</td>
</tr>
<tr class="even">
<td><p>Subject</p></td>
<td><p>This property returns messages that have the specified string within the subject of the message.</p></td>
<td><p>String</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "Subject -like '*meeting*'"
```

</td>
</tr>
<tr class="odd">
<td><p>To</p></td>
<td><p>This property returns sent messages that have the specified recipient in the To field.</p></td>
<td><p>Display name</p>
<p>Alias</p>
<p>SMTP address</p>
<p>LegacyDN</p>
<p>Wildcard</p></td>
<td>

```powershell
-ContentFilter "To -eq 'aylakol'"
```

</td>
</tr>
</tbody>
</table>
