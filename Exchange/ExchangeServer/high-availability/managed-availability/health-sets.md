---
localization_priority: Normal
description: 'Summary: Exchange Management Shell cmdlets that can help you monitor the health of your Exchange organization.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a4f84312-6cfa-4f17-9707-676aadab1143
ms.reviewer:
title: Manage health sets and server health
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage health sets and server health

You can use the built-in health reporting cmdlets to perform a variety of tasks related to managed availability, such as:

- Viewing the health of a server or group of servers

- Viewing a list of health sets

- Viewing a list of probes, monitors, and responders associated with a particular health set

- View a list of monitors and their current health

For more information about health reporting and managed availability, see [Managed availability](managed-availability.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes

- The procedures in this topic require the Exchange Management Shell. For more information, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to view server health

You can use the Exchange Management Shell to get a summary of the health of an Exchange server.

Run either of the following commands to view the health sets and health information on an Exchange server:

```powershell
Get-HealthReport -Identity <ServerName>
```

```powershell
Get-ServerHealth -Identity <ServerName> | Format-Table Server,CurrentHealthSetState,Name,HealthSetName,AlertValue,HealthGroupName -Auto
```

Run any of the following commands to view the health sets on an Exchange server or database availability group:

```powershell
Get-ExchangeServer | Get-HealthReport -RollupGroup
```

```powershell
Get-ExchangeServer | Get-HealthReport -RollupGroup -HealthSetName <HealthSet>
```

```powershell
(Get-DatabaseAvailabilityGroup <DAGName>).Servers | Get-HealthReport -RollupGroup
```

For detailed syntax and parameter information, see [Get-HealthReport](https://docs.microsoft.com/powershell/module/exchange/get-healthreport).

## Use the Exchange Management Shell to view a list of health sets

A *health set* is a group of monitors, probes and responders for a component that determine whether the component is healthy or unhealthy.

Run the following command to view the health sets on an Exchange server:

```powershell
Get-HealthReport -Server <ServerName>
```

For detailed syntax and parameter information, see [Get-HealthReport](https://docs.microsoft.com/powershell/module/exchange/get-healthreport).

## Use the Exchange Management Shell to view the probes, monitors and responders for a health set

You can use the Exchange Management Shell to view the list of probes, monitors, and responders associated with a health set on an Exchange server.

Run the following command to view the probes, monitors and responders associated with a health set on an Exchange server:

```powershell
Get-MonitoringItemIdentity -Server <ServerName> -Identity <HealthSetName> | Format-Table Identity,ItemType,Name -Auto
```

For detailed syntax and parameter information, see [Get-MonitoringItemIdentity](https://docs.microsoft.com/powershell/module/exchange/get-monitoringitemidentity).

## Use the Exchange Management Shell to View a List of Monitors and Their Current Health

The health of a monitor is reported by using the "worst of" monitors in the health set. You can view the details of a health set to see which monitors are healthy and which ones are unhealthy.

Run the following command to view a list of the monitors and their current health on an Exchange server:

```powershell
Get-ServerHealth -HealthSet <HealthSetName> -Server <ServerName> | Format-Table Name, AlertValue -Auto
```

For detailed syntax and parameter information, see [Get-ServerHealth](https://docs.microsoft.com/powershell/module/exchange/get-serverhealth).
