---
title: 'Use transport rules to inspect message attachments: Exchange 2013 Help'
TOCTitle: Use transport rules to inspect message attachments
ms:assetid: c0de687e-e33c-4e8a-b253-771494678795
ms:mtpsurl: https://technet.microsoft.com/library/JJ674307(v=EXCHG.150)
ms:contentKeyID: 49319929
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Use transport rules to inspect message attachments

_**Applies to:** Exchange Server 2013_

You can inspect email attachments in your organization by setting up transport rules. Exchange offers transport rules that provide the ability to examine email attachments as a part of your messaging security and compliance needs. When you inspect attachments, you can then take action on the messages that were inspected based on the content or characteristics of those attachments. Here are some attachment-related tasks you can do by using transport rules:

- Search files in compressed attachments such as .zip and .rar files and, if there's any text that matches a pattern you specify, add a disclaimer to the end of the message.

- Inspect content within attachments and, if there are any keywords you specify, redirect the message to a moderator for approval before it's delivered.

- Check for messages with attachments that can't be inspected and then block the entire message from being sent.

- Check for attachments that exceed a certain size and then notify the sender of the issue if you choose to prevent the message from being delivered.

- Create notifications that alert users if they send a message that has matched a transport rule.

- Block all messages containing attachments. For examples, see [Common attachment blocking scenarios](https://docs.microsoft.com/exchange/security-and-compliance/mail-flow-rules/common-attachment-blocking-scenarios).

Exchange administrators can create transport rules by going to **Exchange admin center** \> **Mail flow** \> **Rules**. You need to be assigned permissions before you can perform this procedure. After you start to create a new rule, you can see the full list of attachment-related conditions by clicking **More options** \> **Any attachment** under **Apply this rule if**. The attachment-related options are shown in the following diagram.

![Dialog box to select attachment-related rules](images/JJ674307.2ae4a179-bfd2-4a0e-abe1-53ed4e9e3368(EXCHG.150).jpg "Dialog box to select attachment-related rules")

For more information about transport rules, including the full range of conditions and actions that you can choose, see [Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md). Exchange Online Protection (EOP) and hybrid customers can benefit from the transport rules best practices provided in [Best practices for configuring EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/best-practices-for-configuring-eop). If you're ready to start creating rules, see [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md).

## Inspect the content within attachments

You can use the transport rule conditions in the following table to examine the content of attachments to messages. For these conditions, only the first 150 KB of an attachment is inspected. In order to start using these conditions when inspecting messages, you need to add them to a transport rule. Learn about creating or changing rules at [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md)

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Condition name in EAC</th>
<th>Condition name in the Shell</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Any attachment content includes any of these words</strong></p></td>
<td><p><code>AttachmentContainsWords</code></p></td>
<td><p>This condition matches messages with supported file type attachments that contain a specified string or group of characters.</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment content matches these text patterns</strong></p></td>
<td><p><code>AttachmentMatchesPatterns</code></p></td>
<td><p>This condition matches messages with supported file type attachments that contain a text pattern that matches a specified regular expression.</p></td>
</tr>
</tbody>
</table>

The Exchange Management Shell names for the conditions listed here are parameters that require the `TransportRule` cmdlet.

- Learn more about the cmdlet at [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/New-TransportRule).

- Learn more about property types for these conditions at [Conditions and Condition Properties for a Mailbox Server](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md).

Transport rules can inspect only the content of supported file types. If the transport rules agent encounters an attachment that isn't in the list of supported file types, the `AttachmentIsUnsupported` condition is triggered. The supported file types are listed in the following section. Any file not listed will trigger the `AttachmentIsUnsupported` condition.

## Compressed archive files

If the message contains a compressed archive file such as a .zip or .cab file, the transport rules agent will inspect the files contained within that attachment. Such messages are processed in a manner similar to messages that have multiple attachments. The properties of compressed archive files aren't inspected. For example, if the container file type supports comments, that field isn't inspected.

## Supported file types for transport rule content inspection

The following table lists the file types supported by transport rules. The system automatically detects file types by inspecting file properties rather than the actual file name extension, thus helping to prevent malicious hackers from being able to bypass transport rule filtering by renaming a file extension. A list of file types with executable code that can be checked within the context of transport rules is listed later in this topic.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Category</th>
<th>File extension</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Office 2013, Office 2010, and Office 2007</p></td>
<td><p>.docm, .docx, .pptm, .pptx, .pub, .one, .xlsb, .xlsm, .xlsx</p></td>
<td><p>Microsoft OneNote and Microsoft Publisher files aren't supported by default. You can enable support for these file types by using IFilter integration. For more information, see <a href="register-filter-pack-ifilters-with-exchange-2013-exchange-2013-help.md">Register Filter Pack IFilters with Exchange 2013</a>.</p>
<p>The contents of any embedded parts contained within these file types are also inspected. However, any objects that aren't embedded (for example, linked documents) aren't inspected.</p></td>
</tr>
<tr class="even">
<td><p>Office 2003</p></td>
<td><p>.doc, .ppt, .xls</p></td>
<td><p>None</p></td>
</tr>
<tr class="odd">
<td><p>Additional Office files</p></td>
<td><p>.rtf, .vdw, .vsd, .vss, .vst</p></td>
<td><p>None</p></td>
</tr>
<tr class="even">
<td><p>Adobe PDF</p></td>
<td><p>.pdf</p></td>
<td><p>None</p></td>
</tr>
<tr class="odd">
<td><p>HTML</p></td>
<td><p>.html</p></td>
<td><p>None</p></td>
</tr>
<tr class="even">
<td><p>XML</p></td>
<td><p>.xml, .odp, .ods, .odt</p></td>
<td><p>None</p></td>
</tr>
<tr class="odd">
<td><p>Text</p></td>
<td><p>.txt, .asm, .bat, .c, .cmd, .cpp, .cxx, .def, .dic, .h, .hpp, .hxx, .ibq, .idl, .inc, inf, .ini, inx, .js, .log, .m3u, .pl, .rc, .reg, .txt, .vbs, .wtx</p></td>
<td><p>None</p></td>
</tr>
<tr class="even">
<td><p>OpenDocument</p></td>
<td><p>.odp, .ods, .odt</p></td>
<td><p>No parts of .odf files are processed. For example, if the .odf file contains an embedded document, the contents of that embedded document aren't inspected.</p></td>
</tr>
<tr class="odd">
<td><p>AutoCAD Drawing</p></td>
<td><p>.dxf</p></td>
<td><p>AutoCAD 2013 files aren't supported.</p></td>
</tr>
<tr class="even">
<td><p>Image</p></td>
<td><p>.jpg, .tiff</p></td>
<td><p>Only the metadata text associated with these image files is inspected. There is no optical character recognition.</p></td>
</tr>
</tbody>
</table>

## Inspect the file properties of attachments

The following transport rule conditions inspect the properties of a file that is attached to a message. In order to start using these conditions when inspecting messages, you need to add them to a transport rule. A list of supported file types with executable code that can be checked within the context of transport rules is listed here. For more information about creating or changing rules, see [Manage transport rules in Exchange 2013](manage-transport-rules-exchange-2013-help.md).

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Condition name in EAC</th>
<th>Condition name in the Shell</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Any attachment file name matches these text patterns</strong></p></td>
<td><p><code>AttachmentNameMatchesPatterns</code></p></td>
<td><p>This condition matches messages with supported file type attachments when those attachments have a name that contains the characters you specify.</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment file extension includes these words</strong></p></td>
<td><p><code>AttachmentExtensionMatchesWords</code></p></td>
<td><p>This condition matches messages with supported file type attachments when the file name extension matches what you specify.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Any attachment size is greater than or equal to</strong></p></td>
<td><p><code>AttachmentSizeOver</code></p></td>
<td><p>This condition matches messages with supported file type attachments when those attachments are larger than the size you specify.</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment didn't complete scanning</strong></p></td>
<td><p><code>AttachmentProcessingLimitExceeded</code></p></td>
<td><p>This condition matches messages when an attachment is not inspected by the transport rules agent.</p></td>
</tr>
<tr class="odd">
<td><p><strong>Any attachment has executable content</strong></p></td>
<td><p><code>AttachmentHasExecutableContent</code></p></td>
<td><p>This condition matches messages that contain executable files as attachments. The supported file types are listed here.</p></td>
</tr>
<tr class="even">
<td><p><strong>Any attachment is password protected</strong></p></td>
<td><p><code>AttachmentIsPasswordProtected</code></p></td>
<td><p>This condition matches messages with supported file type attachments when those attachments are protected by a password.</p></td>
</tr>
</tbody>
</table>

The Exchange Management Shell names for the conditions listed here are parameters that require the `TransportRule` cmdlet.

- Learn more about the cmdlet at [New-TransportRule](https://docs.microsoft.com/powershell/module/exchange/New-TransportRule).

- Learn more about property types for these conditions at [Conditions and Condition Properties for a Mailbox Server](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md).

## Supported executable file types for transport rule inspection

The transport agent uses true type detection by inspecting file properties rather than merely the file extensions. This helps to prevent malicious hackers from being able to bypass your rule by renaming a file extension. The following table lists the executable file types supported by these conditions. If a file is found that is not listed here, the `AttachmentIsUnsupported` condition is triggered.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type of file</th>
<th>Native extension</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Self-extracting archive file created with the WinRAR archiver.</p></td>
<td><p>.rar</p></td>
</tr>
<tr class="even">
<td><p>32-bit Windows executable file with a dynamic link library extension.</p></td>
<td><p>.dll</p></td>
</tr>
<tr class="odd">
<td><p>Self-extracting executable program file.</p></td>
<td><p>.exe</p></td>
</tr>
<tr class="even">
<td><p>Java archive file.</p></td>
<td><p>.jar</p></td>
</tr>
<tr class="odd">
<td><p>Uninstallation executable file.</p></td>
<td><p>.exe</p></td>
</tr>
<tr class="even">
<td><p>Program shortcut file.</p></td>
<td><p>.exe</p></td>
</tr>
<tr class="odd">
<td><p>Compiled source code file or 3-D object file or sequence file.</p></td>
<td><p>.obj</p></td>
</tr>
<tr class="even">
<td><p>32-bit Windows executable file.</p></td>
<td><p>.exe</p></td>
</tr>
<tr class="odd">
<td><p>Microsoft Visio XML drawing file.</p></td>
<td><p>.vxd</p></td>
</tr>
<tr class="even">
<td><p>OS/2 operating system file.</p></td>
<td><p>.os2</p></td>
</tr>
<tr class="odd">
<td><p>16-bit Windows executable file.</p></td>
<td><p>.w16</p></td>
</tr>
<tr class="even">
<td><p>Disk-operating system file.</p></td>
<td><p>.dos</p></td>
</tr>
<tr class="odd">
<td><p>European Institute for Computer Antivirus Research standard antivirus test file.</p></td>
<td><p>.com</p></td>
</tr>
<tr class="even">
<td><p>Windows program information file.</p></td>
<td><p>.pif</p></td>
</tr>
<tr class="odd">
<td><p>Windows executable program file.</p></td>
<td><p>.exe</p></td>
</tr>
</tbody>
</table>

## Extending the number of supported file types

The supported file types listed in this topic can be revised at any time using IFilter integration. For more information, see [Register Filter Pack IFilters with Exchange 2013](register-filter-pack-ifilters-with-exchange-2013-exchange-2013-help.md).

The file types you add using this process become supported file types and no longer trigger the `AttachmentIsUnsupported` condition.

## Data loss prevention policies and attachment transport rules

To help you manage important business information in email, you can include any of the attachment-related conditions along with the rules of a data loss prevention (DLP) policy. For example, you might want to allow messages with passport numbers to be sent but only if the passport numbers are in a password-protected attachment. To accomplish this, do the following:

- Create a DLP policy that inspects mail for passport-related sensitive information. Learn more at [DLP procedures](dlp-procedures-exchange-2013-help.md).

- Add the **Any attachment is password protected** exception in the **Except if...** transport rule area.

- Define an action to take on mail that contains passport numbers that are not in the protected file.

DLP policies and attachment-related conditions can help you enforce your business needs by defining those needs as transport rule conditions, exceptions, and actions. When you include the sensitive information inspection in a DLP policy, any attachments to messages are scanned for that information only. However, attachment-related conditions such as size or file type are not included until you add the conditions listed in this topic. DLP is not available with all versions of Exchange; learn more at [Data loss prevention](https://docs.microsoft.com/exchange/security-and-compliance/data-loss-prevention/data-loss-prevention).

## For more information

[Data loss prevention in Exchange 2013](data-loss-prevention-exchange-2013-help.md)

[Mail flow or transport rules](mail-flow-rules-transport-rules-in-exchange-2013-exchange-2013-help.md)

[Transport rule conditions (predicates)](mail-flow-rule-conditions-and-exceptions-predicates-in-exchange-2013-exchange-2013-help.md)
