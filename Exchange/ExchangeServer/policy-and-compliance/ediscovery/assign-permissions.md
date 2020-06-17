---
localization_priority: Normal
description: 'Summary: Learn how to give users the permissions they need to use In-Place eDiscovery in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 729e09d8-614b-431f-ae04-ae41fb4c628e
ms.reviewer:
title: Assign eDiscovery permissions in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Assign eDiscovery permissions in Exchange Server

If you want users to be able to use Exchange Server In-Place eDiscovery, you first need to add them to the Discovery Management role group. Members of the Discovery Management role group have Full Access mailbox permissions to the default discovery mailbox, which is called **Discovery Search Mailbox**.

> [!CAUTION]
> Members of the Discovery Management role group can access sensitive message content. Specifically, these members can use In-Place eDiscovery to search all mailboxes in your Exchange organization, preview the search results (and other mailbox items), copy them to a discovery mailbox, and export the search results to a .pst file. In most organizations, this permission is assigned to legal, compliance, or Human Resources personnel.

## What do you need to know before you begin?

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](../../permissions/feature-permissions/rbac-permissions.md) topic.

- By default, the Discovery Management role group doesn't contain any members. Therefore, administrators that have the Organization Management role assigned can't create or manage discovery searches without being added to the Discovery Management role group.

- In Exchange Server, members of the Organization Management role group can create an In-Place Hold to place all mailbox content on hold. However, to create a query-based In-Place Hold, the user must be a member of the Discovery Management role group or have the Mailbox Search role assigned.

- You can only add *security principals* to the Discovery Management role group (users or groups that can be assigned permissions). For example:

  - User mailboxes

  - Mail users

  - Security groups

  - Other role groups

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

## Use the EAC to add a user to the Discovery Management role group

1. In the EAC, go to **Permissions** \> **Admin roles**, select the **Discovery Management** role group, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. On the resulting **Role Group** page, in the **Members** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

3. In the resulting **Select Members** dialog, select an available user or group, and then click **Add**. Repeat this step as many times as necessary. When you're finished, click **OK**.

4. Back on the **Role Group** page, click **Save**.

## Use the Exchange Management Shell to add a user to the Discovery Management role group

To add a user to the Discovery Management role group, use the following syntax:

```PowerShell
Add-RoleGroupMember -Identity "Discovery Management" -Member <Identity>
```

This example adds the user Bsuneja to the Discovery Management role group.

```PowerShell
Add-RoleGroupMember -Identity "Discovery Management" -Member Bsuneja
```

This example add the members of the mail-enabled security group named Contoso Compliance Management.

```PowerShell
Add-RoleGroupMember -Identity "Discovery Management" -Member "Contoso Compliance Management"
```

For more information, see [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember).

## How do you know this worked?

To verify that you've added the user to the Discovery Management role group, use either of the following procedures:

- In the EAC, go to **Permissions** \> **Admin roles**, and select the **Discovery Management** role group. In the details pane, verify that the user is listed in the **Members** section.

- In the Exchange Management Shell, run the following command to view the members of the Discovery Management role group.

  ```PowerShell
  Get-RoleGroupMember -Identity "Discovery Management"
  ```
