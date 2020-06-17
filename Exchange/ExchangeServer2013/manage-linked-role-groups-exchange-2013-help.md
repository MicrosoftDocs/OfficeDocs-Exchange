---
title: 'Manage linked role groups: Exchange 2013 Help'
TOCTitle: Manage linked role groups
ms:assetid: e2a07395-90c2-4d62-b15d-ac3ff28fe786
ms:mtpsurl: https://technet.microsoft.com/library/JJ657502(v=EXCHG.150)
ms:contentKeyID: 49289439
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage linked role groups

_**Applies to:** Exchange Server 2013_

You can use a linked management role group to enable members of a universal security group (USG) in a foreign Active Directory forest to manage a Microsoft Exchange Server 2013 organization in a resource Active Directory forest. By associating a USG in a foreign forest with a linked role group, the members of that USG are granted the permissions provided by the management roles assigned to the linked role group. For more information about linked role groups, see [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md).

To create and configure linked role groups, you need to use the **New-RoleGroup** and **Set-RoleGroup** cmdlets. For detailed syntax and parameter information, see the following topics:

- [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup)

- [Set-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Set-RoleGroup)

For additional management tasks related to role groups, see [Permissions](permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 to 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Role groups" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to create or configure linked role groups. You must use the Exchange Management Shell.

- At a minimum, configuring a linked role group requires that a one-way trust is established between the resource Active Directory forest in which the linked role group will reside, and the foreign Active Directory forest where the users or USGs reside. The resource forest must trust the foreign forest.

- You must have the following information about the foreign Active Directory forest:

  - **Credentials**: You must have a username and password that can access the foreign Active Directory forest. This information is used with the *LinkedCredential* parameter on the **New-RoleGroup** and **Set-RoleGroup** cmdlets.

  - **Domain controller**: You must have the fully qualified domain name (FQDN) of an Active Directory domain controller in the foreign Active Directory forest. This information is used with the *LinkedDomainController* parameter on the **New-RoleGroup** and **Set-RoleGroup** cmdlets.

  - **Foreign USG**: You must have the full name of a USG in the foreign Active Directory forest that contains the members you want to associate with the linked role group. This information is used with the *LinkedForeignGroup* parameter on the **New-RoleGroup** and **Set-RoleGroup** cmdlet.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a linked role group

## Use the Shell to create a linked role group with no scope

To create a linked role group and assign management roles to the linked role group, do the following:

1. Store the foreign Active Directory forest credentials in a variable.

    ```powershell
    $ForeignCredential = Get-Credential
    ```

2. Create the linked role group using the following syntax.

    ```powershell
    New-RoleGroup <role group name> -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -LinkedCredential $ForeignCredential -Roles <role1, role2, role3...>
    ```

3. Add or remove members to or from the foreign USG using Active Directory Users and Computers on a computer in the foreign Active Directory forest.

This example does the following:

- Retrieves the credentials for the users.contoso.com foreign Active Directory forest. These credentials are used to connect to the DC01.users.contoso.com domain controller in the foreign forest.

- Creates a linked role group called Compliance Role Group in the resource forest where Exchange 2013 is installed.

- Links the new role group to the Compliance Administrators USG in the users.contoso.com foreign Active Directory forest.

- Assigns the Transport Rules and Journaling management roles to the new linked role group.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
New-RoleGroup "Compliance Role Group" -LinkedForeignGroup "Compliance Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -Roles "Transport Rules", "Journaling"
```

## Use the Shell to create a linked role group with a custom management scope

You can create linked role groups with custom recipient management scopes, custom configuration management scopes, or both. To create a linked role group and assign management roles with custom scopes to it, do the following:

1. Store the foreign Active Directory forest credentials in a variable.

    ```powershell
    $ForeignCredential = Get-Credential
    ```

2. Create the linked role group using the following syntax.

    ```powershell
    New-RoleGroup <role group name> -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -CustomConfigWriteScope <name of configuration scope> -CustomRecipientWriteScope <name of recipient scope> -LinkedCredential $ForeignCredential -Roles <role1, role2, role3...>
    ```

3. Add or remove members to or from the foreign USG using Active Directory Users and Computers on a computer in the foreign Active Directory forest.

This example does the following:

- Retrieves the credentials for the users.contoso.com foreign Active Directory forest. These credentials are used to connect to the DC01.users.contoso.com domain controller in the foreign forest.

- Creates a linked role group called Seattle Compliance Role Group in the resource forest where Exchange 2013 is installed.

- Links the new role group to the Seattle Compliance Administrators USG in the users.contoso.com foreign Active Directory forest.

- Assigns the Transport Rules and Journaling management roles to the new linked role group with the Seattle Recipients custom recipient scope.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
New-RoleGroup "Seattle Compliance Role Group" -LinkedForeignGroup "Seattle Compliance Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -CustomRecipientWriteScope "Seattle Recipients" -Roles "Transport Rules", "Journaling"
```

For more information about management scopes, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

## Use the Shell to create a linked role group with an OU scope

You can create linked role groups that use an OU recipient scope. To create a linked role group and assign management roles to it with an OU scope, do the following:

1. Store the foreign Active Directory forest credentials in a variable.

    ```powershell
    $ForeignCredential = Get-Credential
    ```

2. Create the linked role group using the following syntax.

    ```powershell
    New-RoleGroup <role group name> -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -LinkedCredential $ForeignCredential -RecipientOrganizationalUnitScope <OU name> -Roles <role1, role2, role3...>
    ```

3. Add or remove members to or from the foreign USG using Active Directory Users and Computers on a computer in the foreign Active Directory forest.

This example does the following:

- Retrieves the credentials for the users.contoso.com foreign Active Directory forest. These credentials are used to connect to the DC01.users.contoso.com domain controller in the foreign forest.

- Creates a linked role group called Executives Compliance Role Group in the resource forest where Exchange 2013 is installed.

- Links the new role group to the Executives Compliance Administrators USG in the users.contoso.com foreign Active Directory forest.

- Assigns the Transport Rules and Journaling management roles to the new linked role group with the OU recipient scope Executives OU.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
New-RoleGroup "Executives Compliance Role Group" -LinkedForeignGroup "Executives Compliance Administrators" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential -RecipientOrganizationalUnitScope "Executives OU" -Roles "Transport Rules", "Journaling"
```

For more information about management scopes, see [Understanding management role scopes](understanding-management-role-scopes-exchange-2013-help.md).

## Change the foreign USG on a linked role group

## Use the Shell to change the foreign USG on a linked role group

To change the foreign USG associated with a linked role group, do the following:

1. Store the foreign Active Directory forest credentials in a variable.

    ```powershell
    $ForeignCredential = Get-Credential
    ```

2. Change the foreign USG on the existing linked role group using the following syntax.

    ```powershell
    Set-RoleGroup <role group name> -LinkedForeignGroup <name of foreign USG> -LinkedDomainController <FQDN of foreign Active Directory domain controller> -LinkedCredential $ForeignCredential
    ```

This example does the following:

- Retrieves the credentials for the users.contoso.com foreign Active Directory forest. These credentials are used to connect to the DC01.users.contoso.com domain controller in the foreign forest.

- Changes the foreign USG on the Compliance Role Group role group to Regulatory Compliance Officers.

```powershell
$ForeignCredential = Get-Credential
```

```powershell
Set-RoleGroup "Compliance Role Group" -LinkedForeignGroup "Regulatory Compliance Officers" -LinkedDomainController DC01.users.contoso.com -LinkedCredential $ForeignCredential
```
