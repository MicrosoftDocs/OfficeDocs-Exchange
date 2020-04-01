---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d9acb371-fd6c-4c14-aa8e-db5cbe39aa57
ms.reviewer: 
description: There are many paths to migrate data from an on-premises email organization to Microsoft Office 365. When planning a migration to Office 365, a common question is about how to improve the performance of data migration and optimize migration velocity.
title: Office 365 migration performance and best practices
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# Office 365 email migration performance and best practices

There are many paths to migrate data from an on-premises email organization to Microsoft Office 365. When planning a migration to Office 365, a common question is about how to improve the performance of data migration and optimize migration velocity.

> [!NOTE]
> The performance information listed in this topic doesn't apply to Office 365 service for dedicated subscription plans. For more information about Dedicated Plans, see [Office 365 Dedicated Plans Service Descriptions](https://technet.microsoft.com/library/mt422899.aspx).

## Overview of migrating email to Office 365

Office 365 supports several methods to migrate email, calendar, and contact data from your existing messaging environment to Office 365 as described in [Ways to migrate multiple email accounts to Office 365](mailbox-migration.md).

For more information about Office 365 networking and performance, see [Network planning and performance tuning for Office 365](https://support.office.com/article/e5f1228c-da3c-4654-bf16-d163daee8848).

**Frequently used migration methods**

|**Migration method**|**Description**|**Resources**|
|:-----|:-----|:-----|
|Internet Message Access Protocol (IMAP) migration|You can use the Exchange admin center or Exchange Online PowerShell to migrate the contents of users' mailboxes from an IMAP messaging system to their Office 365 mailboxes. This includes migrating your mailboxes from other hosted email services, such as Gmail or Yahoo Mail.|[Migrate your IMAP mailboxes to Office 365](migrating-imap-mailboxes/migrating-imap-mailboxes.md)|
|Cutover migration|Using a cutover migration, you migrate all on-premises mailboxes to Office 365 over a few days. Use cutover migration if you plan to move your entire email organization to Office 365 and manage user accounts in Office 365. You can migrate a maximum of 2,000 mailboxes from your on-premises Exchange organization to Office 365 using a cutover migration. The recommended number of mailboxes, however, is **150**. Performance suffers with numbers higher than that. The mail contacts and distribution groups in your on-premises Exchange organization are also migrated.|[Cutover migration to Office 365](cutover-migration-to-office-365.md)|
|Staged migration|You use a staged migration if you plan to eventually migrate all your organization's mailboxes to Office 365. Using a staged migration, you migrate batches of on-premises mailboxes to Office 365 over the course of a few weeks or months.|[What you need to know about a staged email migration to Office 365](what-to-know-about-a-staged-migration.md)|
|Hybrid deployment|A hybrid deployment offers organizations the ability to extend the feature-rich experience and administrative control they have with their existing on-premises Exchange organization to the cloud. A hybrid deployment provides the seamless look and feel of a single Exchange organization between an on-premises Exchange organization and Exchange Online in Microsoft Office 365. In addition, a hybrid deployment can serve as an intermediate step to moving completely to an Office 365 organization.|[Mail migration advisor](https://docs.microsoft.com/Exchange/mail-migration-jump)<br><br>[Exchange Deployment Assistant for Exchange on-premises 2013/2016/2019](https://docs.microsoft.com/exchange/exchange-deployment-assistant)<br><br>[Exchange Server 2013 hybrid deployments](https://go.microsoft.com/fwlink/p/?LinkId=616536)<br><br>[Minimal Hybrid Configuration](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/HCW-Improvement-The-Minimal-Hybrid-Configuration-option/ba-p/605072)|
|Third-party migration|There are many tools available from third parties. They use distinctive protocols and approaches to conduct email migrations from email platforms like IBM Lotus Notes and Novell GroupWise.|Here are some third-party migration tools and partners that can assist with Exchange migrations from third-party platforms: <br/><br/> [Binary Tree](https://binarytree.com): Provider of cross-platform messaging migration and coexistence software, with products that provide for the analysis of and the coexistence and migration between on-premises and online enterprise messaging and collaboration environments based on IBM Lotus Notes and Domino and Exchange and SharePoint. <br/><br/> [BitTitan](https://www.bittitan.com): Provider of migration solutions to Office 365. <br/><br/> [Quest](https://www.quest.com/solutions/office-365/): Provider of migration solutions from Exchange, Office 365, Gmail, GroupWise, Notes, POP/IMAP, Zimbra, Sun ONE/iPlanet, PSTs and email archives to Office 365 and Exchange. Quest solutions synchronize mailboxes, public folders and calendar information while maintaining coexistence throughout the migration. <br/><br/> [Quadrotech](https://www.quadrotech-it.com): Provider of migration solutions to Office 365. <br/><br/> [SkyKick](https://www.skykick.com/): Provider of automated migration solutions to move on-premises Exchange, Gmail, POP3, IMAP, Lotus Notes to Office 365. The end-to-end migration tools help partners with the sales, planning, migration, management, and onsite phases of the migration project. <br/><br/> [TransVault](http://www.transvault.com/): Provider of migration solutions to Office 365.|

## Performance for migration methods

The following sections compare mailbox migration workloads and the observed performance results for the different migration methods for migrating mailboxes and mailbox data to Office 365. These results are based on internal testing and actual customer migrations to Office 365.

> [!IMPORTANT]
> Because of differences in how migrations are performed and when they're performed, your actual migration velocity may vary.

### Customer migration workloads

The following table describes the different workloads involved in a typical migration, and the challenges and options for each.

|**Workload**|**Notes**|
|:-----|:-----|
|Onboarding (Migrating to O365)|Microsoft offers data migration capability and tools for customers to use to migrate their data from Exchange Server on-premises to Exchange Online (M365). There are a number of methods for migrating mailboxes and mailbox data, starting with Cutover migrations and Staged migrations, which are based on merge and sync moves, and which are described earlier in this article. The other main migration method involves hybrid moves, which is currently the most common method. You can decide exactly when you'd like to migrate to Microsoft 365, based on your business needs.|
|Multi-Geo|Multinational companies with offices around the world often have a need to store their employee data at-rest in specific regions, in order to meet their data residency requirements. Multi-Geo enables a single Office 365 tenant to span across multiple Office 365 datacenter geographies (geos), which gives you the ability to store Exchange data, at-rest, on a per-user basis, in your chosen geos. For more details, see [Get enterprise-grade global data location controls with Multi-Geo](https://products.office.com/business/multi-geo-capabilities).|
|Encryption|O365 Service Encryption with Customer Key is a feature that allows a customer to provision and manage the root keys that are used to encrypt data at-rest at the application layer in Office 365. For a mailbox to become encrypted the first time, a mailbox move is required. For more details, see [Service encryption with Customer Key for Office 365 FAQ](https://docs.microsoft.com/office365/securitycompliance/service-encryption-with-customer-key-faq).|
|GoLocal|Microsoft continues to open new datacenters for Office 365 in new regions, or geos. Existing customers, when eligible, can request to have their Office 365 customer data from their original datacenter moved to a new geo. The period of time in which you can make this request is usually one or two years, depending on the overall demand on the service. Note that this period of time during which you can request to have your customer data moved becomes shorter once a datacenter (DC) for the new geo launches (at that point you have approximately three to six months to request a move). Details are available in [Moving core data to new Office 365 datacenter geos](https://docs.microsoft.com/Office365/Enterprise/moving-data-to-new-datacenter-geos).|

When mailboxes are migrated within Microsoft 365 data centers, every mailbox move or bulk-mailbox move requires time for the operation to complete. There are a number of factors, such as Microsoft 365 service activity, that can affect exactly how much time. The service is designed to throttle discretionary workloads like mailbox moves, to ensure that the service runs optimally for all users. You can still expect mailbox moves to be processed, however, depending on the service's discretionary resource availability. More details about resource throttling can be found in [this blog post](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Resource-Based-Throttling-and-Prioritization-in-Exchange-Online/ba-p/608020).

### Estimated migration times

To help you plan your migration, the following tables present guidelines about when to expect bulk mailbox migrations or individual migrations to complete. These estimates are based on a data analysis of previous customer migrations. Because every environment is unique, your exact migration velocity may vary.

**Mailbox migration duration based on mailbox size profiles:**

1. Onboarding / PSTImport

   |**Mailbox size (GB)**|**50th percentile duration (days)**|**90th percentile duration (days)**|
   |:-----|:-----|:-----|
   |\< 1|1|7|
   |1 - 10|1|7|
   |10 - 50|3|14|
   |50 - 100|3|30|
   |100 - 200|8|45|
   |\> 200|Not supported|Not supported|

2. Multi-Geo / GoLocal / Encryption

   |**Mailbox size (GB)**|**50th percentile duration (days)**|**90th percentile duration (days)**|
   |:-----|:-----|:-----|
   |\< 1|1|7|
   |1 - 10|1|10|
   |10 - 50|3|30|
   |50 - 100|15|45|
   |100 - 200|30|60|
   |\> 200|Not supported|Not supported|

   **Migration duration to complete 90% of mailbox moves based on tenant size profiles:**

   |**Tenant size (number of mailboxes)**|**Duration (days)**|**May take up to this many days**|
   |:-----|:-----|:-----|
   |\< 1,000|5|14|
   |1,000 - 5,000|10|30|
   |5,000 - 10,000|20|45|
   |10,000 - 50,000|30|60|
   |50,000 - 100,000|45|90|
   |\> 1000,000|60|180|

Note that some outlier mailboxes would take longer to complete based on the mailbox profile. Also, if a tenant has larger mailboxes on average, this can also contribute to the extended duration of migration.

## Migration performance factors

Email migration has several common factors that can affect migration performance.

### Common migration performance factors

The following table provides a list of common factors that affect migration performance. More details are covered in the sections describing the individual migration methods.

|**Factor**|**Description**|**Example**|
|:-----|:-----|:-----|
|Data source|The device or service that hosts the data to be migrated. Many limitations might apply to the data source because of hardware specifications, end-user workload, and back-end maintenance tasks.|Gmail limits how much data can be extracted during a specific period of time.|
|Data type and density|Because of the unique nature of a customer's business, the type and mix of mail items within mailboxes vary greatly.|One 4-GB mailbox with 400 items, each with 10 megabytes (MB) of attachments, will migrate faster than one 4-GB mailbox with 100,000 smaller items.|
|Migration server|Many migration solutions use a "jump box" type of migration server or workstation to complete the migration.|Customers often use a low-performance virtual machine to host the MRSProxy service for hybrid deployments or for client PC non-hybrid migrations.|
|Migration engine|The data migration engine responsible for pulling data from the source server converts data, if necessary. The engine then transmits the data over the network and injects the data into the Office 365 mailbox. mailbox.|MRSProxy service has its own capabilities and limitations.|
|On-premises network appliances|The end-to-end network performance (from the data source to Exchange Online client access servers) affects migration performance.|Firewall configuration and specifications on the on-premises organization.|
|Office 365 service|Office 365 has built-in support and features to manage the migration workload.|The user-throttling policy has default settings and limits the overall maximum data transfer rate.|

### Network performance factors

This section describes best practices for improving network performance during migration. The discussion is general because the biggest impact on network performance during migration is related to third-party hardware and Internet service providers (ISPs).

Use the Exchange Analyzer to get a deeper understanding of your network connectivity with Office 365. To run the Exchange Analyzer tests in [Support and Recovery Assistant](https://diagnostics.office.com/), go to Advanced Diagnostics \> Exchange Online \> Check Exchange Online network connectivity \> Yes. Read [Fix Outlook and Office 365 issues with Support and Recovery Assistant for Office 365](https://support.office.com/article/12cd632c-d672-4782-b0ce-beb82b511525) to learn more about Support and Recovery Assistant.

|**Factor**|**Description**|**Best practices**|
|:-----|:-----|:-----|
|Network capacity|The amount of time it takes to migrate mailboxes to Office 365 is determined by the available and maximum capacity of your network.|Identify your available network capacity and determine the maximum upload capacity. <br/> Contact your ISP to confirm your allocated bandwidth and to get details about restrictions, such as the total amount of data that can be transferred in a specific period of time. <br/> Use tools to evaluate your actual network capacity. Make sure you test the end-to-end flow of data from your on-premises data source to the Microsoft datacenter gateway servers. <br/> Identify other loads on your network (for example, backup utilities and scheduled maintenance) that can affect your network capacity.|
|Network stability|A fast network doesn't always result in fast migrations. If the network isn't stable, data transfer takes longer because of error correction. Depending on the migration type, error correction can significantly affect migration performance.|Network hardware and driver issues often cause network stability problems. Work with your hardware vendors to understand your network devices and apply the vendor's latest recommended drivers and software updates.|
|Network delays|Intrusion detection functionality configured on a network firewall often causes significant network delays and affects migration performance. <br/> Migrating data to Office 365 mailboxes relies on your internet connection. Internet delays affect overall migration performance. <br/> Also, users in the same company might have cloud mailboxes that reside in datacenters in different geographical locations. Depending on the customer's ISP, migration performance may vary.|Evaluate network delays to all potential Microsoft datacenters to help ensure that the result is consistent. (This also helps ensure a consistent experience for end users.) Work with your ISP to address internet-related issues. <br/> Add IP addresses for Microsoft datacenter servers to your allow list, or bypass all migration-related traffic from your network firewall. For more information about the Office 365 IP ranges, see [Office 365 URLs and IP address ranges](https://support.office.com/article/8548a211-3fe7-47cb-abb1-355ea5aa88a2#BKMK_Home).|

For a deeper analysis of migrations within your environment, check out our [move analysis blog post](https://go.microsoft.com/fwlink/p/?LinkId=399457). The post includes a script to help you analyze move requests.

## Office 365 throttling

Office 365 uses various throttling mechanisms to help ensure security and service availability. The following three types of throttling can affect migration performance:

- User throttling

- Migration-service throttling

- Resource health-based throttling

> [!NOTE]
> The three types of Office 365 throttling don't affect all migration methods.

### Office 365 user throttling

User throttling affects most third-party migration tools and the client-uploading migration method. These migration methods use client access protocols, such as the Remote Procedure Call (RPC) over HTTP Protocol, to migrate mailbox data to Office 365 mailboxes. These tools are used to migrate data from platforms such as IBM Lotus Domino and Novell GroupWise.

User throttling is the most restrictive throttling method in Office 365. Because user throttling is set up to work against an individual end user, any application-level usage will easily exceed the throttling policy and result in slower data migration.

### Office 365 migration-service throttling

Migration-service throttling affects all Office 365 migration tools. Migration-service throttling manages migration concurrency and service resource allocation for Office 365 migration solutions.

Migration-service throttling affects migrations performed by using the following migration methods:

- IMAP migration

- Cutover Exchange migration

- Staged Exchange migration

- Hybrid migrations (MRSProxy service-based moves in a hybrid environment)

An example of migration-service throttling is controlling the number of mailboxes that are migrated simultaneously during simple Exchange migrations and IMAP migrations. The default value is 10. This means that a maximum of 10 mailboxes from all migration batches are migrated at any particular time. You can increase the number of concurrent mailbox migrations for a migration batch in either the Exchange Control Panel or Windows PowerShell. To learn more about how to optimize this setting, see [Manage migration batches in Office 365](manage-migration-batches.md).

### Office 365 resource health-based throttling

All migration methods are subject to the governance of availability throttling. Office 365 service throttling, however, doesn't affect Office 365 migrations as much as the other types of throttling described previously.

Resource health-based throttling is the least aggressive throttling method. It occurs to prevent a service availability issue that could affect end users and critical service operations.

Before performance of the service degrades to the point where end-user performance could be impacted, hybrid migrations will be stalled until performance is recovered and the service returns to a level below the throttling threshold.

The following are examples from an Exchange migration statistics report. They show the entries logged when the service-throttling threshold is exceeded.

```output
1/25/2018 12:56:01 AM [BL2PRD0410CA012] Copy progress: 723/1456 messages, 225.8 MB (236,732,045 bytes)/416.5 MB (436,712,733 bytes).

1/25/2018 12:57:53 AM [BL2PRD0410CA012] Move for mailbox '/o=ExchangeLabs/ou=Exchange Administrative Group (FYDIBOHF23SPDLT)/cn=Recipients/cn=xxxxxxxxxxxxxxxxxxxxxxxxxxxxx' is stalled because DataMoveReplicationConstraint is not satisfied for the database 'NAMPRD04DG031-db081' (agent MailboxDatabaseReplication). Failure Reason: Database edbf0766-1f2a-4552-9115-bb3a53a8380b doesn't satisfy constraint SecondDatacenter. There are no available healthy database copies. Will wait until 1/25/2018 1:27:53 AM.

1/25/2018 12:58:24 AM [BL2PRD0410CA012] Request is no longer stalled and will continue.

6/30/2017 00:03:58 [CY4PR19MB0056] Relinquishing job because of large delays due to unfavorable server health or budget limitations with a request throttling state 'StalledDueToTarget_DiskLatency'.
```

 **Solution and practice**

If you experience a similar situation, wait for the Office 365 resources to become available.

## Performance factors and best practices for non-hybrid deployment migrations

This section describes factors that affect migrations using the IMAP, cutover, or staged migration methods. It also identifies best practices to improve migration performance.

### Factor 1: Data source

The following table describes the impact on migration by the source servers in your current email organization and the best practices for mitigating the impact on migration.

|**Checklist**|**Description**|**Best practices**|
|:-----|:-----|:-----|
|System performance|Data extraction is an intensive task. The source system needs to have sufficient resources, such as CPU time and memory, to provide optimal migration performance. During migration, the source system is often close to full capacity in terms of the regular end-user workload. If system resources are inadequate, the additional workload that results from migration can affect end users.|Monitor system performance during a pilot migration test. If the system is busy, we recommend avoiding an aggressive migration schedule for the specific system because of potential migration slowness and service availability issues. If possible, enhance the source system performance by adding hardware resources and reduce the load on the system by moving tasks and users to other servers that aren't involved in the migration. <br/><br/> For more information, see: <br/>• [Exchange 2013 Server Health and Performance](https://go.microsoft.com/fwlink/p/?LinkId=623551) <br/>• [Understanding Exchange 2010 Performance](https://go.microsoft.com/fwlink/p/?LinkId=623552) <br/>• [Exchange 2007: Monitoring Mailbox Servers](https://go.microsoft.com/fwlink/p/?LinkId=623555) <br/><br/> When migrating from an on-premises Exchange organization where there are multiple mailbox servers, we recommend that you create a migration-user list that is evenly distributed across multiple mailbox servers. Based on individual server performance, the list can be further fine-tuned to maximize throughput. <br/><br/> For example, if server A has 50 percent more resource availability than server B, it's reasonable to have 50 percent more users from server A in the same migration batch. Similar practices can be applied to other source systems. Perform migrations when servers have maximum resource availability such as after hours or on weekends and holidays.|
|Back-end tasks|Other back-end tasks that are running during migration time. Because it's a best practice to perform migration after business hours, it's common that migrations conflict with maintenance tasks (such as data backup) running on your on-premises servers.|Review other system tasks that might be running during migration. We recommend that you perform data migration when no other resource-intensive tasks are running. <br/> **Note**: For customers using on-premises Exchange, the common back-end tasks are backup solutions and [Exchange store maintenance]( https://go.microsoft.com/fwlink/p/?LinkId=623556).|
|Throttling policy|It's a common practice to protect email systems with a throttling policy that sets a specific limit on how fast and how much data can be extracted from the system during a certain amount of time.|Verify what throttling policy is deployed for your email system. For example, Google Mail limits how much data can be extracted in a certain time period. <br/><br/> Depending on the version, Exchange has policies that restrict IMAP access to the on-premises mail server (used by IMAP migrations) and RPC over HTTP Protocol access (used by cutover Exchange migrations and staged Exchange migrations). <br/><br/> To check the throttling settings in an Exchange 2013 organization, run the [Get-ThrottlingPolicy](https://go.microsoft.com/fwlink/p/?LinkId=623557) cmdlet. For more information, see [Exchange Workload Management]( https://go.microsoft.com/fwlink/p/?LinkId=623558). <br/><br/> For more information about IMAP throttling, see [Migrate your IMAP mailboxes to Office 365](migrating-imap-mailboxes/migrating-imap-mailboxes.md) <br/><br/> For more information about RPC over HTTP Protocol throttling, see: <br/>• [Exchange 2013 Workload Management](https://go.microsoft.com/fwlink/p/?LinkID=623558) <br/>• [Exchange 2010: Understanding Client Throttling Policies](https://go.microsoft.com/fwlink/p/?LinkId=623586) <br/>• [Exchange 2007: Understanding Client Throttling](https://go.microsoft.com/fwlink/p/?LinkId=623587)|

### Factor 2: Migration server

IMAP, cutover, and staged migrations are cloud-initiated data-pull migration methods, so there's no need for a dedicated migration server. The internet-facing protocol hosts ( IMAP or RPC over HTTP Protocol), however, function as the migration server for migrating mailboxes and mailbox data to Office 365. Therefore, the migration performance factors and best practices, described in the previous section about the data source server for your current email organization, also apply to the internet edge servers. For Exchange 2007, Exchange 2010, and Exchange 2013, organizations, the client access server functions as a migration server.

For more information, see:

- [Exchange 2013 Workload Management](https://go.microsoft.com/fwlink/p/?LinkID=623558)

- [Exchange 2010: Client Access Server Counters](https://go.microsoft.com/fwlink/p/?LinkId=623589)

- [Exchange 2007: Monitoring Client Access Servers](https://go.microsoft.com/fwlink/p/?LinkId=623594)

### Factor 3: Migration engine

IMAP, cutover, and staged Exchange migrations are performed by using the Migration dashboard in the Exchange admin center . This is subject to Office 365 migration-service throttling.

 **Solution and practice**

Customers now can specify migration concurrency (for example, the number of mailboxes to migrate simultaneously) by using Windows PowerShell. The default is 20 mailboxes. After you create a migration batch, you can use the following Windows PowerShell cmdlet to increase this to a maximum of 100.

``` PowerShell
Set-MigrationEndPoint <Identity> -MaxConcurrentMigrations <value between 1 and 100>
```

For more information, see [Manage migration batches in Office 365](manage-migration-batches.md).

> [!NOTE]
> If your data source doesn't have sufficient resources to handle all the connections, we recommend avoiding high concurrency. Start with a small concurrency value, for example, 10. Increase this number while monitoring the data source performance to avoid end-user access issues.

### Factor 4: Network

 **Verification tests**

Depending on the migration method, you can try the following verification tests:

- **IMAP migrations**: Prepopulate a source mailbox with sample data. Then from the internet (outside your on-premises network), connect to the source mailbox by using a standard IMAP email client such as Microsoft Outlook, and then measure network performance by determining how long it takes to download all the data from the source mailbox. The throughput should be similar to what customers can get by using the IMAP migration tool in Office 365, given that there are no other constraints.

- **Cutover and staged Exchange migrations**: Prepopulate a source mailbox with sample data. Then, from the internet (outside of your on-premises network), connect to the source mailbox with Outlook by using RPC over HTTP Protocol. Make sure that you're connecting by using [cache mode](https://go.microsoft.com/fwlink/p/?LinkId=623596). Measure network performance by checking how long it takes to synchronize all data from the source mailbox. The throughput should be similar to what customers can get by using the simple Exchange migration tools in Office 365, given that there are no other constraints.

There is some overhead during an actual IMAP, cutover, or staged Exchange migration. The actual throughput, however, should be similar to the results of these verification tests.

### Factor 5: Office 365 service

Office 365 resource health-based throttling affects migrations using the native Office 365 simple migration tools. See the [Office 365 resource health-based throttling](#office-365-resource-health-based-throttling) section.

## Move requests in the Office 365 service

For general information about getting status information for move requests, see [View Move Request Properties](https://go.microsoft.com/fwlink/p/?LinkId=623597).

In the Office 365 service, unlike in on-premises Exchange 2010, the migration queue and the service resources allocated for migrations are shared among tenants. This sharing affects how move requests are handled in each stage of the move process.

There are two types of move requests in Office 365:

- **Onboarding move requests**: New customer migrations are considered onboarding move requests. These requests have regular priority.

- **Datacenter internal move requests**: These are mailbox move requests initiated by datacenter operation teams. These requests have a lower priority because the end-user experience isn't affected if the move request is delayed.

### Potential impact and delays to move requests with a status of "Queued" and "In Progress"

- **Queued move requests**: This status specifies that the move has been queued and is waiting to be picked up by the Exchange Mailbox Replication Service. For Exchange 2003 move requests, users can still access their mailboxes at this stage.

   Two factors influence which request will be picked up by the Mailbox Replication Service:

   - **Priority**: Queued move requests with a higher priority are picked up before lower-priority move requests. This helps ensure that customer-migration move requests always get processed before datacenter internal move requests.

   - **Position in the queue**: If move requests have the same priority, the earlier the request gets into the queue, the earlier it will be picked up by the Mailbox Replication Service. Because there might be multiple customers performing mailbox migrations at the same time, it's normal that new move requests remain in the queue before they're processed.

Often, the time that mailbox requests wait in the queue before being processed isn't considered during migration planning. This results in customers not being allocated enough time to complete all planned migrations.

- **In-progress move requests**: This status specifies that the move is still in progress. If this is an online mailbox move, the user will still be able to access the mailbox. For offline mailbox moves, the user's mailbox will be unavailable.

After the mailbox move request has a status of "In Progress," the priority no longer matters and a new move request won't be processed until an existing "In Progress" move request is completed, even if the new move request has a higher priority.

### Best practices

 **Planning**: As previously mentioned, because Exchange 2003 users lose access during a hybrid migration, Exchange 2003 customers are usually more concerned about when to schedule migrations and how long they will take.

When planning how many mailboxes to migrate during a specific time period, consider the following:

- Include the amount of time the move request waits in the queue. Use the following to calculate this:

   (total number of mailboxes to migrate) = ((total time) - (average queue time)) \* (migration throughput)

   where the migration throughput equals the total number of mailboxes that can be migrated per hour.

For example, assume you have a six-hour window to migrate mailboxes. If the average queue time is one hour and you have a migration throughput of 100 mailboxes per hour, you can migrate 500 mailboxes in the six-hour time frame: 500 = (6 - 1) \* 100.

- Start the migration sooner than initially planned to mitigate time in the queue. When mailboxes are queued, Exchange 2003 users can still access their mailboxes.

 **Determine queue time**: The queue time is always changing because Microsoft doesn't manage customers' migration schedules.

To determine the potential queue time, a customer can try to schedule a test move several hours before the actual migration starts. Then, based on the observed amount of time the request is in the queue, the customer can better estimate when to start the migration and how many mailboxes can be moved in a specific period of time.

For example, if a test migration was completed four hours before the start of a planned migration. The customer determines the queue time for the test migration was about one hour. Then, the customer should consider starting the migration one hour earlier than originally planned to make sure there is enough time to complete all migrations.

## Third-party tools for Office 365 migrations

Third-party tools are mostly used in migration scenarios that don't involve Exchange, such as those from Google Mail, IBM Lotus, Domino, and Novell GroupWise. This section focuses on the migration protocols used by third-party migration tools, rather than on the actual products and migration tools. The following table provides a list of factors that apply to third-party tools for Office 365 migration scenarios.

### Factor 1: Data source

|**Checklist**|**Description**|**Best practices**|
|:-----|:-----|:-----|
|System performance|Data extraction is an intensive task. The source system must have sufficient resources, such as CPU time and memory, to provide optimal migration performance. During migration, the source system is often close to full capacity in terms of the regular end-user workload. If system resources are inadequate, the additional workload that results from migration can affect end users.|Monitor system performance during a pilot migration test. If the system is busy, we recommend avoiding an aggressive migration schedule for the specific system because of potential migration slowness and service availability issues. If possible, enhance the source system performance by adding hardware resources and by reducing the load on the system. The system load can be reduced by moving tasks and users to other servers that aren't part of the migration. <br/><br/> For more information, see: <br/>• [Exchange 2013 Server Health and Performance](https://go.microsoft.com/fwlink/p/?LinkID=623551) <br/>• [Understanding Exchange 2010 Performance](https://go.microsoft.com/fwlink/p/?LinkID=623552) <br/>• [Exchange 2007: Monitoring Mailbox Servers](https://go.microsoft.com/fwlink/p/?LinkID=623555) <br/><br/> When migrating from an on-premises Exchange organization where there are multiple mailbox servers, we recommend that you create a migration user list that's evenly distributed across multiple mailbox servers. Based on individual server performance, the list can be further fine-tuned to maximize throughput. <br/><br/> For example, if server A has 50 percent more resource availability than server B, it is reasonable to have 50 percent more users from server A in the same migration batch. A similar practice can be applied to other source systems. <br/><br/> Perform migration when the system has maximum resource availability, such as after hours or on weekends and holidays.|
|Back-end tasks|Other back-end tasks usually run during migration time. Because it's a best practice to perform migration after business hours, it's common that migrations conflict with other maintenance tasks running on your on-premises servers, such as data backup.|Review other system tasks that are running during migration. We recommend that you create a clean time window just for data migration, when there are no other resource-heavy tasks. <br/><br/> For Exchange on-premises customers, the common tasks are backup solutions. For more information, see [Exchange Store Maintenance](https://go.microsoft.com/fwlink/p/?LinkID=623556).|
|Throttling policy|It's a common practice to protect email systems with a throttling policy, which sets a specific limit on how fast and how much data can be extracted from the system within a certain amount of time and by using a specific migration method.|Verify what throttling policy is deployed for your email system. For example, Google Mail limits how much data can be extracted in a certain time period. <br/><br/> Depending on the version, Exchange has policies that restrict IMAP access to the on-premises mail server (used by IMAP migrations) and RPC over HTTP Protocol access (used by cutover Exchange migrations and staged Exchange migrations). <br/><br/> For more information about IMAP throttling, see [Tips for optimizing IMAP migrations](migrating-imap-mailboxes/optimizing-imap-migrations.md). <br/><br/> For more information about RPC over HTTP Protocol throttling, see: <br/>• [Exchange 2013 Workload Management](https://go.microsoft.com/fwlink/p/?LinkID=623558) <br/>• [Exchange 2010: Understanding Client Throttling Policies](https://go.microsoft.com/fwlink/p/?LinkID=623586) <br/>• [Exchange 2007: Understanding Client Throttling](https://go.microsoft.com/fwlink/p/?LinkID=623587) <br/><br/> For more information about how to configure Exchange Web Services throttling, see [Exchange 2010: Understanding Client Throttling Policies](https://go.microsoft.com/fwlink/p/?LinkId=623620).|

### Factor 2: Migration server

Most third-party tools for Office 365 migrations are client initiated and push data to Office 365. These tools typically require a migration server. Factors such as system performance, back-end tasks, and throttling policies for the source servers apply to these migration servers.

> [!NOTE]
> Some third-party migration solutions are hosted on the internet as cloud-based services and don't require an on-premises migration server.

 **Solution and practice**

To improve migration performance when using a migration server, apply the same best practices as the ones described in the [Factor 1: Data source](#factor-1-data-source) section.

### Factor 3: Migration engine

For third-party migration tools, the most common protocols used are Exchange Web Services and RPC over HTTP Protocol.

 **Exchange Web Services**

Exchange Web Services is the recommended protocol to use for migrating to Office 365 because it supports large data batches and has better service-oriented throttling. In Office 365, when used in impersonation mode, migrations using Exchange Web Services don't consume the user's budgeted amount of Office 365Exchange Web Services resources, consuming instead a copy of the budgeted resources:

- All Exchange Web Services impersonating calls made by the same administrator account are calculated separately from the budget applied to this administrator account.

- For each impersonation session, a shadow copy of the actual user's budget is created. All migrations for this particular session will consume this shadow copy.

- Throttling under impersonation is isolated to each user migration session.

 **Best practices**

- Migration performance for customers using third-party migration tools that use EWA impersonation competes with Exchange Web Services-based migrations and service resource usage by other tenants. Therefore, migration performance will vary.

- Whenever possible, customers should use third-party migration tools that use Exchange Web Services impersonation because it's usually faster and more efficient than using client protocols such as RPC over HTTP Protocol.

 **RPC over HTTP Protocol**

Many traditional migration solutions use the RPC over HTTP Protocol. This method is completely based on a client access model such as that of Outlook, and scalability and performance are limited because the Office 365 service throttles access on the assumption that usage is by a user instead of by an application.

 **Best practices**

- For migration tools that use RPC over HTTP Protocol, it's a common practice to increase migration throughput by adding more migration servers and using multiple Office 365 administrative user accounts. This practice can gain data injection parallelism and achieve higher data throughput because each administrative user is subject to Office 365 user throttling. We have received reports that many enterprise customers had to set up more than 40 migration servers to obtain 20-30 GB/hour of migration throughput.

- In a migration tool development phase, it's critical to consider the number of RPC operations needed to migrate a message. To illustrate this, we have collected logs captured by Office 365 services for two third-party migration solutions (developed by third-party companies) used by customers to migrate mailboxes to Office 365. We compared two migration solutions developed by third-party companies. We compared the migration of two mailboxes for each migration solution, and we also compared them to uploading a .pst file in Outlook. Here are the results.

|**Method**|**Mailbox size**|**Item count**|**Time to migrate**|**Total RPC transactions**|**Average client latency (ms)**|**AvgCasRPCProcessingTime (ms)**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Solution A (mailbox 1)|376.9 MB|4,115|4:24:33|132,040|48.4395|18.0807|
|Solution A (mailbox 2)|249.3 MB|12,779|10:50:50|423,188|44.1678|4.8444|
|Solution B (mailbox 1)|618.1 MB|4,322|1:54:58|12,196|37.2931|8.3441|
|Solution B (mailbox 2)|56.7 MB|2,748|0:47:08|5,806|42.1930|7.4439|
|Outlook|201.9MB|3,297|0:29:47|15,775|36.9987|5.6447|

Note that the client and service process times are similar, but solution A takes a lot more RPC operations to migrate data. Because each operation consumes client-latency time and server-process time, solution A is much slower to migrate the same amount of data compared to Solution B and to Outlook.

### Factor 4: Network

 **Best practice**

For third-party migration solutions that use the RPC over HTTP Protocol, here's a good way to measure potential migration performance:

1. From the migration server, connect to the Office 365 mailbox with Outlook by using RPC over HTTP Protocol. Make sure that you aren't connecting by using [cache mode](https://go.microsoft.com/fwlink/p/?LinkID=623596).

2. Import a large .pst file with sample data to the Office 365 mailbox.

3. Measure migration performance by timing how long it takes to upload the .pst file. The migration throughput should be similar to what customers can get from a third-party migration tool that uses RPC over HTTP Protocol, given no other constraints. There's overhead during an actual migration, so the throughput might be slightly different.

### Factor 5: Office 365 service

Office 365 resource health-based throttling affects migrations using third-party migration tools. See [Office 365 resource health-based throttling](#office-365-resource-health-based-throttling) for more details.
