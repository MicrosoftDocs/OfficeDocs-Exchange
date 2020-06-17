---
localization_priority: Normal
description: Admins can learn about add-ins for Outlook in Exchange Online.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 28b6f2a1-a235-4023-b561-6fd304962775
ms.reviewer: 
title: Add-ins for Outlook in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Add-ins for Outlook in Exchange Online

Add-ins for Outlook are applications that extend the usefulness of Outlook clients by adding information or tools that your users can use without having to leave Outlook. Add-ins are built by third-party developers and can be installed either from a file or URL or from the Office Store. By default, all users can install add-ins. Exchange Online admins can control whether users can install add-ins for Office.

> [!TIP]
> For information about add-ins for Outlook from an end-user perspective, check out [Installed add-ins](https://support.microsoft.com/office/a61762b7-7a82-47bd-b14e-bbc15eaeb70f). The topic provides an overview of the add-ins and also shows you some of the add-ins for Outlook that might be installed by default.

## Office Store add-ins and custom add-ins

Outlook clients supports a variety of add-ins that are available through the Office Store. Outlook also supports custom add-ins that you can create and distribute to users in your organization.

**Notes**:

- Access to the Office Store isn't supported for mailboxes or organizations in specific regions. If you don't see **Add from the Office Store** as an option in the **Exchange admin center** under **Organization** \> **Add-ins** \> **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), you may be able to install an add-in for Outlook from a URL or file location. For more information, contact your service provider.

- Some add-ins for Outlook are installed by default. Default add-ins for Outlook only activate on English language content. For example, German postal addresses in the message body won't activate the Bing Maps add-in.

## Add-in access and installation

By default, all users can install and remove add-ins. Exchange Online admins have a number of controls available for managing add-ins and users' access to them. Admins can disable users from installing add-ins that are not downloaded from the Office Store (instead they are "side loaded" from a file or URL). Admins can also disable users from installing Office Store add-ins, and from installing add-ins on behalf of other users.

To install add-ins for some or all users in your organization, see [Manage deployment of Office 365 add-ins in the admin center](https://docs.microsoft.com/office365/admin/manage/manage-deployment-of-add-ins)
