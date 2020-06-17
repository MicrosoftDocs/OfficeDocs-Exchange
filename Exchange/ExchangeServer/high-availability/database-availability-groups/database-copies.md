---
localization_priority: Normal
description: 'Summary: What you should know about mailbox database copies in Exchange Server 2016 and Exchange Server 2019, and your options when creating them.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ce748bca-3e24-493b-b9e6-153157bffd6a
ms.reviewer:
title: Mailbox database copies
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Mailbox database copies

Microsoft Exchange Server leverages the concept of database mobility, which is Exchange-managed database-level failovers. Database mobility disconnects databases from servers, adds support for up to 16 copies of a single database, and provides a native experience for adding database copies to a database.

## Key characteristics

The key characteristics of mailbox database copies are:

- Up to 16 copies of an Exchange Server mailbox database can be created on multiple Mailbox servers, provided the servers are grouped into a database availability group (DAG), which is a boundary for continuous replication. Exchange Server mailbox databases can be replicated only to the same version Exchange Mailbox servers within a DAG. You can't replicate a database outside of a DAG, nor can you replicate an Exchange 2016 or Exchange 2019 mailbox database to a server running Exchange 2013 or earlier. For detailed information about DAGs, see [Database availability groups](database-availability-groups.md).

- All Mailbox servers in a DAG must be in the same Active Directory domain.

- Mailbox database copies support the concepts of replay lag time and truncation lag time. Appropriate planning must be performed before enabling these features.

- All database copies can be backed up using an Exchange-aware, Volume Shadow Copy Service (VSS)-based backup application.

- Database copies can be created only on Mailbox servers that don't host the active copy of a database. You can't create two copies of the same database on the same server.

- All copies of a database use the same path on each server containing a copy. The database and log file paths for a database copy on each Mailbox server must not conflict with any other database paths.

- Database copies can be created in the same or different Active Directory sites, and on the same or different network subnets.

- Database copies aren't supported between Mailbox servers with round trip network latency greater than 500 milliseconds (ms).

## Mailbox database copies

You can create a mailbox database copy at any time. Mailbox database copies can be distributed across Mailbox servers in a flexible and granular way.

You can create a mailbox database copy using the **Add mailbox database copy** wizard in the Exchange admin center or by using the **Add-MailboxDatabaseCopy** cmdlet in the Exchange Management Shell.

When creating a mailbox database copy, specify the following parameters:

- _Identity_: This parameter specifies the name of the database being copied. Database names must be unique within the Exchange organization.

- _MailboxServer_: This parameter specifies the name of the Mailbox server that will host the database copy. This server must be a member of the same DAG and must not already host a copy of the database.

Optionally, you can also specify:

- _ActivationPreference_: This parameter specifies the activation preference number, which is used as part of Active Manager's best copy selection process. It's also used to redistribute active mailbox databases throughout the DAG when using the RedistributeActiveDatabases.ps1 script. The value for the activation preference is a number equal to or greater than one, where one is at the top of the preference order. The position number cannot be larger than the number of mailbox database copies.

- _ReplayLagTime_: This parameter specifies the amount of time that the Microsoft Exchange Replication service should wait before replaying log files that are copied to the database copy. The format for this parameter is (Days.Hours:Minutes:Seconds). The default setting for this value is 0 seconds. The maximum allowable setting for this value is 14 days. The minimum allowable setting is 0 seconds. Setting the value for replay lag time to 0 turns off log replay delay.

- _TruncationLagTime_: This parameter specifies the amount of time that the Microsoft Exchange Replication service should wait before truncating log files that have replayed into a copy of the database. The time period begins after the log has been successfully replayed into the copy of the database. The format for this parameter is (Days.Hours:Minutes:Seconds). The default setting for this value is 0 seconds. The maximum allowable setting for this value is 14 days. The minimum allowable setting is 0 seconds. Setting the value for truncation lag time to 0 turns off log truncation delay.

- _SeedingPostponed_: This parameter specifies that the task shouldn't automatically seed the database copy on the specified Mailbox server. This option is typically used when you intend to seed a new mailbox database copy by using an existing passive copy of the database (for example, adding a second copy of a specific database to a remote location). When you use this parameter, you must manually seed the database copy using the [Update-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/update-mailboxdatabasecopy) cmdlet.

For more information about creating, using, and managing mailbox database copies, see [Managing mailbox database copies](https://docs.microsoft.com/exchange/high-availability/manage-ha/manage-database-copies?view=exchserver-2019)
