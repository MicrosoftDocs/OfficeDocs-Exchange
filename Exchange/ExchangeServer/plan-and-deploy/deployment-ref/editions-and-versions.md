---
localization_priority: Normal
description: 'Summary: Learn about the different versions of Exchange 2016 and Exchange 2019.'
ms.topic: reference
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b563b543-fb3f-4465-9a54-cbfd680aee1f
ms.reviewer: 
title: Exchange Server editions and versions
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange Server editions and versions

Exchange Server 2016 and Exchange Server 2019 are available in two server editions:

- **Enterprise Edition**: Can scale up to 100 mounted databases per server.

- **Standard Edition**: Limited to five mounted databases per server.

A _mounted database_ is a database that's in use (an active mailbox database that's mounted for use by clients or a passive mailbox database that's mounted in recovery for log replication and replay). While you can create more databases than the described limits, you can only mount the maximum number of databases that are allowed by the edition of Exchange. Note that the recovery database doesn't count towards these limits.

The server editions are defined by a product key. When you enter a valid product key, the supported edition for the server is established. For more information, see [Enter your Exchange Server product key](../post-installation-tasks/enter-product-key.md).

**Notes**:

- You can use a valid product key to move from the Trial Edition (evaluation version) of Exchange to either Standard Edition or Enterprise Edition. No loss of functionality occurs after the Trial Edition expires, so you can maintain lab, demo, training, and other non-production environments beyond 120 days without having to reinstall the Trial Edition of Exchange or entering a product key.

- You can use a valid product key to move from Standard Edition to Enterprise Edition.

- You can't use a valid product key to downgrade from Enterprise Edition to Standard Edition or revert to the Trial Edition. You can only do these types of downgrades by uninstalling Exchange, reinstalling Exchange, and entering the correct product key.


## Exchange Server versions

For a list of Exchange Server versions and how to download and upgrade to the latest version of Exchange, see the following topics:

- [Exchange Server build numbers and release dates](../../new-features/build-numbers-and-release-dates.md)

- [Install Exchange Mailbox servers using the Setup wizard](../../plan-and-deploy/deploy-new-installations/install-mailbox-role.md)

- [Upgrade Exchange to the latest Cumulative Update](../install-cumulative-updates.md)

To view the Exchange version and edition information for all Exchange servers in your organization, run the following command in the Exchange Management Shell:

```powershell
Get-ExchangeServer | Format-Table -Auto Name,Edition,AdminDisplayVersion
```

## Exchange Server license types

Exchange 2013 and all later versions use a licensing model that's similar to how Exchange 2010 was licensed:

- **Server licenses**: A license must be assigned for each Exchange server. The Server license is sold in two server editions: Standard Edition and Enterprise Edition.

- **Client Access licenses (CALs)**: Exchange also comes in two CAL editions, which are referred to as a Standard CAL and an Enterprise CAL. You can mix and match the Exchange server editions with the CAL types. For example, you can use Enterprise CALs with Standard Edition or Standard CALs with Enterprise Edition.

For more information about Exchange license types, see [Exchange Licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-licensing-faq-email-for-business).
