---
title: 'File formats indexed by Exchange Search: Exchange 2013 Help'
TOCTitle: File formats indexed by Exchange Search
ms:assetid: e5110ac1-28e1-4554-acc3-85d08c997bc5
ms:mtpsurl: https://technet.microsoft.com/library/Ee633485(v=EXCHG.150)
ms:contentKeyID: 51407274
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# File formats indexed by Exchange Search

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013 and Exchange Online, Exchange Search includes filters for indexing most common types of file formats included as message attachments. You can also install filters to index additional file types.

> [!NOTE]
> In Exchange 2013, it isn't required to install and register Microsoft Office Filter Pack.<BR>By default, the maximum size file that can be indexed by Exchange Server 2013 on-premises is 32 MB. To increase this size limit, you must add the following registry key on all CAS and multi-role servers in your organization:<BR><CODE>@"SOFTWARE\Microsoft\ExchangeServer\V15\Search\SystemParameters" DWORD: "MaxAttachmentSize"</CODE>

When managing or using Exchange Search and the dependent features (such as [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery)), consider the difference between unsearchable items and file formats that are disabled for indexing or contain content that can't be indexed:

- **Unsearchable items**: When Exchange Search can't index a particular file type for any reason (for example, if a filter isn't installed), the search for the file type fails. Messages containing such attachments are marked as *partially indexed*. Unsearchable items can be retrieved using the [Get-FailedContentIndexDocuments](https://docs.microsoft.com/powershell/module/exchange/Get-FailedContentIndexDocuments) cmdlet. When copying In-Place eDiscovery search results to a discovery mailbox or exporting search results to a PST file, you can include unsearchable items. For more information, see [Unsearchable items in Exchange eDiscovery](unsearchable-items-in-exchange-ediscovery-exchange-2013-help.md).

- **File formats with content that can't be indexed**: Certain file types such as Windows Media Video (WMV) don't contain content that can be indexed and therefore aren't indexed. Messages containing attachments of such file types are also returned as unsearchable items in In-Place eDiscovery searches.

- **Disabled file formats**: In on-premises organizations, an administrator can disable indexing of a specified file format. Messages that contain an attachment that is of a disabled format are returned as unsearchable items.

> [!IMPORTANT]
> Although a message attachment may be unsearchable or is of a file format that can't be indexed, the message subject, message body and other metadata may be indexed so that the message can be returned in searches.

For additional management tasks related to Exchange Search in on-premises organizations, see [Exchange Search procedures](exchange-search-procedures-exchange-2013-help.md).

## Default filters

The following table lists the default search filters installed on an Exchange 2013 Mailbox server and in Exchange Online. You can retrieve the list of default filters by using the [Get-SearchDocumentFormat](https://docs.microsoft.com/powershell/module/exchange/Get-SearchDocumentFormat) cmdlet.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Filter</th>
<th>File extension</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Email message</p></td>
<td><p>.eml</p></td>
</tr>
<tr class="even">
<td><p>Graphics Interchange Format</p></td>
<td><p>.gif</p></td>
</tr>
<tr class="odd">
<td><p>JPEG</p></td>
<td><p>.jpeg</p></td>
</tr>
<tr class="even">
<td><p>Microsoft Excel</p></td>
<td><p>.xls, .xlt, .xlsx, .xlsm, .xlb, .xlc, .xlsb</p></td>
</tr>
<tr class="odd">
<td><p>Excel File</p></td>
<td><p>odbcexcel</p></td>
</tr>
<tr class="even">
<td><p>Microsoft InfoPath</p></td>
<td><p>.infopathml</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft Office Binder</p></td>
<td><p>.obt, obd</p></td>
</tr>
<tr class="even">
<td><p>Microsoft PowerPoint</p></td>
<td><p>.pptx, .pptm, .ppt, .ppsx, .ppsm, .pps, .ppam, .potm, .pot, .potx</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft Publisher</p></td>
<td><p>.pub</p></td>
</tr>
<tr class="even">
<td><p>Microsoft Word</p></td>
<td><p>.doc, .docm, .dotx, .dotm, .dot, .docx</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft XML Paper Specification</p></td>
<td><p>.xps</p></td>
</tr>
<tr class="even">
<td><p>OneNote</p></td>
<td><p>.one</p></td>
</tr>
<tr class="odd">
<td><p>OpenDocument Presentation</p></td>
<td><p>.odp</p></td>
</tr>
<tr class="even">
<td><p>OpenDocument Spreadsheet</p></td>
<td><p>.ods</p></td>
</tr>
<tr class="odd">
<td><p>OpenDocument Text</p></td>
<td><p>.odt</p></td>
</tr>
<tr class="even">
<td><p>Outlook Item</p></td>
<td><p>.msg</p></td>
</tr>
<tr class="odd">
<td><p>Portable Document Format</p></td>
<td><p>.pdf</p></td>
</tr>
<tr class="even">
<td><p>Rich Text</p></td>
<td><p>.rtf</p></td>
</tr>
<tr class="odd">
<td><p>Text</p></td>
<td><p>.txt</p></td>
</tr>
<tr class="even">
<td><p>vCalendar</p></td>
<td><p>.vcs</p></td>
</tr>
<tr class="odd">
<td><p>vCard</p></td>
<td><p>.vcf</p></td>
</tr>
<tr class="even">
<td><p>Visio</p></td>
<td><p>.vdw, .vsd, .vss, .vst, .vsx, .vtx, .vssx, .vssm, .vsdm, .vstx, .vstm, .vdx</p></td>
</tr>
<tr class="odd">
<td><p>Web archive</p></td>
<td><p>.mhtml</p></td>
</tr>
<tr class="even">
<td><p>Web page</p></td>
<td><p>.html</p></td>
</tr>
<tr class="odd">
<td><p>XML document</p></td>
<td><p>.xml</p></td>
</tr>
<tr class="even">
<td><p>ZIP archive</p></td>
<td><p>.zip</p></td>
</tr>
</tbody>
</table>

## Disabled file formats

The following table lists the search filters that are disabled for indexing by default on an Exchange 2013 Mailbox server and in Exchange Online. In Exchange 2013, administrators can disable or re-enable a supported file format for indexing by using the [Set-SearchDocumentFormat](https://docs.microsoft.com/powershell/module/exchange/Set-SearchDocumentFormat) cmdlet. This cmdlet isn't available in Exchange Online.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Filter</th>
<th>File extension</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>AVI</p></td>
<td><p>.avi</p></td>
</tr>
<tr class="even">
<td><p>Bitmap</p></td>
<td><p>.bmp</p></td>
</tr>
<tr class="odd">
<td><p>MP3</p></td>
<td><p>.mp3</p></td>
</tr>
<tr class="even">
<td><p>MPEG</p></td>
<td><p>.mpeg</p></td>
</tr>
<tr class="odd">
<td><p>PNG</p></td>
<td><p>.png</p></td>
</tr>
<tr class="even">
<td><p>Microsoft Windows Wave Audio</p></td>
<td><p>.wav</p></td>
</tr>
</tbody>
</table>
