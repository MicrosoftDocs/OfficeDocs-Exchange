---
localization_priority: Critical
monikerRange: exchserver-2016 || exchserver-2019
description: 'Summary: Learn about what you need in your environment before you install Exchange Server 2016 or Exchange Server 2019.'
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid:
ms.reviewer: 
title: Exchange Server 2019 system requirements, Exchange 2019 Requirements, Exchange 2019 Memory Requirements, Exchange 2019 Client Compatibility
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Exchange Server system requirements

::: moniker range="exchserver-2019"
Before you install Exchange Server 2019, we recommend that you review this topic to ensure your network, hardware, software, clients, and other elements meet the requirements for Exchange 2019. Also, make sure you understand the coexistence scenarios that are supported for Exchange 2019 and earlier versions of Exchange.

To actually install Exchange 2019, see [Deploy new installations of Exchange](https://docs.microsoft.com/Exchange/plan-and-deploy/deploy-new-installations/deploy-new-installations?view=exchserver-2019).

## Supported coexistence scenarios for Exchange 2019

The supported coexistence scenarios between Exchange 2019 and earlier versions of Exchange are described in the following table:

|**Exchange version**|**Exchange 2019 organization coexistence**|
|:-----|:-----|
|Exchange 2010 and earlier versions|Not supported|
|Exchange 2013|Supported with Exchange 2013 Cumulative Update 21 (CU21) or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|
|Exchange 2016|Supported with Exchange 2016 CU11 or later on all Exchange 2016 servers in the organization, including Edge Transport servers.
|Mixed Exchange 2013 and Exchange 2016 organization|Supported if all Exchange 2013 and Exchange 2016 servers in the organization meet the requirements as previously described in this table.|

## Supported hybrid deployment scenarios for Exchange 2019

Exchange 2019 supports hybrid deployments with Office 365 tenants that have been upgraded to the latest version of Office 365. For more information about specific hybrid deployments, see [Hybrid deployment prerequisites](https://docs.microsoft.com/Exchange/hybrid-deployment-prerequisites).

## Network and directory server requirements for Exchange 2019

The requirements for the network and the directory servers in your Exchange 2019 organization are described in the following table:

|**Component**|**Requirement**|
|:-----|:-----|
|Domain controllers|All domain controllers in the forest need to be running one of the following versions of Windows Server: <br/>• Windows Server 2019 Standard or Datacenter <br/>• Windows Server 2016 Standard or Datacenter <br/>• Windows Server 2012 R2 Standard or Datacenter|
|Active Directory forest|The Active Directory forest functional level is **Windows Server 2012 R2** or higher.|
|Active Directory site|The Active Directory site where you install the Exchange Server must contain at least one writeable domain controller that's also a global catalog server, or the installation will fail. Furthermore, you can't install the Exchange server and then remove the domain controller from the Active Directory site.|
|DNS namespace|Exchange 2019 supports the following DNS namespaces:  <br/> • Contiguous  <br/> • Noncontiguous  <br/> • Single label domains  <br/> • Disjoint  <br/> For more information about DNS namespaces that are supported by Exchange, see [KB2269838](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=2269838).|
|IPv6|Exchange 2013 and later support IPv6 only when IPv4 is also installed and enabled on the Exchange server. <br/> If you deploy Exchange in this configuration, and your network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see [IPv6 Support in Exchange 2013](https://docs.microsoft.com/exchange/ipv6-support-in-exchange-2013-exchange-2013-help).|

## Directory server architecture for Exchange 2019

Active Directory domain controllers on 64-bit hardware with a 64-bit version of Windows Server will increase directory service performance for Exchange 2019.

### Installing Exchange 2019 on directory servers

For security and performance reasons, we don't recommend installing Exchange 2019 on Active Directory directory servers. Only install Exchange 2019 on member servers.

To learn more about the issues that you'll encounter when you install Exchange on a directory server, see [Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]](../plan-and-deploy/deployment-ref/ms-exch-setupreadiness-warninginstallexchangerolesondomaincontroller.md). After Exchange is installed, changing the server role from a member server to a directory server or vice-versa isn't supported.

## Hardware requirements for Exchange 2019

For information about deploying Exchange in a virtualized environment, see [Exchange Server virtualization](../plan-and-deploy/virtualization.md).

 |**Component**|**Requirement**|**Notes**|
|:-----|:-----|:-----|
|Processor|Either of the following types of 64-bit processors: <br/>• Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T). <br/>• AMD processor that supports the AMD64 platform. <br/><br/> **Notes**: <br/>• Intel Itanium IA64 processors are not supported. <br/>• Recommended supported processor sockets is up to 2 on physical machines.|See the [Supported operating systems for Exchange 2019](#supported-operating-systems-for-exchange-2019) section later in this topic for supported operating systems.|
|Memory|Varies by Exchange server role: <br/>• **Mailbox**: 128 GB minimum recommended <br/>• **Edge Transport**: 64 GB minimum recommended.|Exchange 2019 has large memory support (up to 256 GB).|
|Paging file size|Set the paging file minimum and maximum value to the same size: 25% of installed memory.|None|
|Disk space|• At least 30 GB of free space on the drive where you're installing Exchange. <br/>• At least 200 MB of free space on the system drive. <br/>• At least 500 MB of free space on the drive that contains the message queue database.|None|
|Screen resolution|1024 x 768 pixels (XGA) or higher|None|
|File system|**NTFS**: Required on partitions that contain the following types of files: <br/> • The System partition. <br/> • Exchange binaries. <br/>• Files generated by Exchange diagnostic logging. <br/> • Transport database files (for example, the mail queue database). <br/><br/> **ReFS**: Supported on partitions that contain the following types of Exchange files: <br/> • Mailbox databases. <br/>• Transaction logs.|None|

## Supported operating systems for Exchange 2019

|**Exchange component**|**Requirement**|
|:-----|:-----|
|Mailbox and Edge Transport server roles|Windows Server 2019 Standard or Datacenter|
|Management tools|One of the following versions of Windows:  <br/>• Windows Server 2019 Standard or Datacenter <br/>• 64-bit edition of Windows 10|

**Notes**:

- Installing Exchange 2019 on a computer that's running Windows Server Core is fully supported and recommended. The Desktop Experience feature is no longer required.

- Installing Exchange 2019 on a computer that's running Nano Server isn't supported.

### Supported Powershell versions for Exchange 2019 servers

Exchange 2019 servers support the version of PowerShell that's included in the release of Windows Server where Exchange is installed. Don't install stand-alone downloads of WMF or PowerShell on Exchange servers.

### Installing other software on Exchange 2019 servers

We don't support installing Office client or Office server software on Exchange servers (for example, SharePoint Server, Skype for Business Server, Office Online Server, or Project Server). Other software that you want to install on an Exchange 2019 server needs to be designed to run on the same computer as Exchange Server.

## Supported .NET Framework versions for Exchange 2019

We strongly recommend that you use the latest version of the .NET Framework that's supported by the release of Exchange you're installing.

> [!IMPORTANT]
> • **Releases of .NET Framework that aren't listed in the table below aren't supported on any release of Exchange 2019**. This includes minor and patch-level releases of .NET Framework. <br/><br/>• The complete prerequisite list for Exchange 2019 is available [here](https://docs.microsoft.com/exchange/plan-and-deploy/prerequisites?view=exchserver-2019).

|**Exchange 2019 version**|**.NET Framework 4.8**|**.NET Framework 4.7.2**|
|:-----|:-----|:-----|
|CU4, CU5|Supported||
|CU2, CU3|Supported|Supported|
|RTM, CU1||Supported|

## Supported clients (with latest updates) in Exchange 2019

- Office 365 ProPlus

- Outlook 2019

- Outlook 2016

- Outlook 2013

- Outlook for Mac for Office 365

- Outlook 2016 for Mac

> [!IMPORTANT]
> You need [KB3140245](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-default-secure-protocols-in-wi) to apply registry keys to enable TLS 1.1 & 1.2 support for Windows 7. Otherwise, Outlook 2013 and 2016 will not work on Windows 7.

## Lync/Skype For Business Server integration with Exchange 2019

If you're integrating Lync presence and instant messaging with Exchange Server, Lync Server 2013 Cumulative Update 10 or later is required. If you're integrating Skype for Business presence and instant messaging with Exchange Server, Skype for Business Server Cumulative Update 7 or later is required.
::: moniker-end

::: moniker range="exchserver-2016"
Before you install Exchange Server 2016, we recommend that you review this topic to ensure your network, hardware, software, clients, and other elements meet the requirements for Exchange 2016. Also, make sure you understand the coexistence scenarios that are supported for Exchange 2016 and earlier versions of Exchange.

To actually install Exchange 2016, see [Deploy new installations of Exchange](https://docs.microsoft.com/Exchange/plan-and-deploy/deploy-new-installations/deploy-new-installations?view=exchserver-2016).

## Supported coexistence scenarios for Exchange 2016

The following table lists the scenarios in which coexistence between Exchange 2016 and earlier versions of Exchange is supported.

|**Exchange version**|**Exchange organization coexistence**|
|:-----|:-----|
|Exchange 2007 and earlier versions|Not supported|
|Exchange 2010|Supported with Update Rollup 11 for Exchange 2010 SP3 or later on all Exchange 2010 servers in the organization, including Edge Transport servers.|
|Exchange 2013|Supported with Exchange 2013 Cumulative Update 10 or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|
|Mixed Exchange 2010 and Exchange 2013 organization|Supported with the following minimum versions of Exchange: <br/>• Update Rollup 11 Exchange 2010 SP3 or later on all Exchange 2010 servers in the organization, including Edge Transport servers. <br/>• Exchange 2013 Cumulative Update 10 or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|

## Supported hybrid deployment scenarios for Exchange 2016

Exchange 2016 supports hybrid deployments with Office 365 tenants that have been upgraded to the latest version of Office 365. For more information about specific hybrid deployments, see [Hybrid Deployment Prerequisites](https://docs.microsoft.com/exchange/hybrid-deployment-prerequisites).

## Network and directory server requirements for Exchange 2016

The following table lists the requirements for the network and the directory servers in your Exchange 2016 organization.

|**Component**|**Requirement**|
|:-----|:-----|
|Domain controllers|All domain controllers in the forest need to be running one of the following versions of Windows Server: <br/>• Windows Server 2019 Standard or Datacenter <br/>• Windows Server 2016 Standard or Datacenter <br/>• Windows Server 2012 R2 Standard or Datacenter <br/>• Windows Server 2012 Standard or Datacenter <br/>• Windows Server 2008 R2 Standard or Enterprise <br/>• Windows Server 2008 R2 Datacenter RTM or later|
|Active Directory forest|The Active Directory forest functional level is Windows Server 2008 R2 or higher.|
|Active Directory site|• The Active Directory site where you install the Exchange Server must contain at least one writeable domain controller that's also a global catalog server, or the installation will fail. <br/>• You can't install the Exchange server and then remove the domain controller from the Active Directory site.|
|DNS namespace support|Exchange 2016 supports the following domain name system (DNS) namespaces: <br/>• Contiguous <br/>• Noncontiguous <br/>• Single label domains <br/>• Disjoint <br/> For more information about DNS namespaces supported by Exchange, see Microsoft Knowledge Base article 2269838, [Microsoft Exchange compatibility with Single Label Domains, Disjoined Namespaces, and Discontiguous Namespaces](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=2269838).|
|IPv6 support|In Exchange 2016, IPv6 is supported only when IPv4 is also installed and enabled. If Exchange 2016 is deployed in this configuration, and the network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see [IPv6 Support in Exchange 2013](https://technet.microsoft.com/library/33543023-eb9a-4102-b990-84a818a52814.aspx).|

## Directory server architecture for Exchange 2016

The use of 64-bit Active Directory domain controllers increases directory service performance for Exchange 2016.

> [!NOTE]
> In multi-domain environments, on Windows Server 2008 domain controllers that have the Active Directory language locale set to Japanese (ja-jp), your servers may not receive some attributes that are stored on an object during inbound replication. For more information, see [KB949189](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=949189).

### Installing Exchange 2016 on directory servers

For security and performance reasons, we recommend that you install Exchange 2016 only on member servers and not on Active Directory directory servers. To learn about the issues you can face when installing Exchange 2016 on a directory server, see [Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]](deployment-ref/ms-exch-setupreadiness-warninginstallexchangerolesondomaincontroller.md). After Exchange 2016 is installed, changing its role from a member server to a directory server, or vice versa, isn't supported.

## Hardware requirements for Exchange 2016

For information about deploying Exchange in a virtualized environment, see [Exchange Server virtualization](virtualization.md).

|**Component**|**Requirement**|**Notes**|
|:-----|:-----|:-----|
|Processor|Either of the following types of 64-bit processors: <br/>• Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T). <br/>• AMD processor that supports the AMD64 platform. <br/><br/> **Note**: Intel Itanium IA64 processors are not supported.|For more information, see [Sizing Exchange 2016 Deployments](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Ask-the-Perf-Guy-Sizing-Exchange-2016-Deployments/ba-p/603970). <br/> See the [Supported operating systems for Exchange 2016](#supported-operating-systems-for-exchange-2016) section later in this topic for supported operating systems.|
|Memory|Varies by Exchange server role: <br/>• **Mailbox**: 8 GB minimum. <br/>• **Edge Transport**: 4 GB minimum.|For more information, see [Sizing Exchange 2016 Deployments](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Ask-the-Perf-Guy-Sizing-Exchange-2016-Deployments/ba-p/603970).|
|Paging file size|Set the paging file minimum and maximum value to the same size: <br/>• **Less than 32 GB of RAM installed**: Physical RAM plus 10MB, up to a maximum value of 32GB (32,778MB). <br/>• **32 GB of RAM or more installed**: 32GB|None|
|Disk space|• At least 30 GB of free space on the drive where you're installing Exchange, plus an additional 500 MB for each Unified Messaging (UM) language pack that you plan to install. <br/>• At least 200 MB of free space on the System drive. <br/>• At least 500 MB of free space on the drive that contains the message queue database.|For more information, see [Sizing Exchange 2016 Deployments](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Ask-the-Perf-Guy-Sizing-Exchange-2016-Deployments/ba-p/603970).|
|Drive|DVD-ROM drive, local or network accessible.|None|
|Screen resolution|1024 x 768 pixels (XGA) or higher|None|
|File format|**NTFS**: Required on partitions that contain the following types of files: <br/> • The System partition. <br/> • Exchange binaries. <br/>• Files generated by Exchange diagnostic logging. <br/> • Transport database files (for example, the mail queue database). <br/><br/> **ReFS**: Supported on partitions that contain the following types of Exchange files: <br/> • Mailbox databases. <br/>• Transaction logs. <br/>• Content indexing files.|None|

## Supported operating systems for Exchange 2016

**Important**: We don't support the installation of Exchange 2016 on a computer that's running Windows Server Core or Nano Server. The Windows Server Desktop Experience feature needs to be installed. To install Exchange 2016, you need to do one of the following to install the Desktop Experience on Windows Server prior to starting Exchange 2016 Setup:

- **Windows Server 2012 and Windows Server 2012 R2**: Run the following command in Windows PowerShell

  ```PowerShell
  Install-WindowsFeature Server-Gui-Mgmt-Infra,Server-Gui-Shell -Restart
  ```

- **Windows Server 2016**: Install Windows Server 2016 and choose the **Desktop Experience** installation option. If a computer is running Windows Server 2016 Core mode and you want to install Exchange 2016 on it, you'll need to reinstall the operating system and choose the **Desktop Experience** installation option.

|**Component**|**Requirement**|
|:-----|:-----|
|Mailbox and Edge Transport server roles|• Windows Server 2016 Standard or Datacenter<sup>\*</sup> <br/>• Windows Server 2012 R2 Standard or Datacenter <br/>• Windows Server 2012 Standard or Datacenter|
|Management tools|One of the following versions of Windows: <br/>• Windows Server 2016 Standard or Datacenter<sup>\*</sup> <br/>• Windows Server 2012 R2 Standard or Datacenter <br/>• Windows Server 2012 Standard or Datacenter <br/>• 64-bit edition of Windows 10 <br/>• 64-bit edition of Windows 8.1|

<sup>\*</sup> Requires Exchange Server 2016 Cumulative Update 3 or later.

### Supported Windows Management Framework versions for Exchange 2016

Exchange 2016 only supports the version of Windows Management Framework that's built in to the release of Windows that you're installing Exchange on. Don't install versions of Windows Management Framework that are made available as stand-alone downloads on servers running Exchange.

### Installing other software on Exchange 2016 servers

We don't support installing Office clients or other Office server products (for example, SharePoint Server, Skype for Business Server, Office Online Server, or Project Server) on Exchange 2016 servers. Software that you want to install on an Exchange 2016 server needs to be designed to run on the same computer as Exchange Server.

## Supported .NET Framework versions for Exchange 2016

We strongly recommend that you use the latest version of .NET Framework that's supported by the release of Exchange you're installing.

> [!IMPORTANT]
> • **Releases of .NET Framework that aren't listed in the table below are not supported on any release of Exchange 2016**. This includes minor and patch-level releases of .NET Framework. <br/><br/>• The complete prerequisite list for Exchange 2016 is available [here](https://docs.microsoft.com/exchange/plan-and-deploy/prerequisites?view=exchserver-2016).

<br/>

|**Exchange 2016 version**|**.NET Framework 4.8**|**.NET Framework 4.7.2**|**.NET Framework 4.7.1**|**.NET Framework 4.6.2**|
|:-----|:-----:|:-----:|:-----:|:-----:|
|CU15, CU16|Supported||||
|CU13, CU14|Supported|Supported|||
|CU11, CU12||Supported|Supported||
|CU10|||Supported||
|CU8, CU9|||Supported|Supported|
|CU5, CU6, CU7||||Supported|

**Note**: For older versions, see [Exchange Server supportability matrix](supportability-matrix.md#microsoft-net-framework).

## Supported clients (with latest updates) in Exchange 2016

- Office 365 ProPlus

- Outlook 2019

- Outlook 2016

- Outlook 2013

- Outlook 2010 SP2

- Outlook 2016 for Mac

- Outlook for Mac for Office 365
::: moniker-end

## Exchange third-party clients

Exchange Server offers several well-known protocols, and publishes APIs that third-party vendors often write clients for.

Microsoft makes no warranties, expressed or implied, as to the overall suitability, fitness, compatibility, or security of clients that are created by third-party developers.

If you want to use a third-party client that uses our protocols or APIs, we recommend that you thoroughly review and test all considerations (functionality, security, maintenance, management, and so on) before you deploy the client in the enterprise workspace. We also recommend that you make sure that the third-party vendor offers an appropriate Enterprise Support Agreement (ESA).
