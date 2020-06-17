---
localization_priority: Normal
description: Setup can't continue because the organization contains one or more Exchange 2007 servers.
ms.topic: reference
author: mattpennathe3rd
ms.custom:
- ms.exch.setupreadiness.E16E12CoexistenceMinVersionRequirement
ms.author: v-mapenn
ms.assetid: 4e1b9510-3188-43eb-9252-7c64cb2bc0e3
ms.reviewer: 
title: Can't install Exchange 2016 or later in a forest that contains Exchange 2007 [E16E12CoexistenceMinVersionRequirement]
ms.collection: exchange-server
f1.keywords:
- CSH
audience: Developer
ms.prod: exchange-server-it-pro
manager: serdars

---

# Can't install Exchange 2016 or later in a forest that contains Exchange 2007 [E16E12CoexistenceMinVersionRequirement]

The installation of Exchange Server 2016 or later can't continue because Setup found one or more Exchange 2007 servers in the Active Directory forest. Before you can install Exchange 2016 or later in your organization, you need to remove all Exchange 2007 servers from the forest.

The upgrade steps from Exchange 2007 are:

1. Install Exchange 2013 into your Exchange 2007 organization.

2. Configure Exchange 2013 and Exchange 2007 coexistence.

3. Migrate Exchange 2007 mailboxes, public folders, and other components to Exchange 2013.

4. Decommission and remove all Exchange 2007 servers.

5. Install Exchange 2016 or Exchange 2019 into your Exchange 2013 organization.

6. Configure coexistence with Exchange 2013.

7. Migrate Exchange 2013 mailboxes, public folders, and other components to Exchange 2016 or Exchange 2019.

8. Decommission and remove all Exchange 2013 servers.

The coexistence (and therefore, upgrade) options for Exchange are described in the following table:

|**Current Exchange version**|**Latest Exchange version for coexistence**|
|:-----|:-----|:-----|
|Exchange 2000|Exchange 2007|
|Exchange 2003|Exchange 2010|
|Exchange 2007|Exchange 2013|
|Exchange 2010|Exchange 2016|
|Exchange 2013|Exchange 2019|

When upgrading to Exchange 2013 or later, you can use the Exchange Deployment Assistant to help complete your deployment. For more information, see [Exchange Deployment Assistant](https://assistants.microsoft.com/).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
