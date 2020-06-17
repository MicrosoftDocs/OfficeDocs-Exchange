---
title: Troubleshooting ActiveSync.Protocol Health Set
TOCTitle: Troubleshooting ActiveSync.Protocol Health Set
ms:assetid: 7351f881-08b2-4504-99f2-63e7acdfcc35
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.activesync.protocol(v=EXCHG.150)
ms:contentKeyID: 49720823
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting ActiveSync.Protocol Health Set

_**Applies to:** Exchange Server 2013_

The ActiveSync.Protocol health set monitors the overall health of the Exchange ActiveSync communications protocol on your Mailbox servers. If you receive an alert that specifies that the ActiveSync.Protocol health set is unhealthy, this indicates an issue that may affect the ActiveSync components on the Mailbox server indicated in the alert.

## Explanation

The ActiveSync.Protocol health set works in conjunction with the **ActiveSync** health set. For detailed information about the ActiveSync health set, see [Troubleshooting ActiveSync Health Set](troubleshooting-activesync-health-set.md).

## User Action

It's possible that the ActiveSync service recovered after it issued the alert. Therefore, when you receive an alert that indicates that the ActiveSync health set is unhealthy, first verify that the issue still exists. For more information, see [Troubleshooting ActiveSync Health Set](troubleshooting-activesync-health-set.md).

## For More Information

[Exchange ActiveSync](https://docs.microsoft.com/exchange/exchange-activesync-exchange-2013-help)

[Mobile devices](https://docs.microsoft.com/exchange/mobile-devices-exchange-2013-help)

[Exchange ActiveSync virtual directory management tasks](https://docs.microsoft.com/exchange/exchange-activesync-virtual-directory-management-tasks-exchange-2013-help)
