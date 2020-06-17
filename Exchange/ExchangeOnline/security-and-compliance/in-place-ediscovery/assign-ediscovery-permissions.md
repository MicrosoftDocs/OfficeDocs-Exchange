---
localization_priority: Normal
description: If you want users to be able to use Microsoft Exchange Server In-Place eDiscovery, you must first authorize them by adding them to the Discovery Management role group. Members of the Discovery Management role group have Full Access mailbox permissions for the Discovery mailbox that's created by Exchange Setup.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 729e09d8-614b-431f-ae04-ae41fb4c628e
ms.reviewer: 
f1.keywords:
- NOCSH
title: Assign eDiscovery permissions in Exchange
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Assign eDiscovery permissions in Exchange

If you want users to be able to use Microsoft Exchange Server In-Place eDiscovery, you must first authorize them by adding them to the Discovery Management role group. Members of the Discovery Management role group have Full Access mailbox permissions for the Discovery mailbox that's created by Exchange Setup.

> [!CAUTION]
> Members of the Discovery Management role group can access sensitive message content. Specifically, these members can use [In-Place eDiscovery](in-place-ediscovery.md) to search all mailboxes in your Exchange organization, preview messages (and other mailbox items), copy them to a Discovery mailbox and export the copied messages to a .pst file. In most organizations, this permission is granted to legal, compliance, or Human Resources personnel. >

To learn more about the Discovery Management role group and role based access control (RBAC), see [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md).

Interested in scenarios where this procedure is used? See the following topics:

- [Create an In-Place eDiscovery search](create-in-place-ediscovery-search.md)

- [Create or remove an In-Place Hold](../../security-and-compliance/create-or-remove-in-place-holds.md)

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role assignments" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- By default, the Discovery Management role group doesn't contain any members. Administrators with the Organization Management role are also unable to create or manage discovery searches without being added to the Discovery Management role group.

- In Exchange Server, members of the Organization Management role group can create an [In-Place Hold and Litigation Hold](../../security-and-compliance/in-place-and-litigation-holds.md) to place all mailbox content on hold. However, to create a query-based In-Place Hold, the user must be a member of the Discovery Management role group or have the Mailbox Search role assigned.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

## Use the EAC to add a user to the Discovery Management role group

1. Go to **Permissions** \> **Admin roles**.

2. In the list view, select **Discovery Management** and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif)

3. In **Role Group**, under **Members**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. In **Select Members**, select one or more users, click **Add**, and then click **OK**.

5. In **Role Group**, click **Save**.

## Use Exchange Online PowerShell to add a user to the Discovery Management role group

This example adds the user Bsuneja to the Discovery Management role group.

```PowerShell
Add-RoleGroupMember -Identity "Discovery Management" -Member Bsuneja
```

For detailed syntax and parameter information, see [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember).

## How do you know this worked?

To verify that you've added the user to the Discovery Management role group, do the following:

1. In the EAC, go to **Permissions** \> **Admin roles**.

2. In the list view, select **Discovery Management**.

3. In the details pane, verify that the user is listed under **Members**.

You can also run this command to list the members of the Discovery Management role group.

```PowerShell
Get-RoleGroupMember -Identity "Discovery Management"
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
