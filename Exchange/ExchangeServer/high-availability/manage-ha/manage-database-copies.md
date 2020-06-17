---
localization_priority: Normal
description: 'Summary: Learn about managing mailbox database copies in the Mailbox server in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 28cedf1d-365a-4e36-b2ba-6bf81af8684f
ms.reviewer:
title: Manage mailbox database copies
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage mailbox database copies

In Exchange Server, you can use the Exchange Management Console (EAC) or the Exchange Management Shell to add mailbox database copies after a database availability group (DAG) has been created, configured, and populated with Mailbox server members.

## Managing database copies
<a name="MngDBCopies"> </a>

After multiple copies of a database are created, you can use the EAC or the Exchange Management Shell to monitor the health and status of each copy and to perform other management tasks associated with database copies. Some of the management tasks you may need to perform include suspending or resuming a database copy, seeding a database copy, monitoring database copies, configuring database copy settings, and removing a database copy.

### Suspending and resuming database copies
<a name="Suspend"> </a>

For a variety of reasons, such as performing planned maintenance, you may need to suspend and resume continuous replication activity for a database copy. In addition, some administrative tasks, such as seeding, require that you first suspend a database copy. We recommend that you suspend all replication activity when the path for the database or its log files is being changed. You can suspend and resume database copy activity by using the EAC, or by running the **Suspend-MailboxDatabaseCopy** and **Resume-MailboxDatabaseCopy** cmdlets in the Exchange Management Shell. For detailed steps about how to suspend or resume continuous replication activity for a database copy, see [Suspend or resume a mailbox database copy](suspend-resume-db-copies.md).

### Seeding a database copy
<a name="Seed"> </a>

 *Seeding*, also known as *updating*, is the process in which either a blank database or a copy of the production database, is added to the target copy location on another Mailbox server in the same DAG as the active database. This becomes the baseline database for the copy maintained by that server.

Depending on the situation, you can seed a database by using an automatic process or a manual process that you initiate. When a database copy is added, the copy will be automatically seeded, provided that the target server and its storage are properly configured. If you want to manually seed a database copy and don't want automatic seeding to occur when creating the copy, you can use the _SeedingPostponed_ parameter when running the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) cmdlet.

Database copies rarely need to be reseeded after the initial seeding. However, if reseeding is necessary, or if you want to manually seed a database copy instead of having the system automatically seed the copy, you have two options. You can reseed a database by using the Update Mailbox Database Copy wizard in the EAC or by using the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet in the Exchange Management Shell. Before seeding a database copy, you must first suspend the mailbox database copy. For detailed steps about how to seed a database copy, see [Update a mailbox database copy](update-db-copies.md).

After a manual seed operation has completed, replication for the seeded mailbox database copy is automatically resumed. If you don't want replication to automatically resume, you can use the _ManualResume_ parameter when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet.

#### Choosing what to seed

When you perform a seed operation, you can choose to seed the mailbox database copy, the content index catalog for the mailbox database copy, or both the database copy and the content index catalog copy. The default behavior of the Update Mailbox Database Copy wizard and the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet is to seed both the mailbox database copy and the content index catalog copy. To seed just the mailbox database copy without seeding the content index catalog, use the _DatabaseOnly_ parameter when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet. To seed just the content index catalog copy, use the _CatalogOnly_ parameter when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet.

#### Selecting the seeding source

Any healthy database copy can be used as the seeding source for an additional copy of that database. This is particularly useful when you have a DAG that has been extended across multiple physical locations. For example, consider a four-member DAG deployment, where two members (MBX1 and MBX2) are located in Portland, Oregon and two members (MBX3 and MBX4) are located in New York, New York. A mailbox database named DB1 is active on MBX1 and there are passive copies of DB1 on MBX2 and MBX3. When adding a copy of DB1 to MBX4, you have the option of using the copy on MBX3 as the source for seeding. In doing so, you avoid seeding over the wide area network (WAN) link between Portland and New York.

To use a specific copy as a source for seeding when adding a new database copy, you can do the following:

- Use the _SeedingPostponed_ parameter when running the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) cmdlet to add the database copy. If you don't use the _SeedingPostponed_ parameter, the database copy will be explicitly seeded using the active copy of the database as the source.

- You can specify the source server you want to use as part of the Update Mailbox Database Copy wizard in the EAC, or you can use the _SourceServer_ parameter when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet to specify the desired source server for seeding. In the preceding example, you would specify MBX3 as the source server. If the you don' t use _SourceServer_ parameter, the database copy will be explicitly seeded from the active copy of the database.

#### Seeding and networks

In addition to selecting a specific source server for seeding a mailbox database copy, you can also use the Exchange Management Shell to specify which DAG networks to use. You have the option to override the DAG network's compression and encryption settings during the seed operation.

 You can specify the networks you want to use for seeding by using the _Network_ parameter when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet and specify the DAG networks that you want to use. If you don't use the _Network_ parameter, the system uses the following default behavior for selecting a network to use for the seeding operation:

- If the source server and target server are on the same subnet and a replication network has been configured that includes the subnet, the replication network will be used.

- If the source server and target server are on different subnets, even if a replication network that contains those subnets has been configured, the client (MAPI) network will be used for seeding.

- If the source server and target server are in different datacenters, the client (MAPI) network will be used for seeding.

At the DAG level, DAG networks are configured for encryption and compression. The default settings use encryption and compression only for communications on different subnets. If the source and target are on different subnets and the DAG is configured with the default values for _NetworkCompression_ and _NetworkEncryption_, you can override these values by using the _NetworkCompressionOverride_ and _NetworkEncryptionOverride_ parameters, respectively, when running the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet.

#### Seeding process

When you begin a seeding process by using the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) or [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlets, the following tasks are performed:

1. Database properties from Active Directory are read to validate the specified database and servers, and to verify that the source and target servers are running Exchange Server, they are both members of the same DAG, and that the specified database isn't a recovery database. The database file paths are also read.

2. Preparations occur for reseed checks from the Microsoft Exchange Replication service on the target server.

3. The Microsoft Exchange Replication service on the target server checks for the presence of database and transaction log files in the file directories read by the Active Directory checks in step 1.

4. The Microsoft Exchange Replication service returns the status information from the target server to the administrative interface from where the cmdlet was run.

5. If all preliminary checks have passed, you're prompted to confirm the operation before continuing. If you confirm the operation, the process continues. If an error is encountered during the preliminary checks, the error is reported and the operation fails.

6. The seed operation is started from the Microsoft Exchange Replication service on the target server.

7. The Microsoft Exchange Replication service suspends database replication for the active database copy.

8. The state information for the database is updated by the Microsoft Exchange Replication service to reflect a status of Seeding.

9. If the target server doesn't already have the directories for the target database and log files, they are created.

10. A request to seed the database is passed from the Microsoft Exchange Replication service on the target server to the Microsoft Exchange Replication service on the source server using TCP. This request and the subsequent communications for seeding the database occur on a DAG network that has been configured as a replication network.

11. The Microsoft Exchange Replication service on the source server initiates an Extensible Storage Engine (ESE) streaming backup via the Microsoft Exchange Information Store service interface.

12. The Microsoft Exchange Information Store service streams the database data to the Microsoft Exchange Replication service.

13. The database data is moved from the source server's Microsoft Exchange Replication service to the target server's Microsoft Exchange Replication service.

14. The Microsoft Exchange Replication service on the target server writes the database copy to a temporary directory located in the main database directory called *temp-seeding*.

15. The streaming backup operation on the source server ends when the end of the database is reached.

16. The write operation on the target server completes, and the database is moved from the temp-seeding directory to the final location. The temp-seeding directory is deleted.

17. On the target server, the Microsoft Exchange Replication service proxies a request to the Microsoft Exchange Search service to mount the content index catalog for the database copy, if it exists. If there are existing out-of-date catalog files from a previous instance of the database copy, the mount operation fails, which triggers the need to replicate the catalog from the source server. Likewise, if the catalog doesn't exist on a new instance of the database copy on the target server, a copy of the catalog is required. The Microsoft Exchange Replication service directs the Microsoft Exchange Search service to suspend indexing for the database copy while a new catalog is copied from the source.

18. The Microsoft Exchange Replication service on the target server sends a seed catalog request to the Microsoft Exchange Replication service on the source server.

19. On the source server, the Microsoft Exchange Replication service requests the directory information from the Microsoft Exchange Search service and requests that indexing be suspended.

20. The Microsoft Exchange Search service on the source server returns the search catalog directory information to the Microsoft Exchange Replication service.

21. The Microsoft Exchange Replication service on the source server reads the catalog files from the directory.

22. The Microsoft Exchange Replication service on the source server moves the catalog data to the Microsoft Exchange Replication service on the target server using a connection across the replication network. After the read is complete, the Microsoft Exchange Replication service sends a request to the Microsoft Exchange Search service to resume indexing of the source database.

23. If there are any existing catalog files on the target server in the directory, the Microsoft Exchange Replication service on the target server deletes them.

24. The Microsoft Exchange Replication service on the target server writes the catalog data to a temporary directory called *CiSeed.Temp* until the data is completely transferred.

25. The Microsoft Exchange Replication service moves the complete catalog data to the final location.

26. The Microsoft Exchange Replication service on the target server resumes search indexing on the target database.

27. The Microsoft Exchange Replication service on the target server returns a completion status.

28. The final result of the operation is passed to the administrative interface from which the cmdlet was called.

### Configuring database copies
<a name="Seed"> </a>

After a database copy is created, you can view and modify its configuration settings when needed. You can view some configuration information by examining the **Properties** page for a database copy in the EAC. You can also use the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase) and [Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabasecopy) cmdlets in the Exchange Management Shell to view and configure database copy settings, such as replay lag time, truncation lag time, and activation preference order. For detailed steps about how to view and configure database copy settings, see [Configure mailbox database copy properties](configure-db-properties.md).

### Using replay lag and truncation lag options
<a name="Seed"> </a>

Mailbox database copies support the use of a *replay lag time* and a *truncation lag time*, both of which are configured in minutes. Setting a replay lag time enables you to take a database copy back to a specific point in time. Setting a truncation lag time enables you to use the logs on a passive database copy to recover from the loss of log files on the active database copy. Because both of these features result in the temporary buildup of log files, using either of them will affect your storage design.

#### Replay lag time

Replay lag time is a mailbox database copy property that specifies the amount of time, in minutes, to delay log replay for the database copy. The replay lag timer starts when a log file has been replicated to the passive copy and has successfully passed inspection. By delaying the replay of logs to the database copy, you have the capability to recover the database to a specific point in time in the past. A mailbox database copy configured with a replay lag time greater than 0 is referred to as a *lagged mailbox database copy*, or simply, a *lagged copy*.

A strategy that uses database copies and the litigation hold features in Exchange Server can provide protection against a range of failures that would ordinarily cause data loss. However, these features can't provide protection against data loss in the event of logical corruption, which although rare, can cause data loss. Lagged copies are designed to prevent loss of data in the case of logical corruption. Generally, there are two types of logical corruption:

- **Database logical corruption**: The database pages checksum matches, but the data on the pages is wrong logically. This can occur when ESE attempts to write a database page and even though the operating system returns a success message, the data is either never written to the disk or it's written to the wrong place. This is referred to as a *lost flush*. To prevent lost flushes from losing data, ESE includes a lost flush detection mechanism in the database along with a page patching feature (single page restore).

- **Store logical corruption**: Data is added, deleted, or manipulated in a way that the user doesn't expect. These cases are generally caused by third-party applications. It's generally only corruption in the sense that the user views it as corruption. The Exchange store considers the transaction that produced the logical corruption to be a series of valid MAPI operations. The litigation hold feature in Exchange Server provides protection from store logical corruption (because it prevents content from being permanently deleted by a user or application). However, there may be scenarios where a user mailbox becomes so corrupted that it would be easier to restore the database to a point in time prior to the corruption, and then export the user mailbox to retrieve uncorrupted data.

The combination of database copies, hold policy, and ESE single page restore leaves only the rare but catastrophic store logical corruption case. Your decision on whether to use a database copy with a replay lag (a lagged copy) will depend on which third-party applications you use and your organization's history with store logical corruption.

If you choose to use lagged copies, be aware of the following implications for their use:

- The replay lag time is an administrator-configured value, and by default, it's disabled.

- The replay lag time setting has a default setting of 0 days, and a maximum setting of 14 days.

- Lagged copies aren't considered highly available copies. Instead, they are designed for disaster recovery purposes, to protect against store logical corruption.

- The greater the replay lag time set, the longer the database recovery process. Depending on the number of log files that need to replayed during recovery, and the speed at which your hardware can replay them, it may take several hours or more to recover a database.

- We recommend that you determine whether lagged copies are critical for your overall disaster recovery strategy. If using them is critical to your strategy, we recommend using multiple lagged copies, or using a redundant array of independent disks (RAID) to protect a single lagged copy, if you don't have multiple lagged copies. If you lose a disk or if corruption occurs, you don't lose your lagged point in time.

- Lagged copies cant be patched with the ESE single page restore feature. If a lagged copy encounters database page corruption (for example, a -1018 error), the copy will have to be reseeded. Reseeding will lose the lagged aspect of the copy.

If you want the database to replay all log files and make the database copy current, then activating and recovering a lagged mailbox database copy is an easy process . If you want to replay log files up to a specific point in time, the prosess is more difficult because you have to manually manipulate log files and run Exchange Server Database Utilities (Eseutil.exe).

For detailed steps about how to activate a lagged mailbox database copy, see [Activate a lagged mailbox database copy](activate-lagged-db-copies.md).

#### Truncation lag time

Truncation lag time is the property of a mailbox database copy that specifies the amount of time, in minutes, to delay log deletion for the database copy after the log file has been replayed into the database copy. The truncation lag timer starts when a log file has been replicated to the passive copy, successfully passed inspection, and has been successfully replayed into the copy of the database. By delaying the truncation of log files from the database copy, you have the capability to recover from failures that affect the log files for the active copy of the database.

#### Database copies and log truncation

Log truncation works the same in Exchange 2016 and Exchange 2019 as it did in Exchange 2010. Truncation behavior is determined by the replay lag time and truncation lag time settings for the copy.

The following criteria must be met for a database copy's log file to be truncated when lag settings are left at their default values of 0 (disabled):

- The log file must have been successfully backed up, or circular logging must be enabled.

- The log file must be below the checkpoint (the minimum log file required for recovery) for the database.

- All other lagged copies must have inspected the log file.

- All other copies (except lagged copies) must have replayed the log file.

The following criteria must be met for truncation to occur for a lagged database copy:

- The log file must be below the checkpoint for the database.

- The log file must be older than ReplayLagTime + TruncationLagTime.

- The log file must have been truncated on the active copy.

In Exchange Server, log truncation doesn't occur on an active mailbox database copy when one or more passive copies are suspended. If planned maintenance activities are going to take an extended period of time (for example, several days), you may have considerable log file buildup. To prevent the log drive from filling up with transaction logs, you can remove the affected passive database copy instead of suspending it. When the planned maintenance is completed, you can re-add the passive database copy.

 Exchange Server now has a feature called *loose truncation* that is disabled by default. During normal operations, each database copy keeps logs that need to be shipped to other database copies until all copies of a database confirm they have replayed (passive copies) or received (lagged copies) the log files. This is default log truncation behavior. If a database copy goes offline for some reason, the log files begin accumulating on the disks used by the other copies of the database. If the affected database copy remains offline for an extended period, this can cause the other database copies to run out of disk space.

Truncation behavior is different when loose truncation and circular logging are enabled. Each database copy tracks its own free disk space and applies loose truncation behavior if free space gets low.

- For the active copy, the oldest straggler (the passive database copy that is farthest behind in log replay) is ignored and truncation respects the oldest remaining passive copies. The active database copy is where global truncation is calculated.

- For a passive copy, if space gets low, it will independently truncate its log files using the configured parameters described later in the Registry Value table.The passive copies will attempt to respect the truncation decision made on the active copy. Despite the implication of the name MinCopiesToProtect, Exchange will only ignore the oldest known straggler at the time truncation is run.

When the offline database is brought back online, it will be missing log files that have been deleted from the other healthy copies, and its database copy status will be FailedAndSuspended. In this event, if Autoreseed is configured, the affected copy will be automatically reseeded. If Autoreseed is not configured, the database copy will need to be manually seeded by an administrator.

If circular logging is disabled, loose truncation respects backups if they have been taken, so if logs have not been backed up they will not be removed by loose Truncation.

truncation is a recommended feature for preferred architecture where backups are not used and circular logging is enabled.

The required number of healthy copies, the free disk space threshold, and the number of logs to keep are all configurable parameters. By default, the free disk space threshold is 204800 MB (200 GB), and the number of logs to keep is 100,000 (100 GB) for passive copies, and 10,000 (10 GB) for active copies.

Enabling loose truncation and configuring loose truncation parameters is performed by editing the Windows registry on each DAG member. There are three registry values that can be configured, that are all stored under HKLM\Software\Microsoft\ExchangeServer\v15\BackupInformation. The BackupInformation key the following DWORD values do not exist by default and must be manually created. The DWORD registry values under BackupInformation are described in the following table:

|**Registry Value**|**Description**|**Default Value**|
|:-----|:-----|:-----|
|LooseTruncation_MinCopiesToProtect|This key is used to enable loose truncation. It represents the number of passive copies to protect from loose truncation on the active copy of a database. Setting the value of this key to 0 disables loose truncation.|0|
|LooseTruncation_MinDiskFreeSpaceThresholdInMB|Available disk space (in MB) threshold for triggering loose truncation. If free disk space falls below this value, loose truncation is triggered.|If this registry value is not configured, the default value used by loose truncation is 200 GB.|
|LooseTruncation_MinLogsToProtect|The minimum number of log files to retain on healthy copies whose logs are being truncated. If this registry value is configured, then the configured value applies to both active and passive copies.|If this registry value is not configured, then default values of 100,000 for passive database copies and 10,000 for active database copies is used.|

When using the LooseTruncation_MinLogsToProtect registry value, note that the behavior is different for active and passive database copies. On the active database copy, this is the number of extra logs that are retained preceding those that are required by the protected passive copies and the required range of the active copy.On a passive database copy, this is the number of logs maintained from the latest available log. One tenth of this number is also used to maintain logs prior to the required range of this passive copy. The two limits are in place to ensure that lagged database copies don't take up too much space, since their required range is typically very large.

### Database activation policy
<a name="Seed"> </a>

There are scenarios in which you may want to create a mailbox database copy and prevent the system from automatically activating that copy in the event of a failure, for example:

- If you deploy one or more mailbox database copies to an alternate or standby datacenter.

- If you configure a lagged database copy for recovery purposes.

- If you're performing maintenance or an upgrade of a server.

In each of the preceding scenarios, you have database copies that you don't want the system to activate automatically. To prevent the system from automatically activating a mailbox database copy, you can configure the copy to be blocked (suspended) for activation. This allows the system to maintain the currency of the database through log shipping and replay, but prevents the system from automatically activating and using the copy. Copies blocked for activation must be manually activated by an administrator. You can configure the database activation policy for an entire server by using the [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/set-mailboxserver) cmdlet or an individual database copy by using the [Set-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/set-mailboxdatabasecopy) cmdlet to set the _DatabaseCopyAutoActivationPolicy_ parameter to Blocked.

For more information about configuring database activation policy, see [Configure activation policy for a mailbox database copy](configure-activation-policies.md).

### Effect of mailbox moves on continuous replication
<a name="Effect"> </a>

On a very busy mailbox database with a high log generation rate, there is a greater chance for data loss if replication to the passive database copies can't keep up with log generation. One scenario that can introduce a high log generation rate is mailbox moves. Exchange Server includes a Data Guarantee API that's used by services such as the Exchange Mailbox Replication service (MRS) to check the health of the database copy architecture based on the value of the _DataMoveReplicationConstraint_ parameter that was set by the system or an administrator. Specifically, the Data Guarantee API can be used to:

- **Check replication health**: Confirms that the prerequisite number of database copies is available.

- **Check replication flush**: Confirms that the required log files have been replayed against the prerequisite number of database copies.

When executed, the API returns the following status information to the calling application:

- **Retry**: Signifies that there are transient errors that prevent a condition from being checked against the database.

- **Satisfied**: Signifies that the database meets the required conditions or the database isn't replicated.

- **NotSatisfied**: Signifies that the database doesn't meet the required conditions. In addition, information is provided to the calling application as to why the **NotSatisfied** response was returned.

The value of the _DataMoveReplicationConstraint_ parameter for the mailbox database determines how many database copies should be evaluated as part of the request. The _DataMoveReplicationConstraint_ parameter has the following possible values:

- `None`: When you create a mailbox database, this value is set by default. When this value is set, the Data Guarantee API conditions are ignored. This setting should be used only for mailbox databases that aren't replicated.

- `SecondCopy`: This is the default value when you add the second copy of a mailbox database. When this value is set, at least one passive database copy must meet the Data Guarantee API conditions.

- `SecondDatacenter`: When this value is set, at least one passive database copy in another Active Directory site must meet the Data Guarantee API conditions.

- `AllDatacenters`: When this value is set, at least one passive database copy in each Active Directory site must meet the Data Guarantee API conditions.

- `AllCopies`: When this value is set, all copies of the mailbox database must meet the Data Guarantee API conditions.

 **Check Replication Health**

When the Data Guarantee API is executed to evaluate the health of the database copy infrastructure, several items are evaluated.

In all scenarios, the passive database copy must meet the following conditions:

- Be healthy.

- Have a replay queue within 10 minutes of the replay lag time.

- Have a copy queue length less than 10 logs.

- Have an average copy queue length less than 10 logs. The average copy queue length is computed based on the number of times the application has queried the database status.

****

|**If the _DataMoveReplicationConstraint_ parameter is set to...**|**Then, for a given database...**|
|:-----|:-----|
| `SecondCopy`|At least one passive database copy for a replicated database must meet the previously described conditions.|
| `SecondDatacenter`|At least one passive database copy in another Active Directory site must meet the previously described conditions.|
| `AllDatacenters`|The active copy must be mounted, and a passive copy in each Active Directory site must meet the previously described conditions.|
| `AllCopies`|The active copy must be mounted, and all passive database copies must meet the previously described conditions.|

 **Check Replication Flush**

The Data Guarantee API can also be used to validate that a prerequisite number of database copies have replayed the required transaction logs. This is verified by comparing the last log replayed timestamp with that of the calling service's commit timestamp (in most cases, this is the timestamp of the last log file that contains required data) plus an additional five seconds (to deal with system time clock skews or drift). If the replay timestamp is greater than the commit timestamp, the _DataMoveReplicationConstraint_ parameter is satisfied. If the replay timestamp is less than the commit timestamp, the _DataMoveReplicationConstraint_ isn't satisfied.

Before moving large numbers of mailboxes to or from replication databases within a DAG, we recommend that you configure the _DataMoveReplicationConstraint_ parameter on each mailbox database according to the following:

|**If you're deploying...**|**Set DataMoveReplicationConstraint to...**|
|:-----|:-----|
|Mailbox databases that don't have any database copies| `None`|
|A DAG within a single Active Directory site| `SecondCopy`|
|A DAG in multiple datacenters using a stretched Active Directory site| `SecondCopy`|
|A DAG that spans twoActive Directory sites, and you will have highly available database copies in each site| `SecondDatacenter`|
|A DAG that spans two Active Directory sites, and you will have only lagged database copies in the second site| `SecondCopy` <br/> This is because the Data Guarantee API won't guarantee data being committed until the log file is replayed into the database copy, and due to the nature of the database copy being lagged, this constraint will fail the move request, unless the lagged database copy ReplayLagTime value is less than 30 minutes.|
|A DAG that spans three or more Active Directory sites, and each site will contain highly available database copies| `AllDatacenters`|

### Balancing database copies
<a name="Effect"> </a>

Due to the inherent nature of DAGs, as the result of database switchovers and failovers, active mailbox database copies will change hosts several times throughout a DAG's lifetime. As a result, DAGs can become unbalanced in terms of active mailbox database copy distribution. The following table shows an example of a DAG that has four databases with four copies of each database (for a total of 16 databases on each server) with an uneven distribution of active database copies.

**DAG with unbalanced active copy distribution**

|**Server**|**Number of active databases**|**Number of passive databases**|**Number of mounted databases**|**Number of dismounted databases**|**Preference count list**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|EX1|5|11|5|0|4, 4, 3, 5|
|EX2|1|15|1|0|1, 8, 6, 1|
|EX3|12|4|12|0|13, 2, 1, 0|
|EX4|1|15|1|0|1, 1, 5, 9|

In the preceding example, there are four copies of each database, and therefore, only four possible values for activation preference (1, 2, 3, or 4). The **Preference count list** column shows the count of the number of databases with each of these values. For example, on EX3, there are 13 database copies with an activation preference of 1, two copies with an activation preference of 2, one copy with an activation preference of 3, and no copies with an activation preference of 4.

As you can see, this DAG isn't balanced in terms of the number of active databases hosted by each DAG member, the number of passive databases hosted by each DAG member, or the activation preference count of the hosted databases.

You can use the RedistributeActiveDatabases.ps1 script to balance the active mailbox databases copies across a DAG. This script moves databases between their copies in an attempt to have an equal number of mounted databases on each server in DAG. If required, the script also attempts to balance active databases across sites.

The script provides two options for balancing active database copies within a DAG:

- **BalanceDbsByActivationPreference**: When this option is specified, the script attempts to move databases to their most preferred copy (based on activation preference) without regard to the Active Directory site.

- **BalanceDbsBySiteAndActivationPreference**: When this option is specified, the script attempts to move active databases to their most preferred copy, while also trying to balance active databases within each Active Directory site.

After running the script with the first option, the preceding unbalanced DAG becomes balanced, as shown in the following table.

**DAG with balanced active copy distribution**

|**Server**|**Number of active databases**|**Number of passive databases**|**Number of mounted databases**|**Number of dismounted databases**|**Preference count list**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|EX1|4|12|4|0|4, 4, 4, 4|
|EX2|4|12|4|0|4, 4, 4, 4|
|EX3|4|12|4|0|4, 4, 4, 4|
|EX4|4|12|4|0|4, 4, 4, 4|

As shown in the preceding table, this DAG is now balanced in terms of number of active and passive databases on each server and activation preference across the servers.

The following table lists the available parameters for the RedistributeActiveDatabases.ps1 script.

**RedistributeActiveDatabases.ps1 script parameters**

|**Parameter**|**Description**|
|:-----|:-----|
| _DagName_|Specifies the name of the DAG you want to rebalance. If this parameter is omitted, the DAG of which the local server is a member is used.|
| _BalanceDbsByActivationPreference_|Specifies that the script should move databases to their most preferred copy without regard to the Active Directory site.|
| _BalanceDbsBySiteAndActivationPreference_|Specifies that the script should attempt to move active databases to their most preferred copy, while also trying to balance active databases within each Active Directory site.|
| _ShowFinalDatabaseDistribution_|Specifies that a report of current database distribution be displayed after redistribution is complete.|
| _AllowedDeviationFromMeanPercentage_|Specifies the allowed variation of active databases across sites, expressed as a percentage. The default is 20%. For example, if there were 99 databases distributed between three sites, the ideal distribution would be 33 databases in each site. If the allowed deviation is 20%, the script attempts to balance the databases so that each site has no more than 10% more or less than this number. 10% of 33 is 3.3, which is rounded up to 4. Therefore, the script attempts to have between 29 and 37 databases in each site.|
| _ShowDatabaseCurrentActives_|Specifies that the script produce a report for each database detailing how the database was moved and whether it's now active on its most-preferred copy.|
| _ShowDatabaseDistributionByServer_|Specifies that the script produce a report for each server showing its database distribution.|
| _RunOnlyOnPAM_|Specifies that the script run only on the DAG member that currently has the PAM role. The script verifies it's being run from the PAM. If it isn't being run from the PAM, the script exits.|
| _LogEvents_|Specifies that the script logs an event (MsExchangeRepl event 4115) containing a summary of the actions.|
| _IncludeNonReplicatedDatabases_|Specifies that the script should include non-replicated databases (databases without copies) when determining how to redistribute the active databases. Although non-replicated databases can't be moved, they may affect the distribution of the replicated databases.|
| _Confirm_|The Confirm switch can be used to suppress the confirmation prompt that appears by default when this script is run. To suppress the confirmation prompt, use the syntax -Confirm:$False. You must include a colon ( : ) in the syntax.|

#### RedistributeActiveDatabases.ps1 examples

This example shows the current database distribution for a DAG, including preference count list.

```powershell
RedistributeActiveDatabases.ps1 -DagName DAG1 -ShowDatabaseDistributionByServer | Format-Table
```

This example redistributes and balances the active mailbox database copies in a DAG using activation preference without prompting for input.

```powershell
RedistributeActiveDatabases.ps1 -DagName DAG1 -BalanceDbsByActivationPreference -Confirm:$False
```

This example redistributes and balances the active mailbox database copies in a DAG using activation preference, and produces a summary of the distribution.

```powershell
RedistributeActiveDatabases.ps1 -DagName DAG1 -BalanceDbsByActivationPreference -ShowFinalDatabaseDistribution
```

### Monitoring database copies
<a name="Effect"> </a>

 You can view a variety of information, including copy queue length, replay queue length, status, and content index state information, by examining the details of a database copy in the EAC. You can also use the **Get-MailboxDatabaseCopyStatus** cmdlet in the Exchange Management Shell to view a variety of status information for a database copy.

> [!NOTE]
> A database copy is your first defense if a failure occurs that affects the active copy of a database. It's therefore critical to monitor the health and status of database copies to ensure that they are available when needed.

For more information about monitoring database copies, see [Monitor database availability groups](monitor-dags.md).

### Removing a database copy
<a name="Effect"> </a>

A database copy can be removed at any time by using the EAC or by using the **Remove-MailboxDatabaseCopy** cmdlet in the Exchange Management Shell. After removing a database copy, you must manually delete any database and transaction log files from the server from which the database copy is being removed. For detailed steps about how to remove a database copy, see [Remove a mailbox database copy](remove-db-copies.md).

## Database switchovers
<a name="MngDBCopies"> </a>

The Mailbox server that hosts the active copy of a database is referred to as the mailbox database master. The process of activating a passive database copy changes the mailbox database master for the database and turns the passive copy into the new active copy. This process is called a database switchover. In a database switchover, the active copy of a database is dismounted on one Mailbox server and a passive copy of that database is mounted as the new active mailbox database on another Mailbox server. When performing a switchover, you can optionally override the database mount dial setting on the new mailbox database master.

You can quickly identify which Mailbox server is the current mailbox database master by reviewing the right-hand column under the **Database Copies** tab in the EAC. You can perform a switchover by using the **Activate** link in the EAC, or by using the **Move-ActiveMailboxDatabase** cmdlet in the Exchange Management Shell.

There are several internal checks that will be performed before a passive copy is activated. In some cases, the database switchover is blocked or canceled. In other cases, you can use cmdlets to move or skip over some checks.

- The status of the database copy is checked. If the database copy is in a failed state, the switchover is blocked. You can override this behavior and bypass the health check by using the _SkipHealthChecks_ parameter of the **Move-ActiveMailboxDatabase** cmdlet. This parameter lets you move the active copy to a database copy in a failed state.

- The active database copy is checked to see if it's currently a seeding source for any passive copies of the database. If the active copy is currently being used as a source for seeding, the switchover is blocked. You can override this behavior and bypass the seeding source check by using the _SkipActiveCopyChecks_ parameter of the **Move-ActiveMailboxDatabase** cmdlet. This parameter allows you to move an active copy that's being used as a seeding source. Using this parameter will cause the seeding operation to be cancelled and considered failed.

- The copy queue and replay queue lengths for the database copy are checked to ensure their values are within the configured criteria. Also, the database copy is verified to ensure that it isn't currently in use as a source for seeding. If the values for the queue lengths are outside the configured criteria, or if the database is currently used as a source for seeding, the switchover is blocked. You can override this behavior and bypass these checks by using the _SkipLagChecks_ parameter of the **Move-ActiveMailboxDatabase** cmdlet. This parameter allows a copy to be activated that has replay and copy queues outside of the configured criteria.

- The state of the search catalog (content index) for the database copy is checked. If the search catalog isn't up to date, is in an unhealthy state, or is corrupt, the switchover is blocked. You can override this behavior and bypass the search catalog check by using the _SkipClientExperienceChecks_ parameter of the **Move-ActiveMailboxDatabase** cmdlet. This parameter causes this search to skip the catalog health check. If the search catalog for the database copy you're activating is in an unhealthy or unusable state and you use this parameter to skip the catalog health check and activate the database copy, you will need to either crawl or seed the search catalog again.

When you perform a database switchover, you also have the option of overriding the mount dial settings configured for the server that hosts the passive database copy being activated. Using the _MountDialOverride_ parameter of the **Move-ActiveMailboxDatabase** cmdlet instructs the target server to override its own mount dial settings and use those specified by the _MountDialOverride_ parameter.

For detailed steps about how to perform a switchover of a database copy, see [Activate a mailbox database copy](activate-db-copies.md).
