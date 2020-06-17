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

You can configure a mobile phone, such as a Windows Phone, to use Microsoft Exchange ActiveSync. You should perform this procedure on each mobile phone in your organization.

## Prerequisites

- You've reviewed the manufacturer's documentation for the mobile phone you want to configure.

- Exchange ActiveSync is enabled in your organization.

## Configure a mobile phone to use Exchange ActiveSync

Most mobile phones and devices are capable of using Autodiscover to configure the mobile email client to use Exchange ActiveSync. To configure an email account on most mobile phones, you'll need two pieces of information.

- The user's email address

- The user's password

If the mobile phone is unable to contact the Exchange server automatically through Autodiscover, you'll need to set up the mobile phone manually. Manual setup requires the user's email address and password, as well as the Exchange ActiveSync server name. In most organizations, the Exchange ActiveSync server name is the same as the Outlook Web App server name without the /owa, for example, mail.contoso.com.

### Windows Phone synchronization

If you're configuring a Windows Phone mobile phone to synchronize with an Exchange mailbox using Exchange ActiveSync, only a subset of mobile device mailbox policy settings are supported. Those policy settings are detailed in [Supported mobile device mailbox policies for Windows Phones and devices](supported-mobile-device-mailbox-policies-for-windows-phones-and-devices-exchange-2013-help.md).

If you configure mobile device mailbox policy settings that are not supported for the version of Windows Phone you're using, you must also set the **AllowNonProvisionableDevices** policy setting to true or create a separate mobile device mailbox policy for Windows Phone mobile phones.
