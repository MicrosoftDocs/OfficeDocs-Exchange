---
title: 'Understanding management roles: Exchange 2013 Help'
TOCTitle: Understanding management roles
ms:assetid: 887b0a64-84b1-4b8c-9547-e456ea6f5dbd
ms:mtpsurl: https://technet.microsoft.com/library/Dd298116(v=EXCHG.150)
ms:contentKeyID: 49289340
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Understanding management roles

_**Applies to:** Exchange Server 2013_

Management roles are part of the Role Based Access Control (RBAC) permissions model used in Microsoft Exchange Server 2013. Roles act as a logical grouping of cmdlets that are combined to provide access to view or modify the configuration of Exchange 2013 components, such as mailboxes, transport rules, and recipients. Management roles can be further combined into larger groupings called management role groups and management role assignment policies, which enable management of feature areas and recipient configuration. Role groups and role assignment policies assign permissions to administrators and end users, respectively. For more information about management role groups and management role assignment policies, see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

> [!NOTE]
> This topic focuses on advanced RBAC functionality. If you want to manage basic Exchange 2013 permissions, such as using the Exchange admin center (EAC) to add and remove members to and from role groups, create and modify role groups, or create and modify role assignment policies, see <A href="permissions-exchange-2013-help.md">Permissions</A>.

Management role scopes and management role assignments are important components for the operation of management roles. For more information about these components, see the following topics:

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for management tasks related to management roles? See [Permissions](permissions-exchange-2013-help.md).

## Built-in management roles

Exchange 2013 provides many built-in management roles that you can use to administer your organization. Each role includes the cmdlets and parameters necessary for users to manage specific Exchange components. The following are examples of some built-in management roles:

- **Mail Recipients**: Enables administrators to manage mailboxes, contacts, and mail users.

- **Transport Rules**: Enables administrators or specialist users assigned the role to manage the transport rules feature.

- **Distribution Groups**: Enables administrators or specialist users assigned the role to manage distribution groups and distribution group members.

- **MyPersonalInformation**: Enables end users to modify their own home phone number and Web site address.

For a complete list of the management roles included with Exchange 2013, see [Built-in management roles](built-in-management-roles-exchange-2013-help.md).

You can take the built-in roles provided with Exchange 2013 and combine them in any way to create a permissions model that works with your business. For example, if you want members of a role group to manage recipients and public folders, you assign both the Mail Recipients and Public Folders roles to the role group. Most often, you assign roles to role groups or role assignment policies. You can also assign management roles directly to users if you want to control permissions at a granular level. We recommend that you use role groups and role assignment policies rather than direct user role assignment to simplify your permissions model.

> [!NOTE]
> You can only assign end-user management roles to role assignment policies.

Built-in management roles can't be changed. You can, however, create management roles based on the built-in management roles, and then assign those new roles to role groups or role assignment policies. You can then change the new management roles to suit your needs. Doing so is an advanced task that you should rarely, if ever, need to do.

For more information about creating custom roles based on the built-in Exchange roles, see Custom Management Roles later in this topic.

You need to assign management roles for them to take effect. Most often, you assign management roles to role groups and role assignment policies. In certain circumstances, you might also assign roles directly to users, although this is an advanced task that you should rarely, if ever, need to do.

For more information about assigning management roles, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Manage role assignment policies](manage-role-assignment-policies-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)

For more information about management role assignments, see [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md).

## Unscoped top-level management roles

Unscoped top-level management roles are a special type of management role that enables you to grant access to custom scripts and non-Exchange cmdlets to users using RBAC. Regular management roles provide access only to Exchange cmdlets. If you need to provide access to non-Exchange cmdlets that run on an Exchange server, or if you need to publish a script that can be run by your users, you need to add them to an unscoped role. They're called a top-level role because if an unscoped role is created without deriving it from a parent role, it has no parent and is a peer of the built-in management roles provided with Exchange 2013. To indicate that you want to create an unscoped top-level role entry, you need to use the *UnscopedTopLevel* switch with the **New-ManagementRole** cmdlet.

Unscoped roles are named as such because, unlike regular management roles, they can't be scoped to a specific target. Unscoped roles are always organization wide. This means that someone assigned an unscoped role can modify any object in the Exchange 2013 organization. For this reason, care must be taken to make sure that scripts and cmdlets made available through an unscoped role are thoroughly tested so that you know what they will modify, and that you carefully assign unscoped roles.

Unscoped roles can be assigned to role assignees such as role groups, management roles, users, and universal security groups (USGs). They can't be assigned to management role assignment policies.

Although Exchange cmdlets can't be added as a management role entry on an unscoped role, they can be included in scripts added as role entries. This enables you to create custom scripts that perform Exchange tasks that you can then assign to users. A useful scenario is where a user must perform a highly privileged task that's normally outside his or her administrative level and where crafting a new management role or role group would be problematic. You can create a script that performs this specific function, add it to an unscoped role, and then assign the unscoped role to the user. The user can then perform only the specific function provided by the script.

The role entries that you add to an unscoped role must also be designated as an unscoped top-level role entry. For more information about unscoped top-level role entries, see Unscoped Top-Level Role Entries later in this topic.

The Organization Management role group doesn't, by default, have permissions to create or manage unscoped role groups. This is to prevent unscoped role groups from mistakenly being created or modified. The Organization Management role group can delegate the Unscoped Role Management management role to itself and other role assignees. For more information about how to create an unscoped top-level management role, see [Create an unscoped role](create-an-unscoped-role-exchange-2013-help.md).

## Custom management roles

You can create custom management roles based on built-in Exchange roles when the built-in management roles don't match the needs of your users. When you create a custom management role, the new child role inherits all of the management role entries of the parent role. You can then choose which management role entries you want to keep in the custom management role and remove all of the entries you don't want.

Custom roles become children of the role used to create the new role. You can only use management role entries in the new child role that exist in the parent role. For more information, see the following sections later in this topic:

- Management Role Hierarchy

- Management Role Entries

Creating custom management roles requires multiple steps and is an advanced task that you should rarely, if ever, need to perform. Before you create a custom management role, make sure one of the existing built-in management roles doesn't provide the permissions you need. For more information about the built-in management roles, or if you want to create custom management roles, see the following topics:

- [Built-in management roles](built-in-management-roles-exchange-2013-help.md)

- [Advanced permissions](advanced-permissions-exchange-2013-help.md)

For more information about how to create a management role, see [Create a role](create-a-role-exchange-2013-help.md).

## Management role hierarchy

Management roles exist in a parent and child hierarchy. At the top of the hierarchy are the built-in management roles provided in Exchange 2013 by default. When you create a role, a copy of a parent role is made. The new role is a child of the role you copied from. You can then customize the new role to suit the needs of the administrators or users you want to assign it to.

Customized roles can be used to create roles. When you create a role from an existing customized role, the existing customized role remains a child of its parent role, but also becomes the parent for the new role. Each time a role is copied, the new child role can contain only the role entries that exist in the immediate parent role.

Each management role is given a role type that can't be changed. The role type defines the base context of use for the role. The role type is copied from the parent role when the child role is created.

**Management role hierarchy**

![RBAC management role hierarchical diagram](images/Dd298116.6851c829-ca8f-40e7-a67c-cc9e85c33953(EXCHG.150).gif "RBAC management role hierarchical diagram")

The preceding figure illustrates the hierarchical relationship of several management roles. The Mail Recipients and Help Desk roles are built-in roles. All of the child roles derived from these roles inherit the role type of each built-in role. For example, all child roles derived either directly or indirectly from the Mail Recipients role inherit the `MailRecipients` role type.

The Seattle Recipient Administrators custom role is a child of the Mail Recipients built-in role but it's also the parent of the Seattle Sales Recipient Administrators custom role and the Seattle Legal Recipient Administrators custom role. The Seattle Recipient Administrators custom role contains only a subset of cmdlets that are available in the Mail Recipients role. The child roles of the Seattle Recipient Administrators custom role can only contain cmdlets that also exist in that role. For example, if a cmdlet exists in the Mail Recipients role, but the cmdlet doesn't exist in the Seattle Recipient Administrators custom role, the cmdlet can't be added to the Seattle Sales Recipient Administrators custom role.

All of the custom roles follow the same pattern as the roles discussed previously. For more information about how access to cmdlets is controlled on management roles, see Management Role Entries next in this topic.

## Management role entries

Every management role, whether it's a custom Exchange role or an unscoped role, must have at least one management role entry. An entry consists of a single cmdlet and its parameters, a script, or a special permission that you want to make available. If a cmdlet or script doesn't appear as an entry on a management role, that cmdlet or script isn't accessible via that role. Likewise, if a parameter doesn't exist in an entry, the parameter on that cmdlet or script isn't accessible via that role.

Exchange 2013 enables you to manage role entries based on built-in Exchange management top-level roles and role entries based on unscoped top-level management roles. Roles based on built-in Exchange top-level roles can only contain role entries that are Exchange 2013 cmdlets. To add custom scripts or non-Exchange cmdlets so that your users can use them, you need to add them as unscoped role entries to an unscoped top-level role. For more information about unscoped role entries, see Unscoped Top-Level Role Entries later in this topic.

All role entries, regardless of whether the role entry is an Exchange cmdlet-based role entry or an unscoped role entry, adhere to the same principles explained in the following sections.

For more information about managing role entries, see [Management roles and role entries](management-roles-and-role-entries-exchange-2013-help.md).

## Parent and child management role relationship

As mentioned previously, a management role entry, including the cmdlet and its parameters, must exist in the immediate parent role to add the entry to the child role. For example, if the parent role doesn't have an entry for **New-Mailbox**, the child role can't be assigned that cmdlet. Additionally, if **Set-Mailbox** is on the parent role but the *Database* parameter has been removed from the entry, the *Database* parameter on the **Set-Mailbox** cmdlet can't be added to the entry on the child role.

Because you can't add management role entries to child roles if the entries don't appear in parent roles, and because the role is based on a specific role type, you must carefully choose the parent role to copy when you want to create a customized role.

## Management role entry names

Management role entry names are a combination of the management role that they're associated with, and the name of the cmdlet or script. The role name and the cmdlet or script are separated by a backslash character (\\). For example, the role entry name for the **Set-Mailbox** cmdlet on the Mail Recipients role is `Mail Recipients\Set-Mailbox`. If the name of a role entry contains spaces, enclose the entire name in quotation marks (").

The wildcard character (\*) can be used in the role entry name to return all of the role entries that match the input you provide. The wildcard character can be used on either side of the backslash character. The following table contains a few variations on how you can use the wildcard character in a role entry name.

**Management role entry name with wildcard characters**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Example</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>*\*</code></p></td>
<td><p>Returns a list of all role entries for all roles.</p></td>
</tr>
<tr class="even">
<td><p><code>*\Set-Mailbox</code></p></td>
<td><p>Returns a list of all role entries that contain the <strong>Set-Mailbox</strong> cmdlet.</p></td>
</tr>
<tr class="odd">
<td><p><code>Mail Recipients\*</code></p></td>
<td><p>Returns a list of all role entries on the Mail Recipients role.</p></td>
</tr>
<tr class="even">
<td><p><code>Mail Recipients\*Mailbox</code></p></td>
<td><p>Returns a list of all role entries on the Mail Recipients role that contain cmdlets that end in Mailbox.</p></td>
</tr>
<tr class="odd">
<td><p><code>My*\*Group*</code></p></td>
<td><p>Returns a list of all role entries that contain the string Group in the cmdlet name for all roles that begin with My.</p></td>
</tr>
</tbody>
</table>

## Unscoped top-level role entries

Unscoped top-level role entries are used with unscoped top-level management roles to create roles based on custom scripts or non-Exchange cmdlets. Each unscoped role entry is associated with a single custom script or a non-Exchange cmdlet. To indicate that you want to create an unscoped role entry on an unscoped role, you need to specify the *UnscopedTopLevel* parameter on the **Add-ManagementRoleEntry** cmdlet.

When you add the unscoped role entry, you need to specify all of the parameters that can be used with the script or non-Exchange cmdlet. Exchange attempts to verify the parameters that you provide when you add the role entry. Only the parameters that you add to the role entry when it's created will be available to the users assigned to the unscoped role. If you add parameters to the script or non-Exchange cmdlet, or if a parameter is renamed, you must update the role entry manually. Exchange doesn't check whether existing parameters on an unscoped role entry have changed. If a parameter on a role entry changes in a script and you try to use that parameter, the command fails.

Scripts that you add to an unscoped role entry must reside in the Exchange 2013 scripts directory on every server where administrators and users connect using the Exchange Management Shell. If you try to add an unscoped role entry based on a script that doesn't exist in the Exchange 2013 scripts directory on the server you're using to add the role entry, an error occurs. The default installation location of the Exchange 2013 scripts directory is C:\\Program Files\\Microsoft\\Exchange Server\\V15\\RemoteScripts.

Non-Exchange cmdlets that you add to an unscoped role entry must be installed on every Exchange 2013 server where administrators and users connect using the Shell and want to use the cmdlets. If you try to add an unscoped role entry based on a non-Exchange cmdlet that isn't installed on the Exchange 2013 server you're using to add the role entry, an error occurs. When you add a non-Exchange cmdlet, you must specify the Windows PowerShell snap-in name that contains the non-Exchange cmdlet.

For more information about how to add an unscoped management role entry, see [Add a role entry to a role](add-a-role-entry-to-a-role-exchange-2013-help.md).

## Management role types

Management role types are the foundation of all management roles. Types define the implicit scopes defined on all management roles of a specified role type and also act as a logical grouping of related roles. All management roles derived from the parent built-in management role have the same role type. Refer to the Management role hierarchy figure earlier in this topic for an illustration of this relationship. Management role types also represent the maximum set of cmdlets and their parameters that can be added to a role associated with a role type.

Management role types are split into the following categories:

- **Administrative or specialist**: Roles associated with an administrative or specialist role types have a broader scope of impact in the Exchange organization. Roles of this role type enable tasks such as server or recipient management, organization configuration, compliance administration, auditing, and more.

- **User-focused**: Roles associated with a user-focused role type have a scope of impact closely tied with an individual user. Roles of this role type enable tasks such as user profile configuration and self management, management of user-owned distribution groups, and more.

    The names of roles associated with user-focused role types and user-focused role type names begin with My.

- **Specialty**: Roles associated with specialty role types enable tasks that aren't administrative or user-focused role types. Roles of this role type enable tasks such as application impersonation and the use of non-Exchange cmdlets or scripts.

The following table lists all of the administrative management role types in Exchange 2013 and whether the configuration that's permitted by the role type is applied across the whole Exchange organization or only to an individual server. For more information about each of the management roles associated with these role types, including a description of each role, who may benefit from being assigned the role, and other information, see [Built-in management roles](built-in-management-roles-exchange-2013-help.md).

**Administrative role types**

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Management role type</th>
<th>Built-in management role</th>
<th>Description</th>
<th>Organization or server</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>ActiveDirectoryPermissions</code></p></td>
<td><p><a href="active-directory-permissions-role-exchange-2013-help.md">Active Directory Permissions role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to configure Active Directory permissions in an organization. Some features that use Active Directory permissions or an access control list (ACL) include transport Receive and Send connectors, and Send As and send on behalf permissions for mailboxes.</p>

> [!NOTE]
> Permissions set directly on Active Directory objects may not be enforced through RBAC.

</td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>AddressLists</code></p></td>
<td><p><a href="address-lists-role-exchange-2013-help.md">Address Lists role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage address lists, the global address list (GAL), and offline address lists in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>ApplicationImpersonation</code></p></td>
<td><p><a href="applicationimpersonation-role-exchange-2013-help.md">ApplicationImpersonation role</a></p></td>
<td><p>This role type is associated with roles that enable applications to impersonate users in an organization to perform tasks on behalf of the user.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>ArchiveApplication</code></p></td>
<td><p><a href="archiveapplication-role-exchange-2013-help.md">ArchiveApplication role</a></p></td>
<td><p>This role type is associated with roles that enable partner applications to archive items in user mailboxes in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>AuditLogs</code></p></td>
<td><p><a href="audit-logs-role-exchange-2013-help.md">Audit Logs role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage the administrator audit logging configuration in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>CmdletExtensionAgents</code></p></td>
<td><p><a href="cmdlet-extension-agents-role-exchange-2013-help.md">Cmdlet Extension Agents role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage cmdlet extension agents in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>DataLossPrevention</code></p></td>
<td><p><a href="data-loss-prevention-role-exchange-2013-help.md">Data Loss Prevention role</a></p></td>
<td><p>This role type is associated with roles that create and manage data loss prevention (DLP) policies and the rules within them in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>DatabaseAvailabilityGroups</code></p></td>
<td><p><a href="database-availability-groups-role-exchange-2013-help.md">Database Availability Groups role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage database availability groups (DAGs) in an organization. Administrators assigned this role either directly or indirectly are the highest level administrators responsible for the high availability configuration in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>DatabaseCopies</code></p></td>
<td><p><a href="database-copies-role-exchange-2013-help.md">Database Copies role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage database copies on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="even">
<td><p><code>Databases</code></p></td>
<td><p><a href="databases-role-exchange-2013-help.md">Databases role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create, manage, mount, and dismount mailbox databases on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="odd">
<td><p><code>DisasterRecovery</code></p></td>
<td><p><a href="disaster-recovery-role-exchange-2013-help.md">Disaster Recovery role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to restore mailboxes and mailbox databases, create mailbox databases, and perform datacenter switchovers and switchbacks for database availability groups in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>DistributionGroups</code></p></td>
<td><p><a href="distribution-groups-role-exchange-2013-help.md">Distribution Groups role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create and manage distribution groups and distribution group members in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>EdgeSubscriptions</code></p></td>
<td><p><a href="edge-subscriptions-role-exchange-2013-help.md">Edge Subscriptions role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage edge synchronization and subscription configuration between Exchange 2010 Edge Transport servers and Exchange 2013 Mailbox servers in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>EmailAddressPolicies</code></p></td>
<td><p><a href="e-mail-address-policies-role-exchange-2013-help.md">E-Mail Address Policies role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage email address policies in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>ExchangeConnectors</code></p></td>
<td><p><a href="exchange-connectors-role-exchange-2013-help.md">Exchange Connectors role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create, modify, view, and remove delivery agent connectors.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>ExchangeServerCertificates</code></p></td>
<td><p><a href="exchange-server-certificates-role-exchange-2013-help.md">Exchange Server Certificates role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create, import, export, and manage Exchange server certificates on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="odd">
<td><p><code>ExchangeServers</code></p></td>
<td><p><a href="exchange-servers-role-exchange-2013-help.md">Exchange Servers role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage Exchange server configuration on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="even">
<td><p><code>ExchangeVirtualDirectories</code></p></td>
<td><p><a href="exchange-virtual-directories-role-exchange-2013-help.md">Exchange Virtual Directories role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage Outlook Web App, Microsoft ActiveSync, offline address book (OAB), Autodiscover, Windows PowerShell, and Exchange admin center virtual directories on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="odd">
<td><p><code>FederatedSharing</code></p></td>
<td><p><a href="federated-sharing-role-exchange-2013-help.md">Federated Sharing role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage cross-forest and cross-organization sharing in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>InformationRightsManagement</code></p></td>
<td><p><a href="information-rights-management-role-exchange-2013-help.md">Information Rights Management role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage the Information Rights Management (IRM) features of Exchange in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>Journaling</code></p></td>
<td><p><a href="journaling-role-exchange-2013-help.md">Journaling role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage journaling configuration in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>LegalHold</code></p></td>
<td><p><a href="legal-hold-role-exchange-2013-help.md">Legal Hold role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to configure whether data within a mailbox should be retained for litigation purposes in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>MailboxImportExport</code></p></td>
<td><p><a href="mailbox-import-export-role-exchange-2013-help.md">Mailbox Import Export role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to import and export mailbox content and to purge unwanted content from a mailbox.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>MailboxSearch</code></p></td>
<td><p><a href="mailbox-search-role-exchange-2013-help.md">Mailbox Search role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to search the content of one or more mailboxes in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>MailboxSearchApplication</code></p></td>
<td><p><a href="mailboxsearchapplication-role-exchange-2013-help.md">MailboxSearchApplication role</a></p></td>
<td><p>This role type is associated with roles that enable partner applications to set and view the legal hold status of a mailbox in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>MailEnabledPublicFolders</code></p></td>
<td><p><a href="mail-enabled-public-folders-role-exchange-2013-help.md">Mail Enabled Public Folders role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to configure whether individual public folders are mail-enabled or mail-disabled in an organization.</p>
<p>This role type enables you to manage the email properties of public folders only. It doesn't enable you to manage properties of public folders that aren't email properties. To manage properties of public folders that aren't email properties, you need to be assigned a role associated with the <code>PublicFolders</code> role type.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>MailRecipientCreation</code></p></td>
<td><p><a href="mail-recipient-creation-role-exchange-2013-help.md">Mail Recipient Creation role</a></p>
<p></p></td>
<td><p>This role type is associated with roles that enable administrators to create mailboxes, mail users, mail contacts, distribution groups, and dynamic distribution groups in an organization. Roles associated with this role type can be combined with roles associated with the <code>MailRecipients</code> role type to enable the creation and management of recipients.</p>
<p>This role type doesn't enable you to mail-enable public folders. To mail-enable public folders, you must be assigned a role associated with the <code>MailEnabledPublicFolders</code> role type.</p>
<p>If your organization maintains a split permissions model where recipient creation is performed by a different group from the group that performs recipient management, assign the <code>MailRecipientCreation</code> role to the group that performs recipient creation, and the <code>MailRecipients</code> role to the group that performs recipient management.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>MailRecipients</code></p></td>
<td><p><a href="mail-recipients-role-exchange-2013-help.md">Mail Recipients role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage existing mailboxes, mail users, mail contacts, distribution groups, and dynamic distribution groups in an organization. Roles associated with this role type can't create these recipients but can be combined with roles associated with the <code>MailRecipientCreation</code> role type to enable the creation and management of recipients.</p>
<p>This role type doesn't enable you to manage mail-enabled public folders or distribution groups. To manage mail-enabled public folders, you must be assigned a role associated with the <code>MailEnabledPublicFolders</code> role type. To manage distribution groups, you must be assigned a role associated with the <code>DistributionGroups</code> role type.</p>
<p>If your organization maintains a split permissions model where recipient creation is performed by a different group from the group that performs recipient management, assign the <code>MailRecipientCreation</code> role to the group that performs recipient creation, and the <code>MailRecipients</code> role to the group that performs recipient management.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>MailTips</code></p></td>
<td><p><a href="mail-tips-role-exchange-2013-help.md">Mail Tips role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage MailTips in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>MessageTracking</code></p></td>
<td><p><a href="message-tracking-role-exchange-2013-help.md">Message Tracking role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to track messages in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>Migration</code></p></td>
<td><p><a href="migration-role-exchange-2013-help.md">Migration role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to migrate mailboxes and mailbox content into or out of a server.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="even">
<td><p><code>Monitoring</code></p></td>
<td><p><a href="monitoring-role-exchange-2013-help.md">Monitoring role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to monitor the Microsoft Exchange services and component availability in an organization. In addition to administrators, roles associated with this role type can be used with the service account used by monitoring applications to collect information about the state of Exchange servers.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>MoveMailboxes</code></p></td>
<td><p><a href="move-mailboxes-role-exchange-2013-help.md">Move Mailboxes role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to move mailboxes between servers in an organization and between servers in the local organization and another organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>OfficeExtensionApplication</code></p></td>
<td><p><a href="officeextensionapplication-role-exchange-2013-help.md">OfficeExtensionApplication role</a></p></td>
<td><p>This role type is associated with roles that enable Microsoft Office extension applications to access user mailboxes in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>OrgCustomApps</code></p></td>
<td><p><a href="org-custom-apps-role-exchange-2013-help.md">Org Custom Apps role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to view and modify their organization's custom apps in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>OrgMarketplaceApps</code></p></td>
<td><p><a href="org-marketplace-apps-role-exchange-2013-help.md">Org Marketplace Apps role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to view and modify their organization's marketplace apps in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>OrganizationClientAccess</code></p></td>
<td><p><a href="organization-client-access-role-exchange-2013-help.md">Organization Client Access role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage Client Access server settings in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>OrganizationConfiguration</code></p></td>
<td><p><a href="organization-configuration-role-exchange-2013-help.md">Organization Configuration role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage organization-wide settings in an organization. Organization configuration that can be controlled with this role type include the following and more:</p>
<ul>
<li><p>Whether MailTips are enabled or disabled for the organization.</p></li>
<li><p>The URL for the managed folder home page.</p></li>
<li><p>The Microsoft Exchange recipient SMTP address and alternate email addresses.</p></li>
<li><p>The resource mailbox property schema configuration.</p></li>
<li><p>The Help URLs for the Exchange admin center and Outlook Web App.</p></li>
</ul>
<p>This role type doesn't include the permissions included in the <code>OrganizationClientAccess</code> or <code>OrganizationTransportSettings</code> role types.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>OrganizationTransportSettings</code></p></td>
<td><p><a href="organization-transport-settings-role-exchange-2013-help.md">Organization Transport Settings role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage organization-wide transport settings, such as system messages, Active Directory site configuration, and other organization-wide transport settings in an organization.</p>
<p>This role doesn't enable you to create or manage transport Receive or Send connectors, queues, hygiene, agents, remote and accepted domains, or rules. To create or manage each of the transport features, you must be assigned roles associated with the following role types:</p>
<ul>
<li><p><strong>Receive connectors</strong>   <code>ReceiveConnectors</code></p></li>
<li><p><strong>Send connectors</strong>   <code>SendConnectors</code></p></li>
<li><p><strong>Transport queues</strong>   <code>TransportQueues</code></p></li>
<li><p><strong>Transport hygiene</strong>   <code>TransportHygiene</code></p></li>
<li><p><strong>Transport agents</strong>   <code>TransportAgents</code></p></li>
<li><p><strong>Remote and accepted domains</strong>   <code>RemoteAndAcceptedDomains</code></p></li>
<li><p><strong>Transport rules</strong>   <code>TransportRules</code></p></li>
</ul></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>POP3AndIMAP4Protocols</code></p></td>
<td><p><a href="pop3-and-imap4-protocols-role-exchange-2013-help.md">POP3 and IMAP4 Protocols role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage POP3 and IMAP4 configuration, such as authentication and connection settings, on individual servers.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="odd">
<td><p><code>PublicFolders</code></p></td>
<td><p><a href="public-folders-role-exchange-2013-help.md">Public Folders role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage public folders in an organization.</p>
<p>This role type doesn't enable you to manage whether public folders are mail-enabled. To mail-enable or disable a public folder, you must be assigned a role associated with the <code>MailEnabledPublicFolders</code> role type.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>ReceiveConnectors</code></p></td>
<td><p><a href="receive-connectors-role-exchange-2013-help.md">Receive Connectors role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage transport Receive connector configuration, such as size limits on an individual server.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="odd">
<td><p><code>RecipientPolicies</code></p></td>
<td><p><a href="recipient-policies-role-exchange-2013-help.md">Recipient Policies role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage recipient policies, such as provisioning and mobile device policies, in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>RemoteAndAcceptedDomains</code></p></td>
<td><p><a href="remote-and-accepted-domains-role-exchange-2013-help.md">Remote and Accepted Domains role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage remote and accepted domains in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>ResetPassword</code></p></td>
<td><p><a href="reset-password-role-exchange-2013-help.md">Reset Password role</a></p></td>
<td><p>This role type is associated with roles that enable users to reset their own passwords and administrators to reset users' passwords in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>RetentionManagement</code></p></td>
<td><p><a href="retention-management-role-exchange-2013-help.md">Retention Management role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage retention policies in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>RoleManagement</code></p></td>
<td><p><a href="role-management-role-exchange-2013-help.md">Role Management role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage management role groups, role assignment policies, management roles, role entries, assignments, and scopes in an organization.</p>
<p>Users assigned roles associated with this role type can override the <strong>role group managed by</strong> property, configure any role group, and add or remove members to or from any role group.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>SecurityGroupCreationAndMembership</code></p></td>
<td><p><a href="security-group-creation-and-membership-role-exchange-2013-help.md">Security Group Creation and Membership role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create and manage USGs and their memberships in an organization.</p>
<p>If your organization maintains a split permissions model where USG creation and management is performed by a different group from the group that manages Exchange servers, assign roles associated with this role type to that group.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>SendConnectors</code></p></td>
<td><p><a href="send-connectors-role-exchange-2013-help.md">Send Connectors role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage transport Send connectors in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>SupportDiagnostics</code></p></td>
<td><p><a href="support-diagnostics-role-exchange-2013-help.md">Support Diagnostics role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to perform advanced diagnostics under the direction of Microsoft support services in an organization.</p>

> [!WARNING]
> Roles associated with this role type grant permissions to cmdlets and scripts that should only be used under the direction of Microsoft Customer Service and Support.

</td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>TeamMailboxes</code></p></td>
<td><p><a href="team-mailboxes-role-exchange-2013-help.md">Team Mailboxes role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to define one or more site mailbox provisioning policies and manage site mailboxes in an organization. Administrators assigned roles associated with this role type can manage site mailboxes they don't own.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>TeamMailboxLifecycleApplication</code></p></td>
<td><p><a href="teammailboxlifecycleapplication-role-exchange-2013-help.md">TeamMailboxLifecycleApplication role</a></p></td>
<td><p>This role type is associated with roles that enable partner applications to update site mailbox lifecycle states in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>TransportAgents</code></p></td>
<td><p><a href="transport-agents-role-exchange-2013-help.md">Transport Agents role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage transport agents in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>TransportHygiene</code></p></td>
<td><p><a href="transport-hygiene-role-exchange-2013-help.md">Transport Hygiene role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage anti-spam and anti-malware features in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>TransportQueues</code></p></td>
<td><p><a href="transport-queues-role-exchange-2013-help.md">Transport Queues role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage transport queues on an individual server.</p></td>
<td><p>Server</p></td>
</tr>
<tr class="even">
<td><p><code>TransportRules</code></p></td>
<td><p><a href="transport-rules-role-exchange-2013-help.md">Transport Rules role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage transport rules in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>UMMailboxes</code></p></td>
<td><p><a href="um-mailboxes-role-exchange-2013-help.md">UM Mailboxes role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage the Unified Messaging (UM) configuration of mailboxes and other recipients in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>UMPrompts</code></p></td>
<td><p><a href="um-prompts-role-exchange-2013-help.md">UM Prompts role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create and manage custom UM voice prompts in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>UnifiedMessaging</code></p></td>
<td><p><a href="unified-messaging-role-exchange-2013-help.md">Unified Messaging role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage Unified Messaging services in an organization.</p>
<p>This role doesn't enable you to manage UM-specific mailbox configuration or UM prompts. To manage UM-specific mailbox configuration, use roles associated with the <code>UMMailboxes</code> role type. To manage UM prompts, use the roles associated with the <code>UMPrompts</code> role type.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>UnScopedRoleManagement</code></p></td>
<td><p><a href="unscoped-role-management-role-exchange-2013-help.md">Unscoped Role Management role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to create and manage unscoped top-level management roles in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>UserOptions</code></p></td>
<td><p><a href="user-options-role-exchange-2013-help.md">User Options role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to view the Outlook Web App options of a user in an organization. Roles associated with this role type can be used to help a user with diagnosing problems with his or her configuration.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>UserApplication</code></p></td>
<td><p><a href="userapplication-role-exchange-2013-help.md">UserApplication role</a></p></td>
<td><p>This role type is associated with roles that enable partner applications to act on behalf of end users in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>ViewOnlyAuditLogs</code></p></td>
<td><p><a href="view-only-audit-logs-role-exchange-2013-help.md">View-Only Audit Logs role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to search the administrator audit log in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>ViewOnlyConfiguration</code></p></td>
<td><p><a href="view-only-configuration-role-exchange-2013-help.md">View-Only Configuration role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to view all of the non-recipient Exchange configuration settings in an organization. Examples of configuration that are viewable are server configuration, transport configuration, database configuration, and organization-wide configuration.</p>
<p>Roles associated with this role type can be combined with roles associated with the <code>ViewOnlyRecipients</code> role type to create a role that can view every object in an organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="odd">
<td><p><code>ViewOnlyRecipients</code></p></td>
<td><p><a href="view-only-recipients-role-exchange-2013-help.md">View-Only Recipients role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to view the configuration of recipients, such as mailboxes, mail users, mail contacts, distribution groups, and dynamic distribution groups.</p>
<p>Roles associated with this role type can be combined with roles associated with the <code>ViewOnlyConfiguration</code> role type to create a role that can view every object in the organization.</p></td>
<td><p>Organization</p></td>
</tr>
<tr class="even">
<td><p><code>WorkloadManagement</code></p></td>
<td><p><a href="workloadmanagement-role-exchange-2013-help.md">WorkloadManagement role</a></p></td>
<td><p>This role type is associated with roles that enable administrators to manage workload management policies in an organization. Administrators can configure resource health definitions, workload classifications, and enable or disable workload management.</p></td>
<td><p>Organization</p></td>
</tr>
</tbody>
</table>

The following table lists all of the user-focused management role types and their associated built-in management roles in Exchange 2013.

**User-focused role types**

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Management role type</th>
<th>Built-in user-focused roles</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>MyBaseOptions</code></p></td>
<td><p><a href="mybaseoptions-role-exchange-2013-help.md">MyBaseOptions role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view and modify the basic configuration of their own mailbox and associated settings.</p></td>
</tr>
<tr class="even">
<td><p><code>MyContactInformation</code></p></td>
<td><p><a href="myaddressinformation-role-exchange-2013-help.md">MyAddressInformation role</a></p>
<p><a href="mycontactinformation-role-exchange-2013-help.md">MyContactInformation role</a></p>
<p><a href="mymobileinformation-role-exchange-2013-help.md">MyMobileInformation role</a></p>
<p><a href="mypersonalinformation-role-exchange-2013-help.md">MyPersonalInformation role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to modify their contact information. This information includes their address and phone numbers.</p></td>
</tr>
<tr class="odd">
<td><p>MyCustomApps</p></td>
<td><p><a href="my-custom-apps-role-exchange-2013-help.md">My Custom Apps role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view and modify their custom apps.</p></td>
</tr>
<tr class="even">
<td><p>MyDiagnostics</p></td>
<td><p><a href="mydiagnostics-role-exchange-2013-help.md">MyDiagnostics role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to perform basic diagnostics on their mailbox, such as retrieving calendar diagnostic information.</p></td>
</tr>
<tr class="odd">
<td><p><code>MyDistributionGroupMembership</code></p></td>
<td><p><a href="mydistributiongroupmembership-role-exchange-2013-help.md">MyDistributionGroupMembership role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view and modify their membership in distribution groups in an organization, provided that those distribution groups allow manipulation of group membership.</p></td>
</tr>
<tr class="even">
<td><p><code>MyDistributionGroups</code></p></td>
<td><p><a href="mydistributiongroups-role-exchange-2013-help.md">MyDistributionGroups role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to create, modify, and view distribution groups and modify, view, remove, and add members to distribution groups they own.</p></td>
</tr>
<tr class="odd">
<td><p><code>MyProfileInformation</code></p></td>
<td><p><a href="mydisplayname-role-exchange-2013-help.md">MyDisplayName role</a></p>
<p><a href="myname-role-exchange-2013-help.md">MyName role</a></p>
<p><a href="myprofileinformation-role-exchange-2013-help.md">MyProfileInformation role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to modify their name.</p></td>
</tr>
<tr class="even">
<td><p>MyMarketplaceApps</p></td>
<td><p><a href="my-marketplace-apps-role-exchange-2013-help.md">My Marketplace Apps role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view and modify their marketplace apps.</p></td>
</tr>
<tr class="odd">
<td><p><code>MyRetentionPolicies</code></p></td>
<td><p><a href="myretentionpolicies-role-exchange-2013-help.md">MyRetentionPolicies role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view their retention tags and view and modify their retention tag settings and defaults.</p></td>
</tr>
<tr class="even">
<td><p>MyTeamMailboxes</p></td>
<td><p><a href="myteammailboxes-role-exchange-2013-help.md">MyTeamMailboxes role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to create site mailboxes and connect them to Microsoft SharePoint sites.</p></td>
</tr>
<tr class="odd">
<td><p><code>MyTextMessaging</code></p></td>
<td><p><a href="mytextmessaging-role-exchange-2013-help.md">MyTextMessaging role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to create, view, and modify their text messaging settings.</p></td>
</tr>
<tr class="even">
<td><p><code>MyVoiceMail</code></p></td>
<td><p><a href="myvoicemail-role-exchange-2013-help.md">MyVoiceMail role</a></p></td>
<td><p>This role type is associated with roles that enable individual users to view and modify their voice mail settings.</p></td>
</tr>
</tbody>
</table>

## For more information

[New-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRole)

[New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment)

[Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment)

[New-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/New-ManagementScope)

[Set-ManagementScope](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementScope)

[New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment)

[Set-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Set-ManagementRoleAssignment)
