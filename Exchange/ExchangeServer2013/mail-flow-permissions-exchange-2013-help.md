---
title: 'Mail flow permissions: Exchange 2013 Help'
TOCTitle: Mail flow permissions
ms:assetid: f49f4fb5-af75-43cb-900f-c5f7b8cfa143
ms:mtpsurl: https://technet.microsoft.com/library/Dd638213(v=EXCHG.150)
ms:contentKeyID: 48385715
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Mail flow permissions

_**Applies to:** Exchange Server 2013_

The permissions required to perform tasks related to mail flow vary depending on the procedure being performed or the cmdlet you want to run. For more information about transport features, see [Mail flow](mail-flow-exchange-2013-help.md).

This topic lists the permissions required to manage the mail flow features in Microsoft Exchange Server 2013. For information about how Office 365 permissions relate to Exchange permissions, see [About admin roles](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the <STRONG>Get-ManagementRoleAssignment</STRONG> cmdlet. If you don't have permissions to run the <STRONG>Get-ManagementRoleAssignment</STRONG> cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](delegate-role-assignments-exchange-2013-help.md).

> [!NOTE]
> Some features that you want to manage might exist on Edge Transport servers. To manage features on Edge Transport servers, you need to become a member of the Local Administrators group on the Edge Transport server you want to manage. Edge Transport servers don't use Role Based Access Control (RBAC). Features that can be managed on Edge Transport servers have Edge Transport Local Administrator in the "Permissions required" column in the table below.

> [!NOTE]
> Some features may require that you have local administrator permissions on the server you want to manage. To manage these features, you must be a member of the Local Administrators group on that server.

## Mail flow permissions

You can use the features in the following tables to configure mail flow settings in the Front End Transport service on Client Access servers, in the Transport service on Mailbox servers, in the Mailbox Transport service on Mailbox servers, and on Edge Transport servers. The permissions that are required to configure each feature are listed.

Users who are assigned the View Only Management role group can view the configuration of the features shown in the following table. For more information, see [View-only Organization Management](view-only-organization-management-exchange-2013-help.md).

**Mailbox servers and Client Access servers**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Permissions required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Accepted domains</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>Active Directory site and site link management</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Anti-spam features</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="even">
<td><p>Anti-spam updates</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Certificate management</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>Delivery Agent connectors</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p></td>
</tr>
<tr class="odd">
<td><p>DSNs</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>EdgeSync</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Foreign connectors</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>Front End Transport service</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Journaling</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="even">
<td><p>Mailbox access</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Mailbox junk email configuration</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p>
<p><a href="help-desk-exchange-2013-help.md">Help Desk</a></p></td>
</tr>
<tr class="even">
<td><p>Mailbox Transport service</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="odd">
<td><p>MailTips</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>Message classifications</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Message tracking</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p></td>
</tr>
<tr class="even">
<td><p>Moderated transport</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Queues</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p></td>
</tr>
<tr class="even">
<td><p>Receive connectors</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Remote domains</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>SafeList aggregation</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Send connectors</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="even">
<td><p>Shadow redundancy</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Testing mail flow</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p></td>
</tr>
<tr class="even">
<td><p>Testing Transport rule processing</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Transport agents</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="even">
<td><p>Transport configuration</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Transport logs</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p></td>
</tr>
<tr class="even">
<td><p>Transport rules</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Transport service</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p>
<p><a href="hygiene-management-exchange-2013-help.md">Hygiene Management</a></p></td>
</tr>
<tr class="even">
<td><p>X.400 domains</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
</tbody>
</table>

**Edge Transport servers**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>Permissions required</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Accepted domains - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="even">
<td><p>Address Rewriting - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="odd">
<td><p>Edge Transport server</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="even">
<td><p>EdgeSync - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="odd">
<td><p>Queues - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="even">
<td><p>Receive connectors - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="odd">
<td><p>Send connectors - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="even">
<td><p>Transport configuration - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="odd">
<td><p>Transport logs - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
<tr class="even">
<td><p>Transport rules - Edge Transport</p></td>
<td><p>Edge Transport Local Administrator</p></td>
</tr>
</tbody>
</table>
