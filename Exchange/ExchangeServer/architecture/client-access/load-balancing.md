---
localization_priority: Normal
description: 'Summary: Learn about the ways load balancing in Exchange Server handles mail-enabled connections, resulting in improved availability and resiliency in your Exchange enterprise network.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f572c193-6f3a-400e-9085-a9d3e5e18c59
ms.reviewer: 
title: Load balancing in Exchange Server
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Load balancing in Exchange Server

Load balancing in Exchange 2016 and later build on the Microsoft high availability and network resiliency platform delivered in Exchange 2013. When this is combined with the availability of third-party load balancing solutions (both hardware and software), there are multiple options for implementing load balancing in your Exchange organization.

Exchange architecture changes introduced in Exchange 2013 brought about the Mailbox server and Client Access server roles. Compare this to Exchange 2010, where Client Access, Mailbox, Hub Transport, and Unified Messages ran on separate servers.

Using minimal server roles, Exchange 2016 and 2019 deliver:

- Simplified deployment with the Mailbox server running Client Access services and Edge Transport server roles.

- Mail flow managed in the transport pipeline, which is the collection of services, connections, queues, and components that route messages to the Transport service categoriser on the Mailbox server.

- High availability by deploying load balancers to distribute client traffic.

The HTTP protocol standard introduced with Exchange 2013 means that session affinity is no longer required in Exchange 2016 and Exchange 2019. Session affinity allows a persistent connection for messaging-enabled services so that a user doesn't have to reenter their username and password multiple times.

Previously, Exchange 2007 and Exchange 2010 supported RPC over HTTP for Outlook Anywhere. Exchange 2013 introduced MAPI over HTTP, although it wasn't enabled by default. It's now enabled by default in Exchange 2016 and Exchange 2019.

With the HTTP protocol in use, all native clients connect using HTTP and HTTPs in Exchange Server. This standard protocol removes the need for affinity, which was previously required to avoid a new prompting for user credentials whenever load balancing redirected the connection to a different server.

## Server roles in Exchange Server

The reduced number of server roles for Exchange 2016 and Exchange 2019 simplifies Exchange implementation and hardware requirements. The number of server roles in Exchange 2016 and 2019 shrinks from seven to two: the Mailbox server and the Edge Transport server. The Mailbox server role includes Client Access services, while the Edge Transport server provides secure mail flow in Exchange 2016 and Exchange 2019, just as it did in earlier versions of Exchange.

![Conceptual overview of the Exchange load balancing process](../../media/30bc24ea-a9df-42ca-82c6-54cd9f009003.png)

In Exchange 2013, the Client Access server role made sure that when a user attempted to access their mailbox, the server proxied the request back to the Mailbox server actively serving the user's mailbox. This meant that services such as Outlook on the web (previously known as Outlook Web App) were rendered for the user on the Mailbox itself, removing any need for affinity.

The same functionality remains in Exchange 2016 and Exchange 2019. If two Mailbox servers host different mailboxes, they can proxy traffic for each other when necessary. The Mailbox server that hosts the active copy of the mailbox serves the user accessing it, even if the user connects to a different Mailbox server.

Read more about the server role changes in Exchange Server in the topic, [Exchange Server architecture](../../architecture/architecture.md).

|**Server Role**|**Services**|
|:-----|:-----|
|Mailbox server|Uses EdgeSync to manage one-way replication of receipt and configuration info from Active Directory to the AD LDS instance on the Edge Transport server.  <br/> Copies only information needed to let Edge Transport perform antispam and enable end-to-end mail flow.|
|Edge Transport|Manages all inbound and outbound Internet mail flow using:  <br/> • mail relay  <br/> • smart hosting  <br/> • agents that provide additional antispam service  <br/> • agents that apply transport to control mail flow  <br/> Not a member of the Active Directory forest|

Although not required, the Edge Transport server sits in the perimeter network , just as in earlier Exchange versions, to provide secure inbound and outbound mail flow for your Exchange organization.

Read more about the transport service in the topic, [Understanding the Transport service on Edge Transport servers](../../mail-flow/mail-flow.md#understanding-the-transport-service-on-edge-transport-servers).

## Protocols in Exchange Server

Beginning with Exchange 2016, all native Exchange clients use the HTTP protocol to connect to a designated service, with HTTP cookies provided to the user at log in which are encrypted using the Client Access services SSL certificate. A logged in user can resume the session on a different Mailbox server running Client Access services without reauthenticating. Servers using the same SSL certificate can decrypt the client authentication cookie.

HTTP makes possible the use of service or application health checks in your Exchange network. Depending on your load balancer solution, you can implement health probes to check different components of your system.

The effect of HTTP-only access for clients is that load balancing is simpler, too. If you wanted, you could use DNS to load balance your Exchange traffic. You would simply provide the client with the IP address of every Mailbox server, and the HTTP client would handle the chores. If an Exchange server fails, the protocol attempts to connect to another server. However, there are drawbacks to load balancing to DNS, discussed in the following section *Load balancing options in Exchange Server*.

Read more about HTTP and Exchange Server in the topic [MAPI over HTTP in Exchange Server](../../clients/mapi-over-http/mapi-over-http.md).

## Load balancing options in Exchange Server

In the example shown here, multiple servers configured in a database availability group (DAG) host the Mailbox servers running Client Access services. This provides high availability with a small Exchange server footprint. The client connects to the load balancer rather than directly to the Exchange servers. There is no requirement for load balancer pairs, however we recommend deploying in clusters to improve network resilience.

![Client connections to Load balancers that distribute requests to DAG](../../media/4be3de77-92ae-4afc-a56e-79260e2d7aea.png)

Be aware that DAGs use Microsoft Clustering Services. These services can't be enabled on the same server as Windows Network Load Balancing (NLB). Accordingly, Windows NLB is not an option when using DAGs. There are third-party software and virtual appliance solutions in this case.

Using DNS is the simplest option for load balancing your Exchange traffic . With DNS load balancing, you only have to provide your clients with the IP address of every Mailbox server. After that, DNS round robin distributes that traffic to your Mailbox servers. The HTTP client is smart enough to connect to another server should one Exchange server fail completely.

Simplicity comes at a price, however. In this case, DNS round robin isn't truly load-balancing the traffic, because there isn't a way programmatically to make sure that each server gets a fair share of the traffic. Also, there is no service level monitoring so that when a single service fails, clients are not automatically redirected to an available service. For example, if Outlook on the web is in failure mode, the clients see an error page.

DNS load balancing requires more external IP addresses when you publish externally. That means that each individual Exchange server in your organization would require an external IP address.

There are more elegant solutions to load balancing your traffic, such as hardware that uses Transport Layer 4 or Application Layer 7 to help distribute client traffic. Load balancers monitor each Exchange client-facing service, and in the event of service failure, load balancers can direct traffic to another server and take the problem server offline. Additionally, some level of load distribution makes sure that no single Mailbox server is proxying the majority of client access.

Load balancing services can use Layer 4 or Layer 7, or a combination, to manage traffic. There are benefits and drawbacks to each solution.

- Layer 4 load balancers work at the Transport layer to direct traffic without examining the contents.

    Because they don't examine the traffic contents, Layer 4 load balancers save time in transit. However, this comes with trade-offs. Layer 4 load balancers know only the IP address, protocol, and TCP port. Knowing only a single IP address, the load balancer can monitor only a single service.

    Layer 4 load balancing benefits include:

  - Requires fewer resources (no content examination).

  - Distributes traffic at the Transport layer.

    The risk with a Layer 4 solution is that if a service fails but the server is still available, clients can connect to the failed service. This means that a resilient Layer 4 implementation requires multiple IP addresses configured with separate HTTP namespaces per service, for example, owa.contoso.com, eas.contoso.com, mapi.contoso.com, which allows for service-level monitoring.

- Layer 7 load balancers work at the Application layer and can inspect the traffic content and direct it accordingly.

    Layer 7 load balancers forego the raw performance benefits of Layer 4 load balancing for the simplicity of a single namespace, for example, mail.contoso.com, and per-service monitoring. Layer 7 load balancers understand the HTTP path, such as /owa or /Microsoft-Server-ActiveSync, or /mapi, and can direct traffic to working servers based on monitoring data.

    Layer 7 load balancing benefits include:

  - Needs only a single IP address.

  - Inspects content and can direct traffic.

  - Provides notification of failed service that can be taken offline.

  - Handles load balancer SSL termination.

  - Distributes traffic at the application layer and understands the destination URL.

SSL should terminate at the load balancer as this offers a centralized place to correct SSL attacks.

The ports that need to be load balanced include some, such as those for IMAP4 or POP3, that may not even be used in your Exchange organization.

|**TCP Port**|**Roles**|**Uses**|
|:-----:|:-----:|:-----|
|25|Mailbox|Inbound SMTP|
|587|Mailbox|Inbound SMTP for clients|
|110|Mailbox|POP3 clients|
|143|Mailbox|IMAP4 clients|
|443|Mailbox|HTTPS (Outlook on the web, AutoDiscover, web services, ActiveSync, <br>MAPI over HTTP, RPC over HTTP, OAB, EAC)|
|993|Mailbox|Secure IMAP4 clients|
|995|Mailbox|Secure POP3 clients|

## Load balancing deployment scenarios in Exchange Server

Exchange 2016 introduced significant flexibility for your namespace and load balancing architecture. With many options for deploying load balancing in your Exchange organization, from simple DNS to sophisticated third-party Layer 4 and Layer 7 solution, we recommend that you review them all in light of your organization's needs.

The following scenarios come with benefits and limitations, and understanding each is key to implementing the solution that best fits your Exchange organization:

- **Scenario A** Single namespace, no session affinity: Layer 4 or Layer 7

- **Scenario B** Single namespace, no session affinity: Layer 7

- **Scenario C** Single namespace with session affinity, Layer 7

- **Scenario D** Multiple namespaces and no session affinity, Layer 4

 **Scenario A** Single namespace, no session affinity: Layer 4 or Layer 7

In this Layer 4 scenario, a single namespace, mail.contoso.com, is deployed for the HTTP protocol clients. The load balancer doesn't maintain session affinity. Because this is a layer 4 solution, the load balancer is configured to check the health of only a single virtual directory as it cannot distinguish Outlook on the web requests from RPC requests.

From the perspective of the load balancer in this example, health is per-server and not per-protocol for the designated namespace. Administrators will have to choose which virtual directory they want to target for the health probe; we recommend that you choose a heavily used virtual directory. For example, if the majority of your users utilize Outlook on the web, then choose the Outlook on the web virtual directory in the health probe.

As long as the Outlook on the web health probe response is healthy, the load balancer keeps the destination Mailbox server in the load balancing pool. However, if the Outlook on the web health probe fails for any reason, then the load balancer removes the destination Mailbox server from the load balancing pool for all requests associated with that namespace. This means that if the health probe fails, all client requests for that namespace are directed to another server, regardless of protocol.

 **Scenario B** Single namespace, no session affinity: Layer 7

In this Layer 7 scenario, a single namespace, mail.contoso.com, is deployed for all the HTTP protocol clients. The load balancer doesn't maintain session affinity. Since the load balancer is configured for Layer 7, there is SSL termination and the load balancer knows the destination URL.

We recommend this configuration for Exchange 2016 and Exchange 2019. The load balancer is configured to check the health of the destination Mailbox servers in the load balancing pool, and a health probe is configured on each virtual directory.

For example, as long as the Outlook on the web health probe response is healthy, the load balancer will keep the destination Mailbox server in the Outlook on the web load balancing pool. However, if the Outlook on the web health probe fails for any reason, then the load balancer removes the target Mailbox server from the load balancing pool for Outlook on the web requests. In this example, health is per-protocol, which means that if the health probe fails, only the affected client protocol is directed to another server.

 **Scenario C** Single namespace with session affinity, Layer 7

In this Layer 7 scenario, a single namespace, mail.contoso.com, is deployed for all the HTTP protocol clients. Because the load balancer is configured for Layer 7, there is SSL termination and the load balancer knows the destination URL. The load balancer is also configured to check the health of the target Mailbox servers in the load balancing pool. The health probe is configured on each virtual directory.

However, enabling session affinity decreases capacity and utilization. This is because the more involved affinity options, cookie-based load balancing or Secure Sockets Layer (SSL) session-ID, require more processing and resources. We recommend that you check with your vendor on how session affinity affects your load balancing scalability.

Just as in the previous scenario, as long as the Outlook on the web health probe response is healthy, the load balancer keeps the destination Mailbox server in the Outlook on the web load balancing pool. However, if the Outlook on the web health probe fails for any reason, then the load balancer removes the target Mailbox server from the load balancing pool for Outlook on the web requests. Here, health is per-protocol, which means that if the health probe fails, only the affected client protocol is directed to another server.

 **Scenario D** Multiple namespaces and no session affinity

This last scenario with multiple namespaces and no session affinity offers per-protocol health checks and Layer 4 power. A unique namespace is deployed for each HTTP protocol client. For example, you would configure the HTTP protocol clients as mail.contoso.com, mapi.contoso.com, and eas.contoso.com.

This scenario provides per-protocol health checking while not requiring complex load-balancing logic. The load balancer uses Layer 4 and is not configured to maintain session affinity. The load balancer configuration checks the health of the destination Mailbox servers in the load balancing pool. In this setting, the health probes are configured to target the health of each virtual directory, as each virtual directory has a unique namespace. Because it's configured for Layer 4, the load balancer doesn't know the URL is being accessed, yet the result is as if it does know. Since health is per-protocol, if the health probe fails, only the affected client protocol is directed to another server.

## Load balancing and managed availability in Exchange Server

Monitoring the available servers and services is key to high availability networks. Since some load balancing solutions have no knowledge of the target URL or the content of the request, this can introduce complexities for Exchange health probes.

Exchange 2016 and Exchange 2019 include a built-in monitoring solution, known as Managed Availability. Managed availability, also known as Active Monitoring or Local Active Monitoring, is the integration of built-in monitoring and recovery actions with the Exchange high availability platform.

Managed Availability includes an offline responder. When the offline responder is invoked, the affected protocol (or server) is removed from service.

To ensure that load balancers do not route traffic to a Mailbox server that Managed Availability has marked as offline, load balancer health probes must be configured to check \<virtualdirectory\>/healthcheck.htm , for example, https://mail.contoso.com/owa/healthcheck.htm.

Read more about managed availability in [Managed availability](../../high-availability/managed-availability/managed-availability.md).
