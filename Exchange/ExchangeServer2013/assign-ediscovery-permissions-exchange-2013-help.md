---
title: 'Assign eDiscovery permissions in Exchange: Exchange 2013 Help'
TOCTitle: Assign eDiscovery permissions in Exchange
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 729e09d8-614b-431f-ae04-ae41fb4c628e
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Assign eDiscovery permissions in Exchange 2013

_**Applies to:** Exchange Server 2013_

If you want users to be able to use Microsoft Exchange Server 2013 In-Place eDiscovery, you must first authorize them by adding them to the Discovery Management role group. Members of the Discovery Management role group have Full Access mailbox permissions for the Discovery mailbox that's created by Exchange Setup.

> [!CAUTION]
> Members of the Discovery Management role group can access sensitive message content. Specifically, these members can use [In-Place eDiscovery](in-place-ediscovery-exchange-2013-help.md) to search all mailboxes in your Exchange organization, preview messages (and other mailbox items), copy them to a Discovery mailbox and export the copied messages to a .pst file. In most organizations, this permission is granted to legal, compliance, or Human Resources personnel. >

To learn more about the Discovery Management role group, see [Discovery Management](discovery-management-exchange-2013-help.md). To learn more about Role Based Access Control (RBAC), see [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md).

Interested in scenarios where this procedure is used? See the following topics:

- [Create an In-Place eDiscovery search](create-in-place-ediscovery-search-exchange-2013-help.md)

- [Create or remove an In-Place Hold](create-or-remove-in-place-holds-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- By default, the Discovery Management role group doesn't contain any members. Administrators with the Organization Management role are also unable to create or manage discovery searches without being added to the Discovery Management role group.

- In Exchange 2013, members of the Organization Management role group can create an [In-Place Hold and Litigation Hold](in-place-and-litigation-holds-exchange-2013-help.md) to place all mailbox content on hold. However, to create a query-based In-Place Hold, the user must be a member of the Discovery Management role group or have the Mailbox Search role assigned.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to add a user to the Discovery Management role group

1. Go to **Permissions** \> **Admin roles**.

2. In the list view, select **Discovery Management** and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif)

3. In **Role Group**, under **Members**, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

4. In **Select Members**, select one or more users, click **Add**, and then click **OK**.

5. In **Role Group**, click **Save**.

## Use the Shell to add a user to the Discovery Management role group

This example adds the user Bsuneja to the Discovery Management role group.

```powershell
Add-RoleGroupMember -Identity "Discovery Management" -Member Bsuneja
```

For detailed syntax and parameter information, see [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember).

## How do you know this worked?

To verify that you've added the user to the Discovery Management role group, do the following:

1. In the EAC, go to **Permissions** \> **Admin roles**.

2. In the list view, select **Discovery Management**.

3. In the details pane, verify that the user is listed under **Members**.

You can also run this command to list the members of the Discovery Management role group.

```powershell
Get-RoleGroupMember -Identity "Discovery Management"
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
