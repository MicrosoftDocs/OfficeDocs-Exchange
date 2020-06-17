---
title: Troubleshooting POP Health Set
TOCTitle: Troubleshooting POP Health Set
ms:assetid: 6114e9fe-d037-4cb9-a643-933eb5fabc45
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.pop(v=EXCHG.150)
ms:contentKeyID: 49720812
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting POP Health Set

_**Applies to:** Exchange Server 2013_

The POP health set monitors the overall health and availability of the Microsoft Exchange POP3 service and POP3 client connectivity. The POP health set is closely related to the following health sets:

- [Troubleshooting POP.Protocol Health Set](troubleshooting-pop-protocol-health-set.md)

- [Troubleshooting POP.Proxy Health Set](troubleshooting-pop-proxy-health-set.md)

If you receive an alert that specifies that the POP service is unhealthy, this indicates an issue that may prevent users from accessing their mailboxes by using POP3 in the Exchange environment.

## Explanation

The POP service is monitored by using the following probes and monitors.

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
<td><p>PopCTPProbe</p></td>
<td><p>POP</p></td>
<td><p>Active Directory</p>
<p>Authentication</p>
<p>Mailbox Server Authentication</p>
<p>Information Store</p>
<p>High Availability</p>
<p>Network</p></td>
<td><p>PopCTPMonitor (POP health set)</p></td>
</tr>
<tr class="even">
<td><p>PopProxyTestProbe</p></td>
<td><p>POP.Proxy</p></td>
<td><p>None</p></td>
<td><p>PopProxyTestMonitor (POP.Proxy health set)</p></td>
</tr>
<tr class="odd">
<td><p>PopDeepTestProbe</p></td>
<td><p>POP.Protocol</p></td>
<td><p>Active Directory</p>
<p>Authentication</p>
<p>Information Store</p>
<p>High Availability</p></td>
<td><p>PopDeepTestMonitor (POP.Protocol health set)</p></td>
</tr>
<tr class="even">
<td><p>PopSelfTestProbe</p></td>
<td><p>POP.Protocol</p></td>
<td><p>None</p></td>
<td><p>PopSelfTestMonitor (POP.Protocol health set)</p>
<p>AverageCommandProcessingTimeGt60sMonitor (POP health set)</p></td>
</tr>
</tbody>
</table>

## User Action

It's possible that the service recovered after it issued the alert. Therefore, when you receive an alert that specifies that the health set is unhealthy, first verify that the issue still exists. If the issue does exist, perform the appropriate recovery actions outlined in the following sections.

## Verifying the issue still exists

1. Identify the health set name and the server name in the alert.

2. The message details provide information about the exact cause of the alert. In most cases, the message details provide sufficient troubleshooting information to identify the root cause. If the message details are not clear, do the following:

   1. Open the Exchange Management Shell, and then run the following command to retrieve the details of the health set that issued the alert:

      Get-ServerHealth <server name> | ?{$_.HealthSetName -eq "<health set name>"}

      For example, to retrieve the POP health set details about server1.contoso.com, run the following command:

      Get-ServerHealth server1.contoso.com | ?{$_.HealthSetName -eq "POP"}

   2. Review the command output to determine which monitor reported the error. The **AlertValue** value for the monitor that issued the alert will be `Unhealthy`.

   3. Rerun the associated probe for the monitor that's in an unhealthy state. Refer to the table in the Explanation section to find the associated probe. To do this, run the following command:

      Invoke-MonitoringProbe <health set name>\<probe name> -Server <server name> | Format-List

      For example, assume that the failing monitor is **PopCTPMonitor**. The probe associated with that monitor is **PopCTPProbe**. To run that probe on server1.contoso.com, run the following command:

      Invoke-MonitoringProbe POP\PopCTPProbe -Server server1.contoso.com | Format-List

   4. In the command output, review the **Result** value of the probe. If the value is **Succeeded**, the issue was a transient error, and it no longer exists. Otherwise, refer to the recovery steps outlined in the following sections.

## PopTestDeepMonitor and PopSelfTestMonitor Recovery Actions

This monitor alert is typically issued on Mailbox servers.

1. Restart the POP3 back-end service. For more information, see [Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help).

2. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

3. If the probe still fails, failover the databases that are hosted on the Mailbox server by using the following command:

   Set-MailboxServer -Identity <ServerName> -DatabaseCopyActivationDisabledAndMoveNow $true

4. After all the databases are removed from the Mailbox server, you must verify that the databases have been moved successfully. To do this, run the following command:

   Get-MailboxDatabaseCopyStatus -Server <ServerName> | group status

5. Make sure that the server does not host any active copies of the database. Then, restart the server.

6. After the server has successfully restarted, rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

7. If the probe succeeds, failover the databases by running the following command:

   Set-MailboxServer -Identity <ServerName> -DatabaseCopyActivationDisabledAndMoveNow $false

8. If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## POPCTPMonitor Recovery Actions

This monitor alert is typically issued on CA servers (CAS).

1. Restart the POP3 service on the servers that are running the CAS role. For more information, see [Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help).

2. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

3. If the issue still exists, do the following steps:

   1. Run the following command in the Exchange Management Shell:

      ```powershell
      Set-PopSettings -server <CAS server name> -ProtocolLoggingEnabled $true
      ```

   2. Restart the POP3 service on the servers that are running the CAS role.

   3. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

   4. Run the following command, and then find the location of the log file:

      ```powershell
      Get-PopSettings -server <CAS server name>
      ```

   5. Run the following command to determine which mailbox is serving the command by comparing time stamps with the probe:

      ```powershell
      Get-ServerHealth <mailbox server name> | ?{ $_.HealthSetName -like "POP*"}
      ```

   6. If any of these servers are reported as unhealthy, follow the steps listed in the PopTestDeepMonitor and PopSelfTestMonitor Recovery Actions section.

4. If the Mailbox server is reported as healthy, restart the CAS.

5. After the server restarts, rerun the associated probe as described in step 2c in the Verifying the issue still exists section.

6. Turn off protocol logging by running the following command:

   ```powershell
   Set-PopSettings -server <CAS server name> -ProtocolLoggingEnabled $false
   ```

7. Restart the POP3 service on the servers that are running the CAS role. For more information, see [Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help).

8. If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## PopProxyTestMonitor recovery actions

1. Restart the POP3 service on the servers that are running the CAS role. For more information, see [Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help).

2. Rerun the associated probe as shown in step 2c in the Verifying the issue still exists section.

3. If the probe continues to fail, restart the CAS.

4. After the server restarts, rerun the associated probe as described in step 2c in the Verifying the issue still exists section.

5. If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## AverageCommandProcessingTimeGt60sMonitor recovery actions

This monitor alert is typically issued on CA or Mailbox servers.

1. Restart the POP3 service on the CA or Mailbox servers. For more information, see [Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help).

2. Wait 10 minutes to see whether the monitor stays healthy. After 10 minutes, run the following command (the example uses server1.contoso.com).

   ```powershell
   Get-ServerHealth server1.contoso.com | ?{$_.HealthSetName -like "POP*"}
   ```

3. If the issue still exists, you need to restart the server. If the server is a CAS, just restart the server. If the server is a Mailbox server, you must failover the database, and verify the results. To do this, follow these steps:

   1. Failover the databases hosted on the server by using the following command:

      ```powershell
      Set-MailboxServer -Identity <ServerName> -DatabaseCopyActivationDisabledAndMoveNow $true
      ```

      **Note**: In this and all subsequent code examples, replace *server1.contoso.com* with the actual server name.

   2. Verify that all databases have been moved off the server that's reporting the issue. To do this, run the following command:

      ```powershell
      Get-MailboxDatabaseCopyStatus -Server <ServerName> | group status
      ```

      If the command output shows no active copies on the server, restart the server.

4. After the server restarts, wait 10 minutes, and then run the command shown in step 2 again to see whether the monitor stays healthy.

5. If the monitor is healthy, and this is a Mailbox server, failover the databases back to the Mailbox server by running the following command:

   ```powershell
   Set-MailboxServer -Identity <ServerName> -DatabaseCopyActivationDisabledAndMoveNow $false
   ```

6. If the probe continues to fail, you may need assistance to resolve this issue. Contact a Microsoft Support professional to resolve this issue. To contact a Microsoft Support professional, visit [Support for business](https://support.microsoft.com/supportforbusiness/productselection) and then select **Servers** \> **Exchange Server**. Because your organization may have a specific procedure for directly contacting Microsoft Product Support Services, be sure to review your organization's guidelines first.

## For More Information

[Start and stop the POP3 services](https://docs.microsoft.com/exchange/start-and-stop-the-pop3-services-exchange-2013-help)

[Test-PopConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-PopConnectivity)
