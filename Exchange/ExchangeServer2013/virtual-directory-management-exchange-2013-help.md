---
title: 'Virtual directory management: Exchange 2013 Help'
TOCTitle: Virtual directory management
ms:assetid: 1af30fd5-621c-4acb-b6df-d8fa64d719ba
ms:mtpsurl: https://technet.microsoft.com/library/Ff952752(v=EXCHG.150)
ms:contentKeyID: 49289186
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Virtual directory management

_**Applies to:** Exchange Server 2013_

Many of the client protocols used with Exchange Server 2013 are accessed through virtual directories. A virtual directory is used by Internet Information Services (IIS) to allow access to a web application such as Exchange ActiveSync, Outlook Web App, or the Autodiscover service. You can manage a variety of virtual directory settings on Exchange 2013 including authentication, security, and reporting settings.

## Understanding virtual directories

The tasks that you can perform on the various virtual directories vary per client protocol. For example, on the Exchange ActiveSync virtual directory you can enable bad item logging and configure the accepted authentication types for the Exchange ActiveSync virtual directory among other tasks. For the Outlook Web App virtual directory, you can configure authentication, segmentation, and file access settings.

## Managing virtual directories

Virtual directory management can be performed in three places. You can manage a variety of settings in the Exchange admin center (EAC), as well as the Exchange Management Shell. You can also manage certain virtual directory settings in Internet Information Services Manager. For more information about the various settings you can manage on the virtual directories for Exchange ActiveSync, Outlook Web App, and the Autodiscover service, see the following topics:

- [View or configure Outlook Web App virtual directories](view-or-configure-outlook-web-app-virtual-directories-exchange-2013-help.md)

- [Exchange ActiveSync virtual directory management tasks](exchange-activesync-virtual-directory-management-tasks-exchange-2013-help.md)
