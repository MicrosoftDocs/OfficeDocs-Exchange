---
localization_priority: Normal
description: Microsoft Exchange Server 2016 Setup has detected that the computer you're attempting to install Exchange 2016 on is an Active Directory domain controller. Installing Exchange 2016 on a domain controller isn't recommended.
ms.topic: reference
author: chrisda
f1_keywords:
- ms.exch.setupreadiness.WarningInstallExchangeRolesOnDomainController
ms.author: chrisda
ms.assetid: 48922de2-a68c-4092-96a5-d38c8e5f49f5
ms.date: 8/2/2018
ms.reviewer: 
title: Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]
ms.collection: exchange-server
audience: Developer
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]

Exchange Server 2016 or Exchange 2019 Setup has detected that the target computer is an Active Directory domain controller, and we don't recommed installing Exchange on domain controllers.

If you install Exchange on a domain controller, be aware of the following issues:

- Configuring Exchange for Active Directory split permissions isn't supported. For more information about split permissions, see [Understanding split permissions](https://technet.microsoft.com/library/dd638106(v=exchg.150).aspx).

- The Exchange Trusted Subsystem universal security group (USG) is added to the Domain Admins group. This action grants all Exchange servers domain administrator rights in the domain.

- Exchange Server and Active Directory are both resource-intensive applications. There are performance implications when both applications are running on the same computer.

- The domain controller must be a global catalog server, but Exchange services might not start correctly on a global catalog server.

- System shutdown will take considerably longer if Exchange you don't stop the Exchange services before you shut down or restart the server.

- Demoting the domain controller to a member server isn't supported.

- Running Exchange on a clustered node that's also an Active Directory domain controller isn't supported.

Therefore, we recommend that you install Exchange on a member server, not on a domain controller.

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
