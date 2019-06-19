---
localization_priority: Normal
description: Learn about the support life cycle for Exchange
ms.topic: article
author: chrisda
ms.author: chrisda
ms.date:
ms.reviewer: 
title: Exchange Server supportability matrix
ms.collection: exchange-server
audience: ITPro
ms:assetid: dbac2d40-da8b-469f-a265-1d1f948fe446
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Exchange Server supportability matrix

The Exchange Server supportability matrix provides a central source for Exchange administrators to easily locate information about the level of support available for any configuration or required component for supported versions of Microsoft Exchange Server.

## Release model

The following table identifies the release model for each supported version of Exchange. The release model is identified by an X character.

In Exchange Server 2010 and earlier, each update rollup package (RU) is cumulative with regard to the whole product. When you apply an RU to Exchange Server 2010, you apply all the fixes in that update rollup package, and all the fixes in each earlier update rollup package. When an update or a hotfix is created for Exchange 2010 or earlier, one or more of the binary files included in the update or included in the hotfix are cumulative. They are cumulative in regard to the contents of the files, not in regard to the whole Exchange product. For more information, see [Exchange 2010 Servicing](https://go.microsoft.com/fwlink/p/?linkid=298627).

In Exchange Server 2013 or later, we changed the way we deliver hotfixes and service packs by using a scheduled delivery model. In this model, cumulative updates (CUs) are released quarterly (every three months). A CU is released as a full, self-contained refresh of that version of Exchange, similar to a product upgrade or a service pack release. For more information, see [Updates for Exchange Server](../new-features/updates.md).

|**Servicing release model**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013**|**Exchange 2010**|
|:-----|:-----|:-----|:-----|:-----|
|Cumulative updates (CUs)|X|X|X||
|Update rollups (RUs)||||X|
|Security hotfixes delivered separately|X|X|X||

## Support lifecycle

For more information about the support lifecycle for a specific version of Exchange, or of the Microsoft Windows server or client operating systems, see the [Microsoft Support Lifecycle](https://go.microsoft.com/fwlink/p/?linkid=55839) page. For more information about the Microsoft Support Lifecycle, see the [Microsoft Support Lifecycle Policy FAQ](https://go.microsoft.com/fwlink/p/?linkid=158902).

## Exchange Server 2007 End-of life

Exchange 2007 reached end of support on April 11, 2017, per the [Microsoft Lifecycle Policy](https://go.microsoft.com/fwlink/p/?linkid=833210). As a reminder, after that date there will be no new security updates, non-security updates, free or paid assisted support options or online technical content updates. Furthermore, as adoption of Office 365 accelerates and cloud usage increases, custom support options for Office products will not be available. This includes Exchange Server as well as Office Suites; SharePoint Server; Office Communications Server; Lync Server; Skype for Business Server; Project Server and Visio. Having reached the end of support date for Exchange 2007, we encourage customers to complete their migration and upgrade plans. We recommend customers to leverage deployment benefits provided by Microsoft and Microsoft Certified Partners including [Microsoft FastTrack](https://go.microsoft.com/fwlink/p/?linkid=238431) for cloud migrations, and [Software Assurance Deployment & Planning Services](https://go.microsoft.com/fwlink/p/?linkid=833211) for on-premises upgrades.

## Supported operating system platforms

The following tables identify the operating system platforms on which each version of Exchange can run. Supported platforms are identified by an X character.

> [!IMPORTANT]
> Releases of Windows Server and Windows client that aren't listed in the tables below are not supported for use with any version or release of Exchange.

|**Server operating system**|**Exchange 2019**|**Exchange 2016 CU3 and later**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|Windows Server 2019|X|||||
|Windows Server 2016||X||||
|Windows Server 2012 R2||X|X|X|X|
|Windows Server 2012||X|X|X|X|
|Windows Server 2008 R2 SP1||||X|X|
|Windows Server 2008 SP2|||||X|

> [!NOTE]
> Client operating systems only support the Exchange management tools.

|**Client operating system**|**Exchange 2019**|**Exchange 2016 CU3 and later**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Windows 10|X|X|||
|Windows 8.1||X|X||
|Windows 8|||X|X|
|Windows 7 SP1|||X|X|
|Windows Vista SP2||||X|

## Supported Active Directory environments

The following table identifies the Active Directory environments that Exchange can communicate with. Supported environments are identified by an X character. An Active Directory server refers to both writable global catalog servers and to writable domain controllers. Read-only global catalog servers and read-only domain controllers aren't supported.

|**Operating system environment**|**Exchange 2019**|**Exchange 2016 CU12 and later**|**Exchange 2016 CU7 and later**|**Exchange 2016 CU3 to CU6**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3 RU22 or later**|**Exchange 2010 SP3 RU5 - RU21**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Windows Server 2019 Active Directory servers|X|X|||||||
|Windows Server 2016 Active Directory servers|X|X|X|X|X|X|X||
|Windows Server 2012 R2 Active Directory servers|X|X|X|X|X|X|X|X|
|Windows Server 2012 Active Directory servers||X|X|X|X|X|X|X|
|Windows Server 2008 R2 SP1 Active Directory servers||X|X|X|X|X|X|X|
|Windows Server 2008 SP2 Active Directory servers||||X|X|X|X|X|
|Windows Server 2003 SP2 Active Directory servers|||||X|X|X|X|

|**AD forest functional level**|**Exchange 2019**|**Exchange 2016 CU7 and later**|**Exchange 2016 CU3 to CU6**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3 RU22 or later**|**Exchange 2010 SP3 RU5 - RU21**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Windows Server 2016|X|X|X|X|X|X||
|Windows Server 2012 R2|X|X|X|X|X|X|X|
|Windows Server 2012||X|X|X|X|X|X|
|Windows Server 2008 R2||X|X|X|X|X|X|
|Windows Server 2008|||X|X|X|X|X|
|Windows Server 2003||||X|X|X|X|

## Web browsers supported for use with the premium version of Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for use together with the premium version of Outlook Web App or Outlook on the web. Supported browsers are identified by an X character.

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Microsoft Edge|X|X|N/A|N/A|
|Internet Explorer 11|X|X|X|X|
|Internet Explorer 10|||X|X|
|Internet Explorer 9|||X|X|
|Internet Explorer 8|||X|X|
|Internet Explorer 7||||X|
|Firefox|Current release of Firefox<sup>1</sup>|Current release of Firefox<sup>1</sup>|N/A|N/A|
|Firefox 3.0.1 or later||||X|
|Firefox 12 or later|||X|X|
|Safari|Current release of Safari|Current release of Safari|N/A|N/A|
|Safari 3.1 or later||||X|
|Safari 5.0 or later|||X|X|
|Chrome|Current release of Chrome<sup>1</sup>|Current release of Chrome<sup>1</sup>|N/A|N/A|
|Chrome 3.0.195 or later||||X|
|Chrome 18 or later|||X|X|

<sup>1</sup>Current release of Firefox or Chrome means the latest version or the immediately previous version.

## Web browsers supported for use with the basic version of Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for use together with the light (basic) version of Outlook Web App or Outlook on the web. Supported browsers are identified by an X character.

> [!NOTE]
> Outlook Web App Basic (Outlook Web App Light) is supported for use in mobile browsers. However, if rendering or authentication issues occur in a mobile browser, determine whether the issue can be reproduced by using Outlook Web App Light in the full client of a supported browser. For example, test the use of Outlook Web App Light in Safari, Chrome, or Internet Explorer. If the issue can't be reproduced in the full client, we recommend that you contact the mobile device vendor for help. In these cases, we collaborate with the vendor as appropriate.

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Microsoft Edge|X|X|||
|Internet Explorer 11|X|X|X|X|
|Internet Explorer 10|||X|X|
|Internet Explorer 9|||X|X|
|Internet Explorer 8|||X|X|
|Internet Explorer 7|||X|X|
|Safari|Current release of Safari|Current release of Safari|X|X|
|Firefox||Current release of Firefox<sup>1</sup>|X|X|
|Chrome||Current release of Chrome<sup>1</sup>|N/A|N/A|
|Opera|||X|X|

<sup>1</sup>Current release of Firefox or Chrome means the latest version or the immediately previous version.

## Web browsers supported for use of S/MIME with Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for the use of S/MIME together with Outlook Web App or Outlook on the web. Supported browsers are identified by an X character.

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Microsoft Edge|X|X|||
|Internet Explorer 11|X|X|X|X|
|Internet Explorer 10|||X|X|
|Internet Explorer 9|||X|X|
|Internet Explorer 8||||X|
|Internet Explorer 7||||X|

## Clients

The following tables identify the mail clients that are supported for use together with each version of Exchange. Supported clients are identified by an X character.

|**Client**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Office 365 ProPlus|X|X|X||
|Outlook 2019|X|X|X||
|Outlook 2016|X<sup>1</sup>|X<sup>1</sup>|X|X|
|Outlook 2013|X<sup>1</sup>|X<sup>1</sup>|X|X|
|Outlook 2010||X<sup>1</sup>|X<sup>2</sup>|X|
|Outlook 2007|||X<sup>3</sup>|X|
|Outlook for Mac for Office 365||X<sup>1</sup>|X|X|
|Entourage 2008 (EWS)||X<sup>4</sup>|X<sup>4</sup>|X<sup>4</sup>|

<sup>1</sup>Supported with the latest Office service pack and public updates.

<sup>2</sup>Requires Outlook 2010 Service Pack 1 and the latest public update.

<sup>3</sup>Requires Outlook 2007 Service Pack 3 and the latest public update.

<sup>4</sup>EWS only. There is no DAV support for Exchange 2010.

|**Client**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|:-----|
|Windows Mobile 10||X|X|X|
|Windows Phone 8.1||X|X|X|
|Windows Phone 8||X|X|X|
|Windows Phone 7.5|||X|X|
|Windows Phone 7|||X|X|

## Tools

The following table identifies the version of Microsoft Exchange that can be used together with the Microsoft Exchange Inter-Organization Replication tool (Exscfg.exe; Exssrv.exe). The tool is used to replicate public folder information (including free/busy information) between Exchange organizations. For more information, see [Microsoft Exchange Server Inter-Organization Replication](https://go.microsoft.com/fwlink/?linkid=22455). Supported versions are identified by an X character.

|**Tool**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|
|Inter-Organization Replication tool|||X|

## Microsoft .NET Framework

The following tables identify the versions of the Microsoft .NET Framework that can be used with the specified versions of Exchange. Supported versions are identified by an X character.

> [!IMPORTANT]
> **Versions of .NET Framework that aren't listed in the tables below are not supported on any version or release of Exchange.** This includes minor and patch-level releases of .NET Framework.

> [!NOTE]
> When upgrading Exchange from an unsupported CU to the current CU and no intermediate CUs are available, you should upgrade to the latest version of .NET that's supported by Exchange first and then immediately upgrade to the current CU. This method doesn't replace the need to keep your Exchange servers up to date and on the latest supported, CU. <br/> Microsoft makes no claim that an upgrade failure will not occur using this method, which may result in the need to contact Microsoft Support Services.

### Exchange 2019

|**.NET Framework**|**CU2**|**RTM, CU1**|
|:-----|:-----|:-----|
|.NET Framework 4.8|X||
|.NET Framework 4.7.2|X|X|

### Exchange 2016

|**.NET Framework**|**CU13**|**CU11, CU12**|**CU10**|**CU8, CU9**|**CU5, CU6, CU7**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|.NET Framework 4.8|X|||||
|.NET Framework 4.7.2|X|X||||
|.NET Framework 4.7.1||X|X|X||
|.NET Framework 4.6.2||||X|X|

### Exchange 2013

|**.NET Framework**|**CU23**|**CU21, CU22**|**CU19, CU20**|**CU16, CU17, CU18**|
|:-----|:-----|:-----|:-----|:-----|
|.NET Framework 4.8|X||||
|.NET Framework 4.7.2|X|X|||
|.NET Framework 4.7.1||X|X||
|.NET Framework 4.6.2|||X|X|

### Exchange 2010 SP3

|**.NET Framework**|**Exchange 2010 SP3**|
|:-----|:-----|
|.NET Framework 4.5|X<sup>1,2</sup>|
|.NET Framework 4.0|X<sup>1,2</sup>|
|.NET Framework 3.5 SP|X|
|.NET Framework 3.5|X<sup>1</sup>|

<sup>1</sup>If you're using Windows Server 2012, the .NET Framework 3.5 must be installed before you can use Exchange 2010 SP3.

<sup>2</sup>Exchange 2010 uses only the .NET Framework 3.5 and the .NET Framework 3.5 SP1 libraries. It doesn't use the .NET Framework 4.5 libraries if they're installed on the server. We support the installation of any major or minor version of the .NET Framework 4.5 (for example, .NET Framework 4.5.1, .NET Framework 4.5.2, etc.) as long as the .NET Framework 3.5 or the .NET Framework 3.5 SP1 are also installed on the server.

## Windows PowerShell

Exchange 2010 requires Windows PowerShell 2.0 on all supported versions of Windows. Exchange 2013 or later requires the version of Windows PowerShell that's included in Windows (unless otherwise specified by an Exchange Setup-enforced prerequisite rule). Exchange does not support the use of Windows Management Framework add-ons on any version of Windows PowerShell or Windows. If there are other installed versions of Windows PowerShell that support side-by-side operation, Exchange will use only the version that it requires.

## Microsoft Management Console

The following table identifies the version of Microsoft Management Console (MMC) that can be used together with each version of Exchange. Supported versions are identified by an X character.

|**MMC**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|
|MMC 3.0|X|X|X|

## Windows Installer

The following table identifies the version of Windows Installer that is used together with each version of Exchange. Supported versions are identified by an X character.

|**Windows Installer**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----|:-----|:-----|
|Windows Installer 4.5|X|X|X|
|Windows Installer 5.0|X|X||
