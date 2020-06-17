---
title: 'Exchange 2013: editions and versions: Exchange 2013 Help'
TOCTitle: 'Exchange 2013: editions and versions'
ms:assetid: b563b543-fb3f-4465-9a54-cbfd680aee1f
ms:mtpsurl: https://technet.microsoft.com/library/Bb232170(v=EXCHG.150)
ms:contentKeyID: 50407954
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013: editions and versions

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 is available in two server editions: Standard Edition and Enterprise Edition. Enterprise Edition can scale to 50 mounted databases per server in the Release to Manufacturing (RTM) and Cumulative Update 1 (CU1) versions, and 100 mounted databases per server in Cumulative Update 2 (CU2) and later versions; Standard Edition is limited to 5 mounted databases per server. A mounted database is a database that is in use. A mounted database can be an active mailbox database that is mounted for use by clients, or a passive mailbox database that is mounted in recovery for log replication and replay. While you can create more databases than the limits described above, you can only mount the maximum number specified above. The recovery database does not count towards this limit.

These licensing editions are defined by a product key. When you enter a valid license product key, the supported edition for the server is established. Product keys can be used for the same edition key swaps and upgrades only; they can't be used for downgrades. You can use a valid product key to move from the evaluation version (Trial Edition) of Exchange 2013 to either Standard Edition or Enterprise Edition. You can also use a valid product key to move from Standard Edition to Enterprise Edition.

You can also license the server again using the same edition product key. For example, if you had two Standard Edition servers with two keys, but you accidentally used the same key on both servers, you can change the key for one of them to the other key that you were issued. You can take these actions without having to reinstall or reconfigure anything. After you enter the product key and restart the Microsoft Exchange Information Store service, the edition corresponding to that product key will be reflected.

No loss of functionality will occur when the Trial Edition expires, so you can maintain lab, demo, training, and other non-production environments beyond 120 days without having to reinstall the Trial Edition of Exchange 2013.

As mentioned earlier, you can't use product keys to downgrade from Enterprise Edition to Standard Edition, nor can you use them to revert to the Trial Edition. These types of downgrades can only be done by uninstalling Exchange 2013, reinstalling Exchange 2013, and entering the correct product key.

## Exchange 2013 versions

For a list of Exchange 2013 versions and information on how to download and upgrade to the latest version of Exchange 2013, see the following topics:

- [Exchange Server Updates: build numbers and release dates](https://docs.microsoft.com/Exchange/new-features/build-numbers-and-release-dates)

- [Upgrade Exchange 2013 to the latest cumulative update or service pack](upgrade-exchange-2013-to-the-latest-cumulative-update-or-service-pack-exchange-2013-help.md)

To view the build number for the version of Exchange 2013 that you're running, run the following command in the Exchange Management Shell.

```powershell
Get-ExchangeServer | fl name,edition,admindisplayversion
```

## Exchange 2013 license types

Exchange 2013 is licensed in the Server/Client Access License (CAL) model similar to how Exchange 2010 was licensed. Following are the types of licenses:

- **Server licenses**: A license must be assigned for each instance of the server software that is being run. The Server license is sold in two server editions: Standard Edition and Enterprise Edition.

- **Client Access licenses (CALs)**: Exchange 2013 also comes in two client access license (CAL) editions, which are referred to as a Standard CAL and an Enterprise CAL. You can mix and match the server editions with the CAL types. For example, you can use Enterprise CALs with Exchange 2013 Standard Edition. Similarly, you can use Standard CALs with Exchange 2013 Enterprise Edition.

For more information about Exchange license types, see [Exchange Licensing FAQs](https://www.microsoft.com/microsoft-365/exchange/microsoft-exchange-licensing-faq-email-for-business).
