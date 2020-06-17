---
title: Troubleshooting DataProtection Health Set
TOCTitle: Troubleshooting DataProtection Health Set
ms:assetid: cde3cc34-2076-4e30-8d3c-265b66d00ae8
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.dataprotection(v=EXCHG.150)
ms:contentKeyID: 49720873
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting DataProtection Health Set

_**Applies to:** Exchange Server 2013_

The DataProtection Health set monitors the redundancy of databases in a database availability group (DAG).

If you receive an alert that specifies that DataProtection is unhealthy, this indicates an issue that may affect the replication or cluster components, and that can prevent access to the Exchange databases.

## Explanation

The DataProtection Health service is monitored by using the following probes and monitors.

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
<td><p>ClusterEndpointProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ClusterEndpointMonitor</p></td>
</tr>
<tr class="even">
<td><p>ClusterGroupProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ClusterGroupMonitor        </p></td>
</tr>
<tr class="odd">
<td><p>ClusterNetworkProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ClusterNetworkMonitor</p></td>
</tr>
<tr class="even">
<td><p>ClusterServiceCrashProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ClusterServiceCrashMonitor</p></td>
</tr>
<tr class="odd">
<td><p>ServerOneCopyProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Director</p></td>
<td><p>ServerOneCopyMonitor</p></td>
</tr>
<tr class="even">
<td><p>ServerOneCopyInternalMonitorProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ServerOneCopyInternalMonitorMonitor</p></td>
</tr>
<tr class="odd">
<td><p>ServiceHealthMSExchangeReplEndpointProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ServiceHealthMSExchangeReplEndpointMonitor</p></td>
</tr>
<tr class="even">
<td><p>ServiceHealthMSExchangeReplCrashProbe </p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ServiceHealthMSExchangeReplCrashMonitor </p></td>
</tr>
<tr class="odd">
<td><p>ServerSiteFailureProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>ServerSiteFailureMonitor</p></td>
</tr>
<tr class="even">
<td><p>StorageApparentControllerIssuesProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>StorageApparentControllerIssuesMonitor</p></td>
</tr>
<tr class="odd">
<td><p>DatabaseHealthTooManyMountedDatabaseProbe</p></td>
<td><p>DataProtection</p></td>
<td><p>Active Directory</p></td>
<td><p>DatabaseHealthTooManyMountedDatabaseMonitor</p></td>
</tr>
</tbody>
</table>

For more information about probes and monitors, see [Server health and performance](https://docs.microsoft.com/exchange/server-health-and-performance-exchange-2013-help).

## User Action

It's possible that the service recovered after it issued the alert. Therefore, when you receive an alert that specifies that the health set is unhealthy, first verify that the issue still exists. If the issue does exist, perform the appropriate recovery actions outlined in the following sections.

## Verifying the issue still exists

1. Identify the health set name and the server name in the alert.

2. The message details provide information about the exact cause of the alert. In most cases, the message details provide sufficient troubleshooting information to identify the root cause. If the message details are not clear, do the following:

   1. Open the Exchange Management Shell, and then run the following command to retrieve the details of the health set that issued the alert:

      ```powershell
      Get-ServerHealth <server name> | ?{$_.HealthSetName -eq "<health set name>"}
      ```

      For example, to retrieve the Autodiscover.Protocol health set details about server1.contoso.com, run the following command:

      ```powershell
      Get-ServerHealth server1.contoso.com | ?{$_.HealthSetName -eq "Autodiscover.Protocol"}
      ```

      Review the command output to determine which monitor reported the error. The **AlertValue** value for the monitor that issued the alert will be `Unhealthy`.

   2. Identify the probe that the monitor is based on. Note that most probes share the same name prefix. By using the previous example, search for "**ClusterNetwork\***":

      ```powershell
      Get-MonitoringItemIdentity -Identity DataProtection -Server server1.contoso.com | ?{$_.Name -like "ClusterNet ItemType work*"}
      ```

      The returned results should resemble the following.

      <table>
      <colgroup>
      <col style="width: 25%" />
      <col style="width: 25%" />
      <col style="width: 25%" />
      <col style="width: 25%" />
      </colgroup>
      <tbody>
      <tr class="odd">
      <td><p><code>ItemType</code></p></td>
      <td><p><code>HealthSetName</code></p></td>
      <td><p><code>Name</code></p></td>
      <td><p><code>TargetResource</code></p></td>
      </tr>
      <tr class="even">
      <td><p><code>Probe</code></p></td>
      <td><p><code>DataProtection</code></p></td>
      <td><p><code>ClusterNetworkProbe</code></p></td>
      <td><p><code>MSExchangeRepl</code></p></td>
      </tr>
      </tbody>
      </table>

   3. Rerun the associated probe for the monitor that's in an unhealthy state. Refer to the table in the Explanation section to find the associated probe. To do this, run the following command:

      ```powershell
      Invoke-MonitoringProbe <health set name>\<probe name> -Server <server name> | Format-List
      ```

      For example, assume that the failing monitor is **AutodiscoverSelfTestMonitor**. The probe associated with that monitor is **AutodiscoverSelfTestProbe**. To run that probe on server1.contoso.com, run the following command:

      ```powershell
      Invoke-MonitoringProbe Autodiscover.Protocol\AutodiscoverSelfTestProbe -Server server1.contoso.com | Format-List
      ```

   4. In the command output, review the **Result** value of the probe. If the value is **Succeeded**, the issue was a transient error, and it no longer exists. Otherwise, refer to the recovery steps outlined in the following sections.

## Troubleshooting steps

When you receive an alert from a health set, the email message contains the following information:

- Name of the server that sent the alert

- Time and date when the alert occurred

- Authentication mechanism that was used, and credential information

- Full exception trace of the last error, including diagnostic data and specific HTTP header information

  You can use the information in the full exception trace to help troubleshoot the issue. The exception generated by the probe contains a failure Reason that describes why the probe failed.

For most issues that occur in high availability environments, you can run the **Test-ReplicationHealth** cmdlet to help troubleshoot the cluster/networking/ActiveManager/services. Other HealthSet/Components will have different Test-\* cmdlets.

For example:

```powershell
Test-ReplicationHealth <ServerName>
```

The returned results will resemble the following:

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><code>Server</code></p></td>
<td><p><code>Check</code></p></td>
<td><p><code>Result</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>ClusterService</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>ReplayService</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>ActiveManager</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>TasksRpcListener</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>TcpListener</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>ServerLocatorService</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DagMembersUp</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>ClusterNetwork</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>QuorumGroup</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>FileShareQuorum</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DatabaseRedundancyCheck</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DatabaseAvailabilityCheck</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBCopySuspended</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBCopyFailed</code></p></td>
<td><p>Passed</p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBInitializing</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBDisconnected</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="even">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBLogCopyKeepingUp</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
<tr class="odd">
<td><p><em>&lt;ServerName&gt;</em></p></td>
<td><p><code>DBLogReplayKeepingUp</code></p></td>
<td><p><code>Passed</code></p></td>
</tr>
</tbody>
</table>

If all components display **Passed** in the **Result** column, try to rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

If the issue still exists, restart the server. After the server restarts, rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## For More Information

[What's new in Exchange 2013](https://docs.microsoft.com/exchange/what-s-new-in-exchange-2013-exchange-2013-help)

[Exchange PowerShell](https://docs.microsoft.com/powershell/exchange/)
