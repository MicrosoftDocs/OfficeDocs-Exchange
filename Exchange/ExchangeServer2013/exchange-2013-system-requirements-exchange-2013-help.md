---
title: 'Exchange 2013 system requirements: Exchange 2013 Help'
TOCTitle: Exchange 2013 system requirements
ms:assetid: 1e80857c-b870-4a6d-a0f4-ff7b3a7be037
ms:mtpsurl: https://technet.microsoft.com/library/Aa996719(v=EXCHG.150)
ms:contentKeyID: 48384881
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 system requirements

_**Applies to:** Exchange Server 2013_

Learn about the Exchange 2013 requirements that you need to know before you install Exchange 2013. For example, you'll learn about the hardware, network, and operating system requirements.

Before you install Microsoft Exchange Server 2013, we recommend that you review this topic to ensure that your network, hardware, software, clients, and other elements meet the requirements for Exchange 2013. In addition, make sure you understand the coexistence scenarios that are supported for Exchange 2013 and earlier versions of Exchange.

## Supported coexistence scenarios

The following table lists the scenarios in which coexistence between Exchange 2013 and earlier versions of Exchange is supported.

### Coexistence of Exchange 2013 and earlier versions of Exchange Server

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Exchange version</th>
<th>Exchange organization coexistence</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange Server 2003 and earlier versions</p></td>
<td><p>Not supported</p></td>
</tr>
<tr class="even">
<td><p>Exchange 2007</p></td>
<td><p>Supported with the following minimum versions of Exchange:</p>
<ul>
<li><p>Update Rollup 10 for Exchange 2007 Service Pack 3 (SP3) on all Exchange 2007 servers in the organization, including Edge Transport servers (see Note 1 below).</p></li>
<li><p>Exchange 2013 Cumulative Update 2 (CU2) or later on all Exchange 2013 servers in the organization.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Exchange 2010</p></td>
<td><p>Supported with the following minimum versions of Exchange:</p>
<ul>
<li><p>Exchange 2010 SP3 on all Exchange 2010 servers in the organization, including Edge Transport servers (see Note 2 below).</p></li>
<li><p>Exchange 2013 CU2 or later on all Exchange 2013 servers in the organization.</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Mixed Exchange 2010 and Exchange 2007 organization</p></td>
<td><p>Supported with the following minimum versions of Exchange:</p>
<ul>
<li><p>Update Rollup 10 for Exchange 2007 SP3 on all Exchange 2007 servers in the organization, including Edge Transport servers (see Note 1 below).</p></li>
<li><p>Exchange 2010 SP3 on all Exchange 2010 servers in the organization, including Edge Transport servers (see Note 2 below).</p></li>
<li><p>Exchange 2013 CU2 or later on all Exchange 2013 servers in the organization.</p></li>
</ul></td>
</tr>
</tbody>
</table>

**Note 1**: If you want to create an EdgeSync Subscription between an Exchange 2007 Hub Transport server and an Exchange 2013 SP1 Edge Transport server, you need to install Exchange 2007 SP3 Update Rollup 13 or later on the Exchange 2007 Hub Transport server.

**Note 2**: If you want to create an EdgeSync Subscription between an Exchange 2010 Hub Transport server and an Exchange 2013 SP1 Edge Transport server, you need to install Exchange 2010 SP3 Update Rollup 5 or later on the Exchange 2010 Hub Transport server.

## Supported hybrid deployment scenarios

Exchange 2013 supports hybrid deployments with Office 365 tenants that have been upgraded to the latest version of Office 365. For more information about specific hybrid deployments, see [Hybrid deployment prerequisites](https://docs.microsoft.com/exchange/hybrid-deployment-prerequisites).

## Network and directory servers

The following table lists the requirements for the network and the directory servers in your Exchange 2013 organization.

**Network and directory server requirements for Exchange 2013**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Component</th>
<th>Requirement</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Schema master</p></td>
<td><p>By default, the schema master runs on the first Active Directory domain controller installed in a forest. The schema master must be running one of the following:</p>
<ul>
<li><p>Windows Server 2012 R2 Standard or Datacenter1</p></li>
<li><p>Windows Server 2012 Standard or Datacenter</p></li>
<li><p>Windows Server 2008 R2 Standard or Enterprise</p></li>
<li><p>Windows Server 2008 R2 Datacenter RTM or later</p></li>
<li><p>Windows Server 2008 Standard or Enterprise (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2008 Datacenter RTM or later</p></li>
<li><p>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Global catalog server</p></td>
<td><p>In each Active Directory site where you plan to install Exchange 2013, you must have at least one global catalog server running one of the following:</p>
<ul>
<li><p>Windows Server 2012 R2 Standard or Datacenter1</p></li>
<li><p>Windows Server 2012 Standard or Datacenter</p></li>
<li><p>Windows Server 2008 R2 Standard or Enterprise</p></li>
<li><p>Windows Server 2008 R2 Datacenter RTM or later</p></li>
<li><p>Windows Server 2008 Standard or Enterprise (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2008 Datacenter RTM or later</p></li>
<li><p>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</p></li>
</ul>
<p>For more information about global catalog servers, see <a href="https://go.microsoft.com/fwlink/p/?linkid=178996">What is the Global Catalog</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Domain controller</p></td>
<td><p>In each Active Directory site where you plan to install Exchange 2013, you must have at least one writeable domain controller running one of the following:</p>
<ul>
<li><p>Windows Server 2012 R2 Standard or Datacenter1</p></li>
<li><p>Windows Server 2012 Standard or Datacenter</p></li>
<li><p>Windows Server 2008 R2 Standard or Enterprise SP1 or later</p></li>
<li><p>Windows Server 2008 R2 Datacenter RTM or later</p></li>
<li><p>Windows Server 2008 Standard or Enterprise SP1 or later (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2008 Datacenter RTM or later</p></li>
<li><p>Windows Server 2003 Standard Edition with Service Pack 2 (SP2) or later (32-bit or 64-bit)</p></li>
<li><p>Windows Server 2003 Enterprise Edition with SP2 or later (32-bit or 64-bit)</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Active Directory forest</p></td>
<td><p>Active Directory must be at Windows Server 2003 forest functionality mode or higher2.</p></td>
</tr>
<tr class="odd">
<td><p>DNS namespace support</p></td>
<td><p>Exchange 2013 supports the following domain name system (DNS) namespaces:</p>
<ul>
<li><p>Contiguous</p></li>
<li><p>Noncontiguous</p></li>
<li><p>Single label domains</p></li>
<li><p>Disjoint</p></li>
</ul>
<p>For more information about DNS namespaces supported by Exchange, see Microsoft Knowledge Base article 2269838, <a href="https://support.microsoft.com/help/2269838">Microsoft Exchange compatibility with Single Label Domains, Disjoined Namespaces, and Discontiguous Namespaces</a>.</p></td>
</tr>
<tr class="even">
<td><p>IPv6 support</p></td>
<td><p>In Exchange 2013, IPv6 is supported only when IPv4 is also installed and enabled. If Exchange 2013 is deployed in this configuration, and the network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses. For more information, see <a href="ipv6-support-in-exchange-2013-exchange-2013-help.md">IPv6 support in Exchange 2013</a>.</p></td>
</tr>
</tbody>
</table>

1   Windows Server 2012 R2 is supported only with Exchange 2013 SP1 or later.

2   Windows Server 2012 R2 forest functionality mode is supported only with Exchange 2013 SP1 or later.

## Directory server architecture

The use of 64-bit Active Directory domain controllers increases directory service performance for Exchange 2013.

> [!NOTE]
> In multi-domain environments, on Windows Server 2008 domain controllers that have the Active Directory language locale set to Japanese, your servers might not receive some attributes that are stored on an object during inbound replication. For more information, see Microsoft Knowledge Base article 949189, <A href="https://www.microsoft.com/download/details.aspx?id=1050">A Windows Server 2008 domain controller that is configured with the Japanese language locale may not apply updates to attributes on an object during inbound replication</A>.

## Installing Exchange 2013 on directory servers

For security and performance reasons, we recommend that you install Exchange 2013 only on member servers and not on Active Directory directory servers. However, you can't run DCPromo on a computer running Exchange 2013. After Exchange 2013 is installed, changing its role from a member server to a directory server, or vice versa, isn't supported.

## Hardware

The recommended hardware requirements for Exchange 2013 servers vary depending on a number of factors including the server roles that are installed and the anticipated load that will be placed on the servers.

For detailed information on how to properly size and configure your deployment, see [Exchange 2013 Sizing and Configuration Recommendations](exchange-2013-sizing-and-configuration-recommendations-exchange-2013-help.md).

For information about deploying Exchange in a virtualized environment, see [Exchange 2013 virtualization](exchange-2013-virtualization-exchange-2013-help.md).

**Hardware requirements for Exchange 2013**

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Component</th>
<th>Requirement</th>
<th>Notes</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Processor</p></td>
<td><ul>
<li><p>x64 architecture-based computer with Intel processor that supports Intel 64 architecture (formerly known as Intel EM64T)</p></li>
<li><p>AMD processor that supports the AMD64 platform</p></li>
<li><p>Intel Itanium IA64 processors not supported</p></li>
</ul></td>
<td><p>See the &quot;Operating system&quot; section later in this topic for supported operating systems.</p></td>
</tr>
<tr class="even">
<td><p>Memory</p></td>
<td><p>Varies depending on Exchange roles that are installed:</p>
<ul>
<li><p><strong>Mailbox</strong>   8GB minimum</p></li>
<li><p><strong>Client Access</strong>   4GB minimum</p></li>
<li><p><strong>Mailbox and Client Access combined</strong>   8GB minimum</p></li>
<li><p><strong>Edge Transport</strong>   4GB minimum</p></li>
</ul></td>
<td><p>None.</p></td>
</tr>
<tr class="odd">
<td><p>Paging file size</p></td>
<td><p>The page file size minimum and maximum must be set to physical RAM plus 10  MB, to a maximum size of 32778MB if you're using more than 32GB of RAM.</p></td>
<td><p>For detailed pagefile recommendations, see the &quot;Pagefile&quot; section in <a href="exchange-2013-sizing-and-configuration-recommendations-exchange-2013-help.md">Exchange 2013 Sizing and Configuration Recommendations</a>.</p></td>
</tr>
<tr class="even">
<td><p>Disk space</p></td>
<td><ul>
<li><p>At least 30 GB on the drive on which you install Exchange</p></li>
<li><p>An additional 500 MB of available disk space for each Unified Messaging (UM) language pack that you plan to install</p></li>
<li><p>200 MB of available disk space on the system drive</p></li>
<li><p>A hard disk that stores the message queue database on with at least 500 MB of free space.</p></li>
</ul></td>
<td><p>For detailed information on storage recommendations, see <a href="exchange-2013-storage-configuration-options-exchange-2013-help.md">Exchange 2013 storage configuration options</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Drive</p></td>
<td><p>DVD-ROM drive, local or network accessible</p></td>
<td><p>None.</p></td>
</tr>
<tr class="even">
<td><p>Screen resolution</p></td>
<td><p>1024 x 768 pixels or higher</p></td>
<td><p>None.</p></td>
</tr>
<tr class="odd">
<td><p>File format</p></td>
<td><p>Disk partitions formatted as NTFS file systems, which applies to the following partitions:</p>
<ul>
<li><p>System partition</p></li>
<li><p>Partitions that store Exchange binary files or files generated by Exchange diagnostic logging</p></li>
</ul>
<p>Disk partitions containing the following types of files can be formatted as ReFS:</p>
<ul>
<li><p>Partitions containing transaction log files</p></li>
<li><p>Partitions containing database files</p></li>
<li><p>Partitions containing content indexing files</p></li>
</ul></td>
<td><p>None.</p></td>
</tr>
</tbody>
</table>

## Operating system

The following table lists the supported operating systems for Exchange 2013.

> [!IMPORTANT]
> We don't support the installation of Exchange 2013 on a computer that's running in Windows Server Core mode. The computer must be running the full installation of Windows Server. If you want to install Exchange 2013 on a computer that's running in Windows Server Core mode, you must convert the server to a full installation of Windows Server by doing one of the following:
> <UL>
> <LI>
> <P><STRONG>Windows Server 2008 R2</STRONG>: Reinstall Windows Server and select the <STRONG>Full Installation</STRONG> option.</P>
> <LI>
> <P><STRONG>Windows Server 2012 R2</STRONG> or <STRONG>Windows Server 2012</STRONG>: Convert your Windows Server Core mode server to a full installation by running the following command.</P>
>
> ```Powershell
> Install-WindowsFeature Server-Gui-Mgmt-Infra,Server-Gui-Shell -Restart
> ```
> </LI></UL>

### Supported operating systems for Exchange 2013

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Component</th>
<th>Requirement</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Mailbox, Client Access, and Edge Transport server roles</p></td>
<td><p>One of the following:</p>
<ul>
<li><p>Windows Server 2012 R2 Standard or Datacenter1</p></li>
<li><p>Windows Server 2012 Standard or Datacenter</p></li>
<li><p>Windows Server 2008 R2 Standard with Service Pack 1 (SP1)</p></li>
<li><p>Windows Server 2008 R2 Enterprise with Service Pack 1 (SP1)</p></li>
<li><p>Windows Server 2008 R2 Datacenter RTM or later</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Management tools</p></td>
<td><p>One of the following:</p>
<ul>
<li><p>Windows Server 2012 R2 Standard or Datacenter1</p></li>
<li><p>Windows Server 2012 Standard or Datacenter</p></li>
<li><p>Windows Server 2008 R2 Standard with SP1</p></li>
<li><p>Windows Server 2008 R2 Enterprise with SP1</p></li>
<li><p>Windows Server 2008 R2 Datacenter RTM or later</p></li>
<li><p>64-bit edition of Windows 8.12</p></li>
<li><p>64-bit edition of Windows 8</p></li>
<li><p>64-bit edition of Windows 7 with Service Pack 1</p></li>
</ul></td>
</tr>
</tbody>
</table>

1   Windows Server 2012 R2 is supported only with Exchange 2013 SP1 or later.

2   Windows 8.1 is supported only with Exchange 2013 SP1 or later.

### Supported Windows Management Framework versions for Exchange 2013

Exchange 2013 only supports the version of Windows Management Framework that's built into the release of Windows that you're installing Exchange on. Don't install versions of Windows Management Framework that are made available as stand-alone downloads on servers running Exchange.

## .NET Framework

We strongly recommend that you use the latest version of .NET Framework that's supported by the release of Exchange you're installing.

|**Exchange 2013 version**|**.NET Framework 4.8**|**.NET Framework 4.7.2**|**.NET Framework 4.7.1**|**.NET Framework 4.6.2**|
|:-----|:-----:|:-----:|:-----:|:-----:|:-----:|
|CU23|X|X|||
|CU21, CU22||X|X||
|CU19, CU20|||X|X||
|CU16, CU17, CU18||||X|
|CU15||||X|

**Note**: For older versions, see [Exchange Server supportability matrix](https://docs.microsoft.com/exchange/plan-and-deploy/supportability-matrix#microsoft-net-framework)

## Supported clients

Exchange 2013 supports the following versions of Outlook and Entourage for Mac:

- Microsoft 365 Apps for enterprise

- Outlook 2019

- Outlook 2016

- Outlook 2013

- Outlook 2010

- Outlook 2007

- Entourage 2008 for Mac, Web Services Edition

- Outlook for Mac for Office 365

- Outlook for Mac 2011

For a list of Outlook releases that Exchange supports, see [Outlook Updates](https://docs.microsoft.com/officeupdates/outlook-updates-msi).

> [!IMPORTANT]
> We strongly recommend that you install the latest available service packs and updates available so that your users receive the best possible experience when connecting to Exchange.

Outlook clients earlier than Outlook 2007 are not supported. Email clients on Mac operating systems that require DAV, such as Entourage 2008 for Mac RTM and Entourage 2004, are not supported.

Outlook Web App supports several browsers on a variety of operating systems and devices. For detailed information, see [What's new for Outlook Web App in Exchange 2013](what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md).
