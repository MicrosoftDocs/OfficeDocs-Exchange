---
localization_priority: Normal
description: 'Summary: Learn how installing Exchange 2016 or Exchange 2019 affects Active Directory.'
ms.topic: conceptual
author: chrisda
ms.author: chrisda
ms.assetid: 07386078-6103-49a2-8698-2d41db9cec95
ms.date: 9/20/2018
ms.reviewer: 
title: What changes in Active Directory when Exchange is installed?
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# What changes in Active Directory when Exchange is installed?

When you install Exchange Server 2016 or Exchange Server 2019, changes are made to your Active Directory forest and domains to store information about the Exchange servers, mailboxes, and other Exchange-related objects in your organization.

Three steps are required to prepare Active Directory for Exchange:

1. [Extend the Active Directory schema](#extend-the-active-directory-schema)

2. [Prepare Active Directory containers, objects, and other items](#prepare-active-directory-containers-objects-and-other-items)

3. [Prepare Active Directory domains](#prepare-active-directory-domains)

After all three steps are done, your Active Directory forest is ready for Exchange. This topic explains what Exchange does at each step of Active Directory preparation.

You can make these changes before you install the first Exchange 2016 or Exchange 2019 server in the organization by running the _/PrepareSchema_, _/PrepareAD_, and _/PrepareAllDomains_ or _/PrepareDomains_ commands using Exchange command line Setup. For instructions, see [Prepare Active Directory and domains for Exchange](../prepare-ad-and-domains.md). Or, these changes are automatically made for you during the installation of the first Exchange server using the Exchange Setup wizard. For instructions, see [Install Exchange Mailbox servers using the Setup wizard](../../plan-and-deploy/deploy-new-installations/install-mailbox-role.md).


## Extend the Active Directory schema

Extending the Active Directory schema adds and updates classes, attributes, and other items. These changes are needed so that Exchange can create containers and objects to store information about the Exchange organization. Because Exchange makes a lot of changes to the Active Directory schema, there's a topic dedicated to this step. To see all of the changes made to the schema, see [Active Directory schema changes in Exchange Server](ad-schema-changes.md).

After the schema has been extended by running the _/PrepareSchema_ command, the _/PrepareAD command, or installing the first Exchange server using the Exchange Setup wizard, the schema version is set in the **ms-Exch-Schema-Version-Pt** attribute. To verify that the Active Directory schema was extended successfully, you can check the value stored in this attribute. For more information, see [Exchange Active Directory versions](../prepare-ad-and-domains.md#exchange-active-directory-versions).

## Prepare Active Directory containers, objects, and other items

With the schema extended, the next step is to add all of the containers, objects, attributes, and other items that Exchange uses to store information in Active Directory. Most of the changes made in this step are applied to the entire Active Directory forest. A smaller set of changes are made only to the local Active Directory domain where the _/PrepareAD_ command was run (or where the first Exchange server was installed using the Exchange Setup wizard).

Exchange makes the following changes to the Active Directory forest:

- The Microsoft Exchange container is created under CN=Services,CN=Configuration,DC=\<_root domain_\> if it doesn't already exist.

- The following containers and objects are created under CN=\<_organization name_\>,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<_root domain_\> if they don't already exist:

  - CN=Address Lists Container

  - CN=AddressBook Mailbox Policies

  - CN=Addressing

  - CN=Administrative Groups

  - CN=Approval Applications

  - CN=Auth Configuration

  - CN=Availability Configuration

  - CN=Client Access

  - CN=Connections

  - CN=ELC Folders Container

  - CN=ELC Mailbox Policies

  - CN=ExchangeAssistance

  - CN=Federation

  - CN=Federation Trusts

  - CN=Global Settings

  - CN=Hybrid Configuration

  - CN=Mobile Mailbox Policies

  - CN=Mobile Mailbox Settings

  - CN=Monitoring Settings

  - CN=OWA Mailbox Policies

  - CN=Provisioning Policy Container

  - CN=Push Notification Settings

  - CN=RBAC

  - CN=Recipient Policies

  - CN=Remote Accounts Policies Container

  - CN=Retention Policies Container

  - CN=Retention Policy Tag Container

  - CN=ServiceEndpoints

  - CN=System Policies

  - CN=Team Mailbox Provisioning Policies

  - CN=Transport Settings

  - CN=UM AutoAttendant Container (Exchange 2016 only)

  - CN=UM DialPlan Container (Exchange 2016 only)

  - CN=UM IPGateway Container (Exchange 2016 only)

  - CN=UM Mailbox Policies (Exchange 2016 only)

  - CN=Workload Management Settings

- The following containers and objects are created under CN=Transport Settings,CN=\<_Organization Name_\>,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<_root domain_\> if they don't already exist:

  - CN=Accepted Domains

  - CN=ControlPoint Config

  - CN=DNS Customization

  - CN=Interceptor Rules

  - CN=Malware Filter

  - CN=Message Classifications

  - CN=Message Hygiene

  - CN=Rules

  - CN=MicrosoftExchange329e71ec88ae4615bbc36ab6ce41109e

- Permissions are set throughout the configuration partition in Active Directory.

- The Rights.ldf file is imported. This file adds permissions that are needed to install Exchange and configure Active Directory.

- The Microsoft Exchange Security Groups organizational unit (OU) is created in the root domain of the forest, and permissions are assigned to it.

- The following groups are created within the Microsoft Exchange Security Groups OU if they don't already exist:

  - Compliance Management

  - Delegated Setup

  - Discovery Management

  - Exchange Servers

  - Exchange Trusted Subsystem

  - Exchange Windows Permissions

  - ExchangeLegacyInterop

  - Help Desk

  - Hygiene Management

  - Managed Availability Servers

  - Organization Management

  - Public Folder Management

  - Recipient Management

  - Records Management

  - Server Management

  - View-Only Organization Management

- The new management role groups (which appear as universal security groups (USGs) in Active Directory) that were created in the Microsoft Exchange Security Groups OU are added to the **otherWellKnownObjects** attribute stored on the CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=\<_root domain_\> container.

- In Exchange 2016 only, the Unified Messaging Voice Originator contact is created in the Microsoft Exchange System Objects container of the root domain.

- Only the domain where the _/PrepareAD_ command was run (or where the first Exchange server was installed using the Exchange Setup wizard) is prepared for Exchange. For information about what's done to prepare an Active Directory domain for Exchange, see the next section.

## Prepare Active Directory domains

The final step of preparing Active Directory for Exchange is to prepare the Active Directory domains where Exchange servers will be installed or where mailbox-enabled users will be located (all domains in the forest using the _/PrepareAllDomains_ command, specific domains using the _/PrepareDomains_ command, or installing the first Exhange server using the Exchange Setup Wizard). This step is done automatically in the domain where the _PrepareAD_ command was run (or where the first Exchange server was installed using the Exchange Setup wizard).

Exchange makes the follwing changes to the Active Directory domains:

- The Microsoft Exchange System Objects container is created in the root domain partition in Active Directory if it doesn't already exist.

- Permissions are set on the Microsoft Exchange System Objects container for the Exchange Servers, Organization Management, and Authenticated Users security groups.

- The Exchange Install Domain Servers domain global group is created in the current domain and placed in the Microsoft Exchange System Objects container.

- The Exchange Install Domain Servers group is added to the Exchange Servers USG in the root domain.

- Permissions are assigned at the domain level for the Exchange Servers USG and the Organization Management USG.

- The **objectVersion** property in the Microsoft Exchange System Objects container under DC=\<_root domain_\> is set. To verify that the Active Directory domains were successfully prepared, you can check the value stored in this attribute. For more information, see [Exchange Active Directory versions](../prepare-ad-and-domains.md#exchange-active-directory-versions).
