---
title: Understanding how Exchange Server 2013 Management Pack reports system health
TOCTitle: Understanding how Exchange Server 2013 Management Pack reports system health
ms:assetid: 6ca8847f-93fe-458d-bd43-7afad7fdd2f4
ms:mtpsurl: https://technet.microsoft.com/library/Dn195910(v=EXCHG.150)
ms:contentKeyID: 53181786
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Understanding how Exchange Server 2013 Management Pack reports system health

_**Applies to:** Exchange Server 2013_

This topic provides information about how the Exchange Server 2013 Management Pack monitors and reports Exchange system health. In the Exchange 2013 Management Pack, health state information is rolled up in a simple manner. Whenever a healthset is unhealthy and the escalate responder is triggered, the following event is logged in the Windows event log:

## Managed Availability

In Exchange Server 2013, several architectural changes were made. One of the key changes is the *Managed Availability* feature where all Exchange 2013 components have built-in monitors that detect problems and attempt to recover the service availability. The Exchange 2013 Management Pack relies on this feature. Any issues that can't be recovered automatically are escalated to the Exchange 2013 Management Pack as an alert. Each component in Exchange 2013 monitors itself using three basic components called probes, monitors and responders.

![Managed availability](images/Dn195910.dd5febae-d05e-4089-a3f5-1691b2d9a3d7(EXCHG.150).png "Managed availability")

- **Probes**: These are sets of data collectors that measure various components. There are three distinct types of probes:

  - Synthetic transactions that measure synthetic end-to-end user operations and checks that measure actual traffic.

  - Checks that measure actual customer traffic.

  - Notifications that allow Exchange to take immediate action. A good example of this is the notification that is triggered when a certificate expires.

- **Monitors**: The data collected by probes are passed on to monitors that analyze the data for specific conditions and depending on those conditions determine if the particular component is healthy or unhealthy.

- **Responders**: If a monitor determines that a component is unhealthy, it will trigger a responder. If the problem is recoverable, the responder attempts to recover the component using the built-in logic. There are several responders available for each component, but the one responder that's relevant for the Exchange 2013 Management Pack is the *Escalate Responder*. When the Escalate Responder is triggered, it generates an event that the Exchange 2013 Management Pack recognizes and feeds the appropriate information into that alert that provides administrators with the information necessary to address the problem.

Each component in Exchange 2013 uses a specific set of probes, monitors and responders to monitor itself. These collections of probes and monitors are referred to as *health sets*. For example, there are a number of probes that collect data about various aspects of the ActiveSync service. This data is processed by a designated set of monitors that trigger the appropriate responders to correct any issues that they detect in the ActiveSync service. Collectively, these components make up the ActiveSync health set.

The health sets in Exchange are organized into the following four categories that correspond to the management pack dashboard:

- Customer Touch Points

- Service Components

- Server Resources

- Key Dependencies

For a complete list of Exchange health sets, see [Appendix A: Exchange health sets](appendix-a-exchange-health-sets.md).

To learn more about Managed Availability, see [Server health and performance](https://docs.microsoft.com/exchange/server-health-and-performance-exchange-2013-help).

## How health rolls up

This topic provides information about how the Exchange Server 2013 Management Pack monitors and reports Exchange system health. In the Exchange 2013 Management Pack, health state information is rolled up in a simple manner. Whenever a health set is unhealthy and the escalate responder is triggered, the following event is logged in the Windows event log:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Log name</p></td>
<td><p>Microsoft-Exchange-ManagedAvailability/Monitoring</p></td>
</tr>
<tr class="even">
<td><p>Source</p></td>
<td><p>ManagedAvailability</p></td>
</tr>
<tr class="odd">
<td><p>Date</p></td>
<td><p>&lt;<em>date and time of the event</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>Event ID</p></td>
<td><p>4</p></td>
</tr>
<tr class="odd">
<td><p>Task category</p></td>
<td><p>Monitoring</p></td>
</tr>
<tr class="even">
<td><p>Level</p></td>
<td><p>Error</p></td>
</tr>
<tr class="odd">
<td><p>Keywords</p></td>
<td><p>&lt;<em>none</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>User</p></td>
<td><p>SYSTEM</p></td>
</tr>
<tr class="odd">
<td><p>Computer</p></td>
<td><p>&lt;<em>FQDN of the Exchange server</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>Description</p></td>
<td><p>&lt;<em>dynamically generated by the escalate responder</em>&gt;</p></td>
</tr>
</tbody>
</table>

The Management Pack agent detects and processes this event. Using this event, Managed Availability is able to generate alerts within SCOM. When the corresponding issue is resolved, and the health set returns back to the healthy state, the following event is logged in the Windows event log:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Log name</p></td>
<td><p>Microsoft-Exchange-ManagedAvailability/Monitoring</p></td>
</tr>
<tr class="even">
<td><p>Source</p></td>
<td><p>ManagedAvailability</p></td>
</tr>
<tr class="odd">
<td><p>Date</p></td>
<td><p>&lt;<em>date and time of the event</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>Event ID</p></td>
<td><p>1</p></td>
</tr>
<tr class="odd">
<td><p>Task category</p></td>
<td><p>Monitoring</p></td>
</tr>
<tr class="even">
<td><p>Level</p></td>
<td><p>Information</p></td>
</tr>
<tr class="odd">
<td><p>Keywords</p></td>
<td><p>&lt;<em>none</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>User</p></td>
<td><p>SYSTEM</p></td>
</tr>
<tr class="odd">
<td><p>Computer</p></td>
<td><p>&lt;<em>FQDN of the Exchange server</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p>Description</p></td>
<td><p>&lt;<em>dynamically generated by the escalate responder</em>&gt;</p></td>
</tr>
</tbody>
</table>

The management packs that monitored previous versions of Exchange were completely centralized. Agents on each Exchange server would collect data and a central correlation engine would compare and evaluate all the data reported by the agents to determine overall service health. In large scale environments, this process resulted in complex correlations, causing delays in alert generation. In Exchange 2013, alert correlation is no longer used. Instead, each server performs its own monitoring and alerts SCOM if necessary, allowing for a highly scalable architecture.

Depending on the impact of the event, and the health set that triggers it, the problem is shown in the SCOM console in a different category. If the event causes user impact, then the customer touch points indicator is shown as unhealthy. If it causes an entire component like OWA to be unavailable, then the service component indicator is shown as unhealthy. If it's a problem with a particular server, then the corresponding server health indicator is shown as unhealthy. Finally if the problem is related to a resource that Exchange depends on, the key dependencies indicator is shown as unhealthy. For more information about these categories, see [Getting started with Exchange Server 2013 Management Pack](getting-started-with-exchange-server-2013-management-pack.md).
