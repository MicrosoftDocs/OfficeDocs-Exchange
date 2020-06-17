---
title: 'Configure managed availability overrides: Exchange 2013 Help'
TOCTitle: Configure managed availability overrides
ms:assetid: c8f315b3-1d5e-4ad9-8bea-9c3a4a13ebfc
ms:mtpsurl: https://technet.microsoft.com/library/Dn482055(v=EXCHG.150)
ms:contentKeyID: 59888994
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure managed availability overrides

_**Applies to:** Exchange Server 2013 SP1_

Managed availability performs continuous probing to detect possible problems with Exchange components or their dependencies, and it performs recovery actions to make sure the end user experience is not impacted due to a problem with any of these components. However, there may be scenarios where the out-of-box settings may not be suitable for your environment. Managed availability probes, monitors and responders can be customized by creating an override.

There are two types of overrides: local and global. As their names imply, a local override is available only on the server on which it is created, and a global override is used to apply an override to multiple servers. Both types of override can be created for a specific duration or for a specific version of Exchange, but not both at the same time.

> [!NOTE]
> When you create an override, it doesn't take effect immediately. The Microsoft Exchange Health Management service checks for configuration changes every 10 minutes and loads any detected configuration changes. If you don't want to wait, you can restart the service.

For additional management tasks related to managed availability, see [Manage health sets and server health](manage-health-sets-and-server-health-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You can only use the Shell to perform this procedure. To learn how to open the Shell in your on-premises Exchange organization, see [Open the Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Exchange Management Shell to create local overrides

To create a local override for a specific duration, use the following syntax.

```powershell
Add-ServerMonitoringOverride -Server <ServerName> -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <Probe | Monitor | Responder | Maintenance> -PropertyName <PropertyName> -PropertyValue <Value> -Duration <dd.hh:mm:ss>
```

To create a local override for a specific version of Exchange, use the following syntax.

```powershell
Add-ServerMonitoringOverride -Server <ServerName> -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <Probe | Monitor | Responder | Maintenance> -PropertyName <PropertyName> -PropertyValue <Value> -Version <15.01.xxxx.xxx>
```

> [!NOTE]
> When you create the override, the values used in the <EM>Identity</EM> parameter are case-sensitive.

This example adds a local override that disables the responder `ActiveDirectoryConnectivityConfigDCServerReboot` on the server named EXCH03 for 20 days.

```powershell
Add-ServerMonitoringOverride -Server EXCH03 -Identity "AD\ActiveDirectoryConnectivityConfigDCServerReboot" -ItemType Responder -PropertyName Enabled -PropertyValue 0 -Duration 20.00:00:00
```

## How do you know this worked?

To verify that you have successfully created a local override, use the **Get-ServerMonitoringOverride** cmdlet to view the list of local overrides:

```powershell
Get-ServerMonitoringOverride -Server <ServerIdentity> | Format-List
```

The override should appear in the list.

## Use the Exchange Management Shell to remove local overrides

To remove a local override, use the following syntax.

```powershell
Remove-ServerMonitoringOverride -Server <ServerName> -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <ExistingItemTypeValue> -PropertyName <PropertytoRemove>
```

This example removes the existing local override of the `ActiveDirectoryConnectivityConfigDCServerReboot` responder in the Exchange health set from server EXCH01.

```powershell
Remove-ServerMonitoringOverride -Server EXCH01 -Identity Exchange\ActiveDirectoryConnectivityConfigDCServerReboot -ItemType Responder -PropertyName Enabled
```

## How do you know this worked?

To verify that you have successfully removed a local override, use the **Get-ServerMonitoringOverride** cmdlet to view the list of local overrides:

```powershell
Get-ServerMonitoringOverride -Server <ServerIdentity> | Format-List
```

The removed override should not appear in the list.

## Use the Exchange Management Shell to create global overrides

To create a global override for a specific duration, use the following syntax.

```powershell
Add-GlobalMonitoringOverride -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <Probe | Monitor | Responder | Maintenance> -PropertyName <PropertytoOverride> -PropertyValue <NewPropertyValue> -Duration <dd.hh:mm:ss>
```

To create a global override for a specific version of Exchange, use the following syntax.

```powershell
Add-GlobalMonitoringOverride -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <Probe | Monitor | Responder | Maintenance> -PropertyName <PropertytoOverride> -PropertyValue <NewPropertyValue> -ApplyVersion <15.01.xxxx.xxx>
```

> [!NOTE]
> When you create the override, the values used in the <EM>Identity</EM> parameter are case-sensitive.

This example adds a global override that disables the `OnPremisesInboundProxy` probe for 30 days.

```powershell
Add-GlobalMonitoringOverride -Identity "FrontendTransport\OnPremisesInboundProxy" -ItemType Probe -PropertyName Enabled -PropertyValue 0 -Duration 30.00:00:00
```

This example adds a global override that disables the `StorageLogicalDriveSpaceEscalate` responder for all servers running Exchange version 15.01.0225.042.

```powershell
Add-GlobalMonitoringOverride -Identity "MailboxSpace\StorageLogicalDriveSpaceEscalate" -PropertyName Enabled -PropertyValue 0 -ItemType Responder -ApplyVersion "15.01.0225.042"
```

## How do you know this worked?

To verify that you have successfully created a global override, use the **Get-GlobalMonitoringOverride** cmdlet to view the list of global overrides:

```powershell
Get-GlobalMonitoringOverride
```

The override should appear in the list.

## Use the Exchange Management Shell to remove global overrides

To remove a global override, use the following syntax.

```powershell
Remove-GlobalMonitoringOverride -Identity <HealthSetName>\<MonitoringItemName>[\<TargetResource>] -ItemType <ExistingItemTypeValue> -PropertyName <OverriddenProperty>
```

This example removes the existing global override of the `ExtensionAttributes` property of the `OnPremisesInboundProxy` probe in the `FrontEndTransport` health set.

```powershell
Remove-GlobalMonitoringOverride -Identity FrontEndTransport\OnPremisesInboundProxy -ItemType Probe -PropertyName ExtensionAttributes
```

## How do you know this worked?

To verify that you have successfully removed a global override, use the **Get-GlobalMonitoringOverride** cmdlet to view the list of global overrides:

```powershell
Get-GlobalMonitoringOverride
```

The removed override should not appear in the list.
