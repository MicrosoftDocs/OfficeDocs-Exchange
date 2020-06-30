---
title: 'Configure Exchange for SharePoint eDiscovery Center: Exchange 2013 Help'
TOCTitle: Configure Exchange for SharePoint eDiscovery Center
ms:assetid: 795c1a3b-295c-4ee5-ade9-52cf3fda3f19
ms:mtpsurl: https://technet.microsoft.com/library/JJ218665(v=EXCHG.150)
ms:contentKeyID: 48385255
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Exchange for SharePoint eDiscovery Center

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 includes features that work with Microsoft SharePoint Server 2013 and Microsoft Lync Server 2013, known as *partner applications*. To make sure these partner applications can access each other's resources, you need to configure server-to-server authentication.

This topic shows you how to configure server-to-server authentication between Exchange 2013 and SharePoint 2013 so users can use the eDiscovery Center in SharePoint 2013 to search Exchange Server 2013 mailbox content. To fully enable this functionality, you must complete additional steps in SharePoint 2013. For details, see [Configure eDiscovery in SharePoint 2013](https://docs.microsoft.com/SharePoint/governance/configure-ediscovery-0).

## What do you need to know before you begin?

- Estimated time to complete this task: 30 minutes.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- It's supported to install Exchange 2013 and SharePoint 2013 in different domains or forests. A Windows trust relationship between Exchange and SharePoint forests isn't required, because in that circumstance, Exchange and SharePoint will rely on the OAuth 2.0 protocol to trust one another.

- The SharePoint 2013 site must be configured to use Secure Sockets Layer (SSL).

- The [Exchange Web Services Managed API](https://www.microsoft.com/download/details.aspx?id=35371) must be installed on every server that is running SharePoint 2013. Reset Internet Information Server (IIS) after installation.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Step 1: Configure server-to-server authentication for Exchange 2013 on a server running SharePoint Server 2013

Run the following command to create Exchange 2013 as a trusted security token issuer in SharePoint 2013.

```powershell
New-SPTrustedSecurityTokenIssuer -Name Exchange -MetadataEndPoint https://<Exchange Server Name or FQDN>/autodiscover/metadata/json/1
```

## Step 2: Configure server-to-server authentication for SharePoint 2013 on a server running Exchange 2013

Perform this step on an Exchange 2013 server. You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Partner applications - configure" entry in the [Sharing and collaboration permissions](sharing-and-collaboration-permissions-exchange-2013-help.md) topic.

Run this command to configure the SharePoint partner application.

```powershell
cd c:\'Program Files'\Microsoft\'Exchange Server'\V15\Scripts
.\Configure-EnterprisePartnerApplication.ps1 -AuthMetadataUrl <path to SharePoint AuthMetadataUrl> -ApplicationType SharePoint
```

## Step 3: Add authorized users to the Discovery Management role group

Add users who need to perform an eDiscovery search using SharePoint 2013 to the Discovery Management role group in Exchange 2013. For details, see [Assign eDiscovery permissions in Exchange](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/assign-ediscovery-permissions).

> [!WARNING]
> Adding users to the Discovery Management role group allows them to use In-Place eDiscovery to search all Exchange 2013 mailboxes and access potentially sensitive email content in user mailboxes. By default, this permission isn't assigned to any user, including members of the Organization Management role group. Check with your organization's legal or HR departments before assigning this permission to any user.
