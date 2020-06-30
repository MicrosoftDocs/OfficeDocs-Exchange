---
title: 'Exchange 2013 Performance Counters: Exchange 2013 Help'
TOCTitle: Exchange 2013 Performance Counters
ms:assetid: 9143dd77-7c30-4769-8de1-28c717cfa9e9
ms:mtpsurl: https://technet.microsoft.com/library/Dn904093(v=EXCHG.150)
ms:contentKeyID: 63917938
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Exchange 2013 Performance Counters

_**Applies to:** Exchange Server 2013_

## Exchange 2013 Performance Counters

The following sections list helpful performance counters you can use when troubleshooting Exchange 2013 performance issues.

## Exchange Domain Controller Connectivity Counters

The following tables displays acceptable thresholds and information about Exchange domain controller connectivity counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Counter</th>
<th>Description</th>
<th>Threshold</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>MSExchange ADAccess Domain Controllers(*)\LDAP Read Time</p></td>
<td><p>Shows the time in milliseconds (ms) to send an LDAP read request to the specified domain controller and receive a response.</p></td>
<td><p>Should be below 50 ms on average. Spikes (maximum values) shouldn't be higher than 100 ms.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange ADAccess Domain Controllers(*)\LDAP Search Time</p></td>
<td><p>Shows the time (in ms) to send an LDAP search request and receive a response.</p></td>
<td><p>Should be below 50 ms on average. Spikes (maximum values) shouldn't be higher than 100 ms.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange ADAccess Processes(*)\LDAP Read Time</p></td>
<td><p>Shows the time (in ms) to send an LDAP read request to the specified domain controller and receive a response.</p></td>
<td><p>Should be below 50 ms on average. Spikes (maximum values) shouldn't be higher than 100 ms.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange ADAccess Processes(*)\LDAP Search Time</p></td>
<td><p>Shows the time (in ms) to send an LDAP search request and receive a response.</p></td>
<td><p>Should be below 50 ms on average. Spikes (maximum values) shouldn't be higher than 100 ms.</p></td>
</tr>
</tbody>
</table>

## Processor and Process Counters

The following tables displays acceptable thresholds and information about processors and process counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Counter</th>
<th>Description</th>
<th>Threshold</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Processor(_Total)\% Processor Time</p></td>
<td><p>Shows the percentage of time that the processor is executing application or operating system processes. This is when the processor isn't idle.</p></td>
<td><p>Should be less than 75% on average.</p></td>
</tr>
<tr class="even">
<td><p>Processor(_Total)\% User Time</p></td>
<td><p>Shows the percentage of processor time spent in user mode. User mode is a restricted processing mode designed for applications, environment subsystems, and integral subsystems.</p></td>
<td><p>Should be less than 75% on average.</p></td>
</tr>
<tr class="odd">
<td><p>Processor(_Total)\% Privileged Time</p></td>
<td><p>Shows the percentage of processor time spent in privileged mode. Privileged mode is a processing mode designed for operating system components and hardware-manipulating drivers. It allows direct access to hardware and all memory.</p></td>
<td><p>Should be less than 75% on average.</p></td>
</tr>
<tr class="even">
<td><p>System\Processor Queue Length (all instances)</p></td>
<td><p>Indicates the number of threads each processor is servicing. Processor Queue Length can be used to identify if processor contention or high CPU utilization is caused by the processor capacity being insufficient to handle the workloads assigned to it. Processor Queue Length shows the number of threads that are delayed in the Processor Ready Queue and are waiting to be scheduled for execution. The value listed is the last observed value at the time the measurement was taken.</p></td>
<td><p>Shouldn't be greater than 5 per processor.</p></td>
</tr>
<tr class="odd">
<td><p>Process(*)\% Processor Time</p></td>
<td><p>Can be used to identify specific processes consuming CPU.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## Memory Counters

The following tables displays acceptable thresholds and information about memory counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>Memory\Available Mbytes</p></td>
<td><p>Shows the amount of physical memory, in megabytes (MB), immediately available for allocation to a process or for system use. It's equal to the sum of memory assigned to the standby (cached), free, and zero page lists. For a full explanation of the memory manager, refer to Microsoft Developer Network (MSDN) or &quot;System Performance and Troubleshooting Guide&quot; in the Windows Server 2003 Resource Kit.</p></td>
<td><p>Should remain above 5% of total RAM.</p></td>
</tr>
<tr class="odd">
<td><p>Memory\% Committed Bytes In Use</p></td>
<td><p>Shows the ratio of Memory\Committed Bytes to the Memory\Commit Limit. Committed memory is the physical memory in use for which space has been reserved in the paging file should it need to be written to disk. The commit limit is determined by the size of the paging file. If the paging file is enlarged, the commit limit increases, and the ratio is reduced. This counter displays the current percentage value only; it isn't an average.</p></td>
<td><p>If this value is more than 80%, it is an indication that the system is likely under stress to provide more memory.</p></td>
</tr>
</tbody>
</table>

## .NET Framework Counters

The following tables displays acceptable thresholds and information about .NET Framework counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>.NET CLR Memory(*)\% Time in GC</p></td>
<td><p>Shows when garbage collection has occurred. When the counter exceeds the threshold, it indicates that CPU is cleaning up and isn't being used efficiently for load. Adding memory to the server would improve this situation.</p></td>
<td><p>Should be below 10% on average.</p></td>
</tr>
<tr class="odd">
<td><p>.NET CLR Exceptions(*)\# of Excepts Thrown / sec</p></td>
<td><p>Displays the number of exceptions thrown per second. These include both .NET Framework exceptions and unmanaged exceptions that get converted into .NET Framework exceptions. For example, the null pointer reference exception in unmanaged code would get thrown again in managed code as a .NET Framework System.NullReferenceException. This counter includes both handled and unhandled exceptions.</p></td>
<td><p>Should be less than 5% of total requests per second (RPS) (Web Server(_Total)\Connection Attempts/sec * .05).</p></td>
</tr>
<tr class="even">
<td><p>.NET CLR Memory(*)\# Bytes in all Heaps</p></td>
<td><p>Shows the sum of four other counters: Gen 0 Heap Size, Gen 1 Heap Size, Gen 2 Heap Size, and Large Object Heap Size. This counter indicates the current memory allocated in bytes on the GC Heaps.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## Network Counters

The following tables displays acceptable thresholds and information about common network counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>Network Interface(*)\Packets Outbound Errors</p></td>
<td><p>Indicates the number of outbound packets that couldn't be transmitted because of errors.</p></td>
<td><p>Should be 0 at all times.</p></td>
</tr>
<tr class="odd">
<td><p>TCPv6\Connection Failures</p></td>
<td><p>Shows the number of times TCP connections have made a direct transition to the CLOSED state from the SYN-SENT state or the SYN-RCVD state, plus the number of times TCP connections have made a direct transition to the LISTEN state from the SYN-RCVD state.</p></td>
<td><p>An increasing number of failures, or a consistently increasing rate of failures, can indicate a bandwidth shortage.</p></td>
</tr>
<tr class="even">
<td><p>TCPv4\Connections Reset</p></td>
<td><p>Shows the number of times TCP connections have made a direct transition to the CLOSED state from either the ESTABLISHED state or the CLOSE-WAIT state.</p></td>
<td><p>An increasing number of resets, or a consistently increasing rate of resets, can indicate a bandwidth shortage.</p></td>
</tr>
<tr class="odd">
<td><p>TCPv6\Connections Reset</p></td>
<td><p>Shows the number of times TCP connections have made a direct transition to the CLOSED state from either the ESTABLISHED state or the CLOSE-WAIT state.</p></td>
<td><p>An increasing number of resets, or a consistently increasing rate of resets, can indicate a bandwidth shortage.</p></td>
</tr>
</tbody>
</table>

## Netlogon Counters

The following tables displays acceptable thresholds and information about common counters for monitoring NTLM authentication issues and MaxConcurrentAPI issues. See the Microsoft Knowledge Base article [KB2688798](https://support.microsoft.com/help/2688798) for more information.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>\Netlogon\Semaphore Waiters</p></td>
<td><p>The number of the thread that is waiting to obtain the semaphore.</p></td>
<td><p>See the Microsoft Knowledge Base article <a href="https://support.microsoft.com/help/2688798">KB2688798</a></p></td>
</tr>
<tr class="odd">
<td><p>\Netlogon\Semaphore Holders</p></td>
<td><p>The number of the thread that is holding the semaphore.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>\Netlogon\Semaphore Acquires</p></td>
<td><p>The total number of times that the semaphore has been obtained over the lifetime of the security channel connection, or since system startup for _Total.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>\Netlogon\Semaphore Timeouts</p></td>
<td><p>The total number of times that a thread has timed out while it waited for the semaphore over the lifetime of the security channel connection, or since system startup for _Total.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>\Netlogon\Average Semaphore Hold Time</p></td>
<td><p>The average time (in seconds) that the semaphore is held over the last sample.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## Database Counters

The following table shows active log I/O latency requirements counters and their acceptable thresholds. When thresholds are exceeded, the client experience degrades. For example, users may experience message delivery delays or slow system performance.

> [!NOTE]
> Normal storage latency guidance in Exchange 2013 is very similar to the guidance from Exchange 2010. Additional database counters can be found in <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff367871(v=exchg.141)">Mailbox Server Counters</A>.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Reads (Attached) Average Latency</p></td>
<td><p>Shows the average length of time, in milliseconds (ms), per database read operation.</p></td>
<td><p>Should be less than 20ms on average.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Writes (Attached) Average Latency</p></td>
<td><p>Shows the average length of time, in ms, per database write operation.</p></td>
<td><p>Should be less than 50ms on average.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Log Writes Average Latency</p></td>
<td><p>Shows the average length of time, in ms, per Log write operation.</p></td>
<td><p>Should be less than 10ms on average.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Reads (Recovery) Average Latency</p></td>
<td><p>Shows the average length of time, in ms, per passive database read operation.</p></td>
<td><p>Should be less than 200ms on average.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Writes (Recovery) Average Latency</p></td>
<td><p>Shows the average length of time, in ms, per passive database write operation.</p></td>
<td><p>Should be less than the read latency for the same instance, as measured by the MSExchange Database ==&gt; Instances(*)\I/O Database Reads (Recovery) Average Latency counter.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Reads (Attached)/sec</p></td>
<td><p>Shows the number of database read operations per second for each attached database instance.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Database Writes (Attached)/sec</p></td>
<td><p>Shows the number of database write operations per second for each attached database instance.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange Database ==&gt; Instances(*)\I/O Log Writes/sec</p></td>
<td><p>Shows the number of log writes per second for each database instance.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>MSExchange Active Manager(_total)\Database Mounted</p></td>
<td><p>Shows the number of active database copies on the server.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## ASP.NET

The following tables displays acceptable thresholds and information about ASP.NET counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>ASP.NET\Application Restarts</p></td>
<td><p>Shows the number of times the application has been restarted during the Web server's lifetime.</p></td>
<td><p>Should be 0 at all times.</p></td>
</tr>
<tr class="odd">
<td><p>ASP.NET\Worker Process Restarts</p></td>
<td><p>Shows the number of times a worker process has restarted on the computer.</p></td>
<td><p>Should be 0 at all times.</p></td>
</tr>
<tr class="even">
<td><p>ASP.NET\Request Wait Time</p></td>
<td><p>Shows the number of ms the most recent request was waiting in the queue.</p></td>
<td><p>Should be 0 at all times.</p></td>
</tr>
<tr class="odd">
<td><p>ASP.NET Applications(*)\Requests In Application Queue</p></td>
<td><p>Shows the number of requests in the application request queue.</p></td>
<td><p>Should be 0 at all times.</p></td>
</tr>
<tr class="even">
<td><p>ASP.NET Applications(*)\Requests Executing</p></td>
<td><p>Shows the number of requests currently executing.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>ASP.NET Applications(*)\Requests/Sec</p></td>
<td><p>Shows the number of requests executed per second.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## RPC Client Access Counters

The following tables displays acceptable thresholds and information about RPC Client Access counters.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>MSExchange RpcClientAccess\RPC Averaged Latency</p></td>
<td><p>Shows the latency, in milliseconds (ms), averaged for the past 1,024 packets.</p></td>
<td><p>Should be below 250 ms.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange RpcClientAccess\RPC Requests</p></td>
<td><p>Shows the number of client requests currently being processed by the RPC Client Access service.</p></td>
<td><p>Shouldn't be over 40.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange RpcClientAccess\Active User Count</p></td>
<td><p>Shows the number of unique users that have shown some activity in the last 2 minutes.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange RpcClientAccess\Connection Count</p></td>
<td><p>Shows the total number of client connections maintained.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>MSExchange RpcClientAccess\RPC Operations/sec</p></td>
<td><p>Shows the rate at which RPC operations occur, per second.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange RpcClientAccess\User Count</p></td>
<td><p>Shows the number of users connected to the service.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## HTTP Proxy Counters

The following tables displays information about HTTP Proxy counters.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
</tr>
<tr class="even">
<td><p>MSExchange HttpProxy(*)\MailboxServerLocator Average Latency</p></td>
<td><p>Shows the average latency (ms) of MailboxServerLocator web service calls.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange HttpProxy(*)\Average Authentication Latency</p></td>
<td><p>Shows the average time spent authenticating CAS requests over the last 200 samples.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange HttpProxy(*)\Average ClientAccess Server Processing Latency</p></td>
<td><p>Shows the average latency (ms) of CAS processing time (does not include time spent proxying) over the last 200 requests.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange HttpProxy(*)\Mailbox Server Proxy Failure Rate</p></td>
<td><p>Shows the percentage of connectivity related failures between this Client Access Server and MBX servers over the last 200 samples.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange HttpProxy(*)\Outsanding Proxy Requests</p></td>
<td><p>Shows the number of concurrent outstanding proxy requests.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange HttpProxy(*)\Proxy Requests/Sec</p></td>
<td><p>Shows the number of proxy requests processed each second.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange HttpProxy(*)\Requests/Sec</p></td>
<td><p>Shows the number of requests processed each second.</p></td>
</tr>
</tbody>
</table>

## Information Store Counters

The following tables displays acceptable thresholds and information about Information Store counters.

> [!NOTE]
> Normal storage latency guidance in Exchange 2013 is very similar to the guidance from Exchange 2010. Additional Information Store counters can be found in <A href="https://docs.microsoft.com/previous-versions/office/exchange-server-2010/ff367871(v=exchg.141)">Mailbox Server Counters</A>.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
<td><p>Threshold</p></td>
</tr>
<tr class="even">
<td><p>MSExchangeIS Store(*)\RPC Requests</p></td>
<td><p>Indicates the overall RPC requests currently executing within the information store process.</p></td>
<td><p>Should be below 70 at all times.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchangeIS Client Type(*)\RPC Average Latency</p></td>
<td><p>Shows a server RPC latency, in ms, averaged for the past 1,024 packets for a particular client protocol.</p></td>
<td><p>Should be less than 50 ms on average for each client.</p></td>
</tr>
<tr class="even">
<td><p>MSExchangeIS Store(*)\RPC Average Latency</p></td>
<td><p>RPC Latency average (msec) is the average latency in milliseconds of RPC requests per database. Average is calculated over all RPCs since exrpc32 was loaded.</p></td>
<td><p>Should be less than 50ms at all times, with spikes less than 100ms.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchangeIS Store(*)\RPC Operations/sec</p></td>
<td><p>Shows the number of RPC operations per second for each database instance.</p></td>
<td><p>Not applicable</p></td>
</tr>
<tr class="even">
<td><p>MSExchangeIS Client Type(*)\RPC Operations/sec</p></td>
<td><p>Shows the number of RPC operations per second for each client type connection.</p></td>
<td><p>Not applicable</p></td>
</tr>
</tbody>
</table>

## Client Access Server Counters

The following tables displays information about client connection counters and Internet Information Services (IIS) counters.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
</tr>
<tr class="even">
<td><p>MSExchange ActiveSync\Requests/sec</p></td>
<td><p>Shows the number of HTTP requests received from the client via ASP.NET per second. Determines the current Exchange ActiveSync request rate. Used only to determine current user load.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange ActiveSync\Ping Commands Pending</p></td>
<td><p>Shows the number of ping commands currently pending in the queue.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange ActiveSync\Sync Commands/sec</p></td>
<td><p>Shows the number of sync commands processed per second. Clients use this command to synchronize items within a folder.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange Availability Service\Availability Requests (sec)</p></td>
<td><p>Shows the number of requests serviced per second. The request can be only for free/ busy information or include suggestions. One request may contain multiple mailboxes. Determines the rate at which Availability service requests are occurring.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange OWA\Current Unique Users</p></td>
<td><p>Shows the number of unique users currently logged on to Outlook Web App. This value monitors the number of unique active user sessions, so that users are only removed from this counter after they log off or their session times out. Determines current user load.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange OWA\Requests/sec</p></td>
<td><p>Shows the number of requests handled by Outlook Web App per second. Determines current user load.</p></td>
</tr>
<tr class="even">
<td><p>MSExchangeAutodiscover\Requests/sec</p></td>
<td><p>Shows the number of Autodiscover service requests processed each second. Determines current user load.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchangeWS\Requests/sec</p></td>
<td><p>Shows the number of requests processed each second. Determines current user load.</p></td>
</tr>
<tr class="even">
<td><p>Web Service(_Total)\Current Connections</p></td>
<td><p>Shows the current number of connections established with the Web service. Determines current user load.</p></td>
</tr>
<tr class="odd">
<td><p>Web Service(Default Web Site)\Current Connections</p></td>
<td><p>Shows the current number of connections established to the Default website which corresponds to the number of connections hitting the Front End CAS server role. Determines current user load.</p></td>
</tr>
<tr class="even">
<td><p>WebService(_Total)\Connection Attempts/sec</p></td>
<td><p>Shows the rate that connections to the Web service are being attempted. Determines current user load.</p></td>
</tr>
<tr class="odd">
<td><p>Web Service(_Total)\Other Request Methods/sec</p></td>
<td><p>Shows the rate HTTP requests are made that don't use the OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, MOVE, COPY, MKCOL, PROPFIND, PROPPATCH, SEARCH, LOCK, or UNLOCK methods. Determines current user load.</p></td>
</tr>
</tbody>
</table>

## Workload Management Counters

The following tables displays information about Exchange Workload Management counters. These counters are important to monitor because workload management may run tasks in the background during off-peak times.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Counter</p></td>
<td><p>Description</p></td>
</tr>
<tr class="even">
<td><p>MSExchange WorkloadManagement Workloads(*)\ActiveTasks</p></td>
<td><p>Shows the number of active tasks currently running in the background for workload management.</p></td>
</tr>
<tr class="odd">
<td><p>MSExchange WorkloadManagement Workloads(*)\CompletedTasks</p></td>
<td><p>Shows the number of workload management tasks that have been completed.</p></td>
</tr>
<tr class="even">
<td><p>MSExchange WorkloadManagement Workloads(*)\QueuedTasks</p></td>
<td><p>Shows the number of workload management tasks that are currently queued up waiting to be processed.</p></td>
</tr>
</tbody>
</table>
