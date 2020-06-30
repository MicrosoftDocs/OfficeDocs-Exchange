---
localization_priority: Normal
description: 'Summary: The most recent supported Exchange topologies that allow users to access public folders with Outlook 2016 for Mac.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: bc9b8226-bd8b-4edc-882b-4f19cfe118eb
ms.reviewer: 
f1.keywords:
- NOCSH
title: Accessing public folders with Outlook 2016 for Mac
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Accessing public folders with Outlook 2016 for Mac

 **Summary**: The most recent supported Exchange topologies that allow users to access public folders with Outlook 2016 for Mac.

Users of Outlook 2016 for Mac can now access public folders in Exchange Online in a number of different topologies.

## Outlook for Mac limitations

All versions of Outlook for Mac can access Exchange public folders, but until recently these clients could not access public folders in the following deployment scenario:

- **Hybrid topologies**: On-premises users with a mailbox based in Exchange Online could not use Outlook for Mac to access on-premises modern public folders. Similarly, users with an Exchange 2013 or Exchange 2016 mailbox on-premises could not use Outlook for Mac to access public folders deployed in Exchange Online.

## Outlook 2016 for Mac

With the April 2016 update for Outlook 2016 for Mac, as well as CU14 for Exchange 2013 and CU2 for Exchange 2016, the above scenario will now work for Outlook 2016 for Mac clients.

The following table summarizes the supported topologies for users with Outlook 2016 for Mac clients trying to access public folders in Exchange Online.

> [!NOTE]
> The scenarios shown in the following table assume that the April 2016 update for Outlook 2016 for Mac has been applied to all clients.

|**Public folders are deployed on...**|**User mailbox is on Exchange 2010 SP3 or later**|**User mailbox is on Exchange 2013 CU13 or later**|**User mailbox is on Exchange 2016 CU2 or later**|**User mailbox is in Microsoft 365, Office 365, or Exchange Online**|
|:-----|:-----|:-----|:-----|:-----|
|Exchange Server 2010 SP3 or later|Supported|Supported|Supported|Not supported|
|Exchange Server 2013 CU13 or later|Not supported|Supported|Supported|Supported|
|Exchange Server 2016 CU2 or later|Not supported|Supported|Supported|Supported|
|Microsoft 365, Office 365, or Exchange Online|Not supported|Supported|Supported|Supported|

The following articles describe how to deploy public folders in your Exchange organization in a co-existence or hybrid topology. As long as your Outlook 2016 for Mac clients have installed the April 2016 update, they will be able to access public folders in the configurations detailed in these articles:

- [Configure legacy public folders where user mailboxes are on Exchange 2013 servers](https://docs.microsoft.com/exchange/configure-legacy-public-folders-where-user-mailboxes-are-on-exchange-2013-servers-exchange-2013-help)

- [Configure Exchange 2013 public folders for a hybrid deployment](set-up-modern-hybrid-public-folders.md)

- [Configure Exchange Online public folders for a hybrid deployment](set-up-exo-hybrid-public-folders.md)
