---
description: 'Summary: Learn about administrative tasks for managing Outlook on the web (Outlook Web App) in Exchange Server 2016 or Exchange Server 2019.'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: 3814b665-01e8-4881-9a44-163f14789ee4
ms.collection: exchange-server
ms.reviewer: 
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
title: Outlook on the web in Exchange Server

---

# Outlook on the web in Exchange Server

The user interface in Outlook on the web (formerly known as Outlook Web App) for Exchange Server has been optimized and simplified for use with phones and tablets. Supported web browsers give users access to more Outlook features. Unsupported web browsers give users the light version of Outlook on the web that has less features. For more information about features and supported web browsers, see [Outlook on the web (formerly Outlook Web App)](../../new-features/new-features.md#OutlookAppfrom2013) and [Outlook on the web (formerly Outlook Web App)](../../new-features/new-features.md#OutlookAppfrom2010).

When you install Exchange Server, Outlook on the web is automatically available for internal users at `https://<ServerName>/owa` (for example, `https://mailbox01.contoso.com/owa`). But, you'll likely want to configure Outlook on the web for external access (for example, `https://mail.contoso.com/owa`). For more information, see [Step 4: Configure external URLs](../../plan-and-deploy/post-installation-tasks/configure-mail-flow-and-client-access.md#step-4-configure-external-urls) in [Configure mail flow and client access on Exchange servers](../../plan-and-deploy/post-installation-tasks/configure-mail-flow-and-client-access.md).

 In an Outlook 2010 or later installation that's connected to an Exchange mailbox, you can typically see the Outlook on the web URL at **File** \> **Info** \> **Account Information** in the **Account Settings** section.

![The Account Information page in Outlook 2016](../../media/1329d53d-0627-4377-8085-9eb63dcc7f97.png)

Outlook on the web is provided by the Client Access (frontend) services on Mailbox servers. In Exchange Server, Client Access services are part of the Mailbox server, so you can't configure a standalone Client Access server like you could in previous versions of Exchange. For more information, see [Client access protocol architecture](../../architecture/architecture.md#ClientAccessProtocol).

If you're looking for information about Outlook on the web in Microsoft 365 or Office 365, see [Using email in Outlook on the web](https://support.microsoft.com/office/a096dc77-d053-4e04-864d-c278e5712ef9).

## Administrative tasks for managing Outlook on the web
<a name="Managing"> </a>

The configuration and management tasks that are documented for Outlook on the web in Outlook 2016 are listed in the following table.

|**Topic**|**Description**|
|:-----|:-----|
|[View or configure Outlook on the web virtual directories in Exchange Server](virtual-directories.md)|View and configure the properties of Outlook on the web for all users that connect to the server.|
|[Configure http to https redirection for Outlook on the web in Exchange Server](http-to-https-redirection.md)|Redirect Outlook on the web unencrypted http requests to https.|
|[Create a theme for Outlook on the web in Exchange Server](themes.md)|Outlook on the web comes with built-in themes that define the colors and icons that are used in Outlook on the web, but you can also create your own themes.|
|[Customize the Outlook on the web sign-in, language selection, and error pages in Exchange Server](customize-outlook-on-the-web.md)|Customize key pages in Outlook on the web.|
|[Use AD FS claims-based authentication with Outlook on the web](ad-fs-claims-based-auth.md)|Centralize Outlook on the web authentication by using Active Directory Federation Services.|
