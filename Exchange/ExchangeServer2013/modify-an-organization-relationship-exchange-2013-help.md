---
title: 'Modify an organization relationship: Exchange 2013 Help'
TOCTitle: Modify an organization relationship
ms:assetid: 3713ef83-f01a-41bb-b127-62ca242dd7a4
ms:mtpsurl: https://technet.microsoft.com/library/JJ673055(v=EXCHG.150)
ms:contentKeyID: 49289228
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Modify an organization relationship

_**Applies to:** Exchange Server 2013_

An organization relationship lets users in your Exchange organization share calendar free/busy information with a Microsoft 365 or Office 365 organization or another on-premises Exchange organization. You may want to change the settings of an organization relationship, such as changing the name, temporarily disabling calendar sharing, changing the access level, or changing which security groups will share calendars.

Before you can share calendars with another organization, you have to set up an authentication relationship with the cloud (also known as "federation") and must meet minimum software requirements. To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

For additional management tasks related to federation, see [Federation procedures](federation-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the *Calendar and Sharing Permissions* entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- An active federation trust for the on-premises Exchange organization must be configured.

- The external organization you want to configure in the organization relationship must also have a federation trust established with the Azure AD authentication system.

- The procedures in this topic make changes to an organization relationship named Contoso. The examples show how to:

  - Add a domain named service.contoso.com to the external organization.

  - Disable free/busy sharing for the organization relationship.

  - Change the free/busy access level from *Calendar free/busy information with time, subject, and location* to *Calendar free/busy information with time only*.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to add a domain to an organization relationship

1. On an Exchange 2013 server in your on-premises organization, navigate to **organization** \> **sharing**.

2. In list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **organization relationship**, **general** don't change the **Name** for the organization relationship

4. In the **Domains to share with** box, enter the domain **service.contoso.com**, then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

5. Click **save** to update the organization relationship.

## Use the EAC to disable free/busy sharing for the organization relationship

1. Go to **organization** \> **sharing**.

2. In list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **organization relationship**, click **sharing**

4. Select **Calendar free/busy information with time only**.

5. Click **save** to update the organization relationship.

## Use the EAC to change the free/busy access level for the organization relationship

1. Go to **organization** \> **sharing**.

2. In list view, under **Organization Sharing**, select the organization relationship Contoso, and then click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. In **organization relationship**, click **sharing**

4. Select **Calendar free/busy information with time only**.

5. Click **save** to update the organization relationship.

## Use the Shell to modify the organization relationship

- This example adds the domain name service.contoso.com to the organization relationship Contoso.

  ```powershell
  $domains = (Get-OrganizationRelationship Contoso).DomainNames
  $domains += 'service.contoso.com'
  Set-OrganizationRelationship -Identity Contoso -DomainNames $domains
  ```

- This example disables the organization relationship Contoso.

  ```powershell
  Set-OrganizationRelationship -Identity Contoso -Enabled $false
  ```

- This example enables calendar availability information access for the organization relationship WoodgroveBank and sets the access level to `AvailabilityOnly` (calendar free/busy information with time only).

  ```powershell
  Set-OrganizationRelationship -Identity Contoso -FreeBusyAccessEnabled $true -FreeBusyAccessLevel AvailabilityOnly
  ```

For detailed syntax and parameter information, see [Get-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/Get-OrganizationRelationship) and [Set-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/Set-OrganizationRelationship).

## How do you know this worked?

To verify that you have successfully updated the organization relationship, run the following Shell command and verify the organization relationship information.

```powershell
Get-OrganizationRelationship | format-list
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
