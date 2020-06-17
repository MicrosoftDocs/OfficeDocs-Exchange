---
title: 'Create linked role groups that mirror built-in role groups: Exchange 2013 Help'
TOCTitle: Create linked role groups that mirror built-in role groups
ms:assetid: 89dfcbb3-0568-4bbf-a885-746b91ba307e
ms:mtpsurl: https://technet.microsoft.com/library/Dd876918(v=EXCHG.150)
ms:contentKeyID: 49289333
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create linked role groups that mirror built-in role groups

_**Applies to:** Exchange Server 2013_

Using linked management role groups in Microsoft Exchange Server 2013, you can link a role group in an Exchange 2013 resource forest with a universal security group (USG) in a foreign user forest. This is useful when you want administrators with accounts in the user forest to manage the servers running Exchange in the resource forest. For more information about linked role groups, see [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md).

By default, Exchange 2013 includes a number of built-in role groups that provide you with permissions to manage a variety of features and job functions. Each role group is tailored to provide specific permissions for each feature and job function. However, these role groups can't be linked to USGs in a foreign forest. They can only contain users and USGs from the local resource forest. Fortunately, it's possible to replicate these built-in role groups using linked role groups.

You can re-create each built-in role group as a linked role group. All of the management roles and management scopes assigned to each role group are added to the new linked role group. For more information about management roles and scopes, see the following topics:

- [Understanding management roles](understanding-management-roles-exchange-2013-help.md)

- [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md)

Looking for other management tasks related to role groups? Check out [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use the Shell to perform these procedures.

- Configuring a linked role group requires a one-way trust between the resource Active Directory forest in which the linked role group will reside, and the foreign Active Directory forest where the users or USGs reside. The resource forest must trust the foreign forest.

- You must have the following information about the foreign Active Directory forest:

  - **Credentials**: You must have a username and password that can access the foreign Active Directory forest. This information is used with the *LinkedCredential* parameter on the **New-RoleGroup** cmdlet. This information is obtained by running the **Get-Credential** cmdlet. The format of the username is *domain*\\*username*.

  - **Domain controller**: You must have the fully qualified domain name (FQDN) of an Active Directory domain controller in the foreign Active Directory forest. This information is used with the *LinkedDomainController* parameter on the **New-RoleGroup** cmdlet.

  - **Foreign USG**: You must have the full name of a USG in the foreign Active Directory forest that contains the members you want to associate with the linked role group. This information is used with the *LinkedForeignGroup* parameter on the **New-RoleGroup** cmdlet.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to create linked role groups that replicate built-in role groups

Each of the following sections shows you how to re-create each role group as a linked role group. Complete the procedures in each section to re-create all of the built-in role groups as linked role groups.

## Create the Organization Management linked role group

To re-create the Organization Management role group as a linked role group, you perform a procedure that's different than the procedure used to re-create other built-in role groups. This is because the Organization Management role group has delegating role assignments between it and all of the management roles. Re-creating the delegating role assignments requires an additional step.

1. Create a USG in the foreign forest that will be linked to the Organization Management role group.

2. Store the foreign Active Directory forest credentials in a variable.

    ```powershell
    $ForeignCredential = Get-Credential
    ```

3. Store all of the roles assigned to the Organization Management role group in a variable.

    ```powershell
    $OrgMgmt  = Get-RoleGroup "Organization Management"
    ```

4. Create the Organization Management linked role group and add the roles assigned to the built-in Organization Management role group.

    ```powershell
    New-RoleGroup "Organization Management - Linked" -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -LinkedCredential $ForeignCredential -Roles $OrgMgmt.Roles
    ```

5. Remove all of the regular assignments between the new Organization Management linked role group and the My\* end-user roles.

    ```powershell
    Get-ManagementRoleAssignment -RoleAssignee "Organization Management - Linked" -Role My* | Remove-ManagementRoleAssignment
    ```

6. Add delegating role assignments between the new Organization Management linked role group and all management roles.

    ```powershell
    Get-ManagementRole | New-ManagementRoleAssignment -SecurityGroup "Organization Management - Linked" -Delegating
    ```

This example assumes the following values are used for each parameter:

- **LinkedForeignGroup**: `Organization Management Administrators`

- **LinkedDomainController**: `DC01.users.contoso.com`

Using the preceding values, this example re-creates the Organization Management role group as a linked role group.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
$OrgMgmt  = Get-RoleGroup "Organization Management"
New-RoleGroup "Organization Management - Linked" -LinkedForeignGroup "Organization Management Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -Roles $OrgMgmt.Roles
Get-ManagementRoleAssignment -RoleAssignee "Organization Management - Linked" -Role My* | Remove-ManagementRoleAssignment
Get-ManagementRole | New-ManagementRoleAssignment -SecurityGroup "Organization Management - Linked" -Delegating
```

## Create all other linked role groups

To re-create the built-in role groups (other than the Organization Management role group) as linked role groups, use the following procedure for each group.

1. Create a USG in the foreign forest for each role group that will be linked to each new role group.

2. Store the foreign Active Directory forest credentials in a variable. You only need to do this once.

   ```powershell
   $ForeignCredential = Get-Credential
   ```

3. Retrieve a list of role groups using the following cmdlet.

   ```powershell
   Get-RoleGroup
   ```

4. For each role group, other than the Organization Management role group, do the following.

   ```powershell
   $RoleGroup = Get-RoleGroup <name of role group to re-create>
   New-RoleGroup "<role group name> - Linked" -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -LinkedCredential $ForeignCredential -Roles $RoleGroup.Roles
   ```

5. Repeat the preceding step for each built-in role group you want to re-create as a linked role group.

This example assumes the following values are used for each parameter:

- **LinkedDomainController**: `DC01.users.contoso.com`

- **Built-in role groups to be re-created as linked role groups**: `Recipient Management, Server Management`

- **Foreign group for Recipient Management linked role group**: `Recipient Management Administrators`

- **Foreign group for Server Management linked role group**: `Server Management Administrators`

Using the preceding values, this example re-creates the Recipient Management and Server Management role groups as linked role groups.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
Get-RoleGroup
```

```powershell
$RoleGroup = Get-RoleGroup "Recipient Management"
New-RoleGroup "Recipient Management - Linked" -LinkedForeignGroup "Recipient Management Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -Roles $RoleGroup.Roles
$RoleGroup = Get-RoleGroup "Server Management"
New-RoleGroup "Server Management - Linked" -LinkedForeignGroup "Server Management Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -Roles $RoleGroup.Roles
```

## Other tasks

After you create linked role groups, you may also want to:

Add members to the foreign USGs using Active Directory Users and Computers in the foreign forest.

Remove members of built-in role groups. For more information, see [Manage role group members](manage-role-group-members-exchange-2013-help.md).

Add, remove, or change the scope of roles on the new linked role groups. For more information, see [Manage role groups](manage-role-groups-exchange-2013-help.md).

Create additional linked role groups. For more information, see [Manage linked role groups](manage-linked-role-groups-exchange-2013-help.md).
