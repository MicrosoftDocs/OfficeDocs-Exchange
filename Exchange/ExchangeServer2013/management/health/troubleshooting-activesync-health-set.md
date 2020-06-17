---
title: Troubleshooting ActiveSync Health Set
TOCTitle: Troubleshooting ActiveSync Health Set
ms:assetid: 8a0b8b26-b4ef-41b8-8f71-8271c1735a69
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.activesync(v=EXCHG.150)
ms:contentKeyID: 49720831
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting ActiveSync Health Set

_**Applies to:** Exchange Server 2013_

The Exchange ActiveSync health set monitors the overall health of the ActiveSync service for mobile clients in your organization. The ActiveSync health set is closely related to the following health sets:

[Troubleshooting ActiveSync.Protocol Health Set](troubleshooting-activesync-protocol-health-set.md)

[Troubleshooting ActiveSync.Proxy Health Set](troubleshooting-activesync-proxy-health-set.md)

If you receive an alert that indicates that the ActiveSync health set is unhealthy, this indicates an issue that may prevent your users from accessing their mailboxes by using ActiveSync.

## Explanation

The ActiveSync service is monitored using the following probes and monitors.

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th>Probe</th>
<th>Health Set</th>
<th>Dependencies</th>
<th>Associated Monitors</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>ActiveSyncCTPProbe</p></td>
<td><p>ActiveSync</p></td>
<td><p>Active Directory</p>
<p>Authentication</p>
<p>Mailbox Server Authentication</p>
<p>Information Store</p>
<p>High Availability</p>
<p>Network</p></td>
<td><p>ActiveSyncCTPMonitor (ActiveSync health set)</p></td>
</tr>
<tr class="even">
<td><p>ActiveSyncProxyTestProbe</p></td>
<td><p>ActiveSync.Proxy</p></td>
<td><p>-</p></td>
<td><p>ActiveSyncProxyTestMonitor (ActiveSync.Proxy health set)</p></td>
</tr>
<tr class="odd">
<td><p>ActiveSyncDeepTestProbe</p></td>
<td><p>ActiveSync.Protocol</p></td>
<td><p>Active Directory</p>
<p>Authentication</p>
<p>Information Store</p>
<p>High Availability</p></td>
<td><p>ActiveSyncDeepTestMonitor (ActiveSync health set)</p></td>
</tr>
<tr class="even">
<td><p>ActiveSyncSelfTestProbe</p></td>
<td><p>ActiveSync.Protocol</p></td>
<td><p>Active Directory</p>
<p>Authentication</p></td>
<td><p>ActiveSyncSelfTestMonitor (ActiveSync.Protocol health set)</p>
<p>RequestsQueuedGt500Monitor (ActiveSync health set)</p></td>
</tr>
</tbody>
</table>

For more information about probes and monitors, see [Server health and performance](https://docs.microsoft.com/exchange/server-health-and-performance-exchange-2013-help).

## User Action

It's possible that the service recovered after it issued the alert. Therefore, when you receive an alert that specifies that the ActiveSync health set is unhealthy, first verify that the issue still exists. If the issue does exist, perform the appropriate recovery actions outlined in the following sections.

## Verifying the issue

1. Identify the health set name and server name that are given in the alert.

2. The message details provide information about the exact cause of the alert. In most cases, the message details provide sufficient troubleshooting information to help identify the root cause. If the message details are not clear, do the following:

   1. Open the Exchange Management Shell, and run the following command to retrieve the details of the health set that issued the alert:

      ```powershell
      Get-ServerHealth <server name> | ?{$_.HealthSetName -eq "<health set name>"}
      ```

      For example, to retrieve the ActiveSync health set details about server1.contoso.com, run the following command:

      ```powershell
      Get-ServerHealth server1.contoso.com | ?{$_.HealthSetName -eq "ActiveSync"}
      ```

   2. Review the command output to determine which monitor reported the error. The **AlertValue** value for the monitor that issued the alert will be **Unhealthy**.

   3. Rerun the associated probe for the monitor that's in an unhealthy state. Refer to the table in the Explanation section to find the associated probe. To do this, run the following command:

      ```powershell
      Invoke-MonitoringProbe <health set name>\<probe name> -Server <server name> | Format-List
      ```

      For example, assume that the failing monitor is **ActiveSyncCTPMonitor**. The probe associated with that monitor is **ActiveSyncCTPProbe**. To run this probe on server1.contoso.com, run the following command:

      ```powershell
      Invoke-MonitoringProbe ActiveSync\ActiveSyncCTPProbe -Server server1.contoso.com | Format-List
      ```

   4. In the command output, review the "Result" section of the probe. If the value is **Succeeded**, the issue was a transient error, and it no longer exists. Otherwise, refer to the recovery steps outlined in the following sections.

## ActiveSyncDeepTestMonitor and ActiveSyncSelfTestMonitor Recovery Actions

This monitor alert is typically issued on Mailbox servers. To perform recovery actions, follow these steps:

1. Start IIS Manager, and then connect to the server that is reporting the issue. Click **Application Pools**, and then recycle the ActiveSync application pool that's named **MSExchangeSyncAppPool**.

2. Rerun the associated probe as shown in step 2c in the Verifying the issue section.

3. If the issue still exists, recycle the entire IIS service by using the IISReset utility.

4. Rerun the associated probe as shown in step 2c in the Verifying the issue section.

5. If the issue still exists, restart the server. To do this, first failover the databases that are hosted on the server by using the following command:

   ```powershell
   Set-MailboxServer server1.contoso.com -DatabaseCopyActivationDisabledAndMoveNow $true
   ```

   In this and all subsequent code examples, replace *server1.contoso.com* with the actual server name.

6. Next, verify that all databases have been moved off the server that is reporting the issue. To do this, run the following command:

   ```powershell
   Get-MailboxDatabaseCopyStatus -Server server1.contoso.com | Group Status
   ```

7. If the command output in step 6 shows no active copies on the server, restart the server. If the output does show active copies, run steps 5 and 6 again.

8. After the server restarts, rerun the associated probe as shown in step 2c in the Verifying the issue section.

9. If the probe succeeds, failover the databases by running the following command:

   ```powershell
   Set-MailboxServer server1.contoso.com -DatabaseCopyActivationDisabledAndMoveNow $false
   ```

10. If the probe still fails, you may need further assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit the [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and select **Servers** \> **Exchange Server** to contact a Microsoft Support professional. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## ActiveSyncCTPMonitor Recovery Actions

This monitor alert is typically issued on CA servers (CAS).

1. Start IIS Manager, and then connect to the server that is reporting the issue. Click **Application Pools**, and then recycle the ActiveSync application pool that is named **MSExchangeSyncAppPool**.

2. Rerun the associated probe as shown in step 2c in the Verifying the issue section.

3. If the issue still exists, recycle the entire IIS service by using the IISReset utility.

4. Rerun the associated probe as shown in step 2c in the Verifying the issue section.

5. If the issue persists, you must verify the health status on the corresponding Mailbox server. The name of the Mailbox server is the `_Mbx:` value that's given in the error message.

   1. Run the following command for the appropriate Mailbox server. For example, run the following command a Mailbox server that's named mailbox1.contoso.com:

      ```powershell
      Get-ServerHealth mailbox1.contoso.com | ?{$_.HealthSetName -like "ActiveSync*"}
      ```

   2. If any of the monitors that are listed in the command output are reported to be unhealthy, you must address those monitors first. To do this, follow the troubleshooting steps that are outlined in the ActiveSyncDeepTestMonitor and ActiveSyncSelfTestMonitor Recovery Actions section.

6. If all monitors on the Mailbox server are healthy, restart the CAS.

7. After the server restarts, rerun the associated probe as shown in step 2c in the Verifying the issue section.

8. If the probe continues to fail, you may need further assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## RequestsQueuedGt500Monitor Recovery Actions

This monitor alert is typically issued on CA servers.

1. Start IIS Manager, and then connect to the server that is reporting the issue. Click **Application Pools**, and then recycle the ActiveSync application pool that is named **MSExchangeSyncAppPool**.

2. Wait 10 minutes to see whether the monitor remains healthy. After 10 minutes, run the following command for the appropriate server. For example, run the following command for server1.contoso.com:

   ```powershell
   Get-ServerHealth server1.contoso.com | ?{$_.HealthSetName -like "ActiveSync*"}
   ```

3. If the issue persists, recycle the entire IIS service by using the IISReset utility.

4. Wait 10 minutes, and then run the command shown in step 2 again to see whether the monitor remains healthy.

5. If the issue persists, restart the server. If the server is a CAS, just restart the server. If the server is a Mailbox server, do the following:

   1. Failover the databases that are hosted on the server. To do this, run the following command:

      ```powershell
      Set-MailboxServer server1.contoso.com -DatabaseCopyActivationDisabledAndMoveNow $true
      ```

      **Note**: In this and all subsequent code examples, replace *server1.contoso.com* with the actual server name.

   2. Verify that all the databases have been moved off the server that is reporting the issue. To do this, run the following command:

      ```powershell
      Get-MailboxDatabaseCopyStatus -Server server1.contoso.com | Group Status
      ```

      If the command output shows no active copies on the server, restart the server.

6. After the server restarts, wait 10 minutes, and then run the command shown in step 2 again to determine whether the monitor remains healthy.

7. If the monitor remains healthy, and if this is a Mailbox server, failover the databases by running the following command:

   ```powershell
   Set-MailboxServer server1.contoso.com -DatabaseCopyActivationDisabledAndMoveNow $false
   ```

8. If the probe continues to fail, you may need further assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## For More Information

[Exchange ActiveSync](https://docs.microsoft.com/exchange/exchange-activesync-exchange-2013-help)

[Mobile devices](https://docs.microsoft.com/exchange/mobile-devices-exchange-2013-help)

[Exchange ActiveSync virtual directory management tasks](https://docs.microsoft.com/exchange/exchange-activesync-virtual-directory-management-tasks-exchange-2013-help)
