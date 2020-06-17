---
title: 'Create an organization relationship: Exchange 2013 Help'
TOCTitle: Create an organization relationship
ms:assetid: 5ea61b96-c8ca-44fc-b8b5-ca4341af36a6
ms:mtpsurl: https://technet.microsoft.com/library/JJ657451(v=EXCHG.150)
ms:contentKeyID: 49289267
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an organization relationship

_**Applies to:** Exchange Server 2013_

Set up an organization relationship to share calendar information with an external business partner. You can configure an organization relationship between two federated Exchange 2013 organizations or between a federated Exchange 2013 organization and federated Exchange 2010 organizations. You can also set up an organization relationship between your on-premises Exchange organization and an Office 365 organization.

> [!IMPORTANT]
> Creating an organization relationship is one of several steps in setting up federated sharing in your Exchange organization and requires the configuration of a federation trust for your on-premises Exchange organization.

To learn more about federated sharing, see [Sharing](sharing-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Calendar and Sharing Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- An active federation trust for the on-premises Exchange organization must be configured. For details, see [Configure a federation trust](configure-a-federation-trust-exchange-2013-help.md).

- The external organization you want to configure in the organization relationship must also have a federation trust established with the Azure AD authentication system. You'll use the primary federated domain for the external Exchange organization when configuring the organization relationship.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

## Use the EAC to create an organization relationship

1. On an Exchange 2013 server in your on-premises organization, navigate to **organization** \> **sharing**.

2. Under **Organization Sharing**, click **New** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

3. In **new organization relationship**, in the **Relationship name** box, type a friendly name for the organization relationship.

4. In the **Domains to share with** box, type the federated domain or federated subdomain for the Office 365 or Exchange on-premises organization you want to let see your calendars. If you need to enter multiple domains for the external organization, separate the domains with a comma. For example, **contoso.com, service.contoso.com**.

5. Select the **Enable calendar free/busy information sharing** check box to turn on calendar sharing with the domains you listed. Set the sharing level for calendar free/busy information and set which users can share calendar free/busy information.

    To set the free/busy access level, select one of the following:

    - **Calendar free/busy information with time only**

    - **Calendar free/busy with time, subject, and location**

    To set which users will share calendar free/busy information, select one of the following:

    - **Everyone in your organization**

    - **A specified security group**

        To specify a security group, click **browse**.

6. Click **save** to create the organization relationship.

## Use the Shell to create an organization relationship

This example creates an organization relationship with Contoso, Ltd with the following conditions:

- The organization relationship is enabled for contoso.com, northamerica.contoso.com, and europe.contoso.com.

- Free/busy access is enabled.

- The requesting organization receives free/busy time, subject, and location information from the target organization.

```powershell
New-OrganizationRelationship -Name "Contoso" -DomainNames "contoso.com","northamerica.contoso.com","europe.contoso.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails
```

This example attempts to automatically discover configuration information from the external Exchange organization Contoso.com by using the domain names provided in the **Get-FederationInformation** cmdlet. If you use this method to create your organization relationship, you must first make sure that you've created an organization identifier by using the **Set-FederatedOrganizationIdentifier** cmdlet.

```powershell
Get-FederationInformation -DomainName Contoso.com | New-OrganizationRelationship -Name "Contoso" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel LimitedDetails
```

For detailed syntax and parameter information, see [Get-FederationInformation](https://docs.microsoft.com/powershell/module/exchange/Get-FederationInformation) and [New-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/New-OrganizationRelationship).

This example creates an organization relationship with Fourth Coffee. In this example, the connection settings with the external Exchange organization are provided. The following conditions apply:

- The organization relationship is established with the domain fourthcoffee.com, a federated domain of Fourth Coffee.

- The Exchange Web Services application URL is mail.fourthcoffee.com.

- The Autodiscover URL is https://mail.fourthcoffee.com/autodiscover/autodiscover.svc/wssecurity.

- Free/busy access is enabled.

- The requesting organization receives only free/busy information with the time.

```powershell
New-OrganizationRelationship -Name "Fourth Coffee" -DomainNames "fourthcoffee.com" -FreeBusyAccessEnabled $true -FreeBusyAccessLevel AvailabilityOnly -TargetAutodiscoverEpr "https://mail.fourthcoffee.com/autodiscover/autodiscover.svc/wssecurity" -TargetApplicationUri "mail.fourthcoffee.com"
```

For detailed syntax and parameter information, see [New-OrganizationRelationship](https://docs.microsoft.com/powershell/module/exchange/New-OrganizationRelationship).

## How do you know this worked?

The successful completion of the **New organization relationship** wizard will be your first indication that the creation of the organization relationship worked as expected.

To further verify that you have successfully created the organization relationship, run the following Shell command to verify the organization relationship information:

```powershell
Get-OrganizationRelationship | format-list
```

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).
