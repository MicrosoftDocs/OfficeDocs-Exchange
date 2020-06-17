---
localization_priority: Normal
description: The permissions required to perform tasks to manage Microsoft Exchange Online vary depending on the procedure being performed or the cmdlet you want to run.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 15073ce1-0917-403b-8839-02a2ebc96e16
ms.reviewer:
f1.keywords:
- NOCSH
title: Feature permissions in Exchange Online
ms.collection:
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars
ms.custom: seo-marvel-apr2020
---

# Feature permissions in Exchange Online

The permissions required to perform tasks to manage Microsoft Exchange Online vary depending on the procedure being performed or the cmdlet you want to run.

For information about Exchange Online Protection (EOP) permissions, see [Feature Permissions in EOP](https://docs.microsoft.com/microsoft-365/security/office-365-security/feature-permissions-in-eop).

To find out what permissions you need to perform the procedure or run the cmdlet, do the following:

1. In the table below, find the feature that is most related to the procedure you want to perform or the cmdlet you want to run.

2. Next, look at the permissions required for the feature. You must be assigned one of those role groups, an equivalent custom role group, or an equivalent management role. You can also click on a role group to see its management roles. If a feature lists more than one role group, you only need to be assigned one of the role groups to use the feature. For more information about role groups and management roles, see [Understanding Role Based Access Control](https://docs.microsoft.com/Exchange/understanding-role-based-access-control-exchange-2013-help).

3. Now, run the **Get-ManagementRoleAssignment** cmdlet to look at the role groups or management roles assigned to you to see if you have the permissions that are necessary to manage the feature.

    > [!NOTE]
    > You must be assigned the Role Management management role to run the **Get-ManagementRoleAssignment** cmdlet. If you don't have permissions to run the **Get-ManagementRoleAssignment** cmdlet, ask your Exchange administrator to retrieve the role groups or management roles assigned to you.

If you want to delegate the ability to manage a feature to another user, see [Delegate role assignments](https://docs.microsoft.com/Exchange/delegate-role-assignments-exchange-2013-help).

## Exchange Online permissions

You can use the features in the following table to manage your Exchange Online organization and recipients. Users who are assigned the View-Only Management role group can view the configuration of the features in the following table. For more information about these role groups, see [Role groups](permissions-exo.md#role-groups).

|**Feature**|**Permissions required**|
|:-----|:-----|
|Anti-malware|Organization Management <br/><br/> Hygiene Management|
|Anti-spam|Organization Management <br/><br/> Hygiene Management|
|Arbitration|Organization Management|
|Client Access user settings|Organization Management|
|Data loss prevention (DLP)|Organization Management <br/><br/> Compliance Management|
|Discovery mailboxes - Create|Organization Management <br/> Recipient Management|
|Distribution groups|Organization Management <br/> Recipient Management|
|Office 365 connectors|Organization Management|
|In-Place eDiscovery|Discovery Management  <br/> **Note**: By default, the Discovery Management role group doesn't have any members. No users, including adminis, have the required permissions to search mailboxes. For more information, see [Assign eDiscovery permissions in Exchange](../security-and-compliance/in-place-ediscovery/assign-ediscovery-permissions.md).|
|In-Place Hold|Discovery Management <br/> Organization Management <br/> **Notes**:  <br/> • To create a query-based In-Place Hold, a user requires both the Mailbox Search and Legal Hold roles to be assigned via membership in a role group that has both roles assigned. To create an In-Place Hold without using a query, which places all mailbox items on hold, you must have the Legal Hold role assigned. The Discovery Management role group is assigned both roles.  <br/> • The Organization Management role group is assigned the Legal Hold role. Members of the Organization Management role group can place an In-Place Hold on all items in a mailbox, but can't create a query-based In-Place Hold.|
|Journal archiving|Organization Management <br/><br/> Recipient Management|
|Journaling|Organization Management <br/> Records Management|
|Linked user|Organization Management <br/><br/> Recipient Management|
|Mail flow|Organization Management|
|Mail flow rules|Organization Management|
|Mailbox settings|Organization Management <br/><br/> Recipient Management|
|Microsoft Office 365 Message Encryption (OME)|Organization Management <br/><br/> Compliance Management <br/><br/> Records Management|
|Message trace|Organization Management <br/><br/> Compliance Management <br/><br/> Help Desk|
|Messaging records management|Compliance Management <br/> Organization Management <br/> Records Management|
|Mobile devices|Organization Management <br/><br/> Recipient Management
|Organization configuration|Organization Management|
|Outlook on thew web mailbox policies|Organization Management <br/><br/> Recipient Management|
|Permissions and delegation|Organization Management|
|Public folders|Organization Management <br/> Public Folder Management <br/> Mail-enabled public folders require Recipient Management|
|POP3 and IMAP4 permissions|Organization Management|
|Quarantine|Organization Management <br/><br/> Hygiene Management|
|Recipients|Organization Management <br/> Recipient Management|
|Retention policies|Organization Management <br/> Recipient Management <br/> Records Management|
|Role assignments|Organization Management|
|Supervision|Organization Management|
|Unified Messaging|Organization Management <br/> Unified Messaging Management|
|View-only administrator audit logging|Organization Management <br/><br/> Records Management|
|View reports|Organization Management: Users have access to mailbox reports and mail protection reports. <br/><br/> View-Only Organization Management: Users have access to mailbox reports. <br/><br/> View-Only Recipients: Users have access to mail protection reports. <br/><br/> Compliance Management: Users have access to mail protection reports and Data Loss Prevention (DLP) reports (if their subscription has DLP capabilities).|

> [!NOTE]
> To find the permissions that are required to run any Exchange Online cmdlet, see [Find the permissions required to run any Exchange cmdlet](https://docs.microsoft.com/powershell/exchange/find-exchange-cmdlet-permissions).
