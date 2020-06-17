---
title: 'Load balancing: Exchange 2013 Help'
TOCTitle: Load balancing
ms:assetid: f572c193-6f3a-400e-9085-a9d3e5e18c59
ms:mtpsurl: https://technet.microsoft.com/library/JJ898588(v=EXCHG.150)
ms:contentKeyID: 50874017
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Load balancing

_**Applies to:** Exchange Server 2013_

Load balancing is a way to manage which of your servers receive traffic. Load balancing helps distribute incoming client connections over a variety of endpoints (for example, Client Access servers) to ensure that no one endpoint takes on a disproportional share of the load. Load balancing can also provide failover redundancy in case one or more endpoints fails. By using load balancing with Exchange Server 2013, you ensure that your users continue to receive Exchange service in case of a computer failure. Load balancing also enables your deployment to handle more traffic than one server can process while offering a single host name for your clients.

Load balancing serves two primary purposes. It reduces the impact of a single Client Access server failure within one of your Active Directory sites. In addition, load balancing ensures that the load on each of your Client Access servers is evenly distributed.

Exchange 2013 also includes the following solutions for switchover and failover redundancy:

- **High availability**: Exchange 2013 uses database availability groups (DAGs) to keep multiple copies of your mailboxes on different servers synchronized. That way, if a mailbox database fails on one server, users can connect to a synchronized copy of the database on another server.

- **Site resilience**: You can deploy two Active Directory sites in separate geographic locations, keep the mailbox data synchronized between the two, and have one of the sites take on the entire load if the other fails.

- **Online mailbox moves**: In an online mailbox move, users can access their email accounts during the move. Users are locked out of their accounts for only a brief time at the end of the process, when the final synchronization occurs. You can perform online mailbox moves across forests or in the same forest.

- **Shadow redundancy**: Shadow redundancy protects the availability and recoverability of messages while they're in transit. With shadow redundancy, the deletion of a message from the transport databases is delayed until the transport server verifies that all the next hops for that message have completed. If any of the next hops fail before reporting successful delivery, the message is resubmitted for delivery to the hop that didn't complete.

## Architectural changes in load balancing for Exchange Server 2013

In Exchange Server 2010, client connections and processing were handled by the Client Access server role. This required that both external and internal Outlook connections, as well as mobile device and third-party client connections, be load balanced across the array of Client Access servers in a deployment to achieve fault tolerance and efficient utilization of servers. Many Exchange 2010 Client Access protocols required affinity: a relationship between the client and a particular Client Access server. In particular, Outlook Web App, the Exchange Control Panel, Exchange Web Services, Outlook Anywhere, Outlook TCP/IP MAPI connections, Exchange ActiveSync, the Exchange Address Book Service, and Remote PowerShell either required or benefited from client-to-Client Access server affinity. Load balancing options in Exchange 2010 included the following:

- Windows Network Load Balancing with source IP affinity

- Hardware load balancing

Because of the differing needs of client protocols in Exchange 2010, we recommended using a Layer 7 load balancing solution. Layer 7, also known as application-level load balancing, allowed the load balancing solution to use complex rules to determine how to balance each request entering the system, given that the entire conversation between client and server would be available to the load balancer logic. These complex rules ensured that all requests from a specific client went to the same Client Access server endpoint. In Exchange 2010, if all requests from a specific client did not go to the same endpoint for protocols that required affinity, the user experience would be negatively affected. For more information about Exchange 2010 load balancing options, see [Understanding Load Balancing in Exchange 2010](https://go.microsoft.com/fwlink/p/?linkid=196447).

In Exchange Server 2013, there are two primary types of servers: the Client Access server and the Mailbox server. The Client Access servers in Exchange 2013 serve as lightweight, stateless proxy servers, allowing clients to connect to Exchange 2013 Mailbox servers. Exchange 2013 Client Access servers provide a unified namespace and authentication. In addition, Exchange 2013 Client Access servers:

- Support proxy and redirection logic for client protocols.

- Support the use of Layer 4 load balancing.

With session affinity and Layer 7 load balancing, all requests between the client and the server are sent to the same endpoint, as required by various protocols. Requests are distributed at the application layer. With Layer 4 load balancing, the requests are distributed at the transport layer. The load balancing solution distributes requests from the client, which is aware of a single IP address (sometimes called the virtual IP address or VIP), to a set of servers that perform the work. The connection between the client and server must be established before the content of the request is determined, so the load balancer selects a server to receive the request before examining the content of the request. The selection of the target server can be made in various ways such as "round-robin," in which each inbound connection goes to the next target server in a circular list, or "least connections," in which the load balancer sends each new connection to the server that has the fewest established connections at that time. Now that session affinity isn't required, you have more flexibility, choice, and simplicity with respect to the load balancing architecture you deploy. Load balancing without session affinity lets you increase the capacity and utilization of the load balancer, because processing isn't used to maintain more involved affinity options such as cookie-based load balancing or Secure Sockets Layer (SSL) session ID.

## Client Access server arrays and Exchange 2013

In Exchange 2010, we introduced the concept of a Client Access array. After a Client Access array was configured for an Active Directory site, all Client Access servers in the site automatically became members of the array. In current builds of Exchange 2013, no configuration of a Client Access array is required, because the deployment of a load balanced and highly available service is much simpler.

## Load balancing solutions

The use of hardware load balancers is still supported for Exchange 2013. For information about the hardware load balancing solutions that have completed solution testing with Exchange 2010 and will likely work just as well with Exchange 2013, see [Exchange Server 2010 load balancer deployment](https://docs.microsoft.com/exchange/exchange-server-2010-load-balancer-deployment). Keep in mind that this page shows the more complex Layer-7 configuration of hardware load balancers with Exchange 2010. Load balancing Exchange 2013 traffic can be much simpler, given the architectural changes discussed earlier in this topic. Rather than configuring session affinity for each of the Exchange protocols, inbound connections to Exchange 2013 Client Access Servers can be directed to an available server by the load balancer with no additional affinity processing necessary. The hardware load balancer still has an important role in providing high availability of the Exchange service because it can detect when a specific Client Access server has become unavailable and remove it from the set of servers that will handle inbound connections.

## Windows Network Load Balancing

Windows Network Load Balancing (WNLB) is a common software load balancer used for Exchange servers. There are several limitations associated with deploying WNLB with Microsoft Exchange.

- WNLB can't be used on Exchange servers where mailbox DAGs are also being used because WNLB is incompatible with Windows failover clustering. If you're using an Exchange 2013 DAG and you want to use WNLB, you need to have the Client Access server role and the Mailbox server role running on separate servers.

- WNLB doesn't detect service outages. WNLB only detects server outages by IP address. This means that if a particular web service, such as Outlook Web App, fails, but the server is still functioning, WNLB won't detect the failure and will still route requests to that Client Access server. Manual intervention is required to remove the Client Access server experiencing the outage from the load balancing pool.

- Using WNLB can result in port flooding, which can overwhelm networks.

- Because WNLB only performs client affinity using the source IP address, it's not an effective solution when the source IP pool is small. This can occur when the source IP pool is from a remote network subnet or when your organization is using network address translation.
