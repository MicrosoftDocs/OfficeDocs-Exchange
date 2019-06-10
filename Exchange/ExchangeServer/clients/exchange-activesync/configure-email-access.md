---
localization_priority: Normal
description: 'Summary: Learn how to configure a mobile phone or device to use Exchange ActiveSync.'
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: 8d6e2cea-265a-43d9-a074-076f35658436
ms.date: 7/5/2018
ms.reviewer: 
title: Configure mobile phones to access email
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Configure mobile phones to access email

This article is about enabling users in your organization to access their Exchange 2016 or Exchange 2019 mailboxes with their mobile devices using Exchange ActiveSync.

## Prerequisites

- You've reviewed the manufacturer's documentation for the mobile phone you want to configure.

- Exchange ActiveSync is enabled in your organization.

> [!NOTE]
> For device-specific information about setting up Exchange-based email on a phone or tablet, see [Set up a mobile device using Office 365 for business](https://support.office.com/article/7dabb6cb-0046-40b6-81fe-767e0b1f014f).

## Configure a mobile phone or device to use Exchange ActiveSync

Most mobile phones and devices are capable of using Autodiscover in Exchange to configure the mobile email client to use Exchange ActiveSync. To configure an email account on most mobile devices, you'll need two pieces of information.

- The user's email address

- The user's password

If the mobile phone is unable to contact the Exchange server automatically through the Autodiscover service, you'll need to set up the mobile phone manually. Manual setup requires the user's email address and password, as well as the Exchange ActiveSync server name. In most organizations, the Exchange ActiveSync server name is the same as the Outlook on the web server name without the /owa, for example, mail.contoso.com.
