---
title: 'Network ports for clients and mail flow in Exchange 2013: Exchange 2013 Help'
TOCTitle: Network ports for clients and mail flow in Exchange 2013
ms:assetid: fec09455-e99e-42eb-8b32-1ddc08d9a19e
ms:mtpsurl: https://technet.microsoft.com/library/Bb331973(v=EXCHG.150)
ms:contentKeyID: 64916714
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Network ports for clients and mail flow in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic provides information about the network ports that are used by Microsoft Exchange Server 2013 for communication with email clients, Internet mail servers, and other services that are external to your local Exchange organization. Before we get into that, understand the following ground rules:

- We do not support restricting or altering network traffic between internal Exchange servers, between internal Exchange servers and internal Lync or Skype for Business servers, or between internal Exchange servers and internal Active Directory domain controllers in any and all types of topologies. If you have firewalls or network devices that could potentially restrict or alter this kind of network traffic, you need to configure rules that allow free and unrestricted communication between these servers (rules that allow incoming and outgoing network traffic on any port (including random RPC ports) and any protocol that never alter bits on the wire).

- Edge Transport servers are almost always located in a perimeter network, so it's expected that you'll restrict network traffic between the Edge Transport server and the Internet, and between the Edge Transport server and your internal Exchange organization. These network ports are described in this topic.

- It's expected that you'll restrict network traffic between external clients and services and your internal Exchange organization. It's also OK if you decide to restrict network traffic between internal clients and internal Exchange servers. These network ports are described in this topic.

## Network ports required for clients and services

The network ports that are required for email clients to access mailboxes and other services in the Exchange organization are described in the following diagram and table.

**Notes:**

- The destination for these clients and services is a Client Access server. This could be a standalone Client Access server or a Client Access server and Mailbox server installed on the same computer.

- Although the diagram shows clients and services from the Internet, the concepts are the same for internal clients (for example, clients in an accounts forest accessing Exchange servers in a resource forest). Similarly, the table doesn't have a source column because the source could be any location that's external to the Exchange organization (for example, the Internet or an accounts forest).

- Edge Transport servers have no involvement in the network traffic that's associated with these clients and services.

![Network ports required for clients and services](images/Bb331973.f5ba3439-f001-43c8-848e-0e3fd0fce931(EXCHG.150).png)

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Purpose</th>
<th>Ports</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Encrypted web connections are used by the following clients and services:</p>
<ul>
<li><p>Autodiscover service</p></li>
<li><p>Exchange ActiveSync</p></li>
<li><p>Exchange Web Services (EWS)</p></li>
<li><p>Offline address book distribution</p></li>
<li><p>Outlook Anywhere (RPC over HTTP)</p></li>
<li><p>Outlook MAPI over HTTP</p></li>
<li><p>Outlook Web App</p></li>
</ul></td>
<td><p>443/TCP (HTTPS)</p></td>
<td><p>For more information about these clients and services, see the following topics:</p>
<ul>
<li><p><a href="autodiscover-service-for-exchange-2013.md">Autodiscover service</a></p></li>
<li><p><a href="exchange-activesync-exchange-2013-help.md">Exchange ActiveSync</a></p></li>
<li><p><a href="https://docs.microsoft.com/exchange/client-developer/web-service-reference/ews-reference-for-exchange">EWS reference for Exchange</a></p></li>
<li><p><a href="offline-address-books-exchange-2013-help.md">Offline address books</a></p></li>
<li><p><a href="outlook-anywhere-exchange-2013-help.md">Outlook Anywhere</a></p></li>
<li><p><a href="mapi-over-http-exchange-2013-help.md">MAPI over HTTP</a></p></li>
<li><p><a href="what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md">What's new for Outlook Web App in Exchange 2013</a></p></li>
</ul></td>
</tr>
<tr class="even">
<td><p>Unencrypted web connections are used by the following clients and services:</p>
<ul>
<li><p>Internet calendar publishing</p></li>
<li><p>Outlook Web App (redirect to 443/TCP)</p></li>
<li><p>Autodiscover (fallback when 443/TCP isn't available)</p></li>
</ul></td>
<td><p>80/TCP (HTTP)</p></td>
<td><p>Whenever possible, we recommend using encrypted web connections on 443/TCP to help protect data and credentials. However, you may find that some services must be configured to use unencrypted web connections on 80/TCP to the Client Access server.</p>
<p>For more information about these clients and services, see the following topics:</p>
<ul>
<li><p><a href="enable-internet-calendar-publishing-exchange-2013-help.md">Enable Internet calendar publishing</a></p></li>
<li><p><a href="what-s-new-for-outlook-web-app-in-exchange-2013-exchange-2013-help.md">What's new for Outlook Web App in Exchange 2013</a></p></li>
<li><p><a href="autodiscover-service-for-exchange-2013.md">Autodiscover service</a></p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>IMAP4 clients</p></td>
<td><p>143/TCP (IMAP), 993/TCP (secure IMAP)</p></td>
<td><p>IMAP4 is disabled by default. For more information, see <a href="pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md">POP3 and IMAP4 in Exchange Server 2013</a>.</p>
<p>The IMAP4 service on the Client Access server proxies connections to the IMAP4 Backend service on a Mailbox server.</p></td>
</tr>
<tr class="even">
<td><p>POP3 clients</p></td>
<td><p>110/TCP (POP3), 995/TCP (secure POP3)</p></td>
<td><p>POP3 is disabled by default. For more information, see <a href="pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md">POP3 and IMAP4 in Exchange Server 2013</a>.</p>
<p>The POP3 service on the Client Access server proxies connections to the POP3 Backend service on a Mailbox server.</p></td>
</tr>
<tr class="odd">
<td><p>SMTP clients (authenticated)</p></td>
<td><p>587/TCP (authenticated SMTP)</p></td>
<td><p>The default Received connector named &quot;Client Frontend <em>&lt;Server name&gt;</em>&quot; listens for authenticated SMTP client submissions on port 587 on the Client Access server.</p>
<p><strong>Note:</strong></p>
<p>If you have mail clients that can submit authenticated SMTP mail only on port 25, you can modify the network adapter bindings value of this Receive connector to also listen for authenticated SMTP mail submissions on port 25.</p></td>
</tr>
</tbody>
</table>

## Network ports required for mail flow

How mail is delivered to and from your Exchange organization depends on your Exchange topology. The most important factor is whether you have a subscribed Edge Transport server deployed in your perimeter network.

## Network ports required for mail flow (no Edge Transport servers)

The network ports that are required for mail flow in an Exchange organization that has only Client Access servers and Mailbox servers are described in the following diagram and table. Although the diagram shows separate Mailbox and Client Access servers, the concepts are the same whether the Client Access server and the Mailbox server are installed on the same computer or on separate computers.

![Network ports required for mail flow (no Edge Transport servers)](images/Bb331973.af54dfd3-fe6b-4b6e-bb8e-b00df94a0be0(EXCHG.150).png "Network ports required for mail flow (no Edge Transport servers)")

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
<th>Purpose</th>
<th>Ports</th>
<th>Source</th>
<th>Destination</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Inbound mail</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Internet (any)</p></td>
<td><p>Client Access server</p></td>
<td><p>The default Receive connector named &quot;Default Frontend <em>&lt;Client Access server name&gt;</em>&quot; on the Client Access server listens for anonymous inbound SMTP mail on port 25.</p>
<p>Mail is relayed from the Client Access server to a Mailbox server using the implicit and invisible intra-organization Send connector that automatically routes mail between Exchange servers in the same organization.</p></td>
</tr>
<tr class="even">
<td><p>Outbound mail</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Mailbox server</p></td>
<td><p>Internet (any)</p></td>
<td><p>By default, Exchange doesn't create any Send connectors that allow you to send mail to the Internet. You have to create Send connectors manually. For more information, see <a href="send-connectors-exchange-2013-help.md">Send connectors</a>.</p></td>
</tr>
<tr class="odd">
<td><p>Outbound mail (if routed through the Client Access server)</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Client Access server</p></td>
<td><p>Internet (any)</p></td>
<td><p>Outbound mail is routed through a Client Access server only when a Send connector is configured with <strong>Proxy through Client Access server</strong> in the Exchange admin center or <code>-FrontEndProxyEnabled $true</code> in the Exchange Management Shell.</p>
<p>In this case, the default Receive connector named &quot;Outbound Proxy Frontend <em>&lt;Client Access server name&gt;</em>&quot; on the Client Access server listens for outbound mail from the Mailbox server. For more information, see <a href="create-a-send-connector-for-email-sent-to-the-internet-exchange-2013-help.md">Create a Send connector for email sent to the Internet</a>.</p></td>
</tr>
<tr class="even">
<td><p>DNS for name resolution of the next mail hop (not pictured)</p></td>
<td><p>53/UDP,53/TCP (DNS)</p></td>
<td><p>Internet-facing Exchange server (Client Access server or Mailbox server)</p></td>
<td><p>DNS server</p></td>
<td><p>See the Name resolution section.</p></td>
</tr>
</tbody>
</table>

## Network ports required for mail flow with Edge Transport servers

A subscribed Edge Transport server that's installed in your perimeter network basically eliminates SMTP mail flow through the Client Access server. Specifically:

- Outbound mail from the Exchange organization never flows through a Client Access server. Mail always flows from a Mailbox server in the subscribed Active Directory site to the Edge Transport server (regardless of the version of Exchange on the Edge Transport server).

- Inbound mail never flows through a standalone Client Access server. Mail flows from the Edge Transport server to a Mailbox server in the subscribed Active Directory site. If the Mailbox server and the Client Access server are installed on the same computer, mail from an Exchange 2013 Edge Transport server first arrives on the computer at the Front End Transport service (the Client Access server role) before it flows to the Transport service (the Mailbox server role). Exchange 2007 or Exchange 2010 Edge Transport servers always deliver mail directly to the Transport service even when the Mailbox server and the Client Access server are installed on the same computer.

For more information, see [Mail flow](mail-flow-exchange-2013-help.md).

The network ports that are required for mail flow in Exchange organizations that have Edge Transport servers are described in the following diagram and table. Unless otherwise noted, the concepts are the same whether the Client Access server and the Mailbox server are installed on the same computer or on separate computers.

![Network ports required for mail flow with Edge Transport servers](images/Bb331973.110c79b3-dbd9-4cb5-bba1-02048363ee1c(EXCHG.150).png "Network ports required for mail flow with Edge Transport servers")

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
<th>Purpose</th>
<th>Ports</th>
<th>Source</th>
<th>Destination</th>
<th>Comments</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Inbound mail - Internet to Edge Transport server</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Internet (any)</p></td>
<td><p>Edge Transport server</p></td>
<td><p>The default Receive connector named &quot;Default internal Receive connector <em>&lt;Edge Transport server name&gt;</em>&quot; on the Edge Transport server listens for anonymous SMTP mail on port 25.</p></td>
</tr>
<tr class="even">
<td><p>Inbound mail - Edge Transport server to internal Exchange organization</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Edge Transport server</p></td>
<td><p>Mailbox servers in the subscribed Active Directory site</p></td>
<td><p>The default Send connector named &quot;EdgeSync - Inbound to <em>&lt;Active Directory site name&gt;</em>&quot; relays inbound mail on port 25 to any Mailbox server in the subscribed Active Directory site. For more information, see the &quot;Send connectors created during the Edge Subscription process&quot; section in the topic, <a href="edge-subscriptions-exchange-2013-help.md">Edge Subscriptions</a>.</p>
<p>The service that actually receives mail depends on whether the Mailbox server and Client Access server are installed on the same computer or on separate computers.</p>
<ul>
<li><p><strong>Standalone Mailbox server</strong>   The default Receive connector named &quot;Default <em>&lt;Mailbox server name&gt;</em>&quot; listens for inbound mail (including mail from Edge Transport servers) on port 25.</p></li>
<li><p><strong>Mailbox server and Client Access server installed on the same computer</strong>   The default Receive connector named &quot;Default Frontend <em>&lt;Server name&gt;</em>&quot; in the Front End Transport service (the Client Access server role) listens for inbound mail (including mail from Exchange 2013 Edge Transport servers) on port 25.</p></li>
</ul></td>
</tr>
<tr class="odd">
<td><p>Outbound mail - Internal Exchange organization to Edge Transport server</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Mailbox servers in the subscribed Active Directory site</p></td>
<td><p>Edge Transport servers</p></td>
<td><p>Outbound mail always bypasses the Client Access server.</p>
<p>Mail is relayed from any Mailbox server in the subscribed Active Directory site to an Edge Transport server using the implicit and invisible intra-organization Send connector that automatically routes mail between Exchange servers in the same organization.</p>
<p>The default Receive connector named &quot;Default internal Receive connector <em>&lt;Edge Transport server name&gt;</em>&quot; on the Edge Transport server listens for SMTP mail on port 25 from any Mailbox server in the subscribed Active Directory site.</p></td>
</tr>
<tr class="even">
<td><p>Outbound mail - Edge Transport server to Internet</p></td>
<td><p>25/TCP (SMTP)</p></td>
<td><p>Edge Transport server</p></td>
<td><p>Internet (any)</p></td>
<td><p>The default Send connector named &quot;EdgeSync - <em>&lt;Active Directory site name&gt;</em> to Internet&quot; relays outbound mail on port 25 from the Edge Transport server to the Internet.</p></td>
</tr>
<tr class="odd">
<td><p>EdgeSync synchronization</p></td>
<td><p>50636/TCP (secure LDAP)</p></td>
<td><p>Mailbox servers in the subscribed Active Directory site that participate in EdgeSync synchronization</p></td>
<td><p>Edge Transport servers</p></td>
<td><p>When the Edge Transport server is subscribed to the Active Directory site, all Mailbox servers that exist in the site at the time participate in EdgeSync synchronization. However, any Mailbox servers that you add later don't automatically participate in EdgeSync synchronization.</p></td>
</tr>
<tr class="even">
<td><p>DNS for name resolution of the next mail hop (not pictured)</p></td>
<td><p>53/UDP,53/TCP (DNS)</p></td>
<td><p>Edge Transport server</p></td>
<td><p>DNS server</p></td>
<td><p>See the Name resolution section.</p></td>
</tr>
<tr class="odd">
<td><p>Proxy server definition for sender reputation (not pictured)</p></td>
<td><p>user defined</p></td>
<td><p>Edge Transport servers</p></td>
<td><p>Internet</p></td>
<td><p>Sender reputation (the Protocol Analysis agent) analyzes inbound message paths in an effort to reduce spam. If your organization uses a proxy server to control access to the Internet, you need to define details about the proxy server so that sender reputation can work properly (in particular, open proxy detection and sender blocking). You use the <em>ProxyServerName</em>, <em>ProxyServerPort</em> and <em>ProxyServerType</em> parameters on the <strong>Set-SenderReputationConfig</strong> cmdlet to define your organization's proxy server so sender reputation can successfully connect to the Internet. For more information, see <a href="manage-sender-reputation-exchange-2013-help.md">Manage sender reputation</a>.</p></td>
</tr>
</tbody>
</table>

## Name resolution

DNS resolution of the next mail hop is a fundamental part of mail flow in any Exchange organization. Exchange servers that are responsible for receiving inbound mail or delivering outbound mail must be able to resolve both internal and external host names for proper mail routing. And all internal Exchange servers must be able to resolve internal host names for proper mail routing. There are many different ways to design a DNS infrastructure, but the important result is to ensure name resolution for the next hop is working properly for all of your Exchange servers.

## Network ports required for hybrid deployments

The network ports that are required for an organization that uses both Exchange 2013 and Microsoft 365 or Office 365 are covered in the "Hybrid deployment protocols, port and endpoints" section in [Hybrid deployment prerequisites](https://docs.microsoft.com/exchange/hybrid-deployment-prerequisites).

## Network ports required for Unified Messaging

The network ports that are required for Unified Messaging are covered in the following topics:

- [UM protocols, ports, and services](um-protocols-ports-and-services-exchange-2013-help.md)

- [Exchange Server 2013 SP1 Architecture Poster](https://www.microsoft.com/download/details.aspx?id=42542)
