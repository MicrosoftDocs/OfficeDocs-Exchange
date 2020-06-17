---
title: 'Datacenter Activation Coordination mode: Exchange 2013 Help'
TOCTitle: Datacenter Activation Coordination mode
ms:assetid: 57e4bf22-eeae-42a5-beb3-d68d06489592
ms:mtpsurl: https://technet.microsoft.com/library/Dd979790(v=EXCHG.150)
ms:contentKeyID: 48385111
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Datacenter Activation Coordination mode

_**Applies to:** Exchange Server 2013_

Datacenter Activation Coordination (DAC) mode is a property of a database availability group (DAG). DAC mode is disabled by default but should be enabled for all DAGs with two or more members that use continuous replication. DAC mode shouldn't be enabled for DAGs that use third-party replication mode unless specified by the third-party vendor.

DAC mode is used to control the database mount on startup behavior of a DAG. This control is designed to prevent split brain from occurring at the database level during a datacenter switchback. Split brain, also known as split brain syndrome, is a condition that results in a database copying being mounted as an active copy on two members of the same DAG that are unable to communicate. Split brain is prevented using DAC mode because DAC mode requires DAG members to obtain permission to mount databases before they can be mounted.

For example, consider a scenario where a primary datacenter contains two DAG members and the witness server, and a second datacenter contains two other DAG members. In this scenario, the DAG is not in DAC mode. The primary datacenter loses power, so you activate the DAG in the second datacenter. Eventually power to the primary datacenter is restored, and the DAG members in the primary datacenter, which had quorum before the power failure, will start up and mount their databases. Because the primary datacenter was restored without network connectivity to the second datacenter, and because the DAG was not in DAC mode, the active databases within the DAG entered a split brain condition.

## How DAC mode works

DAC mode is designed to prevent split brain from occurring by including a protocol called Datacenter Activation Coordination Protocol (DACP). When DAC mode is enabled, DAG members won't automatically mount databases even if they have quorum. Instead DACP is used to determine the current state of the DAG and whether Active Manager should attempt to mount the databases.

You might think of DAC mode as an application level of quorum for mounting databases. To understand the purpose of DACP and how it works, it's important to understand the primary scenario it's intended to handle. Consider the two-datacenter scenario. Suppose there is a complete power failure in the primary datacenter. In this event, all of the servers and the WAN are down, so the organization makes the decision to activate the standby datacenter. In almost all such recovery scenarios, when power is restored to the primary datacenter, WAN connectivity is typically not immediately restored. This means that the DAG members in the primary datacenter will power up, but they won't be able to communicate with the DAG members in the activated standby datacenter. The primary datacenter should always contain the majority of the DAG quorum voters, which means that when power is restored, even in the absence of WAN connectivity to the DAG members in the standby datacenter, the DAG members in the primary datacenter have a majority and therefore have quorum. This is a problem because with quorum, these servers may be able to mount their databases, which in turn would cause divergence from the actual active databases that are now mounted in the activated standby datacenter.

DACP was created to address this issue. Active Manager stores a bit in memory (either a 0 or a 1) that tells the DAG whether it's allowed to mount local databases that are assigned as active on the server. When a DAG is running in DAC mode, each time Active Manager starts up the bit is set to 0, meaning it isn't allowed to mount databases. Because it's in DAC mode, the server must try to communicate with all other members of the DAG that it knows to get another DAG member to give it an answer as to whether it can mount local databases that are assigned as active to it. The answer comes in the form of the bit setting for other Active Managers in the DAG. If another server responds that its bit is set to 1, it means servers are allowed to mount databases, so the server starting up sets its bit to 1 and mounts its databases.

But when you recover from a primary datacenter power outage where the servers are recovered but WAN connectivity has not been restored, all of the DAG members in the primary datacenter will have a DACP bit value of 0; and therefore none of the servers starting back up in the recovered primary datacenter will mount databases, because none of them can communicate with a DAG member that has a DACP bit value of 1.

## DAC mode for DAGs with two members

DAGs with two members have inherent limitations that prevent the DACP bit alone from fully protecting against application-level split brain syndrome. For DAGs with only two members, DAC mode also uses the boot time of the DAG's witness server to determine whether it can mount databases on startup. The boot time of the witness server is compared to the time when the DACP bit was set to 1.

- If the time the DACP bit was set is earlier than the boot time of the witness server, the system assumes that the DAG member and witness server were rebooted at the same time (perhaps because of power loss in the primary datacenter), and the DAG member isn't permitted to mount databases.

- If the time that the DACP bit was set is more recent than the boot time of the witness server, the system assumes that the DAG member was rebooted for some other reason (perhaps a scheduled outage in which maintenance was performed or perhaps a system crash or power loss isolated to the DAG member), and the DAG member is permitted to mount databases.

> [!IMPORTANT]
> Because the witness server's boot time is used to determine whether a DAG member can mount its active databases on startup, you should never restart the witness server and the sole DAG member at the same time. Doing so may leave the DAG member in a state where it can't mount databases on startup. If this happens, you must run the <A href="https://docs.microsoft.com/powershell/module/exchange/Restore-DatabaseAvailabilityGroup">Restore-DatabaseAvailabilityGroup</A> cmdlet on the DAG. This resets the DACP bit and permits the DAG member to mount databases.

## Other benefits of DAC mode

In addition to preventing split brain syndrome at the application level, DAC mode also enables the use of the built-in site resilience cmdlets used to perform datacenter switchovers. These include the following:

  - [Stop-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Stop-DatabaseAvailabilityGroup)

  - [Restore-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Restore-DatabaseAvailabilityGroup)

  - [Start-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Start-DatabaseAvailabilityGroup)

Performing a datacenter switchover for DAGs that aren't in DAC mode involves using a combination of Exchange tools and cluster management tools. For more information, see [Datacenter Switchovers](datacenter-switchovers-exchange-2013-help.md).

## Enabling DAC mode

DAC mode can be enabled only by using the Exchange Management Shell. Specifically, you can use the [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup) cmdlet to enable DAC mode, as illustrated in the following example.

```powershell
Set-DatabaseAvailabilityGroup -Identity DAG2 -DatacenterActivationMode DagOnly
```

In the preceding example, DAG2 is enabled for DAC mode.

For more information about enabling DAC mode, see [Configure database availability group properties](configure-database-availability-group-properties-exchange-2013-help.md) and [Set-DatabaseAvailabilityGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DatabaseAvailabilityGroup).
