---
title: 'Access control list (ACL) inheritance is blocked'
TOCTitle: Access control list (ACL) inheritance is blocked_InhBlockPublicFolderTree
ms:assetid: e3b89c8a-d6f8-4864-8bf0-35a78ce87cc4
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.inhblockpublicfoldertree(v=EXCHG.150)
ms:contentKeyID: 46629148
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Access control list (ACL) inheritance is blocked\_InhBlockPublicFolderTree

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 or Exchange Server 2010 setup cannot continue because the required permissions have not been able to propagate.

Exchange setup requires that inheritance for permissions be enabled on the following Exchange objects:

- Exchange Organization object

- Exchange Administrative Group object

- Exchange Servers container object

- Exchange Address List object

- Exchange Public Folder object

- Exchange Public Folder tree object

Failure to enable inheritance for permissions on these objects may result in mail flow problems, store mounting issues, and other service outages.

To resolve this issue, make sure that the "Allow permissions to propagate to this object and child objects" setting is enabled for the object, and then rerun Exchange Server 2007 or Exchange 2010 setup.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>To re-enable permissions inheritance for an Exchange configuration object using Exchange Server 2003 Exchange System Manager</p></td>
</tr>
<tr class="even">
<td><ol>
<li><p>Enable the <strong>Security</strong> tab for the object properties box of Exchange System Manager by setting a registry parameter.</p>
<ol>
<li><p>Start Registry Editor (Regedt32.exe).</p></li>
<li><p>Locate the following key in the registry:</p>
<p><strong>HKEY_CURRENT_USER\Software\Microsoft\Exchange\EXAdmin</strong></p></li>
<li><p>On the <strong>Edit</strong> menu, click <strong>New</strong>, and then add the following registry value:</p>
<p><strong>Value Name</strong>: ShowSecurityPage</p>
<p><strong>Data Type</strong>: REG_DWORD</p>
<p><strong>Radix</strong>: Binary</p>
<p><strong>Value</strong>: 1</p></li>
<li><p>Quit Registry Editor.</p></li>
</ol>

> [!NOTE]
> By default, the <STRONG>Security</STRONG> tab is not enabled in the configuration object properties box.

</li>
<li><p>Open Exchange System Manager, find the object in question, right-click the object and select <strong>Properties</strong>.</p></li>
<li><p>Select the <strong>Security</strong> tab and then click <strong>Advanced</strong>.</p></li>
<li><p>Select <strong>Allow inheritable permissions from the parent to propagate to this object and all child objects</strong> to re-enable permissions inheritance.</p></li>
<li><p>Restart Exchange Server.</p></li>
</ol></td>
</tr>
</tbody>
</table>

> [!WARNING]
> If you incorrectly modify the attributes of Active Directory objects when you use ADSI Edit, the LDP tool, or another LDAP version&nbsp;3 client, you may cause serious problems. These problems may require that you reinstall Microsoft Windows&nbsp;Server™&nbsp;2003, Exchange&nbsp;Server, or both. Modify Active Directory object attributes at your own risk.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>To re-enable permissions inheritance for an Exchange configuration object using ADSIEdit from Exchange Server 2007 or Exchange Server 2010</p></td>
</tr>
<tr class="even">
<td><ol>
<li><p>Install ADSI Edit.</p></li>
<li><p>Launch ADSI Edit. Click <strong>Start</strong>, click <strong>Run</strong>, type <strong>adsiedit.msc</strong> in the text box, and then click OK.</p></li>
<li><p>Navigate to the object in question, right-click the object and select <strong>Properties</strong>.</p></li>
<li><p>Select the <strong>Security</strong> tab and then click <strong>Advanced</strong>.</p></li>
<li><p>Select <strong>Allow inheritable permissions from the parent to propagate to this object and all child objects</strong> to re-enable permissions inheritance.</p></li>
<li><p>Select <strong>Ok</strong> twice to apply the change.</p></li>
<li><p>Wait for Active Directory replication to propagate the changes or force Active Directory replication.</p></li>
</ol></td>
</tr>
</tbody>
</table>
