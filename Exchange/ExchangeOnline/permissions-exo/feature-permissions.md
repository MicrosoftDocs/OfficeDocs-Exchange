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

For information about Exchange Online Protection (EOP) permissions, see [Feature Permissions in EOP](https://technet.microsoft.com/library/34674847-a6b7-4a7e-9eaa-b64f22bc150d.aspx).

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
|Data loss prevention|Organization Management <br/><br/> Compliance Management|
|Office 365 connectors|Organization Management|
|Journal archiving|Organization Management <br/><br/> Recipient Management|
|Linked user|Organization Management <br/><br/> Recipient Management|
|Mail flow|Organization Management|
|Mailbox settings|Organization Management <br/><br/> Recipient Management|
|Microsoft Office 365 Message Encryption (OME)|Organization Management <br/><br/> Compliance Management <br/><br/> Records Management|
|Message trace|Organization Management <br/><br/> Compliance Management <br/><br/> Help Desk|
|Organization configuration|Organization Management|
|Outlook on thew web mailbox policies|Organization Management <br/><br/> Recipient Management
|POP3 and IMAP4 permissions|Organization Management|
|Quarantine|Organization Management <br/><br/> Hygiene Management|
|Supervision|Organization Management|
|View reports|Organization Management: Users have access to mailbox reports and mail protection reports. <br/><br/> View-Only Organization Management: Users have access to mailbox reports. <br/><br/> View-Only Recipients: Users have access to mail protection reports. <br/><br/> Compliance Management: Users have access to mail protection reports and Data Loss Prevention (DLP) reports (if their subscription has DLP capabilities).|

> [!NOTE]
> To find the permissions that are required to run any Exchange Online cmdlet, see [Find the permissions required to run any Exchange cmdlet](https://docs.microsoft.com/powershell/exchange/exchange-server/find-exchange-cmdlet-permissions).
