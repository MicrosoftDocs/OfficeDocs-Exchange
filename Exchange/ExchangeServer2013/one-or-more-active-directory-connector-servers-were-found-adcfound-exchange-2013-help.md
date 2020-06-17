---
title: 'One or more Active Directory Connector servers were found'
TOCTitle: One or more Active Directory Connector servers were found_ADCFound
ms:assetid: a874f51f-09a2-4a76-9695-d61fb1ee6c1c
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.setupreadiness.adcfound(v=EXCHG.150)
ms:contentKeyID: 46629070
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# One or more Active Directory Connector servers were found\_ADCFound

_**Applies to:** Exchange Server 2013_

The content in this topic hasn't been updated for Microsoft Exchange Server 2013. While it hasn't been updated yet, it may still be applicable to Exchange 2013. If you still need help, check out the community resources below.

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

Microsoft Exchange Server 2007 and Exchange Server 2010 setup cannot continue because one or more Active Directory Connectors (ADC) have been found in the current Microsoft Exchange environment.

ADC replicates objects from Exchange Server version 5.5 to the Active Directory directory service in a mixed mode Microsoft Exchange environment and is not supported by Exchange 2007 or Exchange 2010.

Exchange 2007 or Exchange 2010 setup requires that all ADC components be removed.

To resolve this issue, remove all ADC components, and rerun Exchange 2007 or Exchange 2010 setup.

<table>
<colgroup>
<col style="width: 100%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>To remove Active Directory Connector components</p></td>
</tr>
<tr class="even">
<td><ol>
<li><p>To disable the ADC service on the server that is running the ADC service, right-click <strong>My Computer</strong> on the desktop, and then click <strong>Manage</strong>.</p></li>
<li><p>Expand the <strong>Services and Applications</strong> node, and then click the <strong>Services</strong> node.</p></li>
<li><p>In the right pane, right-click <strong>Microsoft Active Directory Connector</strong> and then click <strong>Properties</strong>.</p></li>
<li><p>Change the <strong>Startup Type</strong> to <strong>Disabled</strong>. The next time that the computer starts, the ADC service will not start.</p></li>
<li><p>Click <strong>Apply</strong>, and then click <strong>OK</strong>.</p></li>
<li><p>To uninstall the ADC service, use the Active Directory Installation Wizard on the Microsoft Exchange 2000 Server or Microsoft Exchange Server 2003 CD. Open the \ADC\I386 folder and double-click the Setup.exe program. Follow the prompts to <strong>Remove All</strong> ADC service components.</p>

> [!IMPORTANT]
> You must complete step 6 and <STRONG>Remove All</STRONG> ADC components to resolve this issue. It is insufficient to disable the ADC service.

</li>
</ol></td>
</tr>
</tbody>
</table>
