---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: eb39cfa7-7c76-4403-a2f5-403354ebb7ae
ms.reviewer: 
description: When you migrate on-premises Exchange mailboxes to Office 365, certain permissions to access and, in some cases, modify those mailboxes, are required. The user account used to connect to your on-premises Exchange organization during the migration needs those permissions. Known as the migration administrator, the user account is used to create a migration endpoint to your on-premises organization.
title: Assign Exchange permissions to migrate mailboxes to Office 365
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Assign Exchange permissions to migrate mailboxes to Office 365

When you migrate on-premises Exchange mailboxes to Office 365, certain permissions to access and, in some cases, modify those mailboxes, are required. The user account used to connect to your on-premises Exchange organization during the migration needs those permissions. Known as the migration administrator, the user account is used to create a migration endpoint to your on-premises organization.

The migration administrator must have the necessary administrative privileges in your on-premises Exchange organization to successfully create a migration endpoint. Those same administrative privileges are required if the migration administrator wants to create a migration batch if your organization has no migration endpoints. The following list shows the administrative privileges required for the migration administrator account to migrate mailboxes to Office 365 by using the different types of migration:

- **Staged Exchange migration**

    For a staged migration, the migration administrator account must be:

  - A member of the Domain Admins group in Active Directory Domain Services (AD DS) in the on-premises organization.

    or

  - Assigned the FullAccess permission for each on-premises mailbox AND the WriteProperty permission to modify the _TargetAddress_ property on the on-premises user account.

    or

  - Assigned the Receive As permission on the on-premises mailbox database that stores the user mailboxes AND the WriteProperty permission to modify the _TargetAddress_ property for the on-premises user account.

- **Cutover Exchange migration**

    For a cutover migration, the migration administrator account must be:

  - A member of the Domain Admins group in Active Directory Domain Services (AD DS) in the on-premises organization.

    or

  - Assigned the FullAccess permission for each on-premises mailbox.

    or

  - Assigned the Receive As permission on the on-premises mailbox database that stores the user mailboxes.

- **Internet Message Access Protocol 4 (IMAP4) migration**

    For an IMAP4 migration, the comma-separated value (.csv) file for the migration batch must contain:

  - The username and password for each mailbox that you want to migrate.

    or

  - The username and password for an account in your IMAP4 messaging system that has the necessary administrative privileges to access all user mailboxes. To learn whether your IMAP4 server supports this approach and how to enable it, see the documentation for your IMAP4 server.

You can use Exchange Online PowerShell in your on-premises organization to quickly assign the necessary permissions to migrate mailboxes to Office 365.

> [!NOTE]
> Because Exchange Server 2003 doesn't support Exchange Online PowerShell, you have to use Active Directory Users and Computers to assign the FullAccess permission and Exchange Server Manager to assign the Receive As permission.

For information about migrating mailboxes to Office 365 by using different migration types, see [Ways to migrate multiple email accounts to Office 365](mailbox-migration.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Permissions and delegation" entry in the "Recipient Provisioning Permissions" entry in the [Feature permissions in Exchange Online](../permissions-exo/feature-permissions.md) topic.

## Assign the FullAccess permission
<a name="bkmk_fullaccess"> </a>

The following examples show different ways to use the Exchange Online PowerShell **Add-MailboxPermission** cmdlet to assign the FullAccess permission to the migration administrator account for mailboxes in your on-premises organization.

 **Example 1**

FullAccess permission to the mailbox of Terry Adams is assigned to the migration administrator account (for example, migadmin).

```PowerShell
Add-MailboxPermission -Identity "Terry Adams" -User migadmin -AccessRights FullAccess -InheritanceType all
```

 **Example 2**

FullAccess permission for all members of the distribution group MigrationBatch1 is assigned to the migration administrator account.

```PowerShell
Get-DistributionGroupMember MigrationBatch1 | Add-MailboxPermission -User migadmin -AccessRights FullAccess -InheritanceType all
```

 **Example 3**

FullAccess permission for all mailboxes that have the value of `MigBatch2` for _CustomAttribute10_ is assigned to the migration administrator.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "CustomAttribute10 -eq 'MigBatch2'" | Add-MailboxPermission -User migadmin -AccessRights FullAccess -InheritanceType all
```

 **Example 4**

FullAccess permission to all user mailboxes in the on-premises organization is assigned to the migration administrator account.

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Add-MailboxPermission -User migadmin -AccessRights FullAccess -InheritanceType all
```

For detailed syntax and parameter information, see the following topics:

- [Add-MailboxPermission](https://docs.microsoft.com/powershell/module/exchange/Add-MailboxPermission)

- [Filterable Properties for the -Filter Parameter](https://go.microsoft.com/fwlink/p/?LinkId=620739)

### How do you know the assignment of permission worked?

Run one of the following commands to verify you successfully assigned FullAccess permission to the migration administrator account in each example.

```PowerShell
Get-MailboxPermission -Identity <mailbox> -User migadmin
```

```PowerShell
Get-DistributionGroupMember MigrationBatch1 | Get-MailboxPermission -User migadmin
```

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "CustomAttribute10 -eq 'MigBatch2'" | Get-MailboxPermission -User migadmin
```

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Get-MailboxPermission -User migadmin
```

## Assign the Receive As permission
<a name="bkmk_fullaccess"> </a>

The following example shows how to use the Exchange Online PowerShell **Add-ADPermission** cmdlet to assign the Receive As permission to the migration administrator account for "Mailbox Database 1900992314."

```PowerShell
Add-ADPermission -Identity "Mailbox Database 1900992314" -User migadmin -ExtendedRights receive-as
```

For detailed syntax and parameter information, see [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/Add-ADPermission).

### How do you know the assignment of permission worked?

Verify you successfully assigned ReceiveAs permission to the migration administrator account in the example. Run the following command.

```PowerShell
Get-ADPermission -Identity "Mailbox Database 1900992314" -User migadmin
```

## Assign the WriteProperty permission
<a name="bkmk_fullaccess"> </a>

The following examples show different ways to use the Exchange Online PowerShell **Add-ADPermission** cmdlet to assign the migration administrator account the WriteProperty permission to modify the _TargetAddress_ property for on-premises user accounts. This capability is required to perform a staged Exchange migration if the migration administrator isn't a member of the Domain Admins group.

 **Example 1**

WriteProperty permission to modify the _TargetAddress_ property for the user account of Rainer Witte is assigned to the migration administrator account (for example, migadmin).

```PowerShell
Add-ADPermission -Identity "Rainer Witte" -User migadmin -AccessRights WriteProperty -Properties TargetAddress
```

 **Example 2**

WriteProperty permission to modify the _TargetAddress_ property for all members of the distribution group StagedBatch1 is assigned to the migration administrator account.

```PowerShell
Get-DistributionGroupMember StagedBatch1 | Add-ADPermission User migadmin -AccessRights WriteProperty -Properties TargetAddress
```

 **Example 3**

WriteProperty permission to modify the _TargetAddress_ property for all user accounts that have the value of `StagedMigration` for _CustomAttribute15_ is assigned to the migration administrator account.

```PowerShell
Get-User -ResultSize unlimited -Filter "CustomAttribute15 -eq 'StagedMigration'" | Add-ADPermission -User migadmin -AccessRights WriteProperty -Properties TargetAddress
```

 **Example 4**

WriteProperty permission to modify the _TargetAddress_ property for user mailboxes in the on-premises organization is assigned to the migration administrator account.

```PowerShell
Get-User -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Add-ADPermission -User migadmin -AccessRights WriteProperty -Properties TargetAddress
```

For detailed syntax and parameter information, see the following topics:

- [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/Add-ADPermission)

- [Filterable Properties for the -Filter Parameter](https://go.microsoft.com/fwlink/p/?LinkId=620739)

### How do you know the assignment of permission worked?

Verify you successfully assigned the WriteProperty permission to the administrator account, Run one of the following commands to confirm the permission was given to modify the TargetAddress property by using the command in each example.

```PowerShell
Get-ADPermission -Identity <mailbox> -User migadmin
```

```PowerShell
Get-DistributionGroupMember MigrationBatch1 | Get-ADPermission -User migadmin
```

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "CustomAttribute15 -eq 'StagedMigration'" | Get-MailboxPermission -User migadmin
```

```PowerShell
Get-Mailbox -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'UserMailbox'" | Get-ADPermission -User migadmin
```
