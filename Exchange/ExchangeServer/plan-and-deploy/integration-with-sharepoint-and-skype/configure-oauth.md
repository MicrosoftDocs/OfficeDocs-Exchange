---
localization_priority: Normal
description: 'Summary: Learn how to use the Open Authorization (OAuth) authentication protocol to authenticate applications to Exchange. The other applications need to be configured as partner applications in Exchange 2016.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ca3c78a3-80cc-4df2-859f-0106bbd57a07
ms.reviewer:
title: Configure OAuth authentication with SharePoint 2013 and Lync 2013
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure OAuth authentication with SharePoint 2013 and Lync 2013

Exchange 2016 supports partner applications such as SharePoint Server 2016 and Skype for Business Server 2015 by using OAuth configuration with the script, `Configure-EnterpriseApplication.ps1`. You can automate the task using the script to more easily configure authentication with partner applications and reduce configuration errors. The script performs the following tasks:

1. Configures an Enterprise partner application that self-issues OAuth tokens to successfully authenticate to Exchange.

2. Assigns Role Based Access Control (RBAC) roles to the partner application to authorize it for calling specific Exchange Web Services APIs.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- The partner application needs to publish an authentication metadata document for Exchange 2016 to establish a direct trust to this application and accept authentication requests.

- Examples in this topic use the following default location of the `\Scripts` directory: `C:\Program Files\Microsoft\Exchange Server\V15\Scripts`.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Partner applications - configure" entry in the [Sharing and collaboration permissions](../../permissions/feature-permissions/sharing-and-collaboration-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Configure OAuth authentication with a partner application

This procedure uses the `Configure-EnterpriseApplication.ps1` script to configure OAuth authentication with partner applications. Access to resources depends on the permissions assigned to the partner application and/or the user it impersonates by using RBAC. After configuring OAuth authentication from Exchange, the partner application can use Exchange 2016 resources.

1. This example configures OAuth authentication for SharePoint 2016.

   ```console
   Cd C:\Program Files\Microsoft\Exchange Server\V15\Scripts
   Configure-EnterprisePartnerApplication.ps1 -AuthMetaDataUrl https://sharepoint.contoso.com/_layouts/15/metadata/json/1 -ApplicationType SharePoint

   ```

2. This example configures OAuth authentication for Skype for Business or Lync Server 2013.

   ```console
   Cd C:\Program Files\Microsoft\Exchange Server\V15\Scripts
   Configure-EnterprisePartnerApplication.ps1 -AuthMetaDataUrl https://lync.contoso.com/metadata/json/1 -ApplicationType Lync

   ```

 If Exchange 2016 also needs to access resources offered by the partner application, you must also configure OAuth authentication in the partner application.

## How do you know this worked?

To verify that you have successfully configured an enterprise partner application to authenticate to Exchange 2016 , run the [Get-PartnerApplication](https://docs.microsoft.com/powershell/module/exchange/get-partnerapplication) cmdlet in the Exchange Management Shell to retrieve the configuration. You can also run the [Test-OAuthConnectivity](https://docs.microsoft.com/powershell/module/exchange/test-oauthconnectivity) cmdlet to test OAuth connectivity with a partner application for a user.

## Hybrid and on-premises deployments

- In hybrid deployments, you can use OAuth authentication between your on-premises Exchange 2016 organization and the Exchange Online organization. For more information, see [Using Oauth Authentication to Support eDiscovery in an Exchange Hybrid Deployment](https://docs.microsoft.com/exchange/using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help).

- In on-premises deployments, you can configure server-to-server authentication between Exchange 2016 and SharePoint 2016 so administrators and compliance officers can search Exchange 2016 by using the SharePoint 2016 eDiscovery Center.. For more information, see [Configure Exchange for SharePoint eDiscovery Center](https://docs.microsoft.com/exchange/configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help).
