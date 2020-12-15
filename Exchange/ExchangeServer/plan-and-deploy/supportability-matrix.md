---
localization_priority: Normal
description: Learn about the support life cycle for Exchange
ms.topic: article
author: msdmaguire
ms.author: dmaguire
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

The following table identifies the release model for each supported version of Exchange.

In Exchange Server 2010 and earlier, each update rollup package (RU) is cumulative. An RU for Exchange Server 2010 includes all fixes for Exchange Server from all previous update rollup packages, so you only need to install the latest RU to apply all of the fixes that were released up to that point. However, individual updates or hotfixes for Exchange 2010 or earlier do not contain all previous fixes for Exchange Server. The updated files that are included in an individual update or hotfix include all updates that were applied only to those specific files by all previous updates, but any other files on Exchange Server will not be updated. For more information, see [Exchange 2010 Servicing](https://docs.microsoft.com/previous-versions/office/exchange-server-2010/).

In Exchange Server 2013 or later, we changed the way we deliver hotfixes and service packs by using a scheduled delivery model. In this model, cumulative updates (CUs) are released quarterly (every three months). Each CU is a full installation of Exchange that includes updates and changes from all previous CUs, so you don't need to install any previous CUs or Exchange Server RTM first. For more information, see [Updates for Exchange Server](../new-features/updates.md).

| Servicing release model | Exchange 2019 | Exchange 2016 | Exchange 2013 | Exchange 2010 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Cumulative updates (CUs)|Yes|Yes|Yes|No|
|Update rollups (RUs)|No|No|No|Yes|
|Security hotfixes delivered separately|Yes|Yes|Yes|No|

> [!NOTE]
> At this time, no additional CUs are planned for Exchange Server 2013 and no additional RUs are planned for Exchange Server 2010.

## Support lifecycle

For more information about the support lifecycle for specific versions of Exchange, Windows Server, or Windows client operating systems, see the [Microsoft Support Lifecycle](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy) page. For more information about the Microsoft Support Lifecycle, see the [Microsoft Support Lifecycle Policy FAQ](https://support.microsoft.com/help/17140/lifecycle-faq-general-policy).

## Exchange Server 2007 End-of life

Exchange 2007 reached end of support on April 11, 2017, per the [Microsoft Lifecycle Policy](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy). There will be no new security updates, non-security updates, free or paid assisted support options, or online technical content updates. Furthermore, as adoption of Microsoft 365 or Office 365 accelerates and cloud usage increases, custom support options for Office products will not be available. This includes Exchange Server, as well as Microsoft Office, SharePoint Server, Office Communications Server, Lync Server, Skype for Business Server, Project Server, and Visio. At this time, we encourage customers to complete their migration and upgrade plans. We recommend that customers leverage deployment benefits provided by Microsoft and Microsoft Certified Partners including [Microsoft FastTrack](https://www.microsoft.com/fasttrack/) for cloud migrations, and [Software Assurance Planning Services](https://partner.microsoft.com/marketing/planning-services) for on-premises upgrades.

## Supported operating system platforms

The following tables identify the operating system platforms on which each version of Exchange can run.

> [!IMPORTANT]
> Releases of Windows Server and Windows client that aren't listed in the tables below are not supported for use with any version or release of Exchange.

<br/>

| Server operating system | Exchange 2019 | Exchange 2016 CU3 and later | Exchange 2016 CU2 and earlier | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2019|Supported|Not supported|Not supported|Not supported|Not supported|
|Windows Server 2016|Not supported|Supported|Not supported|Not supported|Not supported|
|Windows Server 2012 R2|Not supported|Supported|Supported|Supported|Supported|
|Windows Server 2012|Not supported|Supported|Supported|Supported|Supported|
|Windows Server 2008 R2 SP1|Not supported|Not supported|Not supported|Supported|Supported|
|Windows Server 2008 SP2|Not supported|Not supported|Not supported|Not supported|Supported|

<br/>

> [!NOTE]
> Client operating systems only support the Exchange management tools.

<br/>

| Client operating system | Exchange 2019 | Exchange 2016 CU3 and later | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Windows 10|Supported|Supported|Not supported|Not supported|
|Windows 8.1|Not supported|Supported|Supported|Not supported|
|Windows 8|Not supported|Not supported|Supported|Supported|

## Supported Active Directory environments

The following table identifies the Active Directory environments that Exchange can communicate with. An Active Directory server refers to both writable global catalog servers and to writable domain controllers. Read-only global catalog servers and read-only domain controllers are not supported.

<br/>

| Operating system environment | Exchange 2019 | Exchange 2016 CU12 and later | Exchange 2016 CU7 and later | Exchange 2016 CU3 to CU6 | Exchange 2016 CU2 and earlier | Exchange 2013 SP1 and later | Exchange 2010 SP3 RU22 or later | Exchange 2010 SP3 RU5 - RU21 |
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2019 Active Directory servers|Supported|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|Windows Server 2016 Active Directory servers|Supported|Supported|Supported|Supported|Supported|Supported|Supported|Not supported|
|Windows Server 2012 R2 Active Directory servers|Supported|Supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2012 Active Directory servers|Not supported|Supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2008 R2 SP1 Active Directory servers|Not supported|Supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2008 SP2 Active Directory servers|Not supported|Not supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2003 SP2 Active Directory servers|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|Supported|

<br/>

| AD forest functional level | Exchange 2019 | Exchange 2016 CU7 and later | Exchange 2016 CU3 to CU6 | Exchange 2016 CU2 and earlier | Exchange 2013 SP1 and later | Exchange 2010 SP3 RU22 or later | Exchange 2010 SP3 RU5 - RU21 |
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|Windows Server 2016|Supported|Supported|Supported|Supported|Supported|Supported|Not supported|
|Windows Server 2012 R2|Supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2012|Not supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2008 R2|Not supported|Supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2008|Not supported|Not supported|Supported|Supported|Supported|Supported|Supported|
|Windows Server 2003|Not supported|Not supported|Not supported|Supported|Supported|Supported|Supported|

## Web browsers supported for use with the premium version of Outlook Web App or Outlook on the web

The following table identifies the web browsers supported for use together with the premium version of Outlook Web App or Outlook on the web.

| Browser | Exchange 2019 | Exchange 2016 | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|Supported|Supported|Not supported|Not supported|
|Internet Explorer 11|Supported|Supported|Supported|Supported|
|Internet Explorer 10|Not supported|Not supported|Supported|Supported|
|Internet Explorer 9|Not supported|Not supported|Supported|Supported|
|Internet Explorer 8|Not supported|Not supported|Supported|Supported|
|Internet Explorer 7|Not supported|Not supported|Not supported|Supported|
|Firefox|Current release of Firefox<sup>\*</sup>|Current release of Firefox<sup>\*</sup>|Not supported|Not supported|
|Firefox 3.0.1 or later|Not supported|Not supported|Not supported|Supported|
|Firefox 12 or later|Not supported|Not supported|Supported|Supported|
|Safari|Current release of Safari|Current release of Safari|Not supported|Not supported|
|Safari 3.1 or later|Not supported|Not supported|Not supported|Supported|
|Safari 5.0 or later|Not supported|Not supported|Supported|Supported|
|Chrome|Current release of Chrome<sup>\*</sup>|Current release of Chrome<sup>\*</sup>|Not supported|Not supported|
|Chrome 3.0.195 or later|Not supported|Not supported|Not supported|Supported|
|Chrome 18 or later|Not supported|Not supported|Supported|Supported|

<sup>\*</sup> Current release of Firefox or Chrome refers to the latest version or the immediately previous version.

## Web browsers supported for use with the basic version of Outlook Web App or Outlook on the web

The following table identifies the web browsers supported for use together with the light (basic) version of Outlook Web App or Outlook on the web.

> [!NOTE]
> Outlook Web App Basic (Outlook Web App Light) is supported for use in mobile browsers. However, if rendering or authentication issues occur in a mobile browser, determine whether the issue can be reproduced by using Outlook Web App Light in the full client of a supported browser. For example, test the use of Outlook Web App Light in Safari, Chrome, or Internet Explorer. If the issue can't be reproduced in the full client, we recommend that you contact the mobile device vendor for help. In these cases, we collaborate with the vendor as appropriate.

<br/>

| Browser | Exchange 2019 | Exchange 2016 | Exchange 2013 | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|Supported|Supported|Not supported|Not supported|
|Internet Explorer 11|Supported|Supported|Supported|Supported|
|Internet Explorer 10|Not supported|Not supported|Supported|Supported|
|Internet Explorer 9|Not supported|Not supported|Supported|Supported|
|Internet Explorer 8|Not supported|Not supported|Supported|Supported|
|Internet Explorer 7|Not supported|Not supported|Supported|Supported|
|Safari|Current release of Safari|Current release of Safari|Supported|Supported|
|Firefox|Not supported|Current release of Firefox<sup>\*</sup>|Supported|Supported|
|Chrome|Not supported|Current release of Chrome<sup>\*</sup>|Not supported|Not supported|
|Opera|Not supported|Not supported|Supported|Supported|

<sup>\*</sup> Current release of Firefox or Chrome refers to the latest version or the immediately previous version.

## Web browsers supported for use of S/MIME with Outlook Web App or Outlook on the web

The following table identifies the web browsers supported for the use of S/MIME together with Outlook Web App or Outlook on the web.

| Browser | Exchange 2019 | Exchange 2016 | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft Edge|Supported|Not supported|Not supported|Not supported|
|Internet Explorer 11|Supported|Supported|Supported|Supported|
|Internet Explorer 10|Not supported|Not supported|Supported|Supported|
|Internet Explorer 9|Not supported|Not supported|Supported|Supported|
|Internet Explorer 8|Not supported|Not supported|Not supported|Supported|
|Internet Explorer 7|Not supported|Not supported|Not supported|Supported|

## Clients

The following tables identify the mail clients that are supported for use together with each version of Exchange.

| Client | Exchange 2019 | Exchange 2016 | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|:-----:|
|Microsoft 365 Apps for enterprise|Supported|Supported|Supported|Not supported|
|Outlook 2019|Supported|Supported|Supported|Not supported|
|Outlook 2016|Supported<sup>1</sup>|Supported<sup>1</sup>|Supported|Supported|
|Outlook 2013|Supported<sup>1</sup>|Supported<sup>1</sup>|Supported|Supported|
|Outlook 2010|Not supported|Supported<sup>1</sup>|Supported<sup>2</sup>|Supported|
|Outlook 2007|Not supported|Not supported|Supported<sup>3</sup>|Supported|
|Outlook for Mac for Office 365|Not supported|Supported<sup>1</sup>|Supported|Supported|
|Entourage 2008 (EWS)|Not supported|Supported<sup>4</sup>|Supported<sup>4</sup>|Supported<sup>4</sup>|

<sup>1</sup> Requires the latest Office service pack and the latest public update.

<sup>2</sup> Requires Outlook 2010 Service Pack 1 and the latest public update.

<sup>3</sup> Requires Outlook 2007 Service Pack 3 and the latest public update.

<sup>4</sup> EWS only. There is no DAV support for Exchange 2010.

## Microsoft .NET Framework

The following tables identify the versions of the Microsoft .NET Framework that can be used with the specified versions of Exchange.

> [!IMPORTANT]
> Versions of the .NET Framework that aren't listed in the tables below are not supported on any version of Exchange. This includes minor and patch-level releases of the .NET Framework.
>
> If you are upgrading Exchange Server from an unsupported CU to the current CU and no intermediate CUs are available, you should first upgrade to the latest version of .NET that's supported by your version of Exchange Server and then immediately upgrade to the current CU. This method doesn't replace the need to keep your Exchange servers up to date and on the latest supported CU. Microsoft makes no claim that an upgrade failure will not occur using this method, which may result in the need to contact Microsoft Support Services.

### Exchange 2019

| .NET Framework version | CU8 to CU4 | CU3, CU2 | CU1, RTM |
|:-----|:-----:|:-----:|:-----:|
|4.8|Supported|Supported|Not supported|
|4.7.2|Not supported|Supported|Supported|

### Exchange 2016

| .NET Framework version | CU19 to CU15 | CU14, CU13 | CU12, CU11 | CU10 | CU9, CU8 | CU7, CU6, CU5 | CU4, CU3 | CU2 | CU1, RTM |
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|4.8|Supported|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|4.7.2|Not supported|Supported|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|4.7.1|Not supported|Not supported|Supported|Supported|Supported|Not supported|Not supported|Not supported|Not supported|
|4.6.2|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|Not supported|Not supported|
|4.6.1<sup>\*</sup>|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Not supported|
|4.5.2|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|

<sup>\*</sup> .NET Framework 4.6.1 also requires a hotfix, and a different hotfix is required for different versions of Windows. For more information, see [Released: June 2016 Quarterly Exchange Updates](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Released-June-2016-Quarterly-Exchange-Updates/ba-p/604877).

### Exchange 2013

| .NET Framework version | CU23 | CU21, CU22 | CU19, CU20 | CU16, CU17, CU18 | CU15 | CU13, CU14 | CU12 to SP1 | CU3 to RTM |
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|4.8|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|4.7.2|Supported|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|4.7.1|Not supported|Supported|Supported|Not supported|Not supported|Not supported|Not supported|Not supported|
|4.6.2|Not supported|Not supported|Supported|Supported|Supported|Not supported|Not supported|Not supported|
|4.6.1<sup>\*</sup>|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Not supported|Not supported|
|4.5.2|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|Not supported|
|4.5.1|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|Not supported|
|4.5|Not supported|Not supported|Not supported|Not supported|Not supported|Supported|Supported|Supported|

<sup>\*</sup> .NET Framework 4.6.1 also requires a hotfix, and a different hotfix is required for different versions of Windows. For more information, see [Released: June 2016 Quarterly Exchange Updates](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Released-June-2016-Quarterly-Exchange-Updates/ba-p/604877).

### Exchange 2010 SP3

| .NET Framework version | Exchange 2010 SP3 |
|:-----|:-----:|
|.NET Framework 4.5|Supported<sup>1,2</sup>|
|.NET Framework 4.0|Supported<sup>1,2</sup>|
|.NET Framework 3.5 SP1|Supported|
|.NET Framework 3.5|Supported<sup>1</sup>|

<sup>1</sup> On Windows Server 2012, you need to install the .NET Framework 3.5 before you can use Exchange 2010 SP3.

<sup>2</sup> Exchange 2010 uses only the .NET Framework 3.5 and the .NET Framework 3.5 SP1 libraries. It doesn't use the .NET Framework 4.5 libraries if they're installed on the server. We support the installation of any version of the .NET Framework 4.5 (for example, .NET Framework 4.5.1, .NET Framework 4.5.2, etc.) as long as the .NET Framework 3.5 or the .NET Framework 3.5 SP1 is also installed on the server.

## Windows PowerShell

- Exchange 2013 or later requires the version of Windows PowerShell that's included in Windows (unless otherwise specified by an Exchange Setup-enforced prerequisite rule).

- Exchange 2010 requires Windows PowerShell 2.0 on all supported versions of Windows.

- Exchange does not support the use of Windows Management Framework add-ons on any version of Windows PowerShell or Windows. 

- If there are other installed versions of Windows PowerShell or PowerShell Core that support side-by-side operation, Exchange will use only the version that it requires.

## Microsoft Management Console

The following table identifies the version of Microsoft Management Console (MMC) that can be used together with each version of Exchange.

| MMC | Exchange 2016 | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|
|MMC 3.0|Supported|Supported|Supported|

## Windows Installer

The following table identifies the version of Windows Installer that is used together with each version of Exchange.

| Windows Installer | Exchange 2016 | Exchange 2013 SP1 and later | Exchange 2010 SP3 |
|:-----|:-----:|:-----:|:-----:|
|Windows Installer 4.5|Supported|Supported|Supported|
|Windows Installer 5.0|Supported|Supported|Not supported|
