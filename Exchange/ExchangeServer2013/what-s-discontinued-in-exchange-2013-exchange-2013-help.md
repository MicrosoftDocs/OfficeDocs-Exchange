---
title: "What's discontinued in Exchange 2013: Exchange 2013 Help"
TOCTitle: What's discontinued in Exchange 2013
ms:assetid: 0ac0001c-b314-4108-b895-d9c0e271b489
ms:mtpsurl: https://technet.microsoft.com/library/JJ619283(v=EXCHG.150)
ms:contentKeyID: 49289156
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# What's discontinued in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic discusses the components, features, or functionality that have been removed, discontinued, or replaced in Microsoft Exchange Server 2013.

> [!NOTE]
> The following topics may also interest you:
> <UL>
> <LI>
> <P><A href="what-s-new-in-exchange-2013-exchange-2013-help.md">What's new in Exchange 2013</A>&nbsp;&nbsp;&nbsp;Information about new features and functionality in Exchange Server 2013.</P>
> <LI>
> <P><A href="https://docs.microsoft.com/exchange/client-developer/exchange-server-development">Exchange Online and Exchange development</A></P></LI></UL>

## Discontinued features from Exchange 2010 to Exchange 2013

This section lists the Exchange Server 2010 features that are no longer available in Exchange 2013.

## Architecture

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Hub Transport server role</p></td>
<td><p>The Hub Transport server role has been replaced by Transport services which run on the Mailbox and Client Access server roles. The Mailbox server role includes the Microsoft Exchange Transport, Microsoft Exchange Mailbox Transport Delivery, and the Microsoft Exchange Mailbox Transport Submission service. The Client Access server role includes the Microsoft Exchange Frontend Transport service. For more information, see <a href="mail-flow-exchange-2013-help.md">Mail flow</a>.</p></td>
</tr>
<tr class="even">
<td><p>Unified Messaging server role</p></td>
<td><p>The Unified Messaging server role has been replaced by Unified Messaging services which run on the Mailbox and Client Access server roles. The Mailbox server role includes the Microsoft Exchange Unified Messaging service and the Client Access server role includes the Microsoft Exchange Unified Messaging Call Router service. For more information, see <a href="voice-architecture-changes-exchange-2013-help.md">Voice architecture changes</a>.</p></td>
</tr>
</tbody>
</table>

## Management interfaces

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Management Console and Exchange Control Panel</p></td>
<td><p>The Exchange Management Console and the Exchange Control Panel have been replaced by the Exchange admin center (EAC). EAC uses the same virtual directory (/ecp) as the Exchange Control Panel. For more information, see <a href="exchange-admin-center-in-exchange-2013-exchange-2013-help.md">Exchange admin center in Exchange 2013</a>.</p></td>
</tr>
</tbody>
</table>

## Client access

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2003 is not supported</p></td>
<td><p>To connect Microsoft Outlook to Exchange 2013, the use of the Autodiscover service is required. However, Microsoft Outlook 2003 doesn't support the use of the Autodiscover service.</p></td>
</tr>
<tr class="even">
<td><p>RPC/TCP access for Outlook clients</p></td>
<td><p>In Exchange 2013, Microsoft Outlook clients can connect using Outlook Anywhere (RPC/HTTP) or MAPI over HTTP in Exchange 2013 Service Pack 1 and Outlook 2013 Service Pack 1 and later. If you have Outlook clients in your organization, using Outlook Anywhere and/or MAPI over HTTP is required. For more information, see <a href="outlook-anywhere-exchange-2013-help.md">Outlook Anywhere</a> and <a href="mapi-over-http-exchange-2013-help.md">MAPI over HTTP</a>.</p></td>
</tr>
</tbody>
</table>

## Outlook Web App and Outlook

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Spell check</p></td>
<td><p>Outlook Web App no longer has built-in spell check services. Instead, it uses the spell check features in your Web browsers.</p></td>
</tr>
<tr class="even">
<td><p>Customizable filters</p></td>
<td><p>Outlook Web App no longer has customizable filtered views and no longer supports saving filtered views to Favorites. Customizable filters have been replaced by fixed filters that can be used to view all messages, unread messages, messages sent to the user, or flagged messages.</p></td>
</tr>
<tr class="odd">
<td><p>Message flags</p></td>
<td><p>The ability to set a custom date on a message flag isn't available in Outlook Web App. You can use Outlook to set custom dates.</p></td>
</tr>
<tr class="even">
<td><p>Chat contact list</p>
<p></p></td>
<td><p>The chat contact list that appeared in the folder list in Outlook Web App for Exchange 2010 is no longer available.</p></td>
</tr>
<tr class="odd">
<td><p>Search folders</p></td>
<td><p>The ability for users to use Search folders isn't currently available in Outlook Web App.</p></td>
</tr>
</tbody>
</table>

## Mail flow

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Linked connectors</p></td>
<td><p>The ability to link a Send connector to a Receive connector has been removed. Specifically, the <em>LinkedReceiveConnector</em> parameter has been removed from <a href="https://docs.microsoft.com/powershell/module/exchange/New-SendConnector">New-SendConnector</a> and <a href="https://docs.microsoft.com/powershell/module/exchange/Set-SendConnector">Set-SendConnector</a>.</p></td>
</tr>
</tbody>
</table>

## Anti-spam and anti-malware

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Anti-spam agent management in the EMC</p></td>
<td><p>In Exchange 2010, when you enabled the anti-spam agents on a Hub Transport server, you could manage the anti-spam agents in the Exchange Management Console (EMC). In Exchange 2013, when you enable the anti-spam agents on a Mailbox server, you can't manage the agents using the EAC. You can only use the Shell. For information about how to enable the anti-spam agents on a Mailbox server, see <a href="enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md">Enable anti-spam functionality on Mailbox servers</a>.</p></td>
</tr>
<tr class="even">
<td><p>Connection Filtering agent on Hub Transport servers</p></td>
<td><p>In Exchange 2010, when you enabled the anti-spam agents on a Hub Transport server, the Attachment Filter agent was the only anti-spam agent that wasn't available. In Exchange 2013, when you enable the anti-spam agents on a Mailbox server, the Attachment Filter agent and the Connection Filtering agent aren't available. The Connection Filtering agent provides IP Allow List and IP Block List capabilities. For information about how to enable the anti-spam agents on a Mailbox server, see <a href="enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md">Enable anti-spam functionality on Mailbox servers</a>.</p>

> [!NOTE]
> You can't enable the anti-spam agents on an Exchange 2013 Client Access server. Therefore, the only way to get the Connection Filtering agent is to install an Edge Transport server in the perimeter network. For more information, see <A href="edge-transport-servers-exchange-2013-help.md">Edge Transport servers</A>.

</td>
</tr>
</tbody>
</table>

## Messaging policy and compliance

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Managed Folders</p></td>
<td><p>In Exchange 2010, you use managed folders for messaging retention management (MRM). In Exchange 2013, managed folders aren't supported. You must use retention policies for MRM.</p>

> [!NOTE]
> Cmdlets related to managed folders are still available. You can create managed folders, managed content settings and managed folder mailbox policies, and apply a managed folder mailbox policy to a user, but the MRM assistant skips processing of mailboxes that have a managed folder mailbox policy applied.

</td>
</tr>
<tr class="even">
<td><p>Port Managed Folder wizard</p></td>
<td><p>In Exchange 2010, you use the Port Managed Folder wizard to create retention tags based on managed folder and managed content settings. In Exchange 2013, the Exchange admin center doesn't include this functionality. You can use the <strong>New-RetentionPolicyTag</strong> cmdlet with the <em>ManagedFolderToUpgrade</em> parameter to create a retention tag based on a managed folder.</p></td>
</tr>
</tbody>
</table>

## Unified Messaging and voice mail

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Directory lookups using Automatic Speech Recognition (ASR)</p></td>
<td><p>In Exchange 2010, Outlook Voice Access users can use speech inputs using Automatic Speech Recognition (ASR) to search for users listed in the directory. Speech inputs could be also used in Outlook Voice Access to navigate menus, messages, and other options. However, even if an Outlook Voice Access user is able to use speech inputs, they have to use the telephone key pad to enter their PIN, and navigate personal options.</p>
<p>In Exchange 2013, authenticated and non-authenticated Outlook Voice Access users can't search for users in the directory using speech inputs or ASR in any language. However, callers that call into an auto attendant can use speech inputs in multiple languages to navigate auto attendant menus and search for users in the directory.</p></td>
</tr>
</tbody>
</table>

## Tools

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Best Practice Analyzer</p></td>
<td><p>In Exchange 2010, the Exchange Best Practice Analyzer examined your Exchange deployment and determined whether the configuration was in line with Microsoft best practices. In Exchange 2013, the Exchange Best Practice Analyzer has been replaced by the <a href="https://techcommunity.microsoft.com/t5/exchange-team-blog/beta-of-microsoft-office-365-best-practices-analyzer-for/ba-p/591294">Office 365 Best Practices Analyzer for Exchange Server 2013</a>.</p></td>
</tr>
<tr class="even">
<td><p>Mail flow troubleshooter</p></td>
<td><p>In Exchange 2010, the mail flow troubleshooter assisted you in troubleshooting common mail flow problems. In Exchange 2013, the mail flow troubleshooter has been retired. You can now use the messaging tracking feature in EAC in Exchange 2013. For more information, see <a href="track-messages-with-delivery-reports-exchange-2013-help.md">Track messages with delivery reports</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Performance monitor</p></td>
<td><p>In Exchange 2010, the Performance Monitor was included in the Exchange toolbox to allow you to collect information about the performance of your messaging system. Performance Monitor is commonly used to view key parameters while troubleshooting performance problems. In Exchange 2013, the Performance Monitor has been retired from the toolbox. You can still find the Performance Monitor under <strong>Administrative Tools</strong> in Windows Server 2008.</p></td>
</tr>
<tr class="even">
<td><p>Performance troubleshooter</p></td>
<td><p>In Exchange 2013, the Performance Troubleshooter has been retired.</p></td>
</tr>
<tr class="odd">
<td><p>Routing Log Viewer</p></td>
<td><p>In Exchange 2013, the routing log viewer has been retired.</p></td>
</tr>
</tbody>
</table>

## Mailbox database copies

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><ul>
<li><p>Update-MailboxDatabaseCopy</p></li>
<li><p>Update Mailbox Database Copy wizard</p></li>
</ul></td>
<td><p>Content index catalog seeding is no longer possible over the replication network; it can only be done over a MAPI network. This is true even when you use the <code>-Network</code> parameter in the Update-MailboxDatabaseCopy cmdlet.</p></td>
</tr>
</tbody>
</table>

## Discontinued features from Exchange 2007 to Exchange 2013

This section lists the Exchange Server 2007 features that are no longer available in Exchange 2013.

## APIs and development

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange WebDAV</p></td>
<td><p>Use <a href="https://go.microsoft.com/fwlink/p/?linkid=167197">Exchange Web Services</a> or <a href="https://go.microsoft.com/fwlink/p/?linkid=157179">EWS Managed API</a>. Alternatively, you can maintain an Exchange 2007 server for mailboxes that are managed by applications that use WebDAV. For more information, see <a href="https://go.microsoft.com/fwlink/p/?linkid=169474">Migrating from WebDAV</a>.</p></td>
</tr>
</tbody>
</table>

## Architecture

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Storage groups</p></td>
<td><p>Exchange 2013 no longer uses the storage group construct, and instead you simply manage mailbox databases. For more information, see <a href="manage-mailbox-databases-in-exchange-2013-exchange-2013-help.md">Manage mailbox databases in Exchange 2013</a>.</p></td>
</tr>
<tr class="even">
<td><p>Extensible Storage Engine (ESE) streaming backup APIs</p></td>
<td><p>Exchange 2013 supports only Exchange-aware Volume Shadow Copy Service (VSS)-based backups. Exchange 2013 does include a plug-in for Windows Server Backup that enables you to backup and restore data. For information, see <a href="backup-restore-and-disaster-recovery-exchange-2013-help.md">Backup, restore, and disaster recovery</a>.</p></td>
</tr>
<tr class="odd">
<td><p>User Datagram Protocol (UDP) notifications</p></td>
<td><p>Support for User Datagram Protocol (UDP) notifications is removed from Exchange 2013. This affects the user experience when Outlook 2003 clients connect to their mailboxes on an Exchange 2013 server. For more information, see Microsoft Knowledge Base article 2009942, <a href="https://support.microsoft.com/help/2009942">Folders take a long time to update when an Exchange Server 2010 user uses Outlook 2003 in online mode</a>.</p></td>
</tr>
</tbody>
</table>

## High availability

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Cluster continuous replication (CCR)</p></td>
<td><p>Exchange 2013 uses database availability groups (DAGs) and mailbox database copies. For information, see <a href="high-availability-and-site-resilience-exchange-2013-help.md">High availability and site resilience</a>.</p></td>
</tr>
<tr class="even">
<td><p>Local continuous replication (LCR)</p></td>
<td><p>Exchange 2013 uses DAGs and mailbox database copies. For information, see <a href="high-availability-and-site-resilience-exchange-2013-help.md">High availability and site resilience</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Standby continuous replication (SCR)</p></td>
<td><p>Exchange 2013 uses DAGs and mailbox database copies. For information, see <a href="high-availability-and-site-resilience-exchange-2013-help.md">High availability and site resilience</a>.</p></td>
</tr>
<tr class="even">
<td><p>Single copy cluster (SCC)</p></td>
<td><p>Exchange 2013 uses DAGs and mailbox database copies. For information, see <a href="high-availability-and-site-resilience-exchange-2013-help.md">High availability and site resilience</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Setup /recoverCMS</p></td>
<td><p>Exchange 2013 uses Setup /m:recoverServer. For information, see <a href="recover-an-exchange-server-exchange-2013-help.md">Recover an Exchange Server</a>.</p></td>
</tr>
<tr class="even">
<td><p>Clustered mailbox servers</p></td>
<td><p>Exchange 2013 uses DAGs and mailbox database copies. For information, see <a href="high-availability-and-site-resilience-exchange-2013-help.md">High availability and site resilience</a>.</p></td>
</tr>
</tbody>
</table>

## Client access

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Client authentication using Integrated Windows authentication (NTLM) for POP3 and IMAP4 users</p></td>
<td><p>NTLM isn't supported for POP3 or IMAP4 client connectivity in Exchange 2013. Connections from POP3 or IMAP4 client programs using NTLM will fail. If you're running the RTM version of Exchange 2013, the recommended alternative to NTLM is to use Plain Text Authentication with SSL.</p>
<p>If you're using Exchange 2013, to use NTLM, you must retain an Exchange 2007 server in your Exchange 2013 organization.</p></td>
</tr>
</tbody>
</table>

## Outlook Web App and Outlook

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Document access</p></td>
<td><p>Outlook Web App can't be used to access Microsoft SharePoint document libraries and Windows file shares.</p></td>
</tr>
<tr class="even">
<td><p>Message flags</p></td>
<td><p>The ability to set a custom date on a message flag isn't available in Outlook Web App 2013. You can use Outlook to set custom dates.</p></td>
</tr>
<tr class="odd">
<td><p>Spell check</p></td>
<td><p>Outlook Web App uses the spell check features in your Web browser.</p></td>
</tr>
<tr class="even">
<td><p>Search Folders</p></td>
<td><p>The ability for users to use Search folders isn't currently available in Outlook Web App.</p></td>
</tr>
<tr class="odd">
<td><p>Maximum Cached Views</p></td>
<td><p>Exchange 2007 supported modifying the maximum number of cached views per database (msExchMaxCachedViews) parameter for Outlook clients. Exchange 2013 no longer uses this parameter.</p></td>
</tr>
</tbody>
</table>

## Recipient-related features

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><strong>Export-Mailbox</strong> and <strong>Import-Mailbox</strong> cmdlets</p></td>
<td><p>In Exchange 2013, use export requests or import requests. For more information, see <a href="mailbox-import-and-export-requests-exchange-2013-help.md">Mailbox import and export requests</a>.</p></td>
</tr>
<tr class="even">
<td><p><strong>Move-Mailbox</strong> cmdlet set</p></td>
<td><p>In Exchange 2013, use move requests to move mailboxes. For information, see <a href="mailbox-moves-in-exchange-2013-exchange-2013-help.md">Mailbox moves in Exchange 2013</a>.</p></td>
</tr>
<tr class="odd">
<td><p>ISInteg</p></td>
<td><p>In Exchange 2013, use <a href="https://docs.microsoft.com/powershell/module/exchange/New-MailboxRepairRequest">New-MailboxRepairRequest</a>.</p></td>
</tr>
</tbody>
</table>

## Messaging policy and compliance

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Managed Folders</p></td>
<td><p>In Exchange 2007, you use managed folders for messaging retention management (MRM). In Exchange 2013, managed folders aren't supported. You must use retention policies for MRM.</p>

> [!NOTE]
> Cmdlets related to managed folders are still available. You can create managed folders, managed content settings and managed folder mailbox policies, and apply a managed folder mailbox policy to a user, but the MRM assistant skips processing of mailboxes that have a managed folder mailbox policy applied.

</td>
</tr>
</tbody>
</table>

## Unified Messaging and voice mail

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Comments and mitigation</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Directory lookups using Automatic Speech Recognition (ASR) for Outlook Voice Access</p></td>
<td><p>In Exchange 2007, Outlook Voice Access users can use speech inputs using Automatic Speech Recognition (ASR) in English (US) - (en-US) to search for users listed in the directory. Speech inputs could be also used in Outlook Voice Access to navigate menus, messages, and other options. However, even if an Outlook Voice Access user is able to use speech inputs, they have to use the telephone key pad to enter their PIN, and navigate personal options.</p>
<p>In Exchange 2013, authenticated and non-authenticated Outlook Voice Access users can't search for users in the directory using speech inputs or ASR in any language. However, callers that call into an auto attendant can use speech inputs in multiple languages to navigate auto attendant menus and search for users in the directory.</p></td>
</tr>
</tbody>
</table>
