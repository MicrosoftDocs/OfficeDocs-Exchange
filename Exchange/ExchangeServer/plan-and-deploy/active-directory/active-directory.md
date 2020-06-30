---
localization_priority: Priority
description: 'Summary: Learn how Exchange 2016 and Exchange 2019 interact with Active Directory.'
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8e8464df-2d1d-4d68-82de-b0c158c549c3
ms.reviewer: 
title: Active Directory in Exchange organizations
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Active Directory in Exchange Server organizations

Exchange Server 2016 and Exchange Server 2019 use Active Directory to store and share directory information with Windows. Starting with Exchange 2013, we've made some changes to how Exchange works with Active Directory. These changes are described in this topic.

## Active Directory driver

The Active Directory driver is the core Microsoft Exchange component that allows Exchange services to create, modify, delete, and query for Active Directory Domain Services (AD DS) data. In Exchange 2013 and later, all access to Active Directory is done using the Active Directory driver itself. In previous versions of Exchange, DSAccess provided directory lookup services for components such as SMTP, message transfer agent (MTA), and the Exchange store.

The Active Directory driver also uses the Microsoft Exchange Active Directory Topology (MSExchangeADTopology) server, which allows the Active Directory driver to use Directory Service Access (DSAccess) topology data. This data includes the list of available domain controllers and global catalog servers that are available to handle Exchange requests. For more information about the Active Directory Driver, see [Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/active-directory-domain-services).

## Active Directory schema changes

Exchange add new attributes to the Active Directory domain service schema and also make other modifications to existing classes and attributes. For more information about Active Directory changes when you install Exchange, see [Active Directory schema changes in Exchange Server](ad-schema-changes.md).

## For more information

To learn more about how Exchange stores and retrieves information in Active Directory so that you can plan for access to it, see [Access to Active Directory in Exchange Server](ad-access.md).

For more information about Active Directory forest design, see [AD DS Design Guide](https://go.microsoft.com/fwlink/p/?LinkId=264957).

To learn more about computers running Windows in an Active Directory domain and deploying Exchange 2013 or later in a domain that has a disjoint namespace, see [Disjoint Namespace Scenarios](https://docs.microsoft.com/exchange/disjoint-namespace-scenarios-exchange-2013-help).
