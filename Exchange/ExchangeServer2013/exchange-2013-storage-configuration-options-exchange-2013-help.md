---
title: 'Exchange 2013 storage configuration options: Exchange 2013 Help'
TOCTitle: Exchange 2013 storage configuration options
ms:assetid: 37cdeacf-74f9-4399-9860-4d1dbec12bb1
ms:mtpsurl: https://technet.microsoft.com/library/Ee832792(v=EXCHG.150)
ms:contentKeyID: 49685283
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 storage configuration options

_**Applies to:** Exchange Server 2013_

Understanding storage options and requirements for the Mailbox server role in Microsoft Exchange Server 2013 is an important part of your Mailbox server storage design solution.

## Storage architectures

The following table describes supported storage architectures and provides best practice guidance for each type of storage architecture where appropriate.

### Supported storage architectures

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Storage architecture</th>
<th>Description</th>
<th>Best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Direct-attached storage (DAS)</p></td>
<td><p>DAS is a digital storage system directly attached to a server or workstation, without a storage network in between. For example, DAS transports include Serial Attached Small Computer System Interface (SCSI) and Serial Attached Advanced Technology Attachment (ATA).</p></td>
<td><p>Not available.</p></td>
</tr>
<tr class="even">
<td><p>Storage area network (SAN): Internet Small Computer System Interface (iSCSI)</p></td>
<td><p>SAN is an architecture to attach remote computer storage devices (such as disk arrays and tape libraries) to servers in such a way that the devices appear as locally attached to the operating system (for example, block storage). iSCSI SANs encapsulate SCSI commands within IP packets and use standard networking infrastructure as the storage transport (for example, Ethernet).</p></td>
<td><p>Don't share physical disks backing up Exchange data with other applications.</p>
<p>Use dedicated storage networks.</p>
<p>Use multiple network paths for stand-alone configurations.</p></td>
</tr>
<tr class="odd">
<td><p>SAN: Fibre Channel</p></td>
<td><p>Fibre Channel SANs encapsulate SCSI commands within Fibre Channel packets and generally utilize specialized Fibre Channel networks as the storage transport.</p></td>
<td><p>Don't share physical disks backing up Exchange data with other applications.</p>
<p>Use multiple Fibre Channel network paths for stand-alone configurations.</p>
<p>Follow storage vendor's best practices for tuning Fibre Channel host bus adapters (HBAs), for example, Queue Depth and Queue Target.</p></td>
</tr>
</tbody>
</table>

A network-attached storage (NAS) unit is a self-contained computer connected to a network, with the sole purpose of supplying file-based data storage services to other devices on the network. The operating system and other software on the NAS unit provide the functionality of data storage, file systems, and access to files, and the management of these functionalities (for example, file storage).

All storage used by Exchange for storage of Exchange data must be block-level storage because Exchange 2013 doesn't support the use of NAS volumes, other than in the SMB 3.0 scenario outlined in the topic [Exchange 2013 virtualization](exchange-2013-virtualization-exchange-2013-help.md). Also, in a virtualized environment, NAS storage that's presented to the guest as block-level storage via the hypervisor isn't supported.

Using storage tiers is not recommended, as it could adversely affect system performance. For this reason, do not allow the storage controller to automatically move the most accessed files to "faster" storage.

## Physical disk types

The following table provides a list of supported physical disk types and provides best practice guidance for each physical disk type where appropriate.

### Supported physical disk types

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Physical disk type</th>
<th>Description</th>
<th>Supported or best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Serial ATA (SATA)</p></td>
<td><p>SATA is a serial interface for ATA and integrated device electronics (IDE) disks. SATA disks are available in a variety of form factors, speeds, and capacities.</p>
<p>In general, choose SATA disks for Exchange 2013 mailbox storage when you have the following design requirements:</p>
<ul>
<li><p>High capacity</p></li>
<li><p>Moderate performance</p></li>
<li><p>Moderate power utilization</p></li>
</ul></td>
<td><p>Supported:  512-byte sector disks for Windows Server 2008 and Windows Server 2008 R2. In addition, 512e disks are supported for Windows Server 2008 R2 with the following:</p>
<ul>
<li><p>The hotfix described in <a href="https://support.microsoft.com/help/982018">Microsoft Knowledge Base article 982018</a>, An update that improves the compatibility of Windows 7 and Windows Server 2008 R2 with Advanced Format Disks is available.</p></li>
<li><p>Windows Server 2008 R2 with Service Pack 1 (SP1) and Exchange Server 2010 SP1.</p></li>
</ul>
<p>Exchange 2013 and later supports native 4-kilobyte (KB) sector disks and 512e disks. Support requires that all copies of a database reside on the same physical disk type. For example, it is not a supported configuration to host one copy of a given database on a 512-byte sector disk and another copy of that same database on a 512e disk or 4K disk.</p>
<p>Best practice: Consider enterprise class SATA disks, which generally have better heat, vibration, and reliability characteristics.</p></td>
</tr>
<tr class="even">
<td><p>Serial Attached SCSI</p></td>
<td><p>Serial Attached SCSI is a serial interface for SCSI disks. Serial Attached SCSI disks are available in a variety of form factors, speeds, and capacities.</p>
<p>In general, choose Serial Attached SCSI disks for Exchange 2013 mailbox storage when you have the following design requirements:</p>
<ul>
<li><p>Moderate capacity</p></li>
<li><p>High performance</p></li>
<li><p>Moderate power utilization</p></li>
</ul></td>
<td><p>Supported:  512-byte sector disks for Windows Server 2008 and Windows Server 2008 R2. In addition, 512e disks are supported for Windows Server 2008 R2 with the following:</p>
<ul>
<li><p>The hotfix described in <a href="https://support.microsoft.com/help/982018">Microsoft Knowledge Base article 982018</a>, An update that improves the compatibility of Windows 7 and Windows Server 2008 R2 with Advanced Format Disks is available.</p></li>
<li><p>Windows Server 2008 R2 with Service Pack 1 (SP1) and Exchange Server 2010 SP1.</p></li>
</ul>
<p>Exchange 2013 and later supports native 4-kilobyte (KB) sector disks and 512e disks. Support requires that all copies of a database reside on the same physical disk type. For example, it is not a supported configuration to host one copy of a given database on a 512-byte sector disk and another copy of that same database on a 512e disk or 4K disk.</p>
<p>Best practice: Physical disk-write caching must be disabled when used without a UPS.</p></td>
</tr>
<tr class="odd">
<td><p>Fibre Channel</p></td>
<td><p>Fibre Channel is an electrical interface used to connect disks to Fibre Channel-based SANs. Fibre Channel disks are available in a variety of speeds and capacities.</p>
<p>In general, choose Fibre Channel disks for Exchange 2013 mailbox storage when you have the following design requirements:</p>
<ul>
<li><p>Moderate capacity</p></li>
<li><p>High performance</p></li>
<li><p>SAN connectivity</p></li>
</ul></td>
<td><p>Supported:  512-byte sector disks for Windows Server 2008 and Windows Server 2008 R2. In addition, 512e disks are supported for Windows Server 2008 R2 with the following:</p>
<ul>
<li><p>The hotfix described in <a href="https://support.microsoft.com/help/982018">Microsoft Knowledge Base article 982018</a>, An update that improves the compatibility of Windows 7 and Windows Server 2008 R2 with Advanced Format Disks is available.</p></li>
<li><p>Windows Server 2008 R2 with Service Pack 1 (SP1) and Exchange Server 2010 SP1.</p></li>
</ul>
<p>Exchange 2013 and later supports native 4-kilobyte (KB) sector disks and 512e disks. Support requires that all copies of a database reside on the same physical disk type. For example, it is not a supported configuration to host one copy of a given database on a 512-byte sector disk and another copy of that same database on a 512e disk or 4K disk.</p>
<p>Best practice: Physical disk-write caching must be disabled when used without a UPS.</p></td>
</tr>
<tr class="even">
<td><p>Solid-state drive (SSD) (flash disk)</p></td>
<td><p>An SSD is a data storage device that uses solid-state memory to store persistent data. An SSD emulates a hard disk drive interface. SSD disks are available in a variety of speeds (different I/O performance capabilities) and capacities.</p>
<p>In general, choose SSD disks for Exchange 2013 mailbox storage when you have the following design requirements:</p>
<ul>
<li><p>Low capacity</p></li>
<li><p>Extremely high performance</p></li>
</ul></td>
<td><p>Supported:  512-byte sector disks for Windows Server 2008 and Windows Server 2008 R2. In addition, 512e disks are supported for Windows Server 2008 R2 with the following:</p>
<ul>
<li><p>The hotfix described in <a href="https://support.microsoft.com/help/982018">Microsoft Knowledge Base article 982018</a>, An update that improves the compatibility of Windows 7 and Windows Server 2008 R2 with Advanced Format Disks is available.</p></li>
<li><p>Windows Server 2008 R2 with Service Pack 1 (SP1) and Exchange Server 2010 SP1.</p></li>
</ul>
<p>Exchange 2013 and later supports native 4-kilobyte (KB) sector disks and 512e disks. Support requires that all copies of a database reside on the same physical disk type. For example, it is not a supported configuration to host one copy of a given database on a 512-byte sector disk and another copy of that same database on a 512e disk or 4K disk.</p>
<p>Best practice: Physical disk-write caching must be disabled when used without a UPS.</p>
<p>In general, Exchange 2013 Mailbox servers don't require the performance characteristics of SSD storage.</p></td>
</tr>
</tbody>
</table>

## Factors to consider when choosing disk types

There are several trade-offs when choosing disk types for Exchange 2013 storage. The correct disk is one that balances performance (both sequential and random) with capacity, reliability, power utilization, and capital cost. The following table of supported physical disk types provides information to help you when considering these factors.

From a performance perspective, using large, slower disks for Exchange storage is okay, provided the disks can maintain an average read and write latency of 20ms or less under load.

### Factors in disk type choice

<table style="width:100%;">
<colgroup>
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
<col style="width: 14%" />
</colgroup>
<thead>
<tr class="header">
<th>Disk speed (RPM)</th>
<th>Disk form factor</th>
<th>Interface or transport</th>
<th>Capacity</th>
<th>Random I/O performance</th>
<th>Sequential I/O performance</th>
<th>Power utilization</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>5,400</p></td>
<td><p>2.5-inch</p></td>
<td><p>SATA</p></td>
<td><p>Average</p></td>
<td><p>Poor</p></td>
<td><p>Poor</p></td>
<td><p>Excellent</p></td>
</tr>
<tr class="even">
<td><p>5,400</p></td>
<td><p>3.5-inch</p></td>
<td><p>SATA</p></td>
<td><p>Excellent</p></td>
<td><p>Poor</p></td>
<td><p>Poor</p></td>
<td><p>Above average</p></td>
</tr>
<tr class="odd">
<td><p>7,200</p></td>
<td><p>2.5-inch</p></td>
<td><p>SATA</p></td>
<td><p>Average</p></td>
<td><p>Average</p></td>
<td><p>Average</p></td>
<td><p>Excellent</p></td>
</tr>
<tr class="even">
<td><p>7,200</p></td>
<td><p>2.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Average</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Excellent</p></td>
</tr>
<tr class="odd">
<td><p>7,200</p></td>
<td><p>3.5-inch</p></td>
<td><p>SATA</p></td>
<td><p>Excellent</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
</tr>
<tr class="even">
<td><p>7,200</p></td>
<td><p>3.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Excellent</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
</tr>
<tr class="odd">
<td><p>7,200</p></td>
<td><p>3.5-inch</p></td>
<td><p>Fibre Channel</p></td>
<td><p>Excellent</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Average</p></td>
</tr>
<tr class="even">
<td><p>10,000</p></td>
<td><p>2.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Below average</p></td>
<td><p>Excellent</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
</tr>
<tr class="odd">
<td><p>10,000</p></td>
<td><p>3.5-inch</p></td>
<td><p>SATA</p></td>
<td><p>Average</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
</tr>
<tr class="even">
<td><p>10,000</p></td>
<td><p>3.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
<td><p>Below average</p></td>
</tr>
<tr class="odd">
<td><p>10,000</p></td>
<td><p>3.5-inch</p></td>
<td><p>Fibre Channel</p></td>
<td><p>Average</p></td>
<td><p>Above average</p></td>
<td><p>Above average</p></td>
<td><p>Below average</p></td>
</tr>
<tr class="even">
<td><p>15,000</p></td>
<td><p>2.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Poor</p></td>
<td><p>Excellent</p></td>
<td><p>Excellent</p></td>
<td><p>Average</p></td>
</tr>
<tr class="odd">
<td><p>15,000</p></td>
<td><p>3.5-inch</p></td>
<td><p>Serial Attached SCSI</p></td>
<td><p>Average</p></td>
<td><p>Excellent</p></td>
<td><p>Excellent</p></td>
<td><p>Below average</p></td>
</tr>
<tr class="even">
<td><p>15,000</p></td>
<td><p>3.5-inch</p></td>
<td><p>Fibre Channel</p></td>
<td><p>Average</p></td>
<td><p>Excellent</p></td>
<td><p>Excellent</p></td>
<td><p>Poor</p></td>
</tr>
<tr class="odd">
<td><p>SSD: enterprise class</p></td>
<td><p>Not applicable</p></td>
<td><p>SATA, Serial Attached SCSI, Fibre Channel</p></td>
<td><p>Poor</p></td>
<td><p>Excellent</p></td>
<td><p>Excellent</p></td>
<td><p>Excellent</p></td>
</tr>
</tbody>
</table>

## Best practices for supported storage configurations

This section provides best practice information about supported disk and array controller configurations.

Redundant Array of Independent Disks (RAID) is often used to both improve the performance characteristics of individual disks (by striping data across several disks) as well as to provide protection from individual disk failures. With the advancements in Exchange 2013 high availability, RAID is not a required component for Exchange 2013 storage design. However, RAID is still an essential component of Exchange 2013 storage design for standalone servers as well as solutions that require storage fault tolerance.

**Operating System, System, or Pagefile Volume**

The recommended configuration for an operating system, system or pagefile volume is to utilize RAID technology to protect this data type. The recommended RAID configuration is either RAID-1 or RAID-1/0, however all RAID types are supported.

**Separated Mailbox Database and Log Volumes**

If you are deploying a standalone Mailbox server role architecture, RAID technology is required for the mailbox database and log volumes. The recommended RAID configuration for mailbox volumes is RAID-1/0 (especially if you are using 5.4K or 7.2K disks); however all RAID types are supported. For log volumes, RAID-1 or RAID-1/0 is the recommended RAID configuration.

When using RAID-5 or RAID-6 configurations for the operating system, pagefile, or Exchange data volumes, note the following:

- RAID-5 configurations, including variations such as RAID-50 and RAID-51, should have no more than 7 disks per array group and array controller high-priority scrubbing and surface scanning enabled.

- RAID-6 configurations should have array controller high-priority scrubbing and surface scanning enabled.

While JBOD is supported in high availability architectures that have 3 or more highly available database copies, because the log and mailbox database volumes are separated, JBOD is not recommended.

**Mailbox Database and Log Volume Co-Location**

Mailbox database and log volume co-location is not recommended in standalone architectures. In high availability architectures, there are two possibilities for this scenario:

1. Single database per volume

2. Multiple databases per volume

**Single Database Per Volume**

From an Exchange perspective, JBOD means having both the database and its associated logs stored on a single disk. To deploy on JBOD, you must deploy a minimum of three highly available database copies. Utilizing a single disk is a single point of failure, because when the disk fails, the database copy residing on that disk is lost. Having a minimum of three database copies ensures fault tolerance by having two additional copies in the event that one copy (or one disk) fails. However, placement of three highly available database copies, as well as the use of lagged database copies, can affect storage design. The following table shows guidelines for RAID or JBOD considerations.

### RAID or JBOD Considerations

<table style="width:100%;">
<colgroup>
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th>Datacenter servers</th>
<th>Two highly available copies (total)</th>
<th>Three highly available copies (total)</th>
<th>Two or more highly available copies per datacenter</th>
<th>One lagged copy</th>
<th>Two or more lagged copies per datacenter</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Primary datacenter servers</p></td>
<td><p>RAID</p></td>
<td><p>RAID or JBOD (2 copies)</p></td>
<td><p>RAID or JBOD</p></td>
<td><p>RAID</p></td>
<td><p>RAID or JBOD</p></td>
</tr>
<tr class="even">
<td><p>Secondary datacenter servers</p></td>
<td><p>RAID</p></td>
<td><p>RAID (1 copy)</p></td>
<td><p>RAID or JBOD</p></td>
<td><p>RAID</p></td>
<td><p>RAID or JBOD</p></td>
</tr>
</tbody>
</table>

To deploy on JBOD with the primary datacenter servers, you need three or more highly available database copies within the DAG. If mixing lagged copies on the same server hosting highly available database copies (for example, not using dedicated lagged database copy servers), you need at least two lagged database copies.

For the secondary datacenter servers to use JBOD, you should have at least two highly available database copies in the secondary datacenter. The loss of a copy in the secondary datacenter won't result in requiring a reseed across the WAN or having a single point of failure in the event the secondary datacenter is activated. If mixing lagged database copies on the same server hosting highly available database copies (for example, not using dedicated lagged database copy servers), you need at least two lagged database copies.

For dedicated lagged database copy servers, you should have at least two lagged database copies within a datacenter to use JBOD. Otherwise, the loss of disk results in the loss of the lagged database copy, as well as the loss of the protection mechanism.

**Multiple Databases Per Volume**

Multiple databases per volume is a new JBOD scenario available in Exchange 2013 that allows for active and passive copies (including lagged copies) to be mixed on a single disk, enabling better disk utilization. However, to deploy lagged copies in this manner, automatic lagged copy log file play down must be enabled. The following table shows guidelines for JBOD considerations for multiple databases per volume.

### JBOD Considerations

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Datacenter Servers</th>
<th>3 or more copies (total)</th>
<th>Two or more copies per datacenter</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Primary datacenter servers</p></td>
<td><p>JBOD</p></td>
<td><p>JBOD</p></td>
</tr>
<tr class="even">
<td><p>Secondary datacenter servers</p></td>
<td><p>N/A</p></td>
<td><p>JBOD</p></td>
</tr>
</tbody>
</table>

The following table provides guidance about storage array configurations for Exchange 2013.

### Supported RAID types for the Exchange 2013 Mailbox server role

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>RAID type</th>
<th>Description</th>
<th>Supported or best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Disk array RAID stripe size (KB)</p></td>
<td><p>The stripe size is the per disk unit of data distribution within a RAID set. Stripe size is also referred to as <em>block size</em>.</p></td>
<td><p>Best practice: 256 KB or greater. Follow storage vendor best practices.</p></td>
</tr>
<tr class="even">
<td><p>Storage array cache settings</p></td>
<td><p>The cache settings are provided by a battery-backed caching array controller.</p></td>
<td><p>Best practice: 100 percent write cache (battery or flash-backed cache) for DAS storage controllers in either a RAID or JBOD configuration. 75 percent write cache, 25 percent read cache (battery or flash-backed cache) for other types of storage solutions such as SAN. If your SAN vendor has different best practices for cache configuration on their platform, follow the guidance of your SAN vendor.</p></td>
</tr>
<tr class="odd">
<td><p>Physical disk write caching</p></td>
<td><p>The settings for the cache are on each individual disk.</p></td>
<td><p>Supported: Physical disk write caching must be disabled when used without a UPS.</p></td>
</tr>
</tbody>
</table>

The following table provides guidance about database and log file choices.

### Database and log file choices for the Exchange 2013 Mailbox server role

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Database and log file options</th>
<th>Description</th>
<th>Stand-alone: supported or best practice</th>
<th>High availability: supported or best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>File placement: database per log isolation</p></td>
<td><p>Database per log isolation refers to placing the database file and logs from the same mailbox database onto different volumes backed by different physical disks.</p></td>
<td><p>Best practice: For recoverability, move database (.edb) file and logs from the same database to different volumes backed by different physical disks.</p></td>
<td><p>Supported: Isolation of logs and databases isn't required.</p></td>
</tr>
<tr class="even">
<td><p>File placement: database files per volume</p></td>
<td><p>Database files per volume refers to how you distribute database files within or across disk volumes.</p></td>
<td><p>Best practice: Based on your backup methodology.</p></td>
<td><p>Supported: When using JBOD, create a single volume with separate directories for database(s) and for log files.</p></td>
</tr>
<tr class="odd">
<td><p>File placement: log streams per volume</p></td>
<td><p>Log streams per volume refers to how you distribute database log files within or across disk volumes.</p></td>
<td><p>Best practice: Based on your backup methodology.</p></td>
<td><p>Supported: When using JBOD, create a single volume with separate directories for database(s) and for log files.</p>
<p>Best practice: When using JBOD, leverage multiple databases per volume.</p></td>
</tr>
<tr class="even">
<td><p>Database size</p></td>
<td><p>Database size refers to the disk database (.edb) file size.</p></td>
<td><p>Supported: Approximately 16 terabytes.</p>
<p>Best practice:</p>
<ul>
<li><p>200 gigabytes (GB) or less.</p></li>
<li><p>Provision for 120 percent of calculated maximum database size.</p></li>
</ul></td>
<td><p>Supported: Approximately 16 terabytes.</p>
<p>Best practice:</p>
<ul>
<li><p>2 terabytes or less.</p></li>
<li><p>Provision for 120 percent of calculated maximum database size.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Log truncation method</p></td>
<td><p>Log truncation method is the process for truncating and deleting old database log files. There are two mechanisms:</p>
<ul>
<li><p>Circular logging, in which Exchange deletes the logs.</p></li>
<li><p>Log truncation, which occurs after a successful full or incremental Volume Shadow Copy Service (VSS) backup.</p></li>
</ul></td>
<td><p>Best practice:</p>
<ul>
<li><p>Use backups for log truncation (for example, circular logging disabled).</p></li>
<li><p>Provision for three days of log generation capacity.</p></li>
</ul></td>
<td><p>Best practice:</p>
<ul>
<li><p>Enable circular logging for deployments that use Exchange native data protection features.</p></li>
<li><p>Provision for three days beyond replay lag setting of log generation capacity.</p></li>
</ul></td>
</tr>
</tbody>
</table>

The following table provides guidance about Windows disk types.

### Windows disk types for the Exchange 2013 Mailbox server role

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Windows disk type</th>
<th>Description</th>
<th>Stand-alone: supported or best practice</th>
<th>High availability: supported or best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Basic disk</p></td>
<td><p>A disk initialized for basic storage is called a basic disk. A basic disk contains basic volumes, such as primary partitions, extended partitions, and logical drives.</p></td>
<td><p>Supported.</p>
<p>Best practice: Use basic disks.</p></td>
<td><p>Supported.</p>
<p>Best practice: Use basic disks.</p></td>
</tr>
<tr class="even">
<td><p>Dynamic disk</p></td>
<td><p>A disk initialized for dynamic storage is called a dynamic disk. A dynamic disk contains dynamic volumes, such as simple volumes, spanned volumes, striped volumes, mirrored volumes, and RAID-5 volumes.</p></td>
<td><p>Supported.</p></td>
<td><p>Supported.</p></td>
</tr>
</tbody>
</table>

The following table provides guidance on volume configurations.

### Volume configurations for the Exchange 2013 Mailbox server role

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Volume configuration</th>
<th>Description</th>
<th>Stand-alone: supported or best practice</th>
<th>High availability: supported or best practice</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>GUID partition table (GPT)</p></td>
<td><p>GPT is a disk architecture that expands on the older master boot record (MBR) partitioning scheme. The maximum NTFS formatted partition size is 256 terabytes.</p></td>
<td><p>Supported.</p>
<p>Best practice: Use GPT partitions.</p></td>
<td><p>Supported.</p>
<p>Best practice: Use GPT partitions.</p></td>
</tr>
<tr class="even">
<td><p>MBR</p></td>
<td><p>An MBR, or partition sector, is the 512-byte boot sector that is the first sector (LBA Sector 0) of a partitioned data storage device such as a hard disk. The maximum NTFS formatted partition size is 2 terabytes.</p></td>
<td><p>Supported.</p></td>
<td><p>Supported.</p></td>
</tr>
<tr class="odd">
<td><p>Partition alignment</p></td>
<td><p>Partition alignment refers to aligning partitions on sector boundaries for optimal performance.</p></td>
<td><p>Supported: The Windows Server 2008 R2 and Windows Server 2012 default is 1 megabyte (MB).</p></td>
<td><p>Supported: The Windows Server 2008 R2 and Windows Server 2012 default is 1 MB.</p></td>
</tr>
<tr class="even">
<td><p>Volume path</p></td>
<td><p>Volume path refers to how a volume is accessed.</p></td>
<td><p>Supported: Drive letter or mount point.</p>
<p>Best practice: Mount point host volume must be RAID enabled.</p></td>
<td><p>Supported: Drive letter or mount point.</p>
<p>Best practice: Mount point host volume must be RAID-enabled.</p></td>
</tr>
<tr class="odd">
<td><p>File system</p></td>
<td><p>File system is a method for storing and organizing computer files and the data they contain to make it easy to find and access the files.</p></td>
<td><p>Supported: NTFS and ReFS.</p></td>
<td><p>Supported: NTFS and ReFS.</p></td>
</tr>
<tr class="even">
<td><p>NTFS defragmentation</p></td>
<td><p>NTFS defragmentation is a process that reduces the amount of fragmentation in Windows file systems. It does this by physically organizing the contents of the disk to store the pieces of each file close together and contiguously.</p></td>
<td><p>Supported.</p>
<p>Best practice: Not required and not recommended. On Windows Server 2012, we also recommend disabling the automatic disk optimization and defragmentation feature.</p></td>
<td><p>Supported.</p>
<p>Best practice: Not required and not recommended. On Windows Server 2012, we also recommend disabling the automatic disk optimization and defragmentation feature.</p></td>
</tr>
<tr class="odd">
<td><p>NTFS allocation unit size</p></td>
<td><p>NTFS allocation unit size represents the smallest amount of disk space that can be allocated to hold a file.</p></td>
<td><p>Supported: All allocation unit sizes.</p>
<p>Best practice: 64 KB for both .edb and log file volumes.</p></td>
<td><p>Supported: All allocation unit sizes.</p>
<p>Best practice: 64 KB for both .edb and log file volumes.</p></td>
</tr>
<tr class="even">
<td><p>NTFS compression</p></td>
<td><p>NTFS compression is the process of reducing the actual size of a file stored on the hard disk.</p></td>
<td><p>Supported: Not supported for Exchange database or log files.</p></td>
<td><p>Supported: Not supported for Exchange database or log files.</p></td>
</tr>
<tr class="odd">
<td><p>NTFS Encrypting File System (EFS)</p></td>
<td><p>EFS enables users to encrypt individual files, folders, or entire data drives. Because EFS provides strong encryption through industry-standard algorithms and public key cryptography, encrypted files are confidential even if an attacker bypasses system security.</p></td>
<td><p>Supported: Not supported for Exchange database or log files.</p></td>
<td><p>Not supported for Exchange database or log files.</p></td>
</tr>
<tr class="even">
<td><p>Windows BitLocker (volume encryption)</p></td>
<td><p>Windows BitLocker is a data protection feature in Windows Server 2008. BitLocker protects against data theft or exposure on computers that are lost or stolen, and it offers more secure data deletion when computers are decommissioned.</p></td>
<td><p>Supported: All Exchange database and log files.</p></td>
<td><p>Supported: All Exchange database and log files. Windows failover clusters require Windows Server 2008 R2 or Windows Server 2008 R2 SP1. Exchange volumes with Bitlocker enabled are not supported on Windows failover clusters running earlier versions of Windows.</p>
<p>For more information about Windows 7 BitLocker encryption, see <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-7/ee449438(v=ws.10)">BitLocker Drive Encryption in Windows 7: Frequently Asked Questions</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Server Message Block (SMB) 3.0</p></td>
<td><p>The Server Message Block (SMB) protocol is a network file sharing protocol (on top of TCP/IP or other network protocols) that allows applications on a computer to access files and resources on a remote server. It also allows applications to communicate with any server program that is set up to receive an SMB client request. Windows Server 2012 introduces the new 3.0 version of the SMB protocol with the following features:</p>
<ul>
<li><p>SMB Transparent failover</p></li>
<li><p>SMB Scaleout</p></li>
<li><p>SMB Multichannel</p></li>
<li><p>SMB Direct</p></li>
<li><p>SMB Encryption</p></li>
<li><p>VSS for SMB file shares</p></li>
<li><p>SMB Directory Leasing</p></li>
<li><p>SMB PowerShell</p></li>
</ul></td>
<td><p>Limited Support. Supported scenario is a hardware virtualized deployment where the disks are hosted on VHDs on an SMB 3.0 share. These VHDs are presented to the host via a hypervisor. For more information, see <a href="exchange-2013-virtualization-exchange-2013-help.md">Exchange 2013 virtualization</a>.</p></td>
<td><p>Limited Support. Supported scenario is a hardware virtualized deployment where the disks are hosted on VHDs on an SMB 3.0 share. These VHDs are presented to the host via a hypervisor. For more information, see <a href="exchange-2013-virtualization-exchange-2013-help.md">Exchange 2013 virtualization</a>.</p></td>
</tr>
<tr class="even">
<td><p>Storage Spaces</p></td>
<td><p>Storage Spaces is a new storage solution that delivers virtualization capabilities for Windows Server 2012. Storage Spaces allow you to organize physical disks into storage pools, which can be easily expanded by simply adding disks. These disks can be connected either through USB, SATA or SAS. It also utilizes virtual disks (spaces), which behave just like physical disks, with associated powerful capabilities such as thin provisioning, as well as resiliency to failures of underlying physical media. For more information on Storage Spaces, see <a href="https://docs.microsoft.com/windows-server/storage/storage-spaces/overview">Storage Spaces Overview</a>.</p></td>
<td><p>Supported. Same restrictions as for physical disk types outlined in this topic.</p></td>
<td><p>Supported. Same restrictions as for physical disk types outlined in this topic.</p></td>
</tr>
<tr class="odd">
<td><p>Resilient File System (ReFS)</p></td>
<td><p>ReFS is a newly engineered file system for Windows Server 2012 that is built on the foundations of NTFS. ReFS maintains high degree of compatibility with NTFS while providing enhanced data verification and auto-correction techniques as well as an integrated end-to-end resiliency to corruptions especially when used in conjunction with the storage spaces feature. For more information on ReFS, see <a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">Resilient File System Overview</a>.</p></td>
<td><p>Supported for volumes containing Exchange database files, log files and content indexing files. If deploying on Windows Server 2012, ensure the following hotfixes are installed on Windows Server 2012:</p>
<ul>
<li><p><a href="https://support.microsoft.com/help/2822241">Windows 8 and Windows Server 2012 update rollup: April 2013</a></p></li>
<li><p><a href="https://support.microsoft.com/help/2884597">Virtual Disk Service or applications that use the Virtual Disk Service crash or freeze in Windows Server 2012</a></p></li>
<li><p><a href="https://support.microsoft.com/help/2894875">Windows 8-based or Windows Server 2012-based computer freezes when you run the 'dir' command on an ReFS volume</a></p></li>
</ul>
<p>ReFS is not supported for OS volumes.</p>
<p>Best practice: Data integrity features must be disabled for the Exchange database (.edb) files or the volume that hosts these files.</p></td>
<td><p>Supported for volumes containing Exchange database files, log files and content indexing files. If deploying on Windows Server 2012, ensure the following hotfixes are installed on Windows Server 2012:</p>
<ul>
<li><p><a href="https://support.microsoft.com/help/2822241">Windows 8 and Windows Server 2012 update rollup: April 2013</a></p></li>
<li><p><a href="https://support.microsoft.com/help/2884597">Virtual Disk Service or applications that use the Virtual Disk Service crash or freeze in Windows Server 2012</a></p></li>
<li><p><a href="https://support.microsoft.com/help/2894875">Windows 8-based or Windows Server 2012-based computer freezes when you run the 'dir' command on an ReFS volume</a></p></li>
</ul>
<p>ReFS is not supported for OS volumes.</p>
<p>Best practice: Data integrity features must be disabled for the Exchange database (.edb) files or the volume that hosts these files.</p></td>
</tr>
<tr class="even">
<td><p>Data De-Duplication</p></td>
<td><p>Data deduplication is a new technique to optimize storage utilization for Windows Server 2012. It is a method of finding and removing duplication within data without compromising its fidelity or integrity. The goal is to store more data in less space by segmenting files into small variable-sized chunks, identifying duplicate chunks, and maintaining a single copy of each chunk. Redundant copies of the chunk are replaced by a reference to the single copy, the chunks are organized into container files, and the containers are compressed for further space optimization.</p></td>
<td><p>Not Supported for Exchange database files. Note: Can be used for Exchange database files that are completely offline (used as backups or archives).</p></td>
<td><p>Not Supported for Exchange database files. Note: Can be used for Exchange database files that are completely offline (used as backups or archives).</p></td>
</tr>
</tbody>
</table>
