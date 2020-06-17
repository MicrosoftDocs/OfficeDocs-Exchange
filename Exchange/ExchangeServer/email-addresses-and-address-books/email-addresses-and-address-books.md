---
localization_priority: Normal
description: 'Summary: Learn about the different methods that Exchange Server 2016 and Exchange Server 2019 uses to organize and configure email addresses.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: b97d0f68-691a-42af-9a6c-4dcc37b28a42
ms.reviewer:
title: Email addresses and address books in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Email addresses and address books in Exchange Server

Exchange uses address books to organize and store email address information for recipients in the organization. The topics that will help you learn about and configure email addresses and address books in Exchange Server are described in the following table.

|**Key terminology**|**Description**|**Topic**|
|:-----|:-----|:-----|
|**Address book policies**|The global address list (GAL) is the master list of all recipients in your Exchange organization. Address book policies (ABPs) provide a simpler mechanism for GAL segmentation in organizations that require multiple GALs. An ABP defines a GAL, an offline address book (OAB), a room list, and one or more address lists. You can then assign the ABP to users.|[Address book policies in Exchange Server](address-book-policies/address-book-policies.md)|
|**Address lists**|An address list is a subset of a GAL. Each address list is a dynamic collection of one or more types recipients. You can use address lists to help users find the recipients and resources that they need.|[Address lists in Exchange Server](address-lists/address-lists.md)|
|**Details templates**|Details templates control the appearance of recipient properties that are displayed in address lists in Outlook.|[Details Templates](https://docs.microsoft.com/exchange/details-templates-exchange-2013-help)|
|**Email address policies**|Email address policies are the rules that create email addresses for Exchange recipients.|[Email address policies in Exchange Server](email-address-policies/email-address-policies.md)|
|**Hierarchical address books**|The hierarchical address book (HAB) presents recipients in the GAL by using your organization's unique business structure (for example, seniority or management hierarchy), which provides an efficient method for locating internal recipients.|[Hierarchical Address Books](https://docs.microsoft.com/exchange/address-books/hierarchical-address-books/hierarchical-address-books)|
|**Offline address books**|An offline address book (OAB) is a collection of address lists that can be downloaded and used in Outlook by users that are disconnected from the Exchange organization.|[Offline address books in Exchange Server](offline-address-books/offline-address-books.md)|
