---
title: 'Create a role: Exchange 2013 Help'
TOCTitle: Create a role
ms:assetid: e614ad8f-5946-4135-b130-89ea626afcd4
ms:mtpsurl: https://technet.microsoft.com/library/Dd351214(v=EXCHG.150)
ms:contentKeyID: 49289443
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a role

_**Applies to:** Exchange Server 2013_

You can create a management role, change the management role entries, add a scope if needed, and then assign the role to a role assignee. You should rarely need to perform this procedure. We recommend that you check whether a built-in management role can be used instead of creating a management role. For a list of built-in management roles, see [Built-in management roles](built-in-management-roles-exchange-2013-help.md).

For more information about management roles in Microsoft Exchange Server 2013, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).

> [!NOTE]
> This topic doesn't discuss how to create an unscoped management role. For information about how to create an unscoped management role, see <A href="create-an-unscoped-role-exchange-2013-help.md">Create an unscoped role</A>.

Looking for other management tasks related to roles? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Management roles" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Create the management role

New management roles are based on existing roles. When you create a role, an existing role and its management role entries are copied to the new role. The existing role becomes the parent to the new child role. You must always choose a role that contains all the cmdlets and parameters you need to use, and then remove the ones you don't want. Child roles can't have management role entries that don't exist in the parent role.

Use the following syntax to create the new role.

```powershell
New-ManagementRole -Parent <existing role to copy> -Name <name of new role>
```

This example copies the Mail Recipients role and its management role entries to the Seattle Mail Recipients role.

```powershell
New-ManagementRole -Parent "Mail Recipients" -Name "Seattle Mail Recipients"
```

For detailed syntax and parameter information, see [New-ManagementRole](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRole).

## Step 2: Change the new role's management role entries

After you create your role, you need to change the role's entries. You can remove an entire role entry, which removes access to the associated cmdlet completely. Or, you can remove parameters from a role entry to remove access to those specific parameters on the associated cmdlet.

You can't add new role entries or parameters on role entries unless they exist in the parent role. Because you just created a role from a parent role in Step 1, you can't add any additional role entries or parameters on role entries because they don't exist in the parent role.

When you change a role entry on a role, you can do one of the following:

- Remove a single, entire role entry.

- Remove multiple, entire role entries.

- Remove parameters from a role entry.

To remove role entries from your new role, see [Remove a role entry from a role](remove-a-role-entry-from-a-role-exchange-2013-help.md).

## Step 3: Create a custom management role scope, if required

Management role scopes determine the objects made available to a user to view or change using the role entries configured in Step 2. New management roles inherit the read and write management role scopes of their parent role. These are called *implicit scopes*. However, there may be cases where you want to change the write scope of the new role to match your business needs. When you create a custom scope, you override the implicit write scope of the role. The implicit read scope of the role doesn't change. For more information about management role scopes, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

You can create a custom scope, create an exclusive scope, use a predefined scope, or scope an assignment to an organizational unit (OU). The new scope must be within the implicit read scope of the role. To use a predefined scope or to specify an organizational unit, skip to Step 4.

To add a custom scope to your new role, see [Create a regular or exclusive scope](create-a-regular-or-exclusive-scope-exchange-2013-help.md).

## Step 4: Assign the new management role

The final step when you create and configure a role is to assign it to a role assignee.

When you create a role assignment, you can choose to do one of the following:

- Create the role assignment with no scope.

- Create the role assignment with a predefined scope.

- Create the role assignment with an OU without a domain restriction filter.

- Create the role assignment with the custom or exclusive scope you created in Step 3.

  > [!NOTE]
  > You can't specify a scope when you create an assignment between a role and a management role assignment policy.

You can assign the new role to a role group, a role assignment policy, a user, or a universal security group (USG). For more information, see the following topics:

- [Manage role groups](manage-role-groups-exchange-2013-help.md)

- [Manage role assignment policies](manage-role-assignment-policies-exchange-2013-help.md)

- [Add a role to a user or USG](add-a-role-to-a-user-or-usg-exchange-2013-help.md)
