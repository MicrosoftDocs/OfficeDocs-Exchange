---
title: 'Messaging policy and compliance permissions: Exchange 2013 Help'
TOCTitle: Messaging policy and compliance permissions
ms:assetid: ec4d3b9f-b85a-4cb9-95f5-6fc149c3899b
ms:mtpsurl: https://technet.microsoft.com/library/Dd638205(v=EXCHG.150)
ms:contentKeyID: 48385692
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Messaging policy and compliance permissions

_**Applies to:** Exchange Server 2013_

The permissions required to configure messaging policy and compliance vary depending on the procedure being performed or the cmdlet you want to run. For more information about messaging policy and compliance, see [Messaging policy and compliance](messaging-policy-and-compliance-exchange-2013-help.md).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the <STRONG>Get-ManagementRoleAssignment</STRONG> cmdlet. If you don't have permissions to run the <STRONG>Get-ManagementRoleAssignment</STRONG> cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](delegate-role-assignments-exchange-2013-help.md).

> [!NOTE]
> Some features that you want to manage might exist on Edge Transport servers. To manage features on Edge Transport servers, you need to become a member of the Local Administrators group on the Edge Transport server you want to manage. Edge Transport servers don't use Role Based Access Control (RBAC). Features that can be managed on Edge Transport servers have Edge Transport Local Administrator in the "Permissions required" column in the table below.

## Messaging policy and compliance permissions

You can use the features in the following table to configure messaging policy and compliance features. The role groups that are required to configure each feature are listed.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](view-only-organization-management-exchange-2013-help.md).

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
<td><p>Data loss prevention (DLP)</p></td>
<td><p><a href="compliance-management-exchange-2013-help.md">Compliance Management</a></p></td>
</tr>
<tr class="even">
<td><p>Delete mailbox content (using the <a href="https://docs.microsoft.com/powershell/module/exchange/Search-Mailbox">Search-Mailbox</a> cmdlet with the <em>DeleteContent</em> switch)</p></td>
<td><p><a href="discovery-management-exchange-2013-help.md">Discovery Management</a> <strong>and</strong></p>
<p><a href="mailbox-import-export-role-exchange-2013-help.md">Mailbox Import Export role</a></p>

> [!NOTE]
> By default, the Mailbox Import Export role isn't assigned to any role group. You can assign a management role to a built-in or custom role group, a user or a universal security group. Assigning a role to a role group is recommended. For more information, see <A href="add-a-role-to-a-user-or-usg-exchange-2013-help.md">Add a role to a user or USG</A>.

</td>
</tr>
<tr class="odd">
<td><p>Discovery mailboxes - Create</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p></td>
</tr>
<tr class="even">
<td><p>Journaling</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Mailbox audit logging</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="even">
<td><p>Message classifications</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Messaging records management</p></td>
<td><p><a href="compliance-management-exchange-2013-help.md">Compliance Management</a></p>
<p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="even">
<td><p>In-Place eDiscovery</p></td>
<td><p><a href="discovery-management-exchange-2013-help.md">Discovery Management</a></p>

> [!NOTE]
> By default, the Discovery Management role group doesn't have any members. No users, including administrators, have the required permissions to search mailboxes. For more information, see <A href="https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/assign-ediscovery-permissions">Assign eDiscovery permissions in Exchange</A>.

</td>
</tr>
<tr class="odd">
<td><p>In-Place Hold</p></td>
<td><p><a href="discovery-management-exchange-2013-help.md">Discovery Management</a></p>
<p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>

> [!IMPORTANT]
> To create a query-based In-Place Hold, a user requires the Mailbox Search and Litigation Hold roles to be assigned directly or via membership in a role group that has both roles assigned. To create an In-Place Hold without using a query, which places all mailbox items on hold, you must have the Litigation Hold role assigned. The Discovery Management role group is assigned both roles.<BR>The Organization Management role group is assigned the Litigation Hold role. Members of the Organization Management role group can place an In-Place Hold on all items in a mailbox, but can't create a query-based In-Place Hold.

</td>
</tr>
<tr class="even">
<td><p>In-Place Archive</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p></td>
</tr>
<tr class="odd">
<td><p>In-Place Archive - Test connectivity</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="server-management-exchange-2013-help.md">Server Management</a></p></td>
</tr>
<tr class="even">
<td><p>Information Rights Management (IRM) configuration</p></td>
<td><p><a href="compliance-management-exchange-2013-help.md">Compliance Management</a></p>
<p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p></td>
</tr>
<tr class="odd">
<td><p>Retention policies - Apply</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="recipient-management-exchange-2013-help.md">Recipient Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
<tr class="even">
<td><p>Retention policies - Create</p></td>
<td><p>See the entry for Messaging Records Management</p></td>
</tr>
<tr class="odd">
<td><p>Transport rules</p></td>
<td><p><a href="organization-management-exchange-2013-help.md">Organization Management</a></p>
<p><a href="records-management-exchange-2013-help.md">Records Management</a></p></td>
</tr>
</tbody>
</table>
