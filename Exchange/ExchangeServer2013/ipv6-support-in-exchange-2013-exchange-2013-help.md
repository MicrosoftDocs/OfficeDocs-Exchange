---
title: 'IPv6 support in Exchange 2013: Exchange 2013 Help'
TOCTitle: IPv6 support in Exchange 2013
ms:assetid: 33543023-eb9a-4102-b990-84a818a52814
ms:mtpsurl: https://technet.microsoft.com/library/Gg144561(v=EXCHG.150)
ms:contentKeyID: 48384951
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# IPv6 support in Exchange 2013

_**Applies to:** Exchange Server 2013_

Internet Protocol version 6 (IPv6) is the most recent version of the Internet Protocol (IP). IPv6 is intended to correct many of the shortcomings of IPv4, which was the previous version of the IP.

In Microsoft Exchange Server 2013, IPv6 is supported only when IPv4 is also installed and enabled. If Exchange 2013 is deployed in this configuration, and the network supports IPv4 and IPv6, all Exchange servers can send data to and receive data from devices, servers, and clients that use IPv6 addresses.

This topic discusses IPv6 addressing in Exchange 2013. For additional background information about IPv6, see [Internet Protocol Version 6 (IPv6) Overview](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379473(v=ws.10)).

## IPv6 support in Exchange 2013 components

The following table describes the components in Exchange 2013 that are affected by IPv6.

### Exchange 2013 features and IPv6

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Feature</th>
<th>IPv6 supported</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>IP Allow list and IP Block list in the Connection Filtering agent</p></td>
<td><p>Yes</p></td>
<td><p></p></td>
</tr>
<tr class="even">
<td><p>IP Allow List providers and IP Block List providers in the Connection Filtering agent.</p></td>
<td><p>No</p></td>
<td><p>Currently, there is no widely accepted industry standard protocol for looking up IPv6 addresses. Most IP Block List providers don't support IPv6 addresses. If you allow anonymous connections from unknown IPv6 addresses on a Receive connector, you increase the risk that spammers will bypass IP Block List providers and successfully deliver spam into your organization.</p></td>
</tr>
<tr class="odd">
<td><p>Sender reputation in the Protocol Analysis agent</p></td>
<td><p>No</p></td>
<td><p>The Protocol Analysis agent doesn't compute the sender reputation level (SRL) for messages that originate from IPv6 senders. For more information about sender reputation, see <a href="sender-reputation-and-the-protocol-analysis-agent-exchange-2013-help.md">Sender reputation and the Protocol Analysis agent</a>.</p></td>
</tr>
<tr class="even">
<td><p>Sender ID</p></td>
<td><p>Yes</p></td>
<td><p>For more information, see <a href="sender-id-exchange-2013-help.md">Sender ID</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Receive connectors</p></td>
<td><p>Yes</p></td>
<td><p>IPv6 addresses are accepted for the following components:</p>
<ul>
<li><p>Local IP address bindings</p></li>
<li><p>Remote IP addresses</p></li>
<li><p>IP address ranges</p></li>
</ul>
<p>We strongly recommend against configuring Receive connectors to accept anonymous connections from unknown IPv6 addresses. If your organization must receive mail from senders who use IPv6 addresses, create a dedicated Receive connector that restricts the remote IP addresses to the specific IPv6 addresses that those senders use.</p>
<p>For more information, see <a href="receive-connectors-exchange-2013-help.md">Receive connectors</a>.</p></td>
</tr>
<tr class="even">
<td><p>Send connectors</p></td>
<td><p>Yes</p></td>
<td><p>IPv6 addresses are accepted for the following components:</p>
<ul>
<li><p>Smart host IP addresses</p></li>
<li><p>The <em>SourceIPAddress</em> parameter for Send connectors configured on Edge Transport servers</p></li>
</ul>

> [!NOTE]
> If you want to specify an IPv6 address for the <EM>SourceIPAddress</EM> parameter, make sure that the appropriate DNS AAAA and mail exchange (MX) records are configured correctly. This helps ensure message delivery if a remote messaging server tries any kind of reverse lookup test on the specified IPv6 address.

<p>For more information, see <a href="send-connectors-exchange-2013-help.md">Send connectors</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Incoming message rate limits</p></td>
<td><p>Partial</p></td>
<td><p>Incoming message rate limits that you can set on a Receive connector, such as the <em>MaxInboundConnectionPercentagePerSource</em> parameter, the <em>MaxInboundConnectionPerSource</em> parameter, and the <em>TarpitInterval</em> parameter, only apply to a global IPv6 address. Link local IPv6 addresses and site local IPv6 addresses aren't affected by any specified incoming message rate limits.</p></td>
</tr>
<tr class="even">
<td><p>Unified Messaging</p></td>
<td><p>Yes</p></td>
<td><p>For more information, see <a href="ipv6-support-in-unified-messaging-exchange-2013-help.md">IPv6 support in Unified Messaging</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Database availability group (DAG) member</p></td>
<td><p>Yes</p></td>
<td><p>Static IPv6 addresses are supported by Windows Server and the Cluster service. However, using static IPv6 addresses goes against best practices. Exchange 2013 doesn't support the configuration of static IPv6 addresses during setup.</p>
<p>Failover clusters support Intra-site Automatic Tunnel Addressing Protocol (ISATAP). They support only IPv6 addresses that allow for dynamic registration in DNS. Link local addresses can't be used in a cluster.</p>
<p>For more information about DAG network requirements, see the &quot;Network requirements&quot; section in <a href="planning-for-high-availability-and-site-resilience-exchange-2013-help.md">Planning for high availability and site resilience</a>.</p></td>
</tr>
</tbody>
</table>

## Enable or disable protocols in the operating system

Exchange 2013 servers fully support IPv6 networks. Therefore, even if you aren't using IPv6, you don't need to disable IPv6 on your Exchange servers.

IPv6 support in Exchange 2013 requires IPv4 to be installed and enabled on all Exchange 2013 servers. Uninstalling IPv4 from your Exchange 2013 servers isn't supported.

To learn more about IPv6 support in Microsoft Windows, see [Internet Protocol Version 6 (IPv6) Overview](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh831730(v=ws.11)).

## IPv6 address basics

An IPv6 address is 128-bits long. The address is described by using colon-hexadecimal notation. Colon-hexadecimal notation describes the 128-bit address by using eight 16-bit, 4-digit hexadecimal numbers separated by the colon character (:). An example of an IPv6 address in colon-hexadecimal notation is 2001:0DB8:0000:0000:02AA:00FF:C0A8:640A.

You can express an IPv6 address by using the following methods:

- **Suppress leading zeros**: You can omit the leading zeros in any of the eight 4-digit hexadecimal numbers in an IPv6 address.

- **Double-colon compression**: You can use two colons (::) to represent contiguous 16-bit hexadecimal digits that contain all zeros. These all-zero digits may exist at the beginning, middle, or end of the IPv6 address. You can only use double-colon compression one time in an IPv6 address.

- **Trailing dotted-decimal notation**: You may express the last 32 bits at the end of an IPv6 address in dotted-decimal notation by separating the 8-bit digits with a period (.). Trailing dotted-decimal notation is frequently used with IPv4-compatible addresses.

The following table provides examples of the IPv6 address notation and the equivalent IPv6 address syntax.

### IPv6 address notation and syntax

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>IPv6 address notation</th>
<th>IPv6 address syntax</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Full IPv6 address</p></td>
<td><p>2001:0DB8:0000:0000:02AA:00FF:C0A8:640A</p></td>
</tr>
<tr class="even">
<td><p>IPv6 address that uses suppressed leading zeros</p></td>
<td><p>2001:DB8:0:0:2AA:FF:C0A8:640A</p></td>
</tr>
<tr class="odd">
<td><p>IPv6 address that uses double-colon compression</p></td>
<td><p>2001:DB8::2AA:FF:C0A8:640A</p></td>
</tr>
<tr class="even">
<td><p>IPv6 address that uses trailing dotted-decimal notation</p></td>
<td><p>2001:DB8::2AA:FF:192.168.100.10</p></td>
</tr>
</tbody>
</table>

IPv6 addresses are categorized into the following types:

- **Unicast address**: A packet is delivered to one interface.

- **Multicast address**: A packet is delivered to multiple interfaces.

- **Anycast address**: A packet is delivered to the nearest of multiple interfaces. The distance between interfaces is defined by the routing cost.

IPv6 unicast addresses have the following possible scopes:

- **Link local**: The scope of the IPv6 address is the local subnet. IPv6 link local addresses are comparable to IPv4 link local addresses used in Automatic Private IP Addressing (APIPA).

- **Site local**: The scope of the IPv6 address is the local organization. Site local addresses were deprecated by RFC 3879 and replaced by unique local addresses as defined in RFC 4193. IPv6 site local addresses and IPv6 unique local addresses are comparable to IPv4 private IP addresses.

- **Global**: The scope of the IPv6 address is the whole world. IPv6 global addresses are comparable to IPv4 public IP addresses.

The following table provides a comparison of IPv4 elements and IPv6 elements.

### IPv4 vs. IPv6 elements

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Item</th>
<th>IPv4</th>
<th>IPv6</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Private IP address</p></td>
<td><p>10.0.0.0/8</p>
<p>172.16.0.0/12</p>
<p>192.168.0.0/16</p></td>
<td><p>FD00::/8</p></td>
</tr>
<tr class="even">
<td><p>Link local address</p></td>
<td><p>169.254.0.0/16</p></td>
<td><p>FE80::/64</p></td>
</tr>
<tr class="odd">
<td><p>Loopback address</p></td>
<td><p>127.0.0.1</p></td>
<td><p>::1</p></td>
</tr>
<tr class="even">
<td><p>Unspecified address</p></td>
<td><p>0.0.0.0</p></td>
<td><p>::</p></td>
</tr>
<tr class="odd">
<td><p>Address resolution</p></td>
<td><p>Address Resolution Protocol (ARP)</p></td>
<td><p>Neighbor Discovery (ND)</p></td>
</tr>
<tr class="even">
<td><p>Domain Name System (DNS) host name resolution</p></td>
<td><p>Address record (A record)</p></td>
<td><p>AAAA record or A6 record</p></td>
</tr>
</tbody>
</table>

For more information about IPv6 addressing, see [IPv6 Address Types](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc757359(v=ws.10)).

## Supported IPv6 Address Input Formats

The following types of IPv6 address input formats are supported in Exchange 2013:

- A single IPv6 address

- An IPv6 address range

- An IPv6 address together with a subnet mask

- An IPv6 address together with a subnet mask that uses Classless Interdomain Routing (CIDR) notation

The following table provides examples of the acceptable IPv6 address input formats in Exchange 2013.

### IPv6 address examples

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Type</th>
<th>Example of an IPv6 address</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Single address</p></td>
<td><p>2001:DB8::2AA:FF:C0A8:640A</p></td>
</tr>
<tr class="even">
<td><p>Address range</p></td>
<td><p>2001:DB8::2AA:FF:C0A8:640A-2001:DB8::2AA:FF:C0A8:6414</p></td>
</tr>
<tr class="odd">
<td><p>Address together with subnet mask</p></td>
<td><p>2001:DB8::2AA:FF:C0A8:640A(FFFF:FFFF:FFFF:FFFF::)</p></td>
</tr>
<tr class="even">
<td><p>Address together with subnet mask that uses CIDR notation</p></td>
<td><p>2001:DB8::2AA:FF:C0A8:640A/64</p></td>
</tr>
</tbody>
</table>

In Exchange 2013, the following input formats are supported:

- Suppression of leading zeros

- Double-colon compression

- Trailing dotted-decimal notation
