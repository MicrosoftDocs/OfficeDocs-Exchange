---
title: 'Receive connector authentication mechanisms: Exchange 2013 Help'
TOCTitle: Receive connector authentication mechanisms
ms:assetid: 926424e1-83e3-4c4b-b2dd-bf814d81e877
ms:mtpsurl: https://technet.microsoft.com/library/JJ657472(v=EXCHG.150)
ms:contentKeyID: 49289352
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Receive connector authentication mechanisms

_**Applies to:** Exchange Server 2013_

The Receive connector authentication mechanisms are the following:

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Authentication mechanism</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>None</p></td>
<td><p>No authentication.</p></td>
</tr>
<tr class="even">
<td><p>TLS</p></td>
<td><p>Advertise STARTTLS. Requires availability of a server certificate to offer TLS.</p></td>
</tr>
<tr class="odd">
<td><p>Integrated</p></td>
<td><p>NTLM and Kerberos (Integrated Windows authentication).</p></td>
</tr>
<tr class="even">
<td><p>BasicAuth</p></td>
<td><p>Basic authentication. Requires an authenticated logon.</p></td>
</tr>
<tr class="odd">
<td><p>BasicAuthRequireTLS</p></td>
<td><p>Basic authentication over TLS. Requires a server certificate.</p></td>
</tr>
<tr class="even">
<td><p>ExchangeServer</p></td>
<td><p>Exchange Server authentication (Generic Security Services application programming interface (GSSAPI) and Mutual GSSAPI).</p></td>
</tr>
<tr class="odd">
<td><p>ExternalAuthoritative</p></td>
<td><p>The connection is considered externally secured by using a security mechanism that's external to Exchange. The connection may be an Internet Protocol security (IPsec) association or a virtual private network (VPN). Alternatively, the servers may reside in a trusted physically controlled network. The <code>ExternalAuthoritative</code> authentication method requires the <code>ExchangeServers</code> permission group. This combination of authentication method and security group permits the resolution of anonymous sender email addresses for messages that are received through this connector.</p></td>
</tr>
</tbody>
</table>

For more information about Receive Connector authentication mechanisms, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).
