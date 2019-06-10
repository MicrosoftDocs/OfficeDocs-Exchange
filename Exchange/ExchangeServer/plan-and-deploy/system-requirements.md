---
localization_priority: Critical
monikerRange: exchserver-2016 || exchserver-2019
description: 'Summary: Learn about what you need in your environment before you install Exchange Server 2016 or Exchange Server 2019.'
ms.topic: conceptual
author: chrisda
ms.author: chrisda
ms.assetid:
ms.date:
ms.reviewer: 
title: Exchange Server 2019 system requirements, Exchange 2019 Requirements, Exchange 2019 Memory Requirements, Exchange 2019 Client Compatibility
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Exchange Server system requirements

::: moniker range="exchserver-2019"
Before you install Exchange Server 2019, we recommend that you review this topic to ensure your network, hardware, software, clients, and other elements meet the requirements for Exchange 2019. Also, make sure you understand the coexistence scenarios that are supported for Exchange 2019 and earlier versions of Exchange.

To actually install Exchange 2019, see [Deploy new installations of Exchange](https://docs.microsoft.com/Exchange/plan-and-deploy/deploy-new-installations/deploy-new-installations?view=exchserver-2019).

## Supported coexistence scenarios

The supported coexistence scenarios between Exchange 2019 and earlier versions of Exchange are described in the following table:

|**Exchange version**|**Exchange 2019 organization coexistence**|
|:-----|:-----|
|Exchange 2010 and earlier versions|Not supported|
|Exchange 2013|Supported with Exchange 2013 Cumulative Update 21 (CU21) or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|
|Exchange 2016|Supported with Exchange 2016 CU11 or later on all Exchange 2016 servers in the organization, including Edge Transport servers.
|Mixed Exchange 2013 and Exchange 2016 organization|Supported if all Exchange 2013 and Exchange 2016 servers in the organization meet the requirements as previously described in this table.|

## Supported hybrid deployment scenarios

Exchange 2019 supports hybrid deployments with Office 365 tenants that have been upgraded to the latest version of Office 365. For more information about specific hybrid deployments, see [Hybrid deployment prerequisites](https://docs.microsoft.com/Exchange/hybrid-deployment-prerequisites).

## Network and directory servers

The requirements for the network and the directory servers in your Exchange 2019 organization are described in the following table:

|**Component**|**Requirement**|
|:-----|:-----|
|Domain controllers|All domain controllers in the forest need to be running one of the following versions of Windows Server: <br/>• Windows Server 2019 Standard or Datacenter <br/>• Windows Server 2016 Standard or Datacenter <br/>• Windows Server 2012 R2 Standard or Datacenter|
|Active Directory forest|The Active Directory forest functional level is **Windows Server 2012 R2** or higher.|
|Active Directory site|The Active Directory site where you install the Exchange Server must contain at least one writeable domain controller that's also a global catalog server, or the installation will fail. Furthermore, you can't install the Exchange server and then remove the domain controller from the Active Directory site.|
|DNS namespace|Exchange 2019 supports the following DNS namespaces:  <br/> • Contiguous  <br/> • Noncontiguous  <br/> • Single label domains  <br/> • Disjoint  <br/> For more information about DNS namespaces that are supported by Exchange, see [KB2269838](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=2269838).|
|IPv6|Exchange 2013 and later support IPv6 only when IPv4 is also installed and enabled on the Exchange server. <br/> If you deploy Exchange in this configuration, and your network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see [IPv6 Support in Exchange 2013](https://technet.microsoft.com/library/33543023-eb9a-4102-b990-84a818a52814.aspx).|

## Directory server architecture

Active Directory domain controllers on 64-bit hardware with a 64-bit version of Windows Server will increase directory service performance for Exchange 2019.

### Installing Exchange on directory servers

For security and performance reasons, we don't recommend installing Exchange 2019 on Active Directory directory servers. Only install Exchange 2019 on member servers.

To learn more about the issues that you'll encounter when you install Exchange on a directory server, see [Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]](../plan-and-deploy/deployment-ref/ms-exch-setupreadiness-warninginstallexchangerolesondomaincontroller.md). After Exchange is installed, changing the server role from a member server to a directory server or vice-versa isn't supported.

## Hardware

For information about deploying Exchange in a virtualized environment, see [Exchange Server virtualization](../plan-and-deploy/virtualization.md).

 |**Component**|**Requirement**|**Notes**|
|:-----|:-----|:-----|
|Processor|Either of the following types of 64-bit processors: <br/>• Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T). <br/>• AMD processor that supports the AMD64 platform. <br/> **Note**: Intel Itanium IA64 processors aren't supported. <br/> **Note**: Recommended Supported Processor Sockets is up to 2 on physical machines. |See the [Operating system](#operating-system) section later in this topic for supported operating systems.|
|Memory|Varies depending on Exchange roles that are installed: <br/>• **Mailbox**: 128GB minimum recommended <br/>• **Edge Transport**: 64GB minimum recommended.  <br/> Note that Exchange 2019 has large memory support (up to 256 GB).||
|Paging file size| Set the page file to a size equal to 25% of installed memory.|None|
|Disk space|• At least 30GB of free space on the drive where you're installing Exchange. <br/>• At least 200MB of free space on the system drive. <br/>• At least 500MB on the drive containing the message queue database.||
|Screen resolution|1024 x 768 pixels (XGA) or higher|None|
|File system|NTFS is required on partitions that contain the following types of files: <br/> • The System partition. <br/> • Exchange binaries. <br/>• Files generated by Exchange diagnostic logging. <br/> • Transport database files (for example, the mail queue database). <br/> Optionally, you can use ReFS on the partitions that contain the following types of files: <br/> • Mailbox databases and transaction logs.|None|

## Operating system

The supported operating systems for Exchange 2019 are described in the following table:

|**Exchange component**|**Requirement**|
|:-----|:-----|
|Mailbox and Edge Transport server roles|Windows Server 2019 Standard or Datacenter|
|Management tools|One of the following versions of Windows:  <br/>• Windows Server 2019 Standard or Datacenter <br/>• 64-bit edition of Windows 10|

**Notes**:

- Installing Exchange 2019 on a computer that's running Windows Server Core is fully supported and recommended. The Desktop Experience feature is no longer required.

- Installing Exchange 2019 on a computer that's running Nano Server isn't supported.

**Supported Powershell versions for Exchange 2019 servers**

Exchange 2019 servers support the version of PowerShell that's included in the release of Windows Server where Exchange is installed. Don't install stand-alone downloads of WMF or PowerShell on Exchange servers.

 **Installing other software on Exchange 2019 servers**

We don't support installing Office client or Office server software on Exchange servers (for example, SharePoint Server, Skype for Business Server, Office Online Server, or Project Server). Other software that you want to install on Exchange 2019 servers needs to be designed to run on the same computer as Exchange.

## .NET Framework

We strongly recommend that you use the latest version of the .NET Framework that's supported by the release of Exchange you're installing.

> [!IMPORTANT]
> **Releases of .NET Framework that aren't listed in the table below aren't supported on any release of Exchange 2019**. This includes minor and patch-level releases of .NET Framework. <br/><br/> The complete prerequisite list for Exchange 2019 is available [here](https://docs.microsoft.com/exchange/plan-and-deploy/prerequisites?view=exchserver-2019).

|**Exchange version**|**.NET Framework 4.7.2 or later**|
|:-----|:-----|
|Exchange 2019|Supported|

## Supported clients

- Office 365 ProPlus

- Outlook 2019

- Outlook 2016

- Outlook 2013

- Outlook for Mac for Office 365

- Outlook 2016 for Mac

> [!IMPORTANT]
> You need KB3140245 to apply registry keys to enable TLS 1.1 & 1.2 support for Windows 7, otherwise Outlook 2013 and 2016 will not work on Windows 7.

## Lync/Skype For Business Server integration

If integrating Lync presence and instant messaging with Exchange Server, Lync Server 2013 Cumulative Update 10 or later is required. If integrating Skype for Business presence and instant messaging with Exchange Server, Skype for Business Server Cumulative Update 7 or later is required.
::: moniker-end

::: moniker range="exchserver-2016"
Before you install Exchange Server 2016, we recommend that you review this topic to ensure your network, hardware, software, clients, and other elements meet the requirements for Exchange 2016. Also, make sure you understand the coexistence scenarios that are supported for Exchange 2016 and earlier versions of Exchange.

To actually install Exchange 2016, see [Deploy new installations of Exchange](https://docs.microsoft.com/Exchange/plan-and-deploy/deploy-new-installations/deploy-new-installations?view=exchserver-2016).

## Supported coexistence scenarios

The following table lists the scenarios in which coexistence between Exchange 2016 and earlier versions of Exchange is supported.

**Coexistence of Exchange 2016 with earlier versions of Exchange Server**

|**Exchange version**|**Exchange organization coexistence**|
|:-----|:-----|
|Exchange 2007 and earlier versions|Not supported|
|Exchange 2010|Supported with Update Rollup 11 for Exchange 2010 SP3 or later on all Exchange 2010 servers in the organization, including Edge Transport servers.|
|Exchange 2013|Supported with Exchange 2013 Cumulative Update 10 or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|
|Mixed Exchange 2010 and Exchange 2013 organization|Supported with the following minimum versions of Exchange: <br/> Update Rollup 11 Exchange 2010 SP3 or later on all Exchange 2010 servers in the organization, including Edge Transport servers. <br/> Exchange 2013 Cumulative Update 10 or later on all Exchange 2013 servers in the organization, including Edge Transport servers.|

## Supported hybrid deployment scenarios

Exchange 2016 supports hybrid deployments with Office 365 tenants that have been upgraded to the latest version of Office 365. For more information about specific hybrid deployments, see [Hybrid Deployment Prerequisites](https://technet.microsoft.com/library/e7454db0-fed4-4662-8890-9501126b1ba2.aspx).

## Network and directory servers

The following table lists the requirements for the network and the directory servers in your Exchange 2016 organization.

 **Network and directory server requirements for Exchange 2016**

|**Component**|**Requirement**|
|:-----|:-----|
|Domain controllers|All domain controllers in the forest need to be running one of the following versions of Windows Server: <br/>• Windows Server 2019 Standard or Datacenter <br/>• Windows Server 2016 Standard or Datacenter <br/>• Windows Server 2012 R2 Standard or Datacenter <br/>• Windows Server 2012 Standard or Datacenter <br/>• Windows Server 2008 R2 Standard or Enterprise <br/>• Windows Server 2008 R2 Datacenter RTM or later|
|Active Directory forest|The Active Directory forest functional level is Windows Server 2008 R2 or higher.|
|Active Directory site|The Active Directory site where you install the Exchange Server must contain at least one writeable domain controller that's also a global catalog server, or the installation will fail. Furthermore, you can't install the Exchange server and then remove the domain controller from the Active Directory site.|
|DNS namespace support|Exchange 2016 supports the following domain name system (DNS) namespaces: <br/>• Contiguous <br/>• Noncontiguous <br/>• Single label domains <br/>• Disjoint <br/> For more information about DNS namespaces supported by Exchange, see Microsoft Knowledge Base article 2269838, [Microsoft Exchange compatibility with Single Label Domains, Disjoined Namespaces, and Discontiguous Namespaces](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=2269838).|
|IPv6 support|In Exchange 2016, IPv6 is supported only when IPv4 is also installed and enabled. If Exchange 2016 is deployed in this configuration, and the network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see [IPv6 Support in Exchange 2013](https://technet.microsoft.com/library/33543023-eb9a-4102-b990-84a818a52814.aspx).|

## Directory server architecture

The use of 64-bit Active Directory domain controllers increases directory service performance for Exchange 2016.

> [!NOTE]
> In multi-domain environments, on Windows Server 2008 domain controllers that have the Active Directory language locale set to Japanese (ja-jp), your servers may not receive some attributes that are stored on an object during inbound replication. For more information, see Microsoft Knowledge Base article 949189, [A Windows Server 2008 domain controller that is configured with the Japanese language locale may not apply updates to attributes on an object during inbound replication](https://go.microsoft.com/fwlink/p/?linkid=3052&kbid=949189).

### Installing Exchange 2016 on directory servers

For security and performance reasons, we recommend that you install Exchange 2016 only on member servers and not on Active Directory directory servers. To learn about the issues you can face when installing Exchange 2016 on a directory server, see [Installing Exchange on a domain controller is not recommended [WarningInstallExchangeRolesOnDomainController]](deployment-ref/ms-exch-setupreadiness-warninginstallexchangerolesondomaincontroller.md). After Exchange 2016 is installed, changing its role from a member server to a directory server, or vice versa, isn't supported.

## Hardware

For information about deploying Exchange in a virtualized environment, see [Exchange Server virtualization](virtualization.md).

 **Hardware requirements for Exchange 2016**

|**Component**|**Requirement**|**Notes**|
|:-----|:-----|:-----|
|Processor|x64 architecture-based computer with Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T). <br/> AMD processor that supports the AMD64 platform. <br/> **Note**: Intel Itanium IA64 processors not supported.|For more information, see [Sizing Exchange 2016 Deployments](https://blogs.technet.microsoft.com/exchange/2015/10/15/ask-the-perf-guy-sizing-exchange-2016-deployments/). <br/> See the [Operating system](#operating-system) section later in this topic for supported operating systems.|
|Memory|Varies depending on Exchange roles that are installed: <br/> **Mailbox**: 8GB minimum <br/> **Edge Transport**: 4GB minimum|For more information, see [Sizing Exchange 2016 Deployments](https://blogs.technet.microsoft.com/exchange/2015/10/15/ask-the-perf-guy-sizing-exchange-2016-deployments/).|
|Paging file size|The page file size minimum and maximum must be set to physical RAM plus 10MB, to a maximum size of 32,778MB (32GB) if you're using more than 32GB of RAM.|None|
|Disk space|At least 30 GB on the drive on which you install Exchange. <br/> An additional 500 MB of available disk space for each Unified Messaging (UM) language pack that you plan to install. <br/> 200 MB of available disk space on the system drive. <br/> A hard disk that stores the message queue database on with at least 500 MB of free space.|For more information, see [Sizing Exchange 2016 Deployments](https://blogs.technet.microsoft.com/exchange/2015/10/15/ask-the-perf-guy-sizing-exchange-2016-deployments/).|
|Drive|DVD-ROM drive, local or network accessible.|None|
|Screen resolution|1024 x 768 pixels or higher|None|
|File format|Disk partitions formatted as NTFS file systems, which applies to the following partitions: <br/>• System partition <br/>• Partitions that store Exchange binary files or files generated by Exchange diagnostic logging <br/>• Database files, such as mailbox and transport databases <br/> Disk partitions containing only the following types of files can optionally be formatted as ReFS: <br/>• Partitions containing transaction log files <br/>• Partitions containing mailbox database files <br/>• Partitions containing content indexing files|None|

## Operating system

The following table lists the supported operating systems for Exchange 2016.

 **Important**: We don't support the installation of Exchange 2016 on a computer that's running Windows Server Core or Nano Server. The Windows Server Desktop Experience feature needs to be installed. To install Exchange 2016, you need to do one of the following to install the Desktop Experience on Windows Server prior to starting Exchange 2016 Setup:

- **Windows Server 2012 and Windows Server 2012 R2**: Run the following command in Windows PowerShell

  ```
  Install-WindowsFeature Server-Gui-Mgmt-Infra, Server-Gui-Shell -Restart
  ```

- **Windows Server 2016**: Install Windows Server 2016 and choose the **Desktop Experience** installation option. If a computer is running Windows Server 2016 Core mode and you want to install Exchange 2016 on it, you'll need to reinstall the operating system and choose the **Desktop Experience** installation option.

 **Supported operating systems for Exchange 2016**

|**Component**|**Requirement**|
|:-----|:-----|
|Mailbox and Edge Transport server roles|Windows Server 2016 Standard or Datacenter<sup>\*</sup> <br/> Windows Server 2012 R2 Standard or Datacenter <br/> Windows Server 2012 Standard or Datacenter|
|Management tools|One of the following: <br/> Windows Server 2016 Standard or Datacenter<sup>\*</sup> <br/> Windows Server 2012 R2 Standard or Datacenter <br/> Windows Server 2012 Standard or Datacenter <br/> 64-bit edition of Windows 10 <br/> 64-bit edition of Windows 8.1|

<sup>\*</sup> Requires Exchange Server 2016 Cumulative Update 3 or later.

 **Supported Windows Management Framework versions for Exchange 2016**

Exchange 2016 only supports the version of Windows Management Framework that's built in to the release of Windows that you're installing Exchange on. Don't install versions of Windows Management Framework that are made available as stand-alone downloads on servers running Exchange.

 **Installing other software on an Exchange 2016 server**

We don't support the installation of Office client or Office server software, such as SharePoint Server; Skype for Business Server; Office Online Server; or Project Server, on Exchange 2016 servers. Other software that you want to install on Exchange 2016 servers needs to be designed to run on the same computer as Exchange.

## .NET Framework

We strongly recommend that you use the latest version of .NET Framework that's supported by the release of Exchange you're installing.

> [!IMPORTANT]
> **Releases of .NET Framework that aren't listed in the table below are not supported on any release of Exchange 2016**. This includes minor and patch-level releases of .NET Framework.

> [!IMPORTANT]
> The complete prerequisite list for [** exchange 2016 is available here**](https://docs.microsoft.com/exchange/plan-and-deploy/prerequisites?view=exchserver-2016)


|**Exchange version**|**.NET Framework 4.7.2**|**.NET Framework 4.7.1**|**.NET Framework 4.6.2**|
|:-----|:-----|:-----|:-----|
|Exchange 2016 CU11, CU12|Supported|Supported||
|Exchange 2016 CU10||Supported||
|Exchange 2016 CU8, CU9||Supported|Supported|
|Exchange 2016 CU5, CU6, CU7|||Supported|

## Supported clients (with latest updates)

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
