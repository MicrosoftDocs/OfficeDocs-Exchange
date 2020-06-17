---
localization_priority: Normal
description: 'Summary: Learn about the default virtual directory settings on Mailbox servers in Exchange 2016 and Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d2d89ce6-4721-4737-a325-fba5ad9422e0
ms.reviewer: 
title: Default settings for Exchange virtual directories
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Default settings for Exchange virtual directories

Exchange Server 2016 and Exchange Server 2019 automatically configure multiple Internet Information Services (IIS) virtual directories during the server installation. The tables in the following sections show the settings for the Client Access (frontend) services on Mailbox servers and the default IIS authentication and Secure Sockets Layer (SSL) settings.

## Client Access services (frontend) on Mailbox servers

The following table lists the default settings in the Client Access services (the default web site) on Exchange Mailbox servers.

|**Virtual directory**|**Authentication method**|**SSL settings**|**Management method**|
|:-----|:-----|:-----|:-----|
|Default Web Site|Anonymous|Required|IIS management console|
|API<sup>1</sup>|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption||
|aspnet_client|Anonymous authentication|SSL required <br/> Requires 128-bit encryption|IIS management console|
|Autodiscover|Anonymous authentication <br/> Basic authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|ecp|Anonymous authentication <br/> Basic authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|EWS|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|MAPI|Windows authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|Microsoft-Server-ActiveSync|Basic authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|OAB|Windows authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|owa|Basic authentication|SSL required <br/> Requires 128-bit encryption|EAC or Exchange Management Shell|
|PowerShell|By default, all authentication methods are disabled.|Not required|EAC or Exchange Management Shell|
|Rpc|Basic authentication <br/> Windows authentication|Not required|EAC or Exchange Management Shell|

<sup>1</sup>The API virtual directory is available in Exchange 2016 CU3 or newer.

## Back End Virtual Directories on Mailbox servers

The following table lists the default settings in the back end services on Exchange Mailbox servers.

|**Virtual directory**|**Authentication method**|**SSL settings**|**Management method**|
|:-----|:-----|:-----|:-----|
|Exchange Back End|Anonymous authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|API<sup>1</sup>|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|Autodiscover|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|ecp|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|EWS|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|Microsoft-Server-ActiveSync|Basic authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|OAB|Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|owa|Anonymous authentication <br/> Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|PowerShell|Windows authentication|SSL required <br/> Requires 128-bit encryption|This virtual directory should not be configured by the user.|
|Rpc|Windows authentication|Not required|This virtual directory should not be configured by the user.|
|RpcWithCert|Windows authentication|Not required|This virtual directory should not be configured by the user.|

<sup>1</sup>The API virtual directory is available in Exchange 2016 CU3 or newer.

## See also

[Virtual directory management](https://docs.microsoft.com/exchange/virtual-directory-management-exchange-2013-help)
