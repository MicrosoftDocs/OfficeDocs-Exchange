---
title: "Can't install Exchange 2013 in a forest containing Exchange 2000 or Exchange 2003 servers"
TOCTitle: Can't install Exchange 2013 in a forest containing Exchange 2000 or Exchange 2003 servers.
ms:assetid: a115b182-cbd2-4d31-aa0e-375240939301
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.exchange2000or2003presentinorg(v=EXCHG.150)
ms:contentKeyID: 49090987
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Can't install Exchange 2013 in a forest containing Exchange 2000 or Exchange 2003 servers.

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 can't continue because one or more computers running Exchange 2000 Server or Exchange Server 2003 were found in the Active Directory forest. Before you can install Exchange 2013, all Exchange 2000 and Exchange 2003 servers must be removed from the forest. Mailboxes, public folders, and all other Exchange objects or components must be upgraded to either Exchange Server 2007 or Exchange Server 2010. You can't upgrade from Exchange 2000 or Exchange 2003 directly to Exchange 2013.

The path you need to follow to install Exchange 2013 in your organization depends on the version of Exchange you currently have installed in your forest. See the following table for more information.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>If you have the following installed in your organization</th>
<th>You must take this path to upgrade to Exchange 2013</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange 2000</p></td>
<td><ol>
<li><p>Install Exchange 2007 into your Exchange 2000 organization.</p></li>
<li><p>Configure Exchange 2007 and Exchange 2000 coexistence.</p></li>
<li><p>Migrate Exchange 2000 mailboxes, public folders, and other components to Exchange 2007.</p></li>
<li><p>Decommission and remove all Exchange 2000 servers.</p></li>
<li><p>Install Exchange 2013 into your Exchange 2007 organization.</p></li>
<li><p>Configure Exchange 2013 and Exchange 2007 coexistence.</p></li>
<li><p>Migrate Exchange 2007 mailboxes, public folders, and other components to Exchange 2013.</p></li>
<li><p>Decommission and remove all Exchange 2007 servers.</p></li>
</ol>
<p>For more information, see <a href="https://go.microsoft.com/fwlink/p/?linkid=103281">Upgrading to Exchange 2007</a> and <a href="upgrade-from-exchange-2007-to-exchange-2013-exchange-2013-help.md">Upgrade from Exchange 2007 to Exchange 2013</a>.</p></td>
</tr>
<tr class="even">
<td><p>Exchange 2003</p></td>
<td><ol>
<li><p>Install Exchange 2010 into your Exchange 2003 organization.</p></li>
<li><p>Configure Exchange 2010 and Exchange 2003 coexistence.</p></li>
<li><p>Migrate Exchange 2003 mailboxes, public folders, and other components to Exchange 2010.</p></li>
<li><p>Decommission and remove all Exchange 2003 servers.</p></li>
<li><p>Install Exchange 2013 into your Exchange 2010 organization.</p></li>
<li><p>Configure Exchange 2013 and Exchange 2010 coexistence.</p></li>
<li><p>Migrate Exchange 2010 mailboxes, public folders, and other components to Exchange 2013.</p></li>
<li><p>Decommission and remove all Exchange 2010 servers.</p></li>
</ol>
<p>For more information, see <a href="https://go.microsoft.com/fwlink/p/?linkid=268414">Exchange 2003 - Planning Roadmap for Upgrade and Coexistence</a> and <a href="upgrade-from-exchange-2010-to-exchange-2013-exchange-2013-help.md">Upgrade from Exchange 2010 to Exchange 2013</a>.</p></td>
</tr>
</tbody>
</table>

When upgrading to Exchange 2013 or later, you can use the Exchange Deployment Assistant to help complete your deployment. For more information, see [Exchange Deployment Assistant](https://assistants.microsoft.com/).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Did you find what you're looking for? Please take a minute to [send us feedback](mailto:exsetuphelpfeedback@microsoft.com?subject=exchange%202013%20setup%20help%20feedback) about the information you were hoping to find.
