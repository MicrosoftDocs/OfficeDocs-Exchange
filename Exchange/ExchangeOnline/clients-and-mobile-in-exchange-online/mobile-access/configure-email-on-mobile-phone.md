---
localization_priority: Normal
description: You can configure a mobile phone to use Microsoft Exchange ActiveSync. You should perform this procedure on each mobile phone in your organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8d6e2cea-265a-43d9-a074-076f35658436
ms.reviewer: 
title: Configure mobile phones to access email
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Configure mobile phones to access email

You can configure a mobile phone to use Microsoft Exchange ActiveSync. You should perform this procedure on each mobile phone in your organization.

## Prerequisites

- You've reviewed the manufacturer's documentation for the mobile phone you want to configure.

- Exchange ActiveSync is enabled in your organization.

> [!NOTE]
> For device-specific information about setting up Microsoft Exchange-based email on a phone or tablet, see [Set up Office apps and email on a mobile device](https://support.microsoft.com/office/7dabb6cb-0046-40b6-81fe-767e0b1f014f).

## Configure a mobile phone to use Exchange ActiveSync

Most mobile phones and devices are capable of using Autodiscover to configure the mobile email client to use Exchange ActiveSync. To configure an email account on most mobile phones, you'll need two pieces of information.

- The user's email address

- The user's password

If the mobile phone is unable to contact the Exchange server automatically through Autodiscover, you'll need to set up the mobile phone manually. Manual setup requires the user's email address and password, as well as the Exchange ActiveSync server name. In most organizations, the Exchange ActiveSync server name is the same as the Outlook on the web (formerly known as Outlook Web App) server name without the /owa, for example, mail.contoso.com.
