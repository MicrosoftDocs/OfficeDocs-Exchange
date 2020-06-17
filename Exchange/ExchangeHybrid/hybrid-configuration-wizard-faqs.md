---
title: "Hybrid Configuration wizard FAQs"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
ms.topic: overview
ms.prod: exchange-server-it-pro
localization_priority: Normal
ms.collection:
- Hybrid
- Ent_O365_Hybrid
- M365-email-calendar
ms.assetid: e911e6e0-e36e-4430-ac36-c745a10d6c26
ms.reviewer:
description: "Microsoft has released a new Hybrid Configuration wizard that simplifies the configuration of a hybrid deployment, allows for more flexibility with your hybrid configuration, and ensures you are always running the most up to date versions of the experience. This version of the hybrid wizard is built into Exchange 2016 and releases of Exchange 2013 starting with Cumulative Update 10, but even if you're running an older Exchange 2013 cumulative update (CU) or Exchange 2010 Service Pack 3 (SP3), you can still download the new wizard."
---

# Hybrid Configuration wizard FAQs

Microsoft has released a new Hybrid Configuration wizard (HCW) that simplifies the configuration of a hybrid deployment, allows for more flexibility with your hybrid configuration, and ensures you are always running the most up to date versions of the experience. This version of the hybrid wizard is built into Exchange 2016 and releases of Exchange 2013 starting with Cumulative Update 10, but even if you're running an older Exchange 2013 cumulative update (CU) or Exchange 2010 Service Pack 3 (SP3), you can still download the new wizard.

To download the Hybrid Configuration wizard go to [https://aka.ms/HybridWizard](https://aka.ms/HybridWizard).

## Questions frequently asked by customers

Q: What versions of Exchange support the new Hybrid Configuration wizard?

> A: You can use a combination of one or more of the following: <br/> • Exchange 2010 SP3 (latest RU strongly recommended) <br/>• Exchange 2013 CU1 or later <br/>• Exchange 2016 and Exchange 2019 <br/><br/> **Important note**: To remain in a supported hybrid configuration, you need to make sure you are running the latest available released CU for your Exchange version. Cumulative updates are released on a quarterly cadence. If you are unable to upgrade to the latest available CU, the previous CU is also supported. <br/><br/> For role specific requirements and prerequisites, see [Prerequisites for hybrid deployment](hybrid-deployment-prerequisites.md#prerequisites-for-hybrid-deployment).


Q: Will this Hybrid Configuration wizard work with Exchange 2007?

> A: You can configure a hybrid deployment with Exchange 2007 in your organization. However, to do so you need to deploy at least one server running Exchange 2013 that meets the requirements above.

Q: Can I opt out of the new Hybrid Configuration wizard?

> A: No. The new Hybrid Configuration wizard is the only supported way in Exchange 2010/2013/2016/2019 to configure hybrid mode.

Q: Can I upgrade my current Exchange 2010 hybrid configuration to Exchange 2013 or Exchange 2016 using the new Hybrid Configuration wizard?

> A: Yes. Make sure you have at least one server that meets the current Hybrid Configuration wizard requirements and then run it. The wizard will know the current state of your hybrid configuration and seamlessly take you through the upgrade process.

Q: Does the Exchange hybrid server in my on-premises organization require an Exchange license?

> A: Yes. You need to license your hybrid server when you configure a hybrid deployment. The HCW is now able to detect and license your designated on-premises Exchange 2010, Exchange 2013, or Exchange 2016 hybrid server for free without going to a separate web site or calling Microsoft support. You can access the HCW [here](https://aka.ms/HybridWizard). Note that the free Exchange Server license is not available for Exchange 2019 hybrid servers.
