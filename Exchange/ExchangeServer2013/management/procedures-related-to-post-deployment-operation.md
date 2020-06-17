---
title: Procedures related to post-deployment operation
TOCTitle: Procedures related to post-deployment operation
ms:assetid: d9613a5c-5661-4bce-9a2c-e2c7b601e723
ms:mtpsurl: https://technet.microsoft.com/library/Dn198286(v=EXCHG.150)
ms:contentKeyID: 53496601
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Procedures related to post-deployment operation

_**Applies to:** Exchange Server 2013_

This topic contains the procedures that you can use as a reference when managing the Exchange Server 2013 Management Pack. For procedures related to deployment, see [Procedures related to deployment](procedures-related-to-deployment.md).

## View Exchange 2013 Management Pack monitors

For each Exchange server, only the monitors that are applicable to the roles installed are enabled. For a complete list of the monitors available in Exchange Server 2013 Management Pack, see [Appendix A: Exchange health sets](appendix-a-exchange-health-sets.md). Follow the steps below to see the monitors currently enabled on an Exchange server.

Your user account needs to be a member of the Operations Manager Administrators role to perform this procedure.

1. Log on to your SCOM server and open the SCOM console.

2. In the Operations Console, click **Monitoring**.

3. Expand **Microsoft Exchange Server 2013**, and then click **Server Health**.

4. Right click on one of the Exchange servers listed and click **Open**, **Health Explorer**.

5. By default, the view is scoped to unhealthy child monitors. Click on **Filter Monitors** to clear the filter.

6. Expand **Entity Health** to view the following four key dependency monitors for Exchange Server 2013:

   - Customer Touch Points

   - Key Dependencies

   - Server Resources

   - Service Components

7. Expand any of these monitors to view a list of the monitors that apply to that Exchange Server.

## Create overrides

Exchange Server 2013 management pack is designed to be simple to deploy and use. It is engineered to scale with your environment, and in most cases no additional configuration changes are required once you import it. However, if you see alerts that are not valuable to you, you can configure overrides to turn these specific alerts off. Use these overrides only if you are experiencing specific problems.

## Enable or disable monitors

Depending on your needs, you may want to disable or enable specific alerts within the SCOM console or entire health sets using the Shell. The following sections provide examples of both approaches.

## Enable or disable an alert using SCOM console

Let's say you see an alert in the **Active Alerts** for the Store health set on a server named Server1 and this server doesn't currently have any databases with active mailboxes on it. In this case, you may not want to get alerted for the Store health set on this server. You will still be able to see the health of Server1 in the SCOM console, but won't get alerted for the Store health set.

To disable this alert in the SCOM console:

1. Right click on the alert, and then select **Overrides** \> **Disable the Monitor** \> **For the object: Server1 - Store**.

2. In the **Override Properties** window, clear the checkbox for the **Parameter Name** **Enabled**.

3. Select a destination management pack to store the override, and click **OK**.

![Disabling an alert in SCOM console](images/Dn198286.1c4f15b5-4978-4442-b26b-cc65ba577c9c(EXCHG.150).png "Disabling an alert in SCOM console")

## Enable or disable a health set using the Shell

Let's say that you don't use the POP3 feature in your organization. You may want to disable monitoring support for that feature on your mailbox servers in your organization. You can do so using the following steps:

1. Start the Exchange Management Shell

2. First, you need to determine the list of monitors associated with the POP3 service on a Mailbox server. The list in [Appendix A: Exchange health sets](appendix-a-exchange-health-sets.md) shows that the health set associated with POP3 service on a mailbox server is POP.Protocol. You need to run the [Get-MonitoringItemIdentity](https://docs.microsoft.com/powershell/module/exchange/Get-MonitoringItemIdentity) cmdlet to get a list of all monitors associated with the POP.Protocol healthset. The following command returns all monitoring items for POP.Protocol health set and stores them in the temporary variable `$POPMonitoringItems`. Note that the command uses a mailbox server to get this list as the POP.Protocol health set won't be present on a server that doesn't have the Mailbox role installed.

   ```powershell
   $POPMonitoringItems = Get-MonitoringItemIdentity -Identity POP.Protocol -Server Mailbox1
   ```

3. The `$POPMonitoringItems` contains all monitoring items including probes, monitors and responders. Let's separate just the monitors and store them in the temporary variable `$POPMonitors`by running the following command:

   ```powershell
   $POPMonitors = $POPMonitoringItems | Where {$_.ItemType -eq "Monitor"}
   ```

4. For each of the monitors for POP.Protocol, you will need to create a global override using the **Add-GlobalMonitoringOverride** cmdlet. Instead of doing them one by one, you can just pipe each monitor in the `$POPMonitors` variable to the **Add-GlobalMonitoringOverride** cmdlet by running the following command.

   ```powershell
   $POPMonitors | Where {Add-GlobalMonitoringOverride -Item Monitor -Identity $($_.HealthSetName+"\"+$_.Name) -PropertyName Enabled -PropertyValue 0 -Duration 60
   ```

5. To verify that you have correctly created the global overrides, run the following command:

   ```powershell
   Get-GlobalMonitoringOverride | Where {$_.Identity -like "*POP.Protocol*"} | Format-Table Identity, ItemType, PropertyName, PropertyValue
   ```

## Modify monitoring thresholds

You also may need to modify specific thresholds for various monitor properties. For example, assume that your organization doesn't have a large internal message volume. By default, Exchange will raise an alert if the internal aggregate delivery queue exceeds 250 messages for high priority messages and 500 for low priority messages. Let's assume that these thresholds are high enough that you don't get notified soon enough that there is a problem. You determine that if these queues exceed 50 and 150 messages respectively you need to get notified. You can change the corresponding thresholds by following the steps below:

1. Start the Exchange Management Shell.

2. The delivery queues are monitored by the HubTransport health set. First you need to get the list of monitors associated with this healthset that are responsible for internal delivery queues.

   ```powershell
   Get-MonitoringItemIdentity -Identity HubTransport -Server Mailbox1 | Where {$_.Name -like "*InternalAggregateDeliveryQueue*" -and $_.ItemType -eq "Monitor"} | Format-Table Name
   ```

3. You will see that there are two monitors for internal aggregate delivery queues: InternalAggregateDeliveryQueueLengthLowPriorityMonitor and InternalAggregateDeliveryQueueLengthHighPriorityMonitor. You then add global overrides for each monitor using the commands below:

   ```powershell
   Add-GlobalMonitoringOverride -Item Monitor -Identity HubTransport\InternalAggregateDeliveryQueueLengthLowPriorityMonitor -PropertyName MonitoringThreshold -PropertyValue 150 -Duration 60
   Add-GlobalMonitoringOverride -Item Monitor -Identity HubTransport\InternalAggregateDeliveryQueueLengthHighPriorityMonitor -PropertyName MonitoringThreshold -PropertyValue 50 -Duration 60
   ```

4. To verify that you have correctly created the global overrides, run the following command:

   ```powershell
   Get-GlobalMonitoringOverride | Where {$_.Identity -like "*HubTransport*"} | Format-Table Identity, ItemType, PropertyName, PropertyValue
   ```

## Cmdlet reference for monitoring overrides

See the following topics for more information about the cmdlets you can use to configure monitoring overrides.

- [Add-GlobalMonitoringOverride](https://docs.microsoft.com/powershell/module/exchange/Add-GlobalMonitoringOverride)

- [Get-GlobalMonitoringOverride](https://docs.microsoft.com/powershell/module/exchange/Get-GlobalMonitoringOverride5)

- [Remove-GlobalMonitoringOverride](https://docs.microsoft.com/powershell/module/exchange/Remove-GlobalMonitoringOverride)

- [Add-ServerMonitoringOverride](https://docs.microsoft.com/powershell/module/exchange/Add-ServerMonitoringOverride)

- [Get-ServerMonitoringOverride](https://docs.microsoft.com/powershell/module/exchange/Get-ServerMonitoringOverride)

- [Get-MonitoringItemIdentity](https://docs.microsoft.com/powershell/module/exchange/get-monitoringitemidentity)
