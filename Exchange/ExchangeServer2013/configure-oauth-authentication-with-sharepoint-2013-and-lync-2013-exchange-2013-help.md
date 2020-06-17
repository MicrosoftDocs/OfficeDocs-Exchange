---
title: 'Configure OAuth authentication with SharePoint 2013 and Lync 2013'
TOCTitle: Configure OAuth authentication with SharePoint 2013 and Lync 2013
ms:assetid: ca3c78a3-80cc-4df2-859f-0106bbd57a07
ms:mtpsurl: https://technet.microsoft.com/library/JJ649094(v=EXCHG.150)
ms:contentKeyID: 49317458
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure OAuth authentication with SharePoint 2013 and Lync 2013

_**Applies to:** Exchange Server 2013_

Exchange Server 2013 allows other applications to use OAuth to authenticate to Exchange. The applications must be configured as partner applications in Exchange 2013.

In Exchange 2013, OAuth configuration with partner applications such as SharePoint 2013 and Lync Server 2013 is supported only by using the `Configure-EnterpriseApplication.ps1` script. By automating the task, the script makes it easier to configure authentication with partner applications and reduces configuration errors. The script performs the following tasks:

1. Configures an Enterprise partner application that self-issues OAuth tokens to successfully authenticate to Exchange.

2. Assigns Role Based Access Control (RBAC) roles to the partner application to authorize it for calling specific Exchange Web Services APIs.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- The partner application must publish an auth metadata document for Exchange 2013 to establish a direct trust to this application and accept authentication requests.

- Examples in this topic use the following default location of the `\Scripts` directory: `C:\Program Files\Microsoft\Exchange Server\V15\Scripts`.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Partner applications - configure" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Configure OAuth authentication with a partner application

This procedure uses the `Configure-EntepririseApplication.ps1` script to configure OAuth authentication with partner applications. Access to resources depends on the permissions assigned to the partner application and/or the user it impersonates by using RBAC.

After configuring OAuth authentication from Exchange, the partner application can use Exchange 2013 resources. If Exchange 2013 also needs to access resources offered by the partner application, you must also configure OAuth authentication in the partner application.

This example configures OAuth authentication for SharePoint 2013.

```powershell
Cd C:\Program Files\Microsoft\Exchange Server\V15\Scripts
Configure-EnterprisePartnerApplication.ps1 -AuthMetaDataUrl https://sharepoint.contoso.com/_layouts/15/metadata/json/1 -ApplicationType SharePoint
```

This example configures OAuth authentication for Lync Server 2013.

```powershell
Cd C:\Program Files\Microsoft\Exchange Server\V15\Scripts
Configure-EnterprisePartnerApplication.ps1 -AuthMetaDataUrl https://lync.contoso.com/metadata/json/1 -ApplicationType Lync
```

## How do you know this worked?

To verify that you have successfully configured an enterprise partner application to authenticate to Exchange 2013 , run the [Get-PartnerApplication](https://docs.microsoft.com/powershell/module/exchange/Get-PartnerApplication) cmdlet in the Shell to retrieve the configuration. You can also run the [Test-OAuthConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-OAuthConnectivity) cmdlet to test OAuth connectivity with a partner application for a user.

## More information

- In hybrid deployments, you can use OAuth authentication between your on-premises Exchange 2013 organization and the Exchange Online organization. For more information, see [Using OAuth authentication to support eDiscovery in an Exchange hybrid deployment](using-oauth-authentication-to-support-ediscovery-in-an-exchange-hybrid-deployment-exchange-2013-help.md).

- In on-premises deployments, you can configure server-to-server authentication between Exchange 2013 and SharePoint 2013 so administrators and compliance officers can use the eDiscovery Center in SharePoint 2013 to search Exchange 2013 mailboxes. For more information, see [Configure Exchange for SharePoint eDiscovery Center](configure-exchange-for-sharepoint-ediscovery-center-exchange-2013-help.md).
