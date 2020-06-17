---
localization_priority: Normal
description: An organization relationship lets users in your Microsoft 365 or Office 365 organization share calendar free/busy information with other Office 365 or on-premises Exchange organizations. You may want to change the settings of an organization relationship, such as changing the name, temporarily disabling calendar sharing, changing the access level, or changing which security groups will share calendars.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6875cd79-41d4-422c-80a7-549ded9a1ea7
ms.reviewer: 
f1.keywords:
- NOCSH
title: Modify an organization relationship in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Modify an organization relationship in Exchange Online

An organization relationship lets users in your Microsoft 365 or Office 365 organization share calendar free/busy information with other Office 365 or on-premises Exchange organizations. You may want to change the settings of an organization relationship, such as changing the name, temporarily disabling calendar sharing, changing the access level, or changing which security groups will share calendars.

To learn more about organization relationships, see [Organization relationships in Exchange Online](organization-relationships.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the [Permissions in Exchange Online](../../permissions-exo/permissions-exo.md) topic.

- If you want to share calendars with an on-premises Exchange organization, the on-premises Exchange administrator has to set up an authentication relationship with the cloud (also known as "federation") and must meet minimum software requirements.

- The procedures in this topic make changes to an organization relationship named Contoso. The examples show how to:

  - Add a domain named service.contoso.com to the organization relationship.

  - Disable free/busy sharing for the organization relationship.

  - Change the free/busy access level from _Calendar free/busy information with time, subject, and location_ to _Calendar free/busy information with time only_.

## Use the Exchange admin center to add a domain to an organization relationship
<a name="BKMK_EAC"> </a>

1. From the Microsoft 365 admin center go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. In list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **organization relationship**, **general** don't change the **Name** for the organization relationship.

5. In the **Domains to share with** box, enter the domain service.contoso.com, then click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

6. Click **save** to update the organization relationship.

## Use the Exchange admin center to disable free/busy sharing for the organization relationship
<a name="BKMK_EAC2"> </a>

1. From the Microsoft 365 admin center go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. In the list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **organization relationship** click **sharing**.

5. Clear the **Enable calendar free/busy information sharing** check box to disable free/busy sharing. The free/busy access level and security group buttons will also be disabled.

6. Click **save** to update the organization relationship.

## Use the Exchange admin center to change the free/busy access level for the organization relationship
<a name="BKMK_EAC3"> </a>

1. From the Microsoft 365 admin center go to **Admin** \> **Exchange**.

2. Go to **organization** \> **sharing**.

3. In list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

4. In **organization relationship**, click **sharing**

5. Select **Calendar free/busy information with time only**.

6. Click **save** to update the organization relationship.

## Use Exchange Online PowerShell to modify the organization relationship
<a name="BKMK_Shell"> </a>

- This example adds the domain name service.contoso.com to the organization relationship Contoso.

  ```PowerShell
  $domains = (Get-OrganizationRelationship Contoso).DomainNames
  $domains += 'service.contoso.com'
  Set-OrganizationRelationship -Identity Contoso -DomainNames $domains
  ```

- This example disables the organization relationship Contoso.

  ```PowerShell
  Set-OrganizationRelationship -Identity Contoso -Enabled $false
  ```

- This example enables calendar availability information access for the organization relationship WoodgroveBank and sets the access level to `AvailabilityOnly` (calendar free/busy information with time only).

  ```PowerShell
  Set-OrganizationRelationship -Identity Contoso -FreeBusyAccessEnabled $true -FreeBusyAccessLevel AvailabilityOnly

  ```

For detailed syntax and parameter information, see [Get-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/get-organizationrelationship) and [Set-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/set-organizationrelationship).

## How do you know this worked?

To verify that you have successfully updated the organization relationship, run the following command and verify the organization relationship information.

```PowerShell
Get-OrganizationRelationship | format-list
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
