---
title: 'Exchange admin center in Exchange 2013: Exchange 2013 Help'
TOCTitle: Exchange admin center in Exchange 2013
ms:assetid: a9aea11a-6ba3-4f4a-a76e-79072e7cfc7d
ms:mtpsurl: https://technet.microsoft.com/library/JJ150562(v=EXCHG.150)
ms:contentKeyID: 47560086
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange admin center in Exchange 2013

_**Applies to:** Exchange Server 2013_

The Exchange admin center (EAC) is the web-based management console in Microsoft Exchange Server 2013 that's optimized for on-premises, online, and hybrid Exchange deployments. The EAC replaces the Exchange Management Console (EMC) and the Exchange Control Panel (ECP), which were the two interfaces used to manage Exchange Server 2010.

One advantage a web-based EAC provides is that you can partition Internet and intranet access from within the ECP IIS virtual directory. With this functionality, you can control whether users are allowed to have Internet access to the EAC from outside of your organization, while still allowing an end user to access Outlook Web App Options. For more information, see [Turn off access to the Exchange admin center](turn-off-access-to-the-exchange-admin-center-exchange-2013-help.md).

Looking for the Exchange Online version of this topic? See [Exchange admin center in Exchange Online](https://docs.microsoft.com/exchange/exchange-admin-center).

Looking for the Exchange Online Protection version of this topic? See [Exchange admin center in Exchange Online Protection](https://docs.microsoft.com/microsoft-365/security/office-365-security/exchange-admin-center-in-exchange-online-protection-eop).

## Accessing the EAC

Because the EAC is now a web-based management console, you'll need to use the ECP virtual directory URL to access the console from your web browser. In most cases the EAC's URL will look similar to the following:

- **Internal URL: `https://<CASServerName>/ecp`**: The internal URL is used to access the EAC from within your organization's firewall.

- **External URL: `https://mail.contoso.com/ecp`**: The external URL is used to access the EAC from outside of your organization's firewall. Some organizations may want to turn off external access to the EAC. For details, see [Turn off access to the Exchange admin center](turn-off-access-to-the-exchange-admin-center-exchange-2013-help.md).

To locate the internal or external URL for the EAC, you can use the [Get-EcpVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Get-EcpVirtualDirectory) cmdlet. For details, see [Find the internal and external URLs for the Exchange admin center](find-the-internal-and-external-urls-for-the-exchange-admin-center-exchange-2013-help.md).

If you're in a coexistence scenario, where you're running Exchange 2010 and Exchange 2013 in the same organization, and your mailbox is still housed on the Exchange 2010 Mailbox server, the browser will default to the Exchange 2010 ECP. You can access the EAC by adding the Exchange version to the URL. For example, to access the EAC whose virtual directory is hosted on the Client Access server CAS15-NA, use the following URL: `https://CAS15-NA/ecp/?ExchClientVer=15`. Conversely, if you want to access the Exchange 2010 ECP and your mailbox resides on an Exchange 2013 Mailbox server, use the following URL: `https://CAS14-NA/ecp/?ExchClientVer=14`.

## Common user interface elements in the EAC

The section describes the user interface elements that are common across the EAC.

![Exchange admin center](images/JJ150562.cd617bc0-19ef-47d2-bba1-4e9546b12e0c(EXCHG.150).gif "Exchange admin center")

## Cross-premises navigation

The cross-premises navigation allows you to easily switch between your Exchange Online and on-premises Exchange deployments. If you don't have an Exchange Online organization, the link will direct you to the Office 365 sign-up page. To learn more, see [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid).

## Feature pane

This is the first level of navigation for most of the tasks that you'll perform in the EAC. The feature pane is similar to the console tree from the EMC in Exchange 2010. However, in Exchange 2013 the feature pane is organized by feature areas as opposed to server roles, and there are fewer clicks to find what you need.

- **Recipients**: This is where you'll manage mailboxes, groups, resource mailboxes, contacts, shared mailboxes, and mailbox migrations and moves.

- **Permissions**: This is where you'll manage administrator roles, user roles, and Outlook Web App policies.

- **Compliance management**: This is where you'll manage In-Place eDiscovery, In-Place Hold, auditing, data loss prevention (DLP), retention policies, retention tags, and journal rules.

- **Organization**: This is where you'll manage the tasks that pertain to your Exchange Organization, including federated sharing, Outlook Apps, and address lists.

- **Protection**: This is where you'll manage anti-malware protection for your organization.

- **Mail flow**: This is where you'll manage rules, delivery reports, accepted domains, email address policies, and send and receive connectors.

- **Mobile**: This is where you'll manage the mobile devices that you allow to connect to your organization. You can manage mobile device access and mobile device mailbox policies.

- **Public folders**: In Exchange 2010, you had to manage public folders by using the Public Folder Management Console, which was located outside of the EMC in the Toolbox. In Exchange 2013, public folders can be managed from within the **public folders** feature area.

- **Unified Messaging**: This is where you'll manage UM dial plans and UM IP gateways.

- **Servers**: This is where you'll manage your Mailbox and Client Access servers, databases, database availability groups (DAGs), virtual directories, and certificates.

- **Hybrid**: This is where you'll set up and configure a Hybrid organization.

## Tabs

The tabs are your second level of navigation. Each of the feature areas contains various tabs, each representing a complete feature. The only exception to this rule is the Hybrid feature area. You must first enable your organization for a hybrid deployment by using the Hybrid Configuration wizard.

## Toolbar

When you click most tabs, you'll see a toolbar. The toolbar has icons that perform a specific action. The following table describes the most common icons and their actions. To display the action associated with an icon, simply hover over the icon.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Icon</th>
<th>Name</th>
<th>Action</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><img src="images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif" title="Add Icon" alt="Add Icon" /></p></td>
<td><p>Add, New</p></td>
<td><p>Use this icon to create a new object. Some of these icons have an associated down arrow you can click to show additional objects you can create. For example, in <strong>Recipients</strong> &gt; <strong>Mailboxes</strong>, clicking the down arrow displays <strong>User mailbox</strong> and <strong>Linked mailbox</strong> as additional options.</p></td>
</tr>
<tr class="even">
<td><p><img src="images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif" title="Edit icon" alt="Edit icon" /></p></td>
<td><p>Edit</p></td>
<td><p>Use this icon to edit an object.</p></td>
</tr>
<tr class="odd">
<td><p><img src="images/Dd298078.14f639f6-61e8-4418-bbfb-0db14de9d2f5(EXCHG.150).gif" title="Delete icon" alt="Delete icon" /></p></td>
<td><p>Delete</p></td>
<td><p>Use this icon to delete an object. Some delete icons have a down arrow you can click to show additional options.</p></td>
</tr>
<tr class="even">
<td><p><img src="images/Dn624163.773574d0-9b92-4cab-9f6b-81532c7418b9(EXCHG.150).gif" title="Search icon" alt="Search icon" /></p></td>
<td><p>Search</p></td>
<td><p>Use this icon to open a search box in which you can type the search phrase for an object you want to find.</p></td>
</tr>
<tr class="odd">
<td><p><img src="images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif" title="Refresh Icon" alt="Refresh Icon" /></p></td>
<td><p>Refresh</p></td>
<td><p>Use this icon to refresh the list view.</p></td>
</tr>
<tr class="even">
<td><p><img src="images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif" title="More Options Icon" alt="More Options Icon" /></p></td>
<td><p>More options</p></td>
<td><p>Use this icon to view more actions you can perform for that tab's objects. For example, in <strong>Recipients</strong> &gt; <strong>Mailboxes</strong> clicking this icon shows the following options: <strong>Disable</strong>, <strong>Add/Remove columns</strong>, <strong>Export data to a CSV file</strong>, <strong>Connect a mailbox</strong>, and <strong>Advanced search</strong>.</p></td>
</tr>
<tr class="odd">
<td><p><img src="images/JJ150576.1732c727-328b-4a1a-b84d-6d7252c7dcab(EXCHG.150).gif" title="Up Arrow Icon" alt="Up Arrow Icon" />   <img src="images/JJ150576.ef5ca57d-a033-457b-bd92-6361877c33d0(EXCHG.150).gif" title="Down Arrow Icon" alt="Down Arrow Icon" /></p></td>
<td><p>Up arrow and down arrow</p></td>
<td><p>Use these icons to move an object's priority up or down. For example, in <strong>Mail flow</strong> &gt; <strong>Email address policies</strong> click the up arrow to raise the priority of an email address policy. You can also use these arrows to navigate the public folder hierarchy and to move rules up or down in the list view.</p></td>
</tr>
<tr class="even">
<td><p><img src="images/JJ657480.ed7f7abf-39d8-4f43-a918-ccb3bff87ef5(EXCHG.150).gif" title="Copy Icon" alt="Copy Icon" /></p></td>
<td><p>Copy</p></td>
<td><p>Use this icon to copy an object so you can make changes to it without changing the original object. For example, in <strong>Permissions</strong> &gt; <strong>Admin roles</strong>, select a role from the list view, and then click this icon to create a new role group based on an existing one.</p></td>
</tr>
<tr class="odd">
<td><p><img src="images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif" title="Remove icon" alt="Remove icon" /></p></td>
<td><p>Remove</p></td>
<td><p>Use this icon to remove an item from a list. For example, in the <strong>Public Folder Permissions</strong> dialog box, you can remove users from the list of users allowed to access the public folder by selecting the user and clicking this icon.</p></td>
</tr>
</tbody>
</table>

## List view

When you select a tab, in most cases you'll see a list view. The list view in EAC is designed to remove limitations that existed in ECP. ECP can only display up to 500 objects, and if you want to view objects that aren't listed in the details pane, you need to use search and filter options to find those specific objects. In Exchange 2013, the viewable limit from within the EAC list view is approximately 20,000 objects for on-premises deployments and 10,000 objects in Exchange Online. In addition, paging is included so you can page to the results. In the **Recipients** list view, you can also configure page size and export the data to a CSV file.

## Details pane

When you select an object from the list view, information about that object is displayed in the details pane. In some cases (for example, with recipient objects) the details pane includes quick management tasks. For example, if you navigate to **Recipients** \> **Mailboxes** and select a mailbox from the list view, the details pane displays an option to enable or disable the archive for that mailbox. The details pane can also be used to bulk edit several objects. Simply press the CTRL key, select the objects you want to bulk edit, and use the options in the details pane. For example, selecting multiple mailboxes allows you to bulk update users' contact and organization information, custom attributes, mailbox quota, Outlook Web App settings, and more.

## Notifications

The EAC includes a notification viewer that displays the status of long-running processes and provides notifications when the process completes. In addition, for particularly long-running processes (such as a move requests), you can opt-in to receive email notifications.

## Me tile and Help

The *Me tile* allows you to sign out of the EAC and sign in as a different user. From the Help ![Help Icon](images/JJ150562.a32eac4e-345d-4236-a284-204390aff4ee(EXCHG.150).gif "Help Icon") drop-down menu, you can perform the following actions:

- **Help**: Click ![Help Icon](images/JJ150562.a32eac4e-345d-4236-a284-204390aff4ee(EXCHG.150).gif "Help Icon") to view the online help content.

- **Disable Help bubble**: The Help bubble displays contextual help for fields when you create or edit and object. You can turn off the Help bubble help or turn it on if it has been disabled.

- **Copyright and Privacy**: Click the privacy or copyright link to read the copyright and privacy information for Exchange 2013.

## Supported browsers

For the best experience with the EAC, use one of the operating system and browser combinations labeled "Premium".

> [!NOTE]
> Other operating system and browser combinations not listed in the table are unsupported, including touch.

- **Premium:**: All functional features are supported and fully tested.

- **Supported:**: Has same functional feature support as premium. However, supported browsers will be missing features that the browser and operating system combination doesn't support.

- **Unsupported:**: The browser and operating system isn't supported or tested.

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Web Browser</p></td>
<td><p>Windows XP and Windows Server 2003</p></td>
<td><p>Windows Vista</p></td>
<td><p>Windows 7 and Windows Server 2008</p></td>
<td><p>Windows 8 and Windows Server 2012</p></td>
<td><p>Mac OSX</p></td>
<td><p>Linux</p></td>
</tr>
<tr class="even">
<td><p>Internet Explorer 8</p></td>
<td><p>Supported</p></td>
<td><p>Supported</p></td>
<td><p>Premium</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
</tr>
<tr class="odd">
<td><p>Internet Explorer 9</p></td>
<td><p>Unsupported</p></td>
<td><p>Supported</p></td>
<td><p>Premium</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
</tr>
<tr class="even">
<td><p>Internet Explorer 10 or later</p></td>
<td><p>Unsupported</p></td>
<td><p>Supported</p></td>
<td><p>Premium</p></td>
<td><p>Premium</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
</tr>
<tr class="odd">
<td><p>Firefox 11 or later</p></td>
<td><p>Supported</p></td>
<td><p>Supported</p></td>
<td><p>Premium</p></td>
<td><p>Premium</p></td>
<td><p>Premium</p></td>
<td><p>Supported</p></td>
</tr>
<tr class="even">
<td><p>Safari 5.1 or later</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
<td><p>Unsupported</p></td>
<td><p>Premium</p></td>
<td><p>Unsupported</p></td>
</tr>
<tr class="odd">
<td><p>Chrome 18 or later</p></td>
<td><p>Supported</p></td>
<td><p>Supported</p></td>
<td><p>Premium</p></td>
<td><p>Premium</p></td>
<td><p>Premium</p></td>
<td><p>Unsupported</p></td>
</tr>
</tbody>
</table>
