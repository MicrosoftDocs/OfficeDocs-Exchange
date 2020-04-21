---
title: 'Exchange 2013 Sizing and Configuration Recommendations: Exchange 2013 Help'
TOCTitle: Exchange 2013 Sizing and Configuration Recommendations
ms:assetid: 4c4ba2fc-014a-46fb-949a-2dabba92c4a5
ms:mtpsurl: https://technet.microsoft.com/library/Dn879075(v=EXCHG.150)
ms:contentKeyID: 63917936
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 Sizing and Configuration Recommendations

_**Applies to:** Exchange Server 2013_

Exchange 2013 is more demanding of system resources than previous versions of Exchange. By correctly sizing your Exchange 2013 infrastructure, and then making some recommended configurations to Exchange-related components within that infrastructure, you can lay the groundwork for an optimally performing deployment.

## Exchange 2013 Sizing

Correctly sizing Exchange 2013 is one of the most effective ways of preventing performance problems. The Exchange 2013 Server Role Requirements Calculator is [available here](https://go.microsoft.com/fwlink/p/?linkid=523844). The latest version is 6.6, but we recommend you check back periodically for updates. To use this calculator correctly, you must consult the guidance in the [Exchange 2013 Server Role Requirements Calculator](https://go.microsoft.com/fwlink/p/?linkid=386677) and [Sizing Exchange 2013 Deployments](https://go.microsoft.com/fwlink/p/?linkid=301990) blog posts.

It's important to start with the calculator prior to purchasing and deploying your hardware; you should first determine your overall resource requirements based on the calculator results. You can use the calculator to input your organization's demands, and use the results for guidance on how to scale your hardware. The calculator doesn't tell you how many servers to use, but it will allow you to estimate the impact of an Exchange workload on a given set of servers. You should experiment with different configurations to see how it affects performance, in order to meet the hardware and business needs specific to your environment.

To simplify deployments and get the best use of hardware, the Exchange product group recommends multi-role servers. Using multi-role severs gives you better availability at the Client Access server (CAS) layer, as there are more Client Access servers available to handle requests during a failure scenario. The key design consideration for Exchange 2013 is to utilize "smaller" commodity type servers (scaling out instead of scaling up). Design and testing was done with two socket computers containing up to twenty processor cores, with up to 96 gigabytes (GB) of RAM. If your hardware is larger than this, you should consider other options, such as using that hardware for other needs and buying smaller servers for your Exchange 2013 environment, or virtualizing.

It is preferable to build more servers (scaling out) than it is to add resources to existing, larger servers (scaling up). Scaling out allows your environment to take advantage of the built-in high availability features in Exchange 2013. To understand why this configuration is recommended, please review in detail the posts [The Preferred Architecture](https://go.microsoft.com/fwlink/p/?linkid=523782) and [Site Resilience Impact on Availability](https://go.microsoft.com/fwlink/p/?linkid=523845).

The calculator does not take into account third-party products running on Exchange servers, or products that interact with Exchange (including internally developed applications), which means you must be sure to account for them during your sizing. Lync Server, for example, and third-party Exchange Web Services (EWS) applications and ActiveSync devices can all significantly increase CPU requirements per user. You can reference third-party product documentation for information on how it will affect Exchange. Creating a performance baseline for Exchange prior to implementing third-party solutions is recommended.

## Recommended Performance Configurations

The following performance optimizations are recommended for your Exchange 2013 environment.

## Power

Set BIOS to allow the operating system (OS) to manage power.

In the OS, turn on the High Performance power plan.

## Processing

Turn off hyper-threading on physical Exchange servers. If virtualizing, hyper-threading may be enabled on the physical server, but each virtual server should only be allocated the required number of virtual CPUs (don't over-allocate virtual CPUs), and only utilize the physical processor core count for sizing calculations.

In Exchange Server 2013 Service Pack 1 or later, you can enable SSL offloading to help reduce CPU consumption by Client Access servers, but the complex configuration of SSL offloading may not be worth the benefit.

## .NET Framework

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Exchange version</th>
<th>.NET Framework 4.6.2</th>
<th>.NET Framework 4.6.1</th>
<th>.NET Framework 4.5.2</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Exchange 2013 CU16</p></td>
<td><p> X</p></td>
<td><p> </p></td>
<td><p> </p></td>
</tr>
<tr class="even">
<td><p>Exchange 2013 CU15</p></td>
<td><p> X</p></td>
<td><p>X1,2 </p></td>
<td><p>X </p></td>
</tr>
<tr class="odd">
<td><p>Exchange 2013 CU13 and CU14</p></td>
<td></td>
<td><p>X1,2 </p></td>
<td><p>X</p></td>
</tr>
</tbody>
</table>

1 .NET Framework 4.6.1 requires post-release fixes if you want to install it on a server running Exchange 2013 CU13. For more information. see [Exchange 2013 prerequisites](exchange-2013-prerequisites-exchange-2013-help.md).

2If you're upgrading to Exchange 2013 CU13, CU14, or CU15 from Exchange 2013 CU12 or earlier, we strongly recommend that you install Exchange 2013 CU13 before .NET Framework 4.6.1 and its related post-release fixes.

If you are unable to install .NET 4.5.2, refer to Microsoft Knowledge Base article 2995145 "[Performance issues or delays when you connect to Exchange Server 2013 that is running in Windows Server](https://go.microsoft.com/fwlink/p/?linkid=524159)." The fixes in that article were developed based on internal findings on Store Worker Process memory utilization. By applying these fixes, you will reduce the overall memory consumption for all managed processes (including the store worker process) and you will reduce the overall CPU time that is spent in .NET garbage collection.

## Hot Fixes

The Exchange performance team recommends installing all of the following performance-related hot fixes.

  - [Update that improves cluster resiliency in Windows Server 2012 is available](https://go.microsoft.com/fwlink/p/?linkid=524088)

  - [Recommended hotfixes and updates for Windows Server 2012-based failover clusters](https://go.microsoft.com/fwlink/p/?linkid=524089)

  - [Recommended hotfixes and updates for Windows Server 2012 R2-based failover clusters](https://go.microsoft.com/fwlink/p/?linkid=524090)

  - [Incorrect RSS processor assignment on a Windows 8 or Windows Server 2012-based computer that has multi-core processors](https://go.microsoft.com/fwlink/p/?linkid=324140)

  - [Performance issues or delays when you connect to Exchange Server 2013 that is running in Windows Server](https://go.microsoft.com/fwlink/p/?linkid=312962)

  - [Outlook connectivity issue if SSLOffloading is "True" in Exchange 2013](https://go.microsoft.com/fwlink/p/?linkid=524091)

  - [Long server connection for Outlook after a database failover in Exchange Server 2013](https://go.microsoft.com/fwlink/p/?linkid=524092)

  - [Slow performance in Outlook Web App when Lync is integrated with Exchange Server 2013](https://go.microsoft.com/fwlink/p/?linkid=524093)

  - [EMS takes a long time to execute the first command in an Exchange Server 2013 Cumulative Update 5 environment](https://go.microsoft.com/fwlink/p/?linkid=524094)

  - [Message routing latency if IPv6 is enabled in Exchange Server 2013](https://go.microsoft.com/fwlink/p/?linkid=524095)

  - [High CPU usage by an application that depends on a Microsoft LDAP client in WIndows Server 2008 R2 SP1](https://go.microsoft.com/fwlink/p/?linkid=530287)

  - [CPU usage is high when you use RPC over HTTP protocol in Windows 8.1 or Windows Server 2012 R2](https://go.microsoft.com/fwlink/p/?linkid=619127)

## Networking

With Exchange 2013, a single network adapter is recommended, as it is no longer necessary to split MAPI and replication networks. See [Network requirements](planning-for-high-availability-and-site-resilience-exchange-2013-help.md) for more information.

Use the default SNP offload settings where available, and make sure that RSS is enabled (the default setting in Windows Server 2012 and later). RSS will help scale CPU utilization, especially on 10GbE.

Verify that the OS is not turning off the network card to save power.

Maintain up-to-date NIC drivers. Check with your vendor on a monthly basis for relevant driver updates.

## Internet Information Services (IIS)

During installation, Exchange modifies some connection limits for IIS. No further tuning of IIS is recommended.

Avoid customizations whenever possible. Any change to web.config or registry keys can be overwritten by Exchange Cumulative Updates or Windows updates.

## Storage

Guidelines for Exchange 2013 storage are available in [Exchange 2013 storage configuration options](exchange-2013-storage-configuration-options-exchange-2013-help.md).

## Virtualization

Please review [Requirements for hardware virtualization](exchange-2013-virtualization-exchange-2013-help.md). Also, note that Exchange is not non-uniform memory access (NUMA) aware. Therefore, using the hardware manufacturer's default NUMA settings is recommended.

## Active Directory

Monitor directory server performance, because Active Directory queries directly impact your Exchange deployment.

LDAP search time is a critical counter to measure in regards to Active Directory health. Monitor CPU on your domain controllers. CPU issues on the domain controllers will render as a performance hit on the Exchange servers.

Run the built in "Active Directory Diagnostics" on the Domain Controller in Performance Monitor located under "Data Collector Set" to help isolate the cause of domain controller performance issues.

Plan for enough RAM on Domain Controllers to be able cache the full AD database file.

We recommend deploying 1 Active Directory global catalog core for every 8 mailbox cores that are handling active load (based on 64-bit global catalog cores).

## Load Balancing

All Client Access servers should receive approximately the same number of incoming connections.

For all protocols, Exchange 2013 does not require session affinity between a given Client Access server and the load balancer.

A hardware or software load balancer should be used to manage all inbound traffic to Client Access servers. The selection of the target server can be determined with methods such as "round-robin," in which each inbound connection goes to the next target server in a circular list, or with "least connections," in which the load balancer sends each new connection to the server that has the fewest established connections at that time. These methods are detailed further in [Load balancing](load-balancing-exchange-2013-help.md). You should also consider the following:

  - Round-robin has the problem of slow convergence with long-lived connections (like RPC/HTTP). As new computers are brought online, the balance of connections served across the target computers will take a very long time to converge.

  - With the "least connections" method, be mindful it's possible for a Client Access server to become overloaded and unresponsive during a Client Access server outage or during patching maintenance. In the context of Exchange performance, authentication is an expensive operation.

Due to a number of limitations with Windows Network Load Balancing (NLB) in an Exchange 2013 environment, detailed in [Load balancing](load-balancing-exchange-2013-help.md), we do not recommend using Windows NLB.

## User and Database Distribution

Maintain a well-balanced distribution of users per database and active databases per server. Evenly distribute database disk space consumption as well as balance heavy users across all databases.

You must profile your user base in order to understand how they interact with Exchange (Devices, Outlook, and OWA) and the impact that those interactions will cause from a performance standpoint. Refer to the calculator blogs from Section 2 for a better understanding of how to profile per user Exchange usage.

Configure the DB copy activation preference and the "MaximumPreferredActiveDatabases" (per server) settings to maintain balance during a failover or switchover.

The RedistributeActiveDatabases.ps1 script will rebalance active databases across the DAG nodes.

Consider enforcing strict item count limits that match Office 365. You can do this with the Set-Mailbox cmdlet and the information provided in [Mailbox folder limits](https://go.microsoft.com/fwlink/p/?linkid=398779).

## Pagefile

Set a maximum size for the pagefile of 32778 MB if you're using more than 32GB of RAM.

The pagefile should not be hosted on the same drive as Exchange database files or database log files.

It is imperative that you use a fixed size pagefile and not allow Windows to manage the size. Growing the page file can be a very performance-intensive task and can cause issues when Exchange is under stress.

If you need to get a full kernel dump, then use Microsoft Knowledge Base article 969028, [How to generate a kernel or a complete memory dump file in Windows Server 2008 and Windows Server 2008 R2](https://go.microsoft.com/fwlink/p/?linkid=524044), for dedicated dump file.

## Outlook Mode

Cached mode is recommended. To understand the benefit of using cached mode, see [Choose between Cached Exchange Mode and Online Mode for Outlook 2013](https://go.microsoft.com/fwlink/p/?linkid=524045).

It is important to note that performance can be affected by both server add-ins and Outlook third-party add-ins. When using online mode, clients can expect some performance issues from third-party add-ins, high item counts, restricted views, the number of users accessing the mailbox, among other factors. Legacy clients can experience more impact by high item counts and performance than Outlook 2013 will.

If the primary reason that an organization has Outlook configured in online mode is for security concerns, consider using BitLocker instead.

Outlook 2013 offers a new "Sync Slider" feature to minimize the download time and the size of the OST file. Please refer to [Configure Cached Exchange Mode in Outlook 2013](https://go.microsoft.com/fwlink/p/?linkid=390456) for more information.

Check monthly for Outlook clients updates that are supported in your environment.

## Third-party software

As a best practice, uninstall or disable third-party software while troubleshooting Exchange performance. The following list contains the types of third-party software that Microsoft support has most often seen affecting Exchange 2013 performance.

  - Anti-virus solutions

  - Intrusion prevention software

  - Backup software

  - Auditing software, for both files and users

  - Archiving solutions
