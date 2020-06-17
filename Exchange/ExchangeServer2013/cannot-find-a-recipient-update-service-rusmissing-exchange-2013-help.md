---
title: 'Cannot find a recipient update service_RUSMissing: Exchange 2013 Help'
TOCTitle: Cannot find a recipient update service_RUSMissing
ms:assetid: 920fbf51-d5e4-4ac6-869f-7f1c5d9a3024
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.rusmissing(v=EXCHG.150)
ms:contentKeyID: 46629034
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Cannot find a recipient update service\_RUSMissing

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft® Exchange Server 2007 or Exchange Server 2010 setup cannot continue because the Recipient Update Service (RUS) responsible for a domain in the existing Exchange organization cannot be found.

Microsoft Exchange setup requires that each domain in the existing Exchange organization have an instance of the Recipient Update Service.

If an instance of the Recipient Update Service is missing for a domain, new user objects created in the domain will not receive e-mail addresses issued to them.

To resolve this issue, verify that an instance of the Recipient Update Service exists for each domain and create an instance of the Recipient Update Service for the domains that do not have one and then rerun Microsoft Exchange setup.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>To create a Recipient Update Service instance for a domain</p></td>
</tr>
<tr class="even">
<td><ol>
<li><p>Open Exchange System Manager.</p></li>
<li><p>Expand <strong>Recipients</strong>.</p></li>
<li><p>Right-click the <strong>Recipient Update Services</strong> node, click <strong>New</strong>, and then click <strong>Recipient Update Service</strong>.</p></li>
<li><p>In the New Object window, click <strong>Browse</strong> to locate the name of the domain.</p></li>
<li><p>Select the name of the domain and then click <strong>OK</strong>.</p></li>
<li><p>In the New Object window, click <strong>Next</strong>, and then <strong>Finish</strong>.</p></li>
</ol></td>
</tr>
</tbody>
</table>
