---
title: 'Configure Exchange 2013 for split permissions: Exchange 2013 Help'
TOCTitle: Configure Exchange 2013 for split permissions
ms:assetid: 8c74f893-a6f3-4869-8571-3bc0f662cc87
ms:mtpsurl: https://technet.microsoft.com/library/Dd638155(v=EXCHG.150)
ms:contentKeyID: 49289342
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Exchange 2013 for split permissions

_**Applies to:** Exchange Server 2013_

Split permissions enable two separate groups, such as Active Directory administrators and Microsoft Exchange Server 2013 administrators, to manage their respective services, objects, and attributes. Active Directory administrators manage security principals, such as users, that provide permissions to access an Active Directory forest. Exchange administrators manage the Exchange-related attributes on Active Directory objects and Exchange-specific object creation and management.

Microsoft Exchange Server 2013 offers the following types of split permissions models:

- **RBAC split permissions**: Permissions to create security principals in the Active Directory domain partition are controlled by Role Based Access Control (RBAC). Only those who are members of the appropriate role groups can create security principals.

- **Active Directory split permissions**: Permissions to create security principals in the Active Directory domain partition are completely removed from any Exchange user, service, or server. No option is provided in RBAC to create security principals. Creation of security principals in Active Directory must be performed using Active Directory management tools.

    > [!NOTE]
    > Active Directory split permissions are available in organizations running Microsoft Exchange Server 2010 Service Pack 1 (SP1) or later, Exchange 2013, or both versions of Exchange.

The model that you choose depends on the structure and needs of your organization. Choose the procedure that follows that's applicable to the model you want to configure. We recommend that you use the RBAC split permissions model. The RBAC split permissions model provides significantly more flexibility while providing the same administration separation as Active Directory split permissions.

For more information about shared and split permissions, see [Understanding split permissions](understanding-split-permissions-exchange-2013-help.md).

For more information about management role groups, management roles, and regular and delegating management role assignments, see the following topics:

- [Understanding Role Based Access Control](understanding-role-based-access-control-exchange-2013-help.md)

- [Understanding management role groups](understanding-management-role-groups-exchange-2013-help.md)

- [Understanding management roles](understanding-management-roles-exchange-2013-help.md)

- [Understanding management role assignments](understanding-management-role-assignments-exchange-2013-help.md)

Looking for other management tasks related to permissions? Check out [Advanced permissions](advanced-permissions-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Active Directory split permissions" entry in the [Role management permissions](role-management-permissions-exchange-2013-help.md) topic.

- You must use Windows PowerShell, Windows Command Shell, or both, to perform these procedures. For more information, see each procedure.

- If you have Exchange 2010 servers in your organization, the permissions model you select will also be applied to those servers.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Switch to RBAC split permissions

You can configure your Exchange 2013 organization for RBAC split permissions. When you are done, only Active Directory administrators will be able to create Active Directory security principals. This means that Exchange administrators won't be able to use the following cmdlets:

- **New-Mailbox**

- **New-MailContact**

- **New-MailUser**

- **New-RemoteMailbox**

- **Remove-Mailbox**

- **Remove-MailContact**

- **Remove-MailUser**

- **Remove-RemoteMailbox**

Exchange administrators will only be able to manage the Exchange attributes on existing Active Directory security principals. However, They will be able to create and manage Exchange-specific objects, such as transport rules and distribution groups. For more information, see the "RBAC Split Permissions" section in [Understanding split permissions](understanding-split-permissions-exchange-2013-help.md).

To configure Exchange 2013 for split permissions, you must assign the Mail Recipient Creation role and the Security Group Creation and Membership role to a role group that contains members that are Active Directory administrators. You must then remove the assignments between those roles and any role group or universal security group (USG) that contains Exchange administrators.

To configure RBAC split permissions, do the following:

1. If your organization is currently configured for Active Directory split permissions, do the following from a Windows command shell prompt.

    1. Disable Active Directory split permissions by running the following command from the Exchange 2013 installation media.

       ```powershell
       setup.exe /PrepareAD /ActiveDirectorySplitPermissions:false
       ```

    2. Restart the Exchange 2013 servers in your organization or wait for the Active Directory access token to replicate to all of your Exchange 2013 servers.

       > [!NOTE]
       > If you have Exchange 2010 servers in your organization, you also need to restart those servers.

2. Do the following from the Exchange Management Shell:

    1. Create a role group for the Active Directory administrators. In addition to creating the role group, the command creates regular role assignments between the new role group and the Mail Recipient Creation role and the Security Group Creation and Membership role.

       ```powershell
       New-RoleGroup "Active Directory Administrators" -Roles "Mail Recipient Creation", "Security Group Creation and Membership"
       ```

        > [!NOTE]
        > If you want members of this role group to be able to create role assignments, include the Role Management role. You don't have to add this role now. However, if you ever want to assign either the Mail Recipient Creation role or Security Group Creation and Membership role to other role assignees, the Role Management role must be assigned to this new role group. The steps that follow configure the Active Directory Administrators role group as the only role group that can delegate these roles.

    2. Create delegating role assignments between the new role group and the Mail Recipient Creation role and Security Group Creation and Membership role using the following commands.

       ```powershell
       New-ManagementRoleAssignment -Role "Mail Recipient Creation" -SecurityGroup "Active Directory Administrators" -Delegating
       New-ManagementRoleAssignment -Role "Security Group Creation and Membership" -SecurityGroup "Active Directory Administrators" -Delegating
        ```

    3. Add members to the new role group using the following command.

       ```powershell
       Add-RoleGroupMember "Active Directory Administrators" -Member <user to add>
       ```

    4. Replace the delegate list on the new role group so that only members of the role group can add or remove members.

       ```powershell
       Set-RoleGroup "Active Directory Administrators" -ManagedBy "Active Directory Administrators"
       ```

        > [!IMPORTANT]
        > Members of the Organization Management role group, or those who are assigned the Role Management role, either directly or through another role group or USG, can bypass this delegate security check. If you want to prevent any Exchange administrator from adding himself or herself to the new role group, you must remove the role assignment between the Role Management role and any Exchange administrator and assign it to another role group.

    5. Find all of the regular and delegating role assignments to the Mail Recipient Creation role using the following command. The command displays only the **Name**, **Role**, and **RoleAssigneeName** properties.

       ```powershell
       Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Format-Table Name, Role, RoleAssigneeName -Auto
       ```

    6. Remove all of the regular and delegating role assignments to the Mail Recipient Creation role that aren't associated with the new role group or any other role groups, USGs, or direct assignments you want to keep using the following command.

       ```powershell
       Remove-ManagementRoleAssignment <Mail Recipient Creation role assignment to remove>
       ```

       > [!NOTE]
       > If you want to remove all of the regular and delegating role assignments to the Mail Recipient Creation role on any role assignee other than the Active Directory Administrators role group, use the following command. The <EM>WhatIf</EM> switch lets you see what role assignments will be removed. Remove the <EM>WhatIf</EM> switch and run the command again to remove the role assignments.

       ```powershell
       Get-ManagementRoleAssignment -Role "Mail Recipient Creation" | Where {$_.RoleAssigneeName -NE "Active Directory Administrators"} | Remove-ManagementRoleAssignment -WhatIf
       ```

    7. Find all of the regular and delegating role assignments to the Security Group Creation and Membership role using the following command. The command displays only the **Name**, **Role**, and **RoleAssigneeName** properties.

       ```powershell
       Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Format-Table Name, Role, RoleAssigneeName -Auto
       ```

    8. Remove all of the regular and delegating role assignments to the Security Group Creation and Membership role that aren't associated with the new role group or any other role groups, USGs, or direct assignments you want to keep using the following command.

       ```powershell
       Remove-ManagementRoleAssignment <Security Group Creation and Membership role assignment to remove>
       ```

       > [!NOTE]
       > You can use the same command in the preceding Note to remove all of the regular and delegating role assignments to the Security Group Creation and Membership role on any role assignee other than the Active Directory Administrators role group, as shown in this example.

       ```powershell
       Get-ManagementRoleAssignment -Role "Security Group Creation and Membership" | Where {$_.RoleAssigneeName -NE "Active Directory Administrators"} | Remove-ManagementRoleAssignment -WhatIf
       ```

For detailed syntax and parameter information, see the following topics:

- [New-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/New-RoleGroup)

- [New-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/New-ManagementRoleAssignment)

- [Add-RoleGroupMember](https://docs.microsoft.com/powershell/module/exchange/Add-RoleGroupMember)

- [Set-RoleGroup](https://docs.microsoft.com/powershell/module/exchange/Set-RoleGroup)

- [Get-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Get-ManagementRoleAssignment)

- [Remove-ManagementRoleAssignment](https://docs.microsoft.com/powershell/module/exchange/Remove-ManagementRoleAssignment)

## Switch to Active Directory split permissions

You can configure your Exchange 2013 organization for Active Directory split permissions. Active Directory split permissions completely remove the permissions that allow Exchange administrators and servers from creating security principals in Active Directory or modifying non-Exchange attributes on those objects. When you are done, only Active Directory administrators will be able to create Active Directory security principals. This means that Exchange administrators won't be able to use the following cmdlets:

- **Add-DistributionGroupMember**

- **New-DistributionGroup**

- **New-Mailbox**

- **New-MailContact**

- **New-MailUser**

- **New-RemoteMailbox**

- **Remove-DistributionGroup**

- **Remove-DistributionGroupMember**

- **Remove-Mailbox**

- **Remove-MailContact**

- **Remove-MailUser**

- **Remove-RemoteMailbox**

- **Update-DistributionGroupMember**

Exchange administrators and servers will only be able to manage the Exchange attributes on existing Active Directory security principals. However, they will be able to create and manage Exchange-specific objects, such as transport rules and Unified Messaging dial plans.

> [!WARNING]
> After you enable Active Directory split permissions, Exchange administrators and servers will no longer be able to create security principals in Active Directory, and they won't be able to manage distribution group membership. These tasks must be performed using Active Directory management tools with the required Active Directory permissions. Before you make this change, you should understand the impact it will have on your administration processes and third-party applications that integrate with Exchange 2013 and the RBAC permissions model.<BR>For more information, see the "Active Directory split permissions" section in <A href="understanding-split-permissions-exchange-2013-help.md">Understanding split permissions</A>.

To switch from shared or RBAC split permissions to Active Directory split permissions, do the following:

1. From a Windows command shell, run the following command from the Exchange 2013 installation media to enable Active Directory split permissions.

   ```powershell
   setup.exe /PrepareAD /ActiveDirectorySplitPermissions:true
   ```

2. If you have multiple Active Directory domains in your organization, you must either run `setup.exe /PrepareDomain` in each child domain that contains Exchange servers or objects or run `setup.exe /PrepareAllDomains` from a site that has an Active Directory server from every domain.

3. Restart the Exchange 2013 servers in your organization or wait for the Active Directory access token to replicate to all of you Exchange 2013 servers.

   > [!NOTE]
   > If you have Exchange 2010 servers in your organization, you also need to restart those servers.
