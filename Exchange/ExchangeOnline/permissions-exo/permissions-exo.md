---
localization_priority: Normal
description: Exchange Online in Microsoft 365 and Office 365 includes a large set of predefined permissions, based on the Role Based Access Control (RBAC) permissions model, which you can use right away to easily grant permissions to your administrators and users. You can use the permissions features in Exchange Online so that you can get your new organization up and running quickly.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 3a219732-87e7-4f11-96bc-8edd2cc91926
ms.reviewer: 
f1.keywords:
- NOCSH
title: Permissions in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Permissions in Exchange Online

Exchange Online in Microsoft 365 and Office 365 includes a large set of predefined permissions, based on the Role Based Access Control (RBAC) permissions model, which you can use right away to easily grant permissions to your administrators and users. You can use the permissions features in Exchange Online so that you can get your new organization up and running quickly.

RBAC is also the permissions model that's used in Microsoft Exchange Server. Most of the links in this topic refer to topics that reference Exchange Server. The concepts in those topics also apply to Exchange Online.

For information about permissions across Microsoft 365 or Office 365, see [About admin roles](https://docs.microsoft.com/microsoft-365/admin/add-users/about-admin-roles)

> [!NOTE]
> Several RBAC features and concepts aren't discussed in this topic because they're advanced features. If the functionality discussed in this topic doesn't meet your needs, and you want to further customize your permissions model, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

## Role-based permissions

In Exchange Online, the permissions that you grant to administrators and users are based on management roles. A management role defines the set of tasks that an administrator or user can perform. For example, a management role called `Mail Recipients` defines the tasks that someone can perform on a set of mailboxes, contacts, and distribution groups. When a management role is assigned to an administrator or user, that person is granted the permissions provided by the management role.

Administrative roles and end-user roles are the two types of management roles. Following is a brief description of each type:

- **Administrative roles**: These roles contain permissions that can be assigned to administrators or specialist users using role groups that manage a part of the Exchange Online organization, such as recipients or compliance management.

- **End-user roles**: These roles, which are assigned using role assignment policies, enable users to manage aspects of their own mailbox and distribution groups that they own. End-user roles begin with the prefix `My`.

Management roles give permissions to perform tasks to administrators and users by making cmdlets available to those who are assigned the roles. Because the Exchange admin center (EAC) and Exchange Online PowerShell use cmdlets to manage Exchange Online, granting access to a cmdlet gives the administrator or user permission to perform the task in each of the Exchange Online management interfaces.

Exchange Online includes role groups that you can use to grant permissions. For more information, see the next section.

> [!NOTE]
> Some management roles many be available only to on-premises Exchange Server installations and won't be available in Exchange Online.

## Role groups and role assignment policies

Management roles grant permissions to perform tasks in Exchange Online, but you need an easy way to assign them to administrators and users. Exchange Online provides you with the following to help you make assignments:

- **Role groups**: Role groups enable you to grant permissions to administrators and specialist users.

- **Role assignment policies**: Role assignment policies enable you to grant permissions to end users to change settings on their own mailbox or distribution groups that they own.

The following sections provide more information about role groups and role assignment policies.

### Role groups

Every administrator who manages Exchange Online must be assigned at least one or more roles. Administrators might have more than one role because they may perform job functions that span multiple areas in Exchange Online.

To make it easier to assign multiple roles to an administrator, Exchange Online includes role groups. When a role is assigned to a role group, the permissions granted by the role are granted to all the members of the role group. This enables you to assign many roles to many role group members at once. Role groups typically encompass broader management areas, such as recipient management. They're used only with administrative roles, and not end-user roles. Role group members can be Exchange Online users and other role groups.

> [!NOTE]
> It's possible to assign a role directly to a user without using a role group. However, that method of role assignment is an advanced procedure and isn't covered in this topic. We recommend that you use role groups to manage permissions.

The following figure shows the relationship between users, role groups, and roles.

![Role, role group and member relationship](../media/ITPro_Security_RBAC_EXO_SimplifiedRoleGroupRelationship.png)

Exchange Online includes several built-in role groups, each one providing permissions to manage specific areas in Exchange Online. Some role groups may overlap with other role groups. The following table lists each role group with a description of its use.

|**Role group**|**Description**|**Default roles assigned**|
|:---|:--- |:--- |
|Compliance Management|Members can configure and manage compliance settings within Exchange in accordance with their policies.|Audit Logs <br/><br/> Compliance Admin <br/><br/> Data Loss Prevention <br/><br/> Information Rights Management <br/><br/> Journaling <br/><br/> Message Tracking <br/><br/> Retention Management <br/><br/> Transport Rules <br/><br/> View-Only Audit Logs <br/><br/> View-Only Configuration <br/><br/> View-Only Recipients|
|Discovery Management|Members can perform searches of mailboxes in the Exchange Online organization for data that meets specific criteria and can also configure legal holds on mailboxes.|Legal Hold <br/><br/> Mailbox Search|
|ExchangeServiceAdmins\_-\<unique value\>|Membership in this role group is synchronized across services and is managed centrally. You can't manage this role group in Exchange Online. <br/><br/> This role group doesn't have any roles assigned to it. However, it's a member of the Organization Management role group (as Exchange Service Administrator) and inherits the permissions provided by that role group. <br/><br/> You can add members to this role group by adding users to the Azure AD Exchange admin role in the Microsoft 365 admin center.|n/a|
|Help Desk|Members can view and manage the configuration for individual recipients and view recipients in an Exchange organization. Members of this role group can only manage the configuration each user can manage on their own mailbox.|Reset Password <br/><br/> User Options <br/><br/> View-Only Recipients|
|HelpdeskAdmins\_\<unique value\>|Membership in this role group is synchronized across services and is managed centrally. You can't manage this role group in Exchange Online. <br/><br/> This role group doesn't have any roles assigned to it. However, it's a member of the View-Only Organization Management role group (as Helpdesk Administrator) and inherits the permissions provided by that role group. <br/><br/> You can add members to this role group by adding users to the Azure AD Helpdesk admin role in the Microsoft 365 admin center.|n/a|
|Hygiene Management|Members can manage Exchange anti-spam features, grant permissions for antivirus products to integrate with Exchange, and manage mail flow rules.|Transport Hygiene <br/><br/> View-Only Configuration <br/><br/> View-Only Recipients|
|Organization Management|Members have administrative access to the entire Exchange Online organization and can perform almost any task in Exchange Online. <br/><br/> By default, the following management roles are not assigned to any role group, including Organization Management: <ul><li>Address Lists</li><li>Mailbox Import Export</li></ul> <br/> By default, the Mailbox Search role is only assigned to the Discovery Management role group <br/><br/> **Important**: Because the Organization Management role group is a powerful role, only users that perform organizational-level administrative tasks that can potentially impact the entire Exchange Online organization should be members of this role group.|Audit Logs <br/><br/> Compliance Admin <br/><br/> Data Loss Prevention <br/><br/> Distribution Groups <br/><br/> E-Mail Address Policies <br/><br/> Federated Sharing <br/><br/> Information Rights Management <br/><br/> Journaling <br/><br/> Legal Hold <br/><br/> Mail Enabled Public Folders <br/><br/> Mail Recipient Creation <br/><br/> Mail Recipients <br/><br/> Mail Tips <br/><br/> Message Tracking <br/><br/> Migration <br/><br/> Move Mailboxes <br/><br/> Org Custom Apps <br/><br/> Org Marketplace Apps <br/><br/> Organization Client Access <br/><br/> Organization Configuration <br/><br/> Organization Transport Settings <br/><br/> Public Folders <br/><br/> Recipient Policies <br/><br/> Remote and Accepted Domains <br/><br/> Reset Password <br/><br/> Retention Management <br/><br/> Role Management <br/><br/> Security Admin <br/><br/> Security Group Creation and Membership <br/><br/> Security Reader <br/><br/> Team Mailboxes <br/><br/> Transport Hygiene <br/><br/> Transport Rules <br/><br/> UM Mailboxes <br/><br/> UM Prompts <br/><br/> Unified Messaging <br/><br/> User Options <br/><br/> View-Only Audit Logs <br/><br/> View-Only Configuration <br/><br/> View-Only Recipients|
|Recipient Management|Members have administrative access to create or modify Exchange Online recipients within the Exchange Online organization.|Distribution Groups <br/><br/> Mail Recipient Creation <br/><br/> Mail Recipients <br/><br/> Message Tracking <br/><br/> Migration <br/><br/> Move Mailboxes <br/><br/> Recipient Policies <br/><br/> Reset Password <br/><br/> Team Mailboxes|
|Records Management|Members can configure compliance features, such as retention policy tags, message classifications, and mail flow rules (also known as transport rules).|Audit Logs <br/><br/> Journaling <br/><br/> Message Tracking <br/><br/> Retention Management <br/><br/> Transport Rules|
|Security Administrator|Membership in this role group is synchronized across services and is managed centrally. You can't manage this role group in Exchange Online. <br/><br/> You can add members to this role group by adding users to the Azure AD Security admin role in the Microsoft 365 admin center.|Security Admin|
|Security Reader|Membership in this role group is synchronized across services and is managed centrally. You can't manage this role group in Exchange Online. <br/><br/> You can add members to this role group by adding users to the Azure AD Security reader role in the Microsoft 365 admin center.|Security Reader|
|TenantAdmins\_-\<unique value\>|Membership in this role group is synchronized across services and is managed centrally. You can't manage this role group in Exchange Online. <br/><br/> This role group doesn't have any roles assigned to it. However, it's a member of the Organization Management role group (as Company Administrator) and inherits the permissions provided by that role group. <br/><br/> You can add members to this role group by adding users to the Azure AD **Global admin** role in the Microsoft 365 admin center.|n/a|
|UM Management|Members can manage Exchange Unified Messaging (UM) settings and features.|UM Mailboxes <br/><br/> UM Prompts <br/><br/> Unified Messaging|
|View-Only Organization Management|Members can view the properties of any object in the Exchange Online organization.|View-Only Configuration <br/><br/> View-Only Recipients|

If you work in a small organization that has only a few administrators, you might need to add those administrators to the Organization Management role group only, and you may never need to use the other role groups. If you work in a larger organization, you might have administrators who perform specific tasks administering Exchange Online, such as recipient configuration. In those cases, you might add one administrator to the Recipient Management role group, and another administrator to the Organization Management role group. Those administrators can then manage their specific areas of Exchange Online, but they won't have permissions to manage areas they're not responsible for.

If the built-in role groups in Exchange Online don't match the job function of your administrators, you can create role groups and add roles to them. For more information, see the [Work with role groups](#work-with-role-groups) section later in this topic.

### Role assignment policies

Exchange Online provides role assignment policies so that you can control what settings your users can configure on their own mailboxes and on distribution groups they own. These settings include their display name, contact information, voice mail settings, and distribution group membership.

Your Exchange Online organization can have multiple role assignment policies that provide different levels of permissions for the different types of users in your organizations. Some users can be allowed to change their address or create distribution groups, while others can't, depending on the role assignment policy associated with their mailbox. Role assignment policies are added directly to mailboxes, and each mailbox can only be associated with one role assignment policy at a time.

Of the role assignment policies in your organization, one is marked as default. The default role assignment policy is associated with new mailboxes that aren't explicitly assigned a specific role assignment policy when they're created. The default role assignment policy should contain the permissions that should be applied to the majority of your mailboxes.

Permissions are added to role assignment policies using end-user roles. End-user roles begin with `My` and grant permissions for users to manage only their mailbox or distribution groups they own. They can't be used to manage any other mailbox. Only end-user roles can be assigned to role assignment policies.

When an end-user role is assigned to a role assignment policy, all of the mailboxes associated with that role assignment policy receive the permissions granted by the role. This enables you to add or remove permissions to sets of users without having to configure individual mailboxes. The following figure shows:

- End-user roles are assigned to role assignment policies. Role assignment policies can share the same end-user roles. For details about the end-user roles that are available in Exchange Online, see [Role assignment policies in Exchange Online](role-assignment-policies.md).

- Role assignment policies are associated with mailboxes. Each mailbox can only be associated with one role assignment policy.

- After a mailbox is associated with a role assignment policy, the end-user roles are applied to that mailbox. The permissions granted by the roles are granted to the user of the mailbox.

![Role, role assignment policy, mailbox relationship](../media/ITPro_Security_RBAC_EXO_SimplifiedRAPRelationship.png)

The Default Role Assignment Policy role assignment policy is included with Exchange Online. As the name implies, it's the default role assignment policy. If you want to change the permissions provided by this role assignment policy, or if you want to create role assignment policies, see [Work with role assignment policies](#work-with-role-assignment-policies) later in this topic.

## Microsoft 365 or Office 365 permissions in Exchange Online

When you create a user in Microsoft 365 or Office 365, you can choose whether to assign various administrative roles, such as Global administrator, Service administrator, Password administrator, and so on, to the user. Some, but not all, Microsoft 365 and Office 365 roles grant the user administrative permissions in Exchange Online.

> [!NOTE]
> The user that was used to create your Microsoft 365 or Office 365 organization is automatically assigned to the Global administrator Microsoft 365 or Office 365 role.

The following table lists the Microsoft 365 or Office 365 roles and the Exchange Online role group they correspond to.

|**Microsoft 365 or Office 365 role**|**Exchange Online role group**|
|:-----|:-----|
|Global administrator|Organization Management <br/><br/> **Note**: The Global administrator role and the Organization Management role group are tied together using a special Company Administrator role group. The Company Administrator role group is managed internally by Exchange Online and can't be modified directly.|
|Billing administrator|No corresponding Exchange Online role group.|
|Password administrator|Help Desk administrator.|
|Service administrator|No corresponding Exchange Online role group.|
|User management administrator|No corresponding Exchange Online role group.|

For a description of the Exchange Online role groups, see the table "Built-in role groups" in [Role groups](#role-groups).

In Microsoft 365 or Office 365, when you add a user to either the Global administrator or Password administrator roles, the user is granted the rights provided by the respective Exchange Online role group. Other Microsoft 365 or Office 365 roles don't have a corresponding Exchange Online role group and won't grant administrative permissions in Exchange Online. For more information about assigning a Microsoft 365 or Office 365 role to a user, see [Assign admin roles](https://docs.microsoft.com/microsoft-365/admin/add-users/assign-admin-roles).

Users can be granted administrative rights in Exchange Online without adding them to Microsoft 365 or Office 365 roles. This is done by adding the user as a member of an Exchange Online role group. When a user is added directly to an Exchange Online role group, they'll receive the permissions granted by that role group in Exchange Online. However, they won't be granted any permissions to other Microsoft 365 or Office 365 components. They'll have administrative permissions only in Exchange Online. Users can be added to any of the role groups listed in the "Built-in role groups table" in [Role groups](#role-groups) with the exception of the Company Administrator and Help Desk Administrators role groups. For more information about adding a user directly to an Exchange Online role group, see [Work with role groups](#work-with-role-groups).

## Work with role groups

To manage your permissions using role groups in Exchange Online, we recommend that you use the EAC. When you use the EAC to manage role groups, you can add and remove roles and members, create role groups, and copy role groups with a few clicks of your mouse. The EAC provides simple dialog boxes, such as the **Add role group** dialog box, shown in the following figure, to perform these tasks.

![New role group dialog box in the EAC](../media/add-role-group.png)

Exchange Online includes several role groups that separate permissions into specific administrative areas. If these existing role groups provide the permissions your administrators need to manage your Exchange Online organization, you need only add your administrators as members of the appropriate role groups. After you add administrators to a role group, they can administer the features that relate to that role group. To add or remove members to or from a role group, open the role group in the EAC, and then add or remove members from the membership list. For a list of built-in role groups, see the table "Built-in role groups" in [Role groups](#role-groups).

> [!IMPORTANT]
> If an administrator is a member of more than one role group, Exchange Online grants the administrator all of the permissions provided by the role groups he or she is a member of.

If none of the role groups included with Exchange Online have the permissions you need, you can use the EAC to create a role group and add the roles that have the permissions you need. For your new role group, you will:

1. Choose a name for your role group.

2. Select the roles you want to add to the role group.

3. Add members to the role group.

4. Save the role group.

After you create the role group, you manage it like any other role group.

If there's an existing role group that has some, but not all, of the permissions you need, you can copy it and then make changes to create a role group. You can copy an existing role group and make changes to it, without affecting the original role group. As part of copying the role group, you can add a new name and description, add and remove roles to and from the new role group, and add new members. When you create or copy a role group, you use the same dialog box that's shown in the preceding figure.

Existing role groups can also be modified. You can add and remove roles from existing role groups, and add and remove members from it at the same time, using an EAC dialog box similar to the one in the preceding figure. By adding and removing roles to and from role groups, you turn on and off administrative features for members of that role group.

> [!NOTE]
> Although you can change which roles are assigned to built-in role groups, we recommend that you copy built-in role groups, modify the role group copy, and then add members to the role group copy. > The Company Administrator and Help Desk administrator role groups can't be copied or changed.

## Work with role assignment policies

To manage the permissions that you grant end users to manage their own mailbox in Exchange Online, we recommend that you use the EAC. When you use the EAC to manage end-user permissions, you can add roles, remove roles, and create role assignment policies with a few clicks of your mouse. The EAC provides simple dialog boxes, such as the **role assignment policy** dialog box, shown in the following figure, to perform these tasks.

![Role assignment policy dialog box in the EAC](../media/ITPro_Security_RBAC_SimplifiedEACRAP.jpg)

Exchange Online includes a role assignment policy named Default Role Assignment Policy. This role assignment policy enables users whose mailboxes are associated with it to do the following:

- Join or leave distribution groups that allow members to manage their own membership.

- View and modify basic mailbox settings on their own mailbox, such as Inbox rules, spelling behavior, junk mail settings, and Microsoft ActiveSync devices.

- Modify their contact information, such as work address and phone number, mobile phone number, and pager number.

- Create, modify, or view text message settings.

- View or modify voice mail settings.

- View and modify their marketplace apps.

- Create team mailboxes and connect them to Microsoft SharePoint lists.

- Create, modify, or view email subscription settings, such as message format and protocol defaults.

If you want to add or remove permissions from the Default Role Assignment Policy or any other role assignment policy, you can use the EAC. The dialog box you use is similar to the one in the preceding figure. When you open the role assignment policy in the EAC, select the check box next to the roles you want to assign to it or clear the check box next to the roles you want to remove. The change you make to the role assignment policy is applied to every mailbox associated with it.

If you want to assign different end-user permissions to the various types of users in your organization, you can create role assignment policies. When you create a role assignment policy, you see a dialog box similar to the one in the preceding figure. You can specify a new name for the role assignment policy, and then select the roles you want to assign to the role assignment policy. After you create a role assignment policy, you can associate it with mailboxes using the EAC.

If you want to change which role assignment policy is the default, you must use Exchange Online PowerShell. When you change the default role assignment policy, any mailboxes that are created will be associated with the new default role assignment policy if one wasn't explicitly specified. The role assignment policy associated with existing mailboxes doesn't change when you select a new default role assignment policy.

> [!NOTE]
> If you select a check box for a role that has child roles, the check boxes for the child roles are also selected. If you clear the check box for a role with child roles, the check boxes for the child roles are also cleared.

For detailed role assignment policy procedures, see [Role assignment policies in Exchange Online](role-assignment-policies.md).

## Permissions documentation

The following table contains links to topics that will help you learn about and manage permissions in Exchange Online.

|**Topic**|**Description**|
|:-----|:-----|
|[Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help)|Learn about each of the components that make up RBAC and how you can create advanced permissions models if role groups and management roles aren't enough.|
|[Manage role groups in Exchange Online](role-groups.md)|Configure permissions for Exchange Online administrators and specialist users using role groups, including adding and removing members to and from role groups.|
|[Role assignment policies in Exchange Online](role-assignment-policies.md)|Configure which features end users have access to on their mailboxes using role assignment policies, view, create, modify, and remove role assignment policies, specify the default role assignment policy, and apply role assignment policies to mailboxes.|
|[Feature permissions in Exchange Online](feature-permissions.md)|Learn more about the permissions required to manage Exchange Online features and services.|
