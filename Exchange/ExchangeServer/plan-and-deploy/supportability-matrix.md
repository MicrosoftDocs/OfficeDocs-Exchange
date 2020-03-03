---
localization_priority: Normal
description: Learn about the support life cycle for Exchange
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.reviewer: 
title: Exchange Server supportability matrix
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms:assetid: dbac2d40-da8b-469f-a265-1d1f948fe446
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange Server supportability matrix

The Exchange Server supportability matrix provides a central source for Exchange administrators to easily locate information about the level of support available for any configuration or required component for supported versions of Microsoft Exchange Server.

## Release model

The following table identifies the release model for each supported version of Exchange. The release model is identified by a check mark ( ![check mark](../media/check-mark.png)).

In Exchange Server 2010 and earlier, each update rollup package (RU) is cumulative with regard to the whole product. When you apply an RU to Exchange Server 2010, you apply all the fixes in that update rollup package, and all the fixes in each earlier update rollup package. When an update or a hotfix is created for Exchange 2010 or earlier, one or more of the binary files included in the update or included in the hotfix are cumulative. They are cumulative in regard to the contents of the files, not in regard to the whole Exchange product. For more information, see [Exchange 2010 Servicing](https://go.microsoft.com/fwlink/p/?linkid=298627).

In Exchange Server 2013 or later, we changed the way we deliver hotfixes and service packs by using a scheduled delivery model. In this model, cumulative updates (CUs) are released quarterly (every three months). A CU is released as a full, self-contained refresh of that version of Exchange, similar to a product upgrade or a service pack release. For more information, see [Updates for Exchange Server](../new-features/updates.md).

|**Servicing release model**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013**|**Exchange 2010**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Cumulative updates (CUs)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|Update rollups (RUs)||||![check mark](../media/check-mark.png)|
|Security hotfixes delivered separately|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||

## Support lifecycle

For more information about the support lifecycle for a specific version of Exchange, or of the Microsoft Windows server or client operating systems, see the [Microsoft Support Lifecycle](https://go.microsoft.com/fwlink/p/?linkid=55839) page. For more information about the Microsoft Support Lifecycle, see the [Microsoft Support Lifecycle Policy FAQ](https://go.microsoft.com/fwlink/p/?linkid=158902).

## Exchange Server 2007 End-of life

Exchange 2007 reached end of support on April 11, 2017, per the [Microsoft Lifecycle Policy](https://go.microsoft.com/fwlink/p/?linkid=833210). As a reminder, after that date there will be no new security updates, non-security updates, free or paid assisted support options or online technical content updates. Furthermore, as adoption of Office 365 accelerates and cloud usage increases, custom support options for Office products will not be available. This includes Exchange Server as well as Office Suites; SharePoint Server; Office Communications Server; Lync Server; Skype for Business Server; Project Server and Visio. Having reached the end of support date for Exchange 2007, we encourage customers to complete their migration and upgrade plans. We recommend customers to leverage deployment benefits provided by Microsoft and Microsoft Certified Partners including [Microsoft FastTrack](https://go.microsoft.com/fwlink/p/?linkid=238431) for cloud migrations, and [Software Assurance Deployment & Planning Services](https://go.microsoft.com/fwlink/p/?linkid=833211) for on-premises upgrades.

## Supported operating system platforms

The following tables identify the operating system platforms on which each version of Exchange can run. Supported platforms are identified the check mark ( ![check mark](../media/check-mark.png)) character.

> [!IMPORTANT]
> Releases of Windows Server and Windows client that aren't listed in the tables below are not supported for use with any version or release of Exchange.

<br/>

|**Server operating system**|**Exchange 2019**|**Exchange 2016 CU3 and later**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2019|![check mark](../media/check-mark.png)|||||
|Windows Server 2016||![check mark](../media/check-mark.png)||||
|Windows Server 2012 R2||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2012||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008 R2 SP1||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008 SP2|||||![check mark](../media/check-mark.png)|

<br/>

> [!NOTE]
> Client operating systems only support the Exchange management tools.

<br/>

|**Client operating system**|**Exchange 2019**|**Exchange 2016 CU3 and later**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Windows 10|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|Windows 8.1||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|Windows 8|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows 7 SP1|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Vista SP2||||![check mark](../media/check-mark.png)|

## Supported Active Directory environments

The following table identifies the Active Directory environments that Exchange can communicate with. Supported environments are identified by a check mark ( ![check mark](../media/check-mark.png)). An Active Directory server refers to both writable global catalog servers and to writable domain controllers. Read-only global catalog servers and read-only domain controllers aren't supported.

<br/>

|**Operating system environment**|**Exchange 2019**|**Exchange 2016 CU12 and later**|**Exchange 2016 CU7 and later**|**Exchange 2016 CU3 to CU6**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3 RU22 or later**|**Exchange 2010 SP3 RU5 - RU21**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2019 Active Directory servers|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||||||
|Windows Server 2016 Active Directory servers|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|Windows Server 2012 R2 Active Directory servers|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2012 Active Directory servers||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008 R2 SP1 Active Directory servers||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008 SP2 Active Directory servers||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2003 SP2 Active Directory servers|||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

<br/>

|**AD forest functional level**|**Exchange 2019**|**Exchange 2016 CU7 and later**|**Exchange 2016 CU3 to CU6**|**Exchange 2016 CU2 and earlier**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3 RU22 or later**|**Exchange 2010 SP3 RU5 - RU21**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2016|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|Windows Server 2012 R2|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2012||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008 R2||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2008|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Server 2003||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

## Web browsers supported for use with the premium version of Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for use together with the premium version of Outlook Web App or Outlook on the web. Supported browsers are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|N/A|N/A|
|Internet Explorer 11|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 10|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 9|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 8|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 7||||![check mark](../media/check-mark.png)|
|Firefox|Current release of Firefox<sup>\*</sup>|Current release of Firefox<sup>\*</sup>|N/A|N/A|
|Firefox 3.0.1 or later||||![check mark](../media/check-mark.png)|
|Firefox 12 or later|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Safari|Current release of Safari|Current release of Safari|N/A|N/A|
|Safari 3.1 or later||||![check mark](../media/check-mark.png)|
|Safari 5.0 or later|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Chrome|Current release of Chrome<sup>\*</sup>|Current release of Chrome<sup>\*</sup>|N/A|N/A|
|Chrome 3.0.195 or later||||![check mark](../media/check-mark.png)|
|Chrome 18 or later|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

<sup>\*</sup> Current release of Firefox or Chrome means the latest version or the immediately previous version.

## Web browsers supported for use with the basic version of Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for use together with the light (basic) version of Outlook Web App or Outlook on the web. Supported browsers are identified by a check mark ( ![check mark](../media/check-mark.png)).

> [!NOTE]
> Outlook Web App Basic (Outlook Web App Light) is supported for use in mobile browsers. However, if rendering or authentication issues occur in a mobile browser, determine whether the issue can be reproduced by using Outlook Web App Light in the full client of a supported browser. For example, test the use of Outlook Web App Light in Safari, Chrome, or Internet Explorer. If the issue can't be reproduced in the full client, we recommend that you contact the mobile device vendor for help. In these cases, we collaborate with the vendor as appropriate.

<br/>

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|Internet Explorer 11|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 10|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 9|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 8|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 7|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Safari|Current release of Safari|Current release of Safari|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Firefox||Current release of Firefox<sup>\*</sup>|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Chrome||Current release of Chrome<sup>\*</sup>|N/A|N/A|
|Opera|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

<sup>\*</sup> Current release of Firefox or Chrome means the latest version or the immediately previous version.

## Web browsers supported for use of S/MIME with Outlook Web App or Outlook on the web

The following table identifies the Web browsers supported for the use of S/MIME together with Outlook Web App or Outlook on the web. Supported browsers are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**Browser**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|Internet Explorer 11|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 10|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 9|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Internet Explorer 8||||![check mark](../media/check-mark.png)|
|Internet Explorer 7||||![check mark](../media/check-mark.png)|

## Clients

The following tables identify the mail clients that are supported for use together with each version of Exchange. Supported clients are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**Client**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Office 365 ProPlus|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|Outlook 2019|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|Outlook 2016|![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Outlook 2013|![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Outlook 2010||![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)<sup>2</sup>|![check mark](../media/check-mark.png)|
|Outlook 2007|||![check mark](../media/check-mark.png)<sup>3</sup>|![check mark](../media/check-mark.png)|
|Outlook for Mac for Office 365||![check mark](../media/check-mark.png)<sup>1</sup>|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Entourage 2008 (EWS)||![check mark](../media/check-mark.png)<sup>4</sup>|![check mark](../media/check-mark.png)<sup>4</sup>|![check mark](../media/check-mark.png)<sup>4</sup>|

<sup>1</sup> Supported with the latest Office service pack and public updates.

<sup>2</sup> Requires Outlook 2010 Service Pack 1 and the latest public update.

<sup>3</sup> Requires Outlook 2007 Service Pack 3 and the latest public update.

<sup>4</sup> EWS only. There is no DAV support for Exchange 2010.

|**Client**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Windows Mobile 10||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Phone 8.1||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Phone 8||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Phone 7.5|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Phone 7|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

## Tools

The following table identifies the version of Microsoft Exchange that can be used together with the Microsoft Exchange Inter-Organization Replication tool (Exscfg.exe; Exssrv.exe). The tool is used to replicate public folder information (including free/busy information) between Exchange organizations. For more information, see [Microsoft Exchange Server Inter-Organization Replication](https://go.microsoft.com/fwlink/?linkid=22455). Supported versions are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**Tool**|**Exchange 2019**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|Inter-Organization Replication tool||||![check mark](../media/check-mark.png)|

## Microsoft .NET Framework

The following tables identify the versions of the Microsoft .NET Framework that can be used with the specified versions of Exchange. Supported versions are identified by a check mark ( ![check mark](../media/check-mark.png)).

> [!IMPORTANT]
> • **Versions of .NET Framework that aren't listed in the tables below are not supported on any version or release of Exchange**. This includes minor and patch-level releases of .NET Framework. <br/><br/>• When upgrading Exchange from an unsupported CU to the current CU and no intermediate CUs are available, you should first upgrade to the latest version of .NET that's supported by Exchange and then immediately upgrade to the current CU. This method doesn't replace the need to keep your Exchange servers up to date and on the latest supported CU. Microsoft makes no claim that an upgrade failure will not occur using this method, which may result in the need to contact Microsoft Support Services.

### Exchange 2019

|**.NET Framework version**|**CU4**|**CU3, CU2**|**CU1, RTM**|
|:-----|:-----:|:-----:|:-----:|
|4.8|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|4.7.2||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

### Exchange 2016

|**.NET Framework version**|**CU15**|**CU14, CU13**|**CU12, CU11**|**CU10**|**CU9, CU8**|**CU7, CU6, CU5**|**CU4, CU3**|**CU2**|**CU1, RTM**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|4.8|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||||||||
|4.7.2||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||||||
|4.7.1|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||||
|4.6.2|||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|4.6.1<sup>\*</sup>|||||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|4.5.2|||||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

<sup>\*</sup> .NET Framework 4.6.1 also requires a hotfix based on the version of Windows. For more information, see [Released: June 2016 Quarterly Exchange Updates](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Released-June-2016-Quarterly-Exchange-Updates/ba-p/604877).

### Exchange 2013

|**.NET Framework version**|**CU23**|**CU21, CU22**|**CU19, CU20**|**CU16, CU17, CU18**|**CU15**|**CU13, CU14**|**CU12 to SP1**|**CU3 to RTM**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|4.8|![check mark](../media/check-mark.png)||||||||
|4.7.2|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||||||
|4.7.1||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||||||
|4.6.2|||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||||
|4.6.1<sup>\*</sup>|||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|||
|4.5.2|||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|4.5.1|||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
|4.5||||||![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

<sup>\*</sup> .NET Framework 4.6.1 also requires a hotfix based on the version of Windows. For more information, see [Released: June 2016 Quarterly Exchange Updates](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Released-June-2016-Quarterly-Exchange-Updates/ba-p/604877).

### Exchange 2010 SP3

|**.NET Framework version**|**Exchange 2010 SP3**|
|:-----|:-----:|
|.NET Framework 4.5|![check mark](../media/check-mark.png)<sup>1,2</sup>|
|.NET Framework 4.0|![check mark](../media/check-mark.png)<sup>1,2</sup>|
|.NET Framework 3.5 SP1|![check mark](../media/check-mark.png)|
|.NET Framework 3.5|![check mark](../media/check-mark.png)<sup>1</sup>|

<sup>1</sup> On Windows Server 2012, you need to install the .NET Framework 3.5 before you can use Exchange 2010 SP3.

<sup>2</sup> Exchange 2010 uses only the .NET Framework 3.5 and the .NET Framework 3.5 SP1 libraries. It doesn't use the .NET Framework 4.5 libraries if they're installed on the server. We support the installation of any version of the .NET Framework 4.5 (for example, .NET Framework 4.5.1, .NET Framework 4.5.2, etc.) as long as the .NET Framework 3.5 or the .NET Framework 3.5 SP1 are also installed on the server.

## Windows PowerShell

- Exchange 2013 or later requires the version of Windows PowerShell that's included in Windows (unless otherwise specified by an Exchange Setup-enforced prerequisite rule).

- Exchange 2010 requires Windows PowerShell 2.0 on all supported versions of Windows.

- Exchange does not support the use of Windows Management Framework add-ons on any version of Windows PowerShell or Windows. If there are other installed versions of Windows PowerShell that support side-by-side operation, Exchange will use only the version that it requires.

## Microsoft Management Console

The following table identifies the version of Microsoft Management Console (MMC) that can be used together with each version of Exchange. Supported versions are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**MMC**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|
|MMC 3.0|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|

## Windows Installer

The following table identifies the version of Windows Installer that is used together with each version of Exchange. Supported versions are identified by a check mark ( ![check mark](../media/check-mark.png)).

|**Windows Installer**|**Exchange 2016**|**Exchange 2013 SP1 and later**|**Exchange 2010 SP3**|
|:-----|:-----:|:-----:|:-----:|
|Windows Installer 4.5|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)|
|Windows Installer 5.0|![check mark](../media/check-mark.png)|![check mark](../media/check-mark.png)||
