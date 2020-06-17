---
title: 'MAPI over HTTP: Exchange 2013 Help'
TOCTitle: MAPI over HTTP
ms:assetid: 4663b5db-5b30-4a5a-a302-be6fef7fe5da
ms:mtpsurl: https://technet.microsoft.com/library/Dn635177(v=EXCHG.150)
ms:contentKeyID: 61218727
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# MAPI over HTTP

_**Applies to:** Exchange Server 2013_

Messaging Application Programming Interface (MAPI) over HTTP is a new transport protocol implemented in Microsoft Exchange Server 2013 Service Pack 1 (SP1). MAPI over HTTP improves the reliability and stability of the Outlook and Exchange connections by moving the transport layer to the industry-standard HTTP model. This allows a higher level of visibility of transport errors and enhanced recoverability. Additional functionality includes support for an explicit pause-and-resume function. This enables supported clients to change networks or resume from hibernation while maintaining the same server context.

Implementing MAPI over HTTP does not mean that it is the only protocol that can be used for Outlook to access Exchange. Outlook clients that are not MAPI over HTTP capable can still use Outlook Anywhere (RPC over HTTP) to access Exchange through a MAPI-enabled Client Access server.

## Benefits of MAPI over HTTP

MAPI over HTTP offers the following benefits to clients that support it:

- Enables future innovation in authentication by using an HTTP based protocol.

- Provides faster reconnection times after a communications break because only TCP connections (not RPC connections) need to be rebuilt. Examples of a communication break include:

  - Device hibernation

  - Changing from a wired network to a wireless or cellular network

- Offers a session context that is not dependent on the connection. The server maintains the session context for a configurable period of time, even if the user changes networks.

## Deploy MAPI over HTTP

Consider the following requirements to enable MAPI over HTTP.

- **Supportability**: Verify that your intended configuration versions are supported.

- **Prerequisites**: Verify that your environment has been upgraded and prepared for MAPI over HTTP.

- **Configuration**: Configure the virtual directories, and enable MAPI for your organization.

## Supportability

Use the following matrix to verify that your clients and servers support MAPI over HTTP.

<table>
<colgroup>
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
<col style="width: 20%" />
</colgroup>
<thead>
<tr class="header">
<th>Product</th>
<th>Exchange 2013 SP1</th>
<th>Exchange 2013 RTM</th>
<th>Exchange 2010 SP3</th>
<th>Exchange 2007 SP3</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Outlook 2013 SP1</p></td>
<td><ul>
<li><p>MAPI over HTTP</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><p>Outlook Anywhere</p></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook 2013 RTM</p></td>
<td><p>Outlook Anywhere</p></td>
<td><p>Outlook Anywhere</p></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outlook 2010 SP2 and updates KB2956191 and KB2965295 (April 14, 2015)</p></td>
<td><ul>
<li><p>MAPI over HTTP<span></span></p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><p>Outlook Anywhere</p></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Outlook 2010 SP2 and earlier</p></td>
<td><p>Outlook Anywhere</p></td>
<td><p>Outlook Anywhere</p></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outlook 2007</p></td>
<td><p>Outlook Anywhere</p></td>
<td><p>Outlook Anywhere</p></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
<td><ul>
<li><p>RPC</p></li>
<li><p>Outlook Anywhere</p></li>
</ul></td>
</tr>
</tbody>
</table>

## Prerequisites

Complete the following steps to prepare the clients and servers to support MAPI over HTTP.

1. Upgrade Outlook clients to Outlook 2013 SP1 or Outlook 2010 SP2 and updates KB2956191 and KB2965295 (April 14, 2015).

2. Upgrade Client Access and Mailbox servers to the latest Exchange 2013 cumulative update (CU). For information about how to upgrade, see [Upgrade Exchange 2013 to the latest cumulative update or service pack](upgrade-exchange-2013-to-the-latest-cumulative-update-or-service-pack-exchange-2013-help.md).

   > [!NOTE]
   > All Client Access servers must be upgraded to the latest Exchange 2013 CU, or the immediately previous CU. Otherwise, Outlook can fail to connect to mailboxes.<BR>Failure to upgrade the all the Mailbox servers in a Database Availability Group (DAG) can result in email delays and a client requirement to restart Outlook in case of a database failover.

3. On all Exchange 2013 servers, install the Microsoft .NET Framework version supported by the CU running on your Exchange server. For more information see [Exchange Server Supportability Matrix](exchange-server-supportability-matrix-exchange-2013-help.md) and [Installing the .NET Framework](https://go.microsoft.com/fwlink/p/?linkid=518380).

## Configuration

Complete the following steps to configure MAPI over HTTP for your organization.

1. **Virtual directory configuration**: By default, Exchange 2013 SP1 creates a virtual directory for MAPI over HTTP. You use the **Set-MapiVirtualDirectory** cmdlet to configure the virtual directory. You must configure an internal URL, an external URL, or both. For more information see, [Set-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-MapiVirtualDirectory).

   For example, to configure the default MAPI virtual directory on the local Exchange server by setting the internal URL value to `https://contoso.com/mapi`, and the authentication method to `Negotiate`, run the following command:

   ```powershell
   Set-MapiVirtualDirectory -Identity "Contoso\mapi (Default Web Site)" -InternalUrl https://Contoso.com/mapi -IISAuthenticationMethods Negotiate
   ```

2. **Certificate configuration**: The digital certificate used by your Exchange environment must include the same *InternalURL* and *ExternalURL* values that are defined on the MAPI virtual directory. For more information on Exchange 2013 certificate management, see [Digital certificates and SSL](digital-certificates-and-ssl-exchange-2013-help.md). Make sure the Exchange certificate is trusted on the Outlook client workstation and that there are no certificate errors, especially when you access the URLs configured on the MAPI virtual directory.

3. **Update server rules**: Verify that your load balancers, reverse proxies, and firewalls are configured to allow access to the MAPI over HTTP virtual directory.

4. **Enable MAPI over HTTP in your Exchange Organization**

   Run the following command:

   ```powershell
   Set-OrganizationConfig -MapiHttpEnabled $true
   ```

## Test MAPI over HTTP connections

You can test the end-to-end MAPI over HTTP connection by using the **Test-OutlookConnectivity** cmdlet. To use the **Test-OutlookConnectivity** cmdlet, the Microsoft Exchange Health Manager (MSExchangeHM) service must be started.

The following example tests the MAPI over HTTP connection from the Exchange server named ContosoMail.

```powershell
Test-OutlookConnectivity -RunFromServerId ContosoMail -ProbeIdentity OutlookMapiHttpSelfTestProbe
```

A successful test returns output that's similar to the following example:

```powershell
MonitorIdentity                                        StartTime              EndTime                Result      Error     Exception
---------------                                        ---------              -------                ------      -----     ---------
OutlookMapiHttp.Protocol\OutlookMapiHttpSelfTestProbe  2/14/2014 7:15:00 AM   2/14/2014 7:15:10 AM   Succeeded
```

For more information, see [Test-OutlookConnectivity](https://docs.microsoft.com/powershell/module/exchange/Test-OutlookConnectivity).

Logs for MAPI over HTTP activity are at the following locations:

- %ExchangeInstallPath%Logging\\MAPI Address Book Service\\

- %ExchangeInstallPath%Logging\\MAPI Client Access\\

- %ExchangeInstallPath%Logging\\HttpProxy\\Mapi\\

## Manage MAPI over HTTP

You can manage the configuration of MAPI over HTTP by using the following cmdlets:

- [Set-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-MapiVirtualDirectory)

- [Get-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Get-MapiVirtualDirectory)

- [New-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/New-MapiVirtualDirectory)

- [Remove-MapiVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Remove-MapiVirtualDirectory)
