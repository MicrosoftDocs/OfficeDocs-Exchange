---
title: 'Deployment security checklist: Exchange 2013 Help'
TOCTitle: Deployment security checklist
ms:assetid: 0cbfad59-f503-48a0-8184-6ca999d89e61
ms:mtpsurl: https://technet.microsoft.com/library/Aa996026(v=EXCHG.150)
ms:contentKeyID: 49289162
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Deployment security checklist

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 features are designed to help improve the security of your messaging environment. Generally, for Exchange 2013, the following conditions are true:

- Accounts that are used by Exchange 2013 have the minimum rights that are required to perform the given task sets.

- By default, services are started only when they are required.

- Access control list (ACL) rights for Exchange objects are minimized.

- Administrative permissions are set according to the scope of change on the object that a given modification requires.

- By default, all internal default message paths are encrypted.

This topic lists steps that we recommend you take to harden the messaging environment before you install Microsoft Exchange. We recommend that you refer to this checklist every time that you install a new Exchange server role.

Before installing Exchange 2013, perform the following procedures.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Procedure</th>
<th>Done?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Run Microsoft Update.</p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Run the Microsoft Windows Malicious Software Removal Tool. The Malicious Software Removal Tool is included with Microsoft Update. More information about the tool can be found at <a href="https://www.microsoft.com/download/details.aspx?id=9905">Malicious Software Removal Tool</a>.</p></td>
<td><p> </p></td>
</tr>
<tr class="odd">
<td><p>Run the <a href="https://www.microsoft.com/download/details.aspx?id=19892">Microsoft Baseline Security Analyzer</a>.</p></td>
<td><p> </p></td>
</tr>
</tbody>
</table>
