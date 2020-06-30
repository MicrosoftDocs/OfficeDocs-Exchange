---
title: 'Configure mobile phones to access email: Exchange 2013 Help'
TOCTitle: Configure mobile phones to access email
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 8d6e2cea-265a-43d9-a074-076f35658436
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure mobile phones to access email in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can configure a mobile phone to use Microsoft Exchange ActiveSync. You should perform this procedure on each mobile phone in your organization.

## Prerequisites

- You've reviewed the manufacturer's documentation for the mobile phone you want to configure.

- Exchange ActiveSync is enabled in your organization.

## Configure a mobile phone to use Exchange ActiveSync

Most mobile phones and devices are capable of using Autodiscover to configure the mobile email client to use Exchange ActiveSync. To configure an email account on most mobile phones, you'll need two pieces of information.

- The user's email address

- The user's password

If the mobile phone is unable to contact the Exchange server automatically through Autodiscover, you'll need to set up the mobile phone manually. Manual setup requires the user's email address and password, as well as the Exchange ActiveSync server name. In most organizations, the Exchange ActiveSync server name is the same as the Outlook Web App server name without the /owa, for example, mail.contoso.com.
