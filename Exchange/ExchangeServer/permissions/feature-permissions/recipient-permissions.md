---
localization_priority: Normal
description: 'Summary: Learn about permissions that are required to manage recipients in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5b690bcb-c6df-4511-90e1-08ca91f43b37
ms.reviewer:
title: Recipients Permissions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recipients Permissions

The permissions required to perform tasks to manage recipients vary depending on the procedure being performed or the cmdlet you want to run.

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you need to be assigned to only one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/exchange/delegate-role-assignments-exchange-2013-help).

## Mailbox server permissions

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Calendar repair, server configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Delegating Mailbox servers|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Email address policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange Search|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Exchange Search - diagnostics|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> Support Diagnostics role  <br/> **Note:**: The Support Diagnostics role isn't assigned to a role group. For more information, see [Add a role to a role group](../role-groups.md#add-a-role-to-a-role-group).|
|Group metrics|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Import Export|Mailbox Import Export role  <br/> **Note:**: The Mailbox Import Export role isn't assigned to a role group. For more information, see [Mailbox Import Export Role](https://docs.microsoft.com/exchange/mailbox-import-export-role-exchange-2013-help).|
|Mailbox Assistants|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Mailbox moves|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mailbox recovery|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Mailbox repair request|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mailbox restore request|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Mailbox server configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Manage Exchange Search Indexer service on a Mailbox server|Local Administrator on the Mailbox server|
|MAPI connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|OAB virtual directories|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Remove store mailbox|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|

## Calendar and sharing permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Calendar configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Calendar diagnostics|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Records Management](https://docs.microsoft.com/exchange/records-management-exchange-2013-help) <br/> [Hygiene Management](https://docs.microsoft.com/exchange/hygiene-management-exchange-2013-help) <br/> [Compliance Management](https://docs.microsoft.com/exchange/compliance-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Calendar processing|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Notifications|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Organization relationships|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Sharing policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Resource mailbox configuration permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Booking policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Delegation|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Resource mailbox schema configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|

## Mailbox database permissions
<a name="calendar"> </a>

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Mailbox databases|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|

## Recipient provisioning permissions
<a name="calendar"> </a>

This table contains the various permissions that are required to manage recipients.

Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information, see [View-only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Address list, GAL|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Antispam|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Apps for Outlook|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Applying sharing policies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Arbitration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Archive connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help) <br/> [Server Management](https://docs.microsoft.com/exchange/server-management-exchange-2013-help)|
|Assigning offline address books|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Automatic replies|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Calendar configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Calendar repair|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Contact aggregation settings|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [View-Only Organization Management](https://docs.microsoft.com/exchange/view-only-organization-management-exchange-2013-help)|
|Convert mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Disconnected mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Distribution groups|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Dynamic distribution groups|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Email addresses|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [UM Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|Inbox rules|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Mail contacts|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mail tips|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mail user|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mailbox folder permissions|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Mailbox folders|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|MAPI connectivity|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Message configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Message quotas|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Moderation|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Permissions and delegation|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help)|
|Archive mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Recipient data properties|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Remote mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Retention and legal holds|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Records Management](https://docs.microsoft.com/exchange/records-management-exchange-2013-help)|
|Send As|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Spelling configuration|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|
|Unified Messaging (in Exchange 2016; not available in Exchange 2019)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [UM Management](https://docs.microsoft.com/exchange/um-management-exchange-2013-help)|
|User mailboxes|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|User photos|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help) <br/> [Help Desk](https://docs.microsoft.com/exchange/help-desk-exchange-2013-help)|

## Mailbox move and migration permissions
<a name="calendar"> </a>

The table contains the permissions that are required to move on-premises mailboxes to different domains or forests and to migrate on-premises mailboxes to and from your cloud-based organization.

|**Feature**|**Permissions required**|
|:-----|:-----|
|Mailbox moves (local or cross-forest)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Mailbox moves (hybrid deployment)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
|Migration (on-boarding and off-boarding from the cloud)|[Organization Management](https://docs.microsoft.com/exchange/organization-management-exchange-2013-help) <br/> [Recipient Management](https://docs.microsoft.com/exchange/recipient-management-exchange-2013-help)|
