---
title: Troubleshooting OWA.Protocol.Dep Health Set
TOCTitle: Troubleshooting OWA.Protocol.Dep Health Set
ms:assetid: f39c63d5-f161-4eee-9415-05f3355e7cc7
ms:mtpsurl: https://technet.microsoft.com/library/ms.exch.scom.owa.protocol.dep(v=EXCHG.150)
ms:contentKeyID: 49720914
ms.reviewer:
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Troubleshooting OWA.Protocol.Dep Health Set

_**Applies to:** Exchange Server 2013_

The **OWA.Protocol.DEP** health set monitors the overall health of instant messaging (IM) services in Outlook Web App that are integrated between Lync 2013 and Exchange 2013. For more information about enabling instant messaging in Outlook Web App, see [Integrating Microsoft Lync Server 2013 and Microsoft Outlook Web App 2013](https://go.microsoft.com/fwlink/p/?linkid=280418).

If you receive an alert that indicates that the **OWA.Protocol.DEP** health set is unhealthy, this indicates an issue that may prevent instant messaging from working correctly in Outlook Web App.

## Explanation

The **OWA.Protocol.DEP** service is monitored using the following probes and monitors.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Probe</th>
<th>Health Set</th>
<th>Associated Monitors</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>none (notification or check)</p></td>
<td><p>OWA.Protocol.DEP</p></td>
<td><p>OwaIMInitializationFailedMonitor</p></td>
</tr>
<tr class="even">
<td><p>none (notification or check)</p></td>
<td><p>OWA.Protocol.DEP</p></td>
<td><p>WacDiscoveryFailureEventMonitor</p></td>
</tr>
</tbody>
</table>

For more information about probes and monitors, see [Server health and performance](https://docs.microsoft.com/exchange/server-health-and-performance-exchange-2013-help).

## User Action

It's possible that the service recovered after it issued the alert. Therefore, when you receive an alert that specifies that the **OWA.Protocol.DEP** health set is unhealthy, first verify that the issue still exists. If the issue does exist, perform the appropriate recovery actions outlined in the following sections.

## Recovery actions for error: "InstantMessageOCSProvider.InitializeEndpointManager. No registry setting for IM Provider."

This error indicates a required registry key is missing on the Mailbox server. This registry key should have been configured when the Microsoft Unified Communications Managed API (UCMA) 4.0 was installed on the server. The missing registry key is:

```console
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\MSExchange OWA\InstantMessaging
```

This key should contain an **ImplementationDLLPath** string that points to the `Microsoft.Rtc.Internal.Ucweb` DLL. The default location is `C:\Program Files\Microsoft UCMA 4.0\Runtime\SSP\Microsoft.Rtc.Internal.Ucweb.dll`.

To fix this issue, reinstall UCMA 4.0 or manually create the registry key. You can download UCMA 4.0 here: [Unified Communications Managed API 4.0 Runtime](https://go.microsoft.com/fwlink/p/?linkid=260990).

## Recovery actions for error: "InstantMessageOCSProvider.InitializeEndpointManager. IM provider not found."

This error indicates the `Microsoft.Rtc.Internal.Ucweb` DLL file is missing on the Mailbox server. This file should have been installed when UCMA 4.0 was installed on the server. The default location is `C:\Program Files\Microsoft UCMA 4.0\Runtime\SSP`.

To fix this issue, reinstall UCMA 4.0. For more information, see [Unified Communications Managed API 4.0 Runtime](https://go.microsoft.com/fwlink/p/?linkid=260990).

## Recovery actions for error: "Instant Messaging Server name is set to null or empty on web.config."

This error indicates the Lync 2013 server isn't defined in the Outlook Web App application configuration (web.config) file on the Mailbox server. This `web.config` file is located at `%ExchangeInstallPath%ClientAccess\Owa`, and should contain a key named **IMServerName** that specifies the FQDN of the Lync 2013 server. To fix this issue, follow these steps:

1. In a Command prompt window, open the Outlook Web App web.config file in Notepad by running the following command:

   ```powershell
   Notepad %ExchangeInstallPath%ClientAccess\Owa\web.config
   ```

2. Search for a key named **IMServerName**. If it's found, verify the FQDN of the Lync 2013 server. If the key is not found, add it by performing the following steps.

   1. Find the tag named **\<appSection\>**.

   2. In the **\<appSection\>** node, add the following line:

      ```powershell
      <add key="IMServerName" value="Lync Server FQDN" />
      ```

      For example:

      ```powershell
      <add key="IMServerName" value="lync01.contoso.com" />
      ```

   3. To apply the changes in Outlook Web App, run the following command:

      ```powershell
      %windir%\system32\inetsrv\appcmd.exe recycle apppool /apppool.name:"MSExchangeOWAAppPool"
      ```

## Recovery actions for error: "Instant Messaging Certificate Thumbprint is null or empty on web.config."

This error indicates the certificate that's used to integrate Lync 2013 and Outlook Web App isn't defined in the Outlook Web App application configuration (web.config) file on the Mailbox server. This `web.config` file is located at `%ExchangeInstallPath%ClientAccess\Owa`, and should contain a key named **IMCertificateThumbprint** that specifies the certificate's thumbprint (hash).

You can get the certificate's thumbprint value by using the **Get-ExchangeCertificate** cmdlet, or in the Exchange admin center (EAC) at **Servers** \> **Certificates**.

To fix this issue, follow these steps:

1. In a Command prompt window, open the Outlook Web App web.config file in Notepad by running the following command:

   ```powershell
   Notepad %ExchangeInstallPath%ClientAccess\Owa\web.config
   ```

2. Search for a key named **IMCertificateThumbprint**. If it's found, verify the thumbprint value is correct. If the key is not found, add it by performing the following steps:

   1. Find the tag named **\<appSection\>**.

   2. In the **\<appSection\>** node, add the following line:

      ```powershell
      <add key="IMCertificateThumbprint" value="thumbprint"/>
      ```

      For example:

      ```powershell
      <add key="IMCertificateThumbprint" value="35CB4C441D55506C88E59B7946B567A4F45B3B5C" />
      ```

   3. To apply the changes in Outlook Web App, run the following command:

      ```powershell
      %windir%\system32\inetsrv\appcmd.exe recycle apppool /apppool.name:"MSExchangeOWAAppPool"
      ```

## Recovery actions for error: "IM Certificate with thumbprint \&lt;value\&gt; could not be found."

This error indicates the certificate that's used to integrate Lync 2013 and Outlook Web App isn't found on the Mailbox server. This certificate must be installed on the Mailbox server and the Lync 2013 server, and must be trusted by both servers. For more information about the certificate requirements, see the Enabling Instant Messaging on Outlook Web App section in [Integrating Microsoft Lync Server 2013 and Microsoft Outlook Web App 2013](https://go.microsoft.com/fwlink/p/?linkid=280418).

You can match he thumbprint value in the error to the certificate by using the **Get-ExchangeCertificate** cmdlet, or in the Exchange admin center (EAC) at **Servers** \> **Certificates**.

## Recovery actions for error: "IM Certificate has expired."

This error indicates the certificate that's used to integrate Lync 2013 and Outlook Web App has expired. To resolve this error, you need to renew the certificate.

You can match he thumbprint value in the error to the certificate by using the **Get-ExchangeCertificate** cmdlet, or in the Exchange admin center (EAC) at **Servers** \> **Certificates**.

## Recovery actions for error: "IM Certificate has not become valid yet."

This error indicates the certificate that's used to integrate Lync 2013 and Outlook Web App has invalid dates. To resolve this error, you need to configure a new certificate, and you need to add the new thumbprint value in the **IMCertificateThumbprint** key in `%ExchangeInstallPath%ClientAccess\Owa\web.config` . For more information about the certificate requirements, see the Enabling Instant Messaging on Outlook Web App section in [Integrating Microsoft Lync Server 2013 and Microsoft Outlook Web App 2013](https://go.microsoft.com/fwlink/p/?linkid=280418).

## Recovery actions for error: "IM Certificate does not have a private key."

This error indicates the certificate that's used to integrate Lync 2013 and Outlook Web App does not have a private key. To resolve this error, you need to configure a new certificate that has a private key, and you need to add the new thumbprint value in the **IMCertificateThumbprint** key in `%ExchangeInstallPath%ClientAccess\Owa\web.config` . For more information about the certificate requirements, see the Enabling Instant Messaging on Outlook Web App section in [Integrating Microsoft Lync Server 2013 and Microsoft Outlook Web App 2013](https://go.microsoft.com/fwlink/p/?linkid=280418).
