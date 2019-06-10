---
localization_priority: Normal
description: Learn about the services that Exchange 2016 adds to a Windows server.
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 2ed45d18-2ff3-4099-b841-050eb16a416b
ms.date: 7/31/2018
ms.reviewer: 
title: Overview of Exchange services on Exchange servers
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Overview of Exchange services on Exchange servers

During the installation of Exchange Server 2016 or Exchange Server 2019, Setup runs a set of tasks that install new services in Microsoft Windows. A service is a background process that can be launched during the startup of the server by the Windows Service Control Manager. Services are executable files designed to operate independently and without administrative intervention. A service can run using either a graphical user interface (GUI) mode or a console mode.

All previous versions of Exchange included components that are implemented as services. Each Exchange server role includes services that are part of (or may be needed by) the server role to perform its functions. Note that some services only become active when specific features are used.

The sections in this topic describe the various services that are installed by Exchange 2016 and Exchange 2016 on Mailbox servers and Edge Transport servers. For services that are labeled as optional, you can disable the service if you determine your organization doesn't need the functionality that's provided by the service.

## Exchange services on Mailbox servers

The following table describes the Exchange services that are installed on Mailbox servers.

|**Service name**|**Service short name**|**Description and dependencies**|**Default startup type**|**Security context**|**Dependencies**|**Required or optional**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Microsoft Exchange Active Directory Topology|MSExchangeADTopology|Provides Active Directory topology information to Exchange services. If this service is stopped, most Exchange services can't start.|Automatic|Local System|Net.TCP Port Sharing Service|Required|
|Microsoft Exchange Anti-spam Update|MSExchangeAntispamUpdate|Provides Exchange SmartScreen spam definition updates. <br/> **Note**: In November, 2016, Microsoft stopped producing spam definition updates for the SmartScreen filters in Exchange and Outlook. The existing SmartScreen spam definitions were left in place, but their effectiveness will likely degrade over time. For more information, see [Deprecating support for SmartScreen in Outlook and Exchange](https://go.microsoft.com/fwlink/p/?linkid=835894).|Automatic|Local System|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Compliance Audit|MSComplianceAudit|Provides Exchange auditing features.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Compliance Service|MSExchangeCompliance|Provides a host for Exchange compliance services.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange DAG Management|MSExchangeDagMgmt|Provides storage and database layout management for Mailbox servers in database availability groups (DAGs).|Automatic|Local System|Microsoft Exchange Active Directory TopologyNet.TCP Port Sharing Service|Required|
|Microsoft Exchange Diagnostics|MSExchangeDiagnostics|Provides an agent that monitors Exchange server health.|Automatic|Local System|None|Required|
|Microsoft Exchange EdgeSync|MSExchangeEdgeSync|Replicates configuration and recipient data between the Mailbox server and Active Directory Lightweight Directory Services (AD LDS) on subscribed Edge Transport servers over a secure LDAP channel. <br/> If you don't have any subscribed Edge Transport servers, you can disable this service.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Frontend Transport|MSExchangeFrontEndTransport|Proxies SMTP connections from external hosts to the Microsoft Exchange Transport service on Mailbox servers (the local server or remote servers).|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Health Manager|MSExchangeHM|Part of managed availability that monitors the health of key components on the Exchange server.|Automatic|Local System|Windows Event LogWindows Management Instrumentation|Required|
|Microsoft Exchange Health Manager Recovery|MSExchangeHMRecovery|Part of managed availability that attempts to recover unhealthy components on the Exchange server.|Automatic|Local System|• Windows Event Log <br/>• Windows Management Instrumentation|Required|
|Microsoft Exchange IMAP4|MSExchangeIMAP4|Proxies IMAP4 client connections from the Client Access (frontend) services to the backend IMAP4 service on Mailbox servers. By default, this service isn't running, so IMAP4 clients can't connect to the Exchange server until this service is started. <br/> If you don't have any IMAP4 clients, you can disable this service.|Manual|Local System|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange IMAP4 Backend|MSExchangeIMAP4BE|Receives proxied IMAP4 client connections from the from the Client Access (frontend) IMAP4 service. By default, this service isn't running, so IMAP4 clients can't connect to the Exchange server until this service is started. <br/> If you don't have any IMAP4 clients, you can disable this service.|Manual|Network Service|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Information Store|MSExchangeIS|Manages the mailbox databases on the server. If this service is stopped, mailbox databases on the server are unavailable.|Automatic|Local System|• Microsoft Exchange Active Directory Topology <br/>• Remote Procedure Call (RPC) <br/>• Server  <br/>• Windows Event Log <br/>• Workstation|Required|
|Microsoft Exchange Mailbox Assistants|MSExchangeMailboxAssistants|Performs background processing of mailboxes in mailbox databases on the server.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Mailbox Replication|MSExchangeMailboxReplication|Processes mailbox moves and move requests.|Automatic|Local System|Microsoft Exchange Active Directory TopologyNet.TCP Port Sharing Service|Rquired|
|Microsoft Exchange Mailbox Transport Delivery|MSExchangeDelivery|Receives SMTP messages from the Microsoft Exchange Transport service (on the local or remote Mailbox servers) and delivers them to a local mailbox database using RPC.|Automatic|Network Service|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Mailbox Transport Submission|MSExchangeSubmission|Receives RPC messages from a local mailbox database, and submits them over SMTP to the Microsoft Exchange Transport service (on the local or remote Mailbox servers).|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Notifications Broker|MSExchangeNotificationsBroker|Provides Exchange notifications to local and remote Exchange processes.|Automatic|Local System|Microsoft Exchange Active Directory Topology <br/>• Net.TCP Port Sharing Service|Required|
|Microsoft Exchange POP3|MSExchangePOP3|Proxies POP3 client connections from the Client Access (frontend) services to the backend IMAP4 service on Mailbox servers. By default, this service isn't running, so POP3 clients can't connect to the Exchange server until this service is started.|Manual|Network Service|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange POP3 Backend|MSExchangePOP3BE|Receives proxied POP3 client connections from the from the Client Access (frontend) POP3 service. By default, this service isn't running, so POP3 clients can't connect to the Exchange server until this service is started.|Manual|Network Service|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Replication Service|MSExchangeRepl|Provides replication functionality for mailbox databases in a database availability groups (DAGs).|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange RPC Client Access|MSExchangeRPC|Manages client RPC connections for Exchange.|Automatic|Network Service|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Search|MSExchangeFastSearch|Provides indexing of mailbox content, which improves the performance of content search.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Search Host Controller|HostControllerService|Provides deployment and management services for applications on the local Exchange server.|Automatic|Local System|HTTP Service|Requirerd|
|Microsoft Exchange Server Extension for Windows Server Backup|WSBExchange|Enables Windows Server Backup to back and restore Exchange server data.|Manual|Local System|None|Optional|
|Microsoft Exchange Service Host|MSExchangeServiceHost|Provides a service host for Exchange components that don't have their own services.|Automatic|Local System|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Throttling|MSExchangeThrottling|Provides user workload management that limits the rate of user operations (formerly known as user throttling).|Automatic|Network Service|Microsoft Exchange Active Directory Topology|Required|
|Microsoft Exchange Transport|MSExchangeTransport|Provides SMTP server and transport stack.|Automatic|Network Service|Microsoft Exchange Active Directory Topology <br/>• Microsoft Filtering Management Service|Required|
|Microsoft Exchange Transport Log Search|MSExchangeTransportLogSearch|Provides remote search capability for transport log files (for example, message tracking).|Automatic|Local System|Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Unified Messaging (Exchange 2016 only) |MSExchangeUM|Provides Unified Messaging (UM) features: allows voice and fax messages to be stored in Exchange 2016 and gives users telephone access to email, voice mail, calendar, contacts, or an auto attendant. If this service is stopped, Unified Messaging isn't available. <br/> If you don't use UM in Exchang 2016, you can disable this service.|Automatic|Local System|• CNG Key Isolation <br/>• Microsoft Exchange Active Directory Topology|Optional|
|Microsoft Exchange Unified Messaging Call Router (Exchange 2016 only)|MSExchangeUMCR|Redirects UM client connections from the Client Access (frontend) services to the backend Unified Messaging service on Exchange 2016 Mailbox servers. <br/> If you don't use UM in Exchange 2016, you can disable this service.|Automatic|Local System|• CNG Key Isolation <br/>• Microsoft Exchange Active Directory Topology|Optional|

## Exchange services on Edge Transport servers

The following table describes the Exchange services that are installed on Edge Transport servers.

|**Service name**|**Service short name**|**Description**|**Default startup type**|**Security context**|**Dependencies**|**Required or optional**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Microsoft Exchange ADAM|ADAM_MSExchange|Stores configuration data and recipient data on the Edge Transport server. This service represents the named instance of the Active Directory Lightweight Directory Services (AD LDS) that's automatically created by Exchange Setup.|Automatic|Network Service|COM+ Event System|Required|
|Microsoft Exchange Anti-spam Update|MSExchangeAntispamUpdate|Provides Exchange SmartScreen spam definition updates. <br/> **Note**: In November, 2016, Microsoft stopped producing spam definition updates for the SmartScreen filters in Exchange and Outlook. The existing SmartScreen spam definitions were left in place, but their effectiveness will likely degrade over time. For more information, see [Deprecating support for SmartScreen in Outlook and Exchange](https://go.microsoft.com/fwlink/p/?linkid=835894).|Automatic|Local System|Microsoft Exchange ADAM|Optional|
|Microsoft Exchange Credential Service|MSExchangeEdgeCredential|Monitors credential changes in Active Directory Lightweight Directory Services (AD LDS) and installs the changes on the Edge Transport server.|Automatic|Local System|Microsoft Exchange ADAM|Required|
|Microsoft Exchange Diagnostics|MSExchangeDiagnostics|Provides an agent that monitors Exchange server health.|Automatic|Local System|None|Required|
|Microsoft Exchange Health Manager|MSExchangeHM|Part of managed availability that monitors the health of key components on the Exchange server.|Automatic|Local System|• Windows Event Log <br/>• Windows Management Instrumentation|Required|
|Microsoft Exchange Health Manager Recovery|MSExchangeHMRecovery|Part of managed availability that attempts to recover unhealthy components on the Exchange server.|Automatic|Local System|Windows Event LogWindows Management Instrumentation|Required|
|Microsoft Exchange Service Host|MSExchangeServiceHost|Provides a service host for Exchange components that don't have their own services.|Automatic|Local System|Microsoft Exchange ADAM|Required|
|Microsoft Exchange Transport|MSExchangeTransport|Provides SMTP server and transport stack.|Automatic|Network Service|Microsoft Exchange ADAM|Required|
|Microsoft Exchange Transport Log Search|MSExchangeTransportLogSearch|Provides remote search capability for transport log files (for example, message tracking).|Automatic|Local System|Microsoft Exchange ADAM|Optional|
