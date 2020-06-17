---
title: 'Edge Subscriptions: Exchange 2013 Help'
TOCTitle: Edge Subscriptions
ms:assetid: 3addd71a-4165-401f-a009-002bcd8baba6
ms:mtpsurl: https://technet.microsoft.com/library/Aa997438(v=EXCHG.150)
ms:contentKeyID: 61200285
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Edge Subscriptions

_**Applies to:** Exchange Server 2013_

Edge Transport servers minimize attack surface by handling all Internet-facing mail flow and providing SMTP relay and smart host services for your Exchange organization. Additional layers of message protection and security are provided by a series of agents running on the Edge Transport server in your organization's perimeter network. These agents support features that provide protection against viruses and spam and apply transport rules to control message flow.

Edge Subscriptions are used to populate the Active Directory Lightweight Directory Services (AD LDS) instance on the Edge Transport server with Active Directory data. Although creating an Edge Subscription is optional, subscribing an Edge Transport server to the Exchange organization provides a simpler management experience and enhances antispam features. You need to create an Edge Subscription if you plan to use recipient lookup or safelist aggregation, or if you plan to help secure SMTP communications with partner domains by using Mutual Transport Layer Security (MTLS).

## Edge Subscription process

An Edge Transport server doesn't have direct access to Active Directory. The configuration and recipient information the Edge Transport server uses to process messages is stored locally in AD LDS. Creating an Edge Subscription establishes secure, automatic replication of information from Active Directory to AD LDS. The Edge Subscription process provisions the credentials used to establish a secure LDAP connection between Exchange 2013 Mailbox servers and a subscribed Edge Transport server. The Microsoft Exchange EdgeSync service (EdgeSync) that runs on Mailbox servers performs periodic one-way synchronization to transfer up-to-date data to AD LDS. This reduces the administration tasks you perform in the perimeter network by letting you configure the Mailbox server and then synchronize that information to the Edge Transport server.

You subscribe an Edge Transport server to the Active Directory site that contains the Mailbox servers responsible for transferring messages to and from your Edge Transport servers. The Edge Subscription process creates an Active Directory site membership affiliation for the Edge Transport server. The site affiliation enables Mailbox servers in the Exchange organization to relay messages to the Edge Transport server for delivery to the Internet without having to configure explicit Send connectors.

One or more Edge Transport servers can be subscribed to a single Active Directory site. However, an Edge Transport server can't be subscribed to more than one Active Directory site. If you have more than one Edge Transport server deployed, each server can be subscribed to a different Active Directory site. Each Edge Transport server requires an individual Edge Subscription.

To deploy an Edge Transport server and subscribe it to an Active Directory site, follow these steps:

1. Install the Edge Transport server role.

2. Verify that the Mailbox servers and the Edge Transport server can locate one another using DNS name resolution.

3. On the Mailbox Server, configure the objects and settings to be replicated to the Edge Transport server.

4. On the Edge Transport server, create and export an Edge Subscription file.

5. Copy the Edge Subscription file to a Mailbox server or a file share that's accessible from the Active Directory site containing your Mailbox servers.

6. Import the Edge Subscription file to the Active Directory site.

## What happens when you create a new Edge Subscription

When you create an Edge Subscription file (by running the **New-EdgeSubscription** cmdlet on the Edge Transport server), the following actions occur:

- An AD LDS account called the EdgeSync bootstrap replication account (ESBRA) is created. These ESBRA credentials are used to authenticate the first EdgeSync connection to the Edge Transport server. This account is configured to expire 24 hours after being created. Therefore, you need to complete the six-step subscription process described in the previous section within 24 hours. If the ESBRA expires before the Edge Subscription process is complete, you will need to run the **New-EdgeSubscription** cmdlet again to create a new Edge Subscription file.

- The ESBRA credentials are retrieved from AD LDS and written to the Edge Subscription file. The public key for the Edge Transport server's self-signed certificate is also exported to the Edge Subscription file. The credentials written to the Edge Subscription file are specific to the server that exported the file.

- Any previously created configuration objects on the Edge Transport server that will now be replicated to AD LDS from Active Directory are deleted from AD LDS, and the Exchange Management Shell cmdlets used to configure those objects are disabled. However, you can still use the **Get-\*** cmdlets to view those objects. Running the **New-EdgeSubscription** cmdlet disables the following cmdlets on the Edge Transport server:

  - **Set-SendConnector**

  - **New-SendConnector**

  - **Remove-SendConnector**

  - **New-AcceptedDomain**

  - **Set-AcceptedDomain**

  - **Remove-AcceptedDomain**

  - **New-MessageClassification**

  - **Set-MessageClassification**

  - **Remove-MessageClassification**

  - **New-RemoteDomain**

  - **Set-RemoteDomain**

  - **Remove-RemoteDomain**

When you import the Edge Subscription file on the Mailbox server by running the **New-EdgeSubscription** cmdlet on the Mailbox server:

- The Edge Subscription is created, joining an Edge Transport server to an Exchange organization. EdgeSync will propagate configuration data to this Edge Transport Server, creating an Edge configuration object in Active Directory.

- Each Mailbox server in the Active Directory site receives notification from Active Directory that a new Edge Transport server has been subscribed. The Mailbox server retrieves the ESBRA from the Edge Subscription file. The Mailbox server then encrypts the ESBRA by using the public key of the Edge Transport server's self-signed certificate. The encrypted credentials are then written to the Edge configuration object.

- Each Mailbox server also encrypts the ESBRA using its own public key and then stores the credentials in its own configuration object.

- EdgeSync replication accounts (ESRAs) are created in Active Directory for each Edge Transport-Mailbox server pair. Each Mailbox server stores its ESRA credentials as an attribute of the Mailbox server configuration object.

- Send connectors are automatically created to relay messages outbound from the Edge Transport server to the Internet, and inbound from the Edge Transport server to the Exchange organization.

- The Microsoft Exchange EdgeSync service that runs on Mailbox servers uses the ESBRA credentials to establish a secure LDAP connection between a Mailbox server and the Edge Transport server, and performs the initial replication of data. The following data is replicated to AD LDS:

  - Topology data

  - Configuration data

  - Recipient data

  - ESRA credentials

- The Microsoft Exchange Credential Service that runs on the Edge Transport server installs the ESRA credentials. These credentials are used to authenticate and secure later synchronization connections.

- The EdgeSync synchronization schedule is established.

The Microsoft Exchange EdgeSync service running on the Mailbox servers in the subscribed Active Directory site then performs one-way replication of data from Active Directory to AD LDS on a regular schedule. You can also use the **Start-EdgeSynchronization** cmdlet to override the EdgeSync synchronization schedule and immediately start synchronization.

For more information about ESRA accounts and how they're used to help secure the EdgeSync synchronization process, see [Edge Subscription credentials](edge-subscription-credentials-exchange-2013-help.md).

This example subscribes an Edge Transport server to the specified site and automatically creates the Internet Send connector and the Send connector from the Edge Transport server to the Mailbox servers.

```powershell
New-EdgeSubscription -FileData ([byte[]]$(Get-Content -Path "C:\EdgeSubscriptionInfo.xml" -Encoding Byte -ReadCount 0)) -CreateInternetSendConnector $true -CreateInboundSendConnector $true -Site "Default-First-Site-Name"
```

> [!NOTE]
> The default values of the <EM>CreateInternetSendConnector</EM> and <EM>CreateInboundSendConnector</EM> parameters are both <CODE>$true</CODE>. They are shown here for demonstration only.

This example exports an Edge Subscription file.

```powershell
New-EdgeSubscription -FileName "C:\EdgeSubscriptionInfo.xml"
```

> [!NOTE]
> When the <STRONG>New-EdgeSubscription</STRONG> cmdlet is run on the Edge Transport server, you receive a prompt to acknowledge the commands that will be disabled and the configuration that will be overwritten on the Edge Transport server. To bypass this confirmation, you must use the <EM>Force</EM> parameter. This parameter is useful when you script the <STRONG>New-EdgeSubscription</STRONG> cmdlet. The <EM>Force</EM> parameter is also used to overwrite an existing file with the same name as the file that you're creating when you resubscribe an Edge Transport server.

For detailed syntax and parameter information, see [New-EdgeSubscription](https://docs.microsoft.com/powershell/module/exchange/New-EdgeSubscription).

## Send connectors created during the Edge Subscription process

By default, when you complete the recommended Edge Subscription process by importing the Edge Subscription file to a Mailbox server, the Send connectors required to enable end-to-end mail flow between the Internet and the Exchange organization are created automatically, and any existing Send connectors on the Edge Transport server are deleted. In some scenarios, you may choose to suppress automatic creation of Send connectors and configure Send connectors manually. For more information about manually configuring Send connectors, see [Manually configure Edge Transport server mail flow](manually-configure-edge-transport-server-mail-flow-exchange-2013-help.md) and [Configure Internet mail flow through an Edge Transport server without using EdgeSync](configure-internet-mail-flow-through-an-edge-transport-server-without-using-edgesync-exchange-2013-help.md).

The Edge Subscription process provisions the following Send connectors:

- A Send connector configured to relay email messages from the Exchange organization to the Internet.

- A Send connector configured to relay email messages from the Edge Transport server to the Exchange organization.

Also, subscribing an Edge Transport server to the Exchange organization allows the Mailbox servers in the subscribed Active Directory site to use the intra-organization Send connector to relay messages to that Edge Transport server.

## Automatically create an inbound Send connector to receive messages from the Internet

By default, when you run the **New-EdgeSubscription** cmdlet on the Mailbox server, the Inbound Send Connector parameter *CreateInboundSendConnector* is set to the value `$true`. This creates the Send connector needed to send messages to the Exchange organization. The following table shows the configuration of this Send connector.

### Automatic inbound Send connector configuration

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Name</em></p></td>
<td><p>EdgeSync - Inbound to &lt;<em>Site Name</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p><em>AddressSpaces</em></p></td>
<td><p><code>SMTP:--;1</code></p>
<p>The <code>--</code> value in the address space represents all authoritative and internal relay accepted domains for the Exchange organization. Any messages the Edge Transport server receives for these accepted domains are routed to this Send connector and relayed to the smart hosts.</p></td>
</tr>
<tr class="odd">
<td><p><em>SourceTransportServers</em></p></td>
<td><p>&lt;<em>Edge Subscription name</em>&gt;</p></td>
</tr>
<tr class="even">
<td><p><em>Enabled</em></p></td>
<td><p>True</p></td>
</tr>
<tr class="odd">
<td><p><em>DNSRoutingEnabled</em></p></td>
<td><p>False</p></td>
</tr>
<tr class="even">
<td><p><em>SmartHosts</em></p></td>
<td><p><code>--</code></p>
<p>The <code>--</code> value in the list of smart hosts represents all Mailbox servers in the subscribed Active Directory site. Any Mailbox servers you add to the subscribed Active Directory site after you establish the Edge Subscription don't participate in the EdgeSync synchronization process. However, they are automatically added to the list of smart hosts for the automatically created inbound Send connector. If more than one Mailbox server is located in the subscribed Active Directory site, inbound connections will be load balanced across the smart hosts.</p></td>
</tr>
</tbody>
</table>

You can't modify the address space or list of smart hosts at creation time for the automatically created inbound Send connector. However, you can set the *CreateInboundSendConnector* parameter to the value `$false` when you create an Edge Subscription. This allows you to manually configure a Send connector from the Edge Transport server to the Exchange organization.

## Automatically create an outbound Send connector to send messages to the Internet

By default, when you run the **New-EdgeSubscription** cmdlet on the Mailbox server, the Outbound Send Connector parameter *CreateInternetSendConnector* is set to the value `$true`. This creates the Send connector needed to send messages to the Internet. The following table shows the default configuration of this Send connector.

### Automatic Internet Send connector configuration

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Property</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>Name</em></p></td>
<td><p>EdgeSync - &lt;<em>Site Name</em>&gt; to Internet</p></td>
</tr>
<tr class="even">
<td><p><em>AddressSpaces</em></p></td>
<td><p><code>SMTP:*;100</code></p></td>
</tr>
<tr class="odd">
<td><p><em>SourceTransportServers</em></p></td>
<td><p>&lt;<em>Edge Subscription name</em>&gt;</p>

> [!NOTE]
> The name of the Edge Subscription is the same as the name of the subscribed Edge Transport server.

</td>
</tr>
<tr class="even">
<td><p><em>Enabled</em></p></td>
<td><p>True</p></td>
</tr>
<tr class="odd">
<td><p><em>DNSRoutingEnabled</em></p></td>
<td><p>True</p></td>
</tr>
<tr class="even">
<td><p><em>DomainSecureEnabled</em></p></td>
<td><p>True</p></td>
</tr>
</tbody>
</table>

If more than one Edge Transport server is subscribed to the same Active Directory site, no additional Send connectors to the Internet are created. Instead, all Edge Subscriptions are added to the same Send connector as the source server. This load balances outbound connections to the Internet across the subscribed Edge Transport servers.

The outbound Send connector is configured to send email messages from the Exchange organization to all remote SMTP domains, using DNS routing to resolve domain names to MX resource records. For details about manually configuring a connector's configuration, see [Manually configure Edge Transport server mail flow](manually-configure-edge-transport-server-mail-flow-exchange-2013-help.md).

## Microsoft Exchange EdgeSync service

After you subscribe an Edge Transport server to an Active Directory site, EdgeSync will replicate configuration and recipient data to the Edge Transport servers. The service replicates the following data from Active Directory to AD LDS:

- Send connector configuration

- Accepted domains

- Remote domains

- Message classifications

- Safe Senders Lists

- Blocked Senders Lists

- Recipients

- List of send and receive domains used in domain secure communications with partners

- List of SMTP servers listed as internal in your organization's transport configuration

- List of Mailbox servers in the subscribed Active Directory site

For details about the data replicated to AD LDS and how it's used, see [EdgeSync replication data](edgesync-replication-data-exchange-2013-help.md).

EdgeSync uses a mutually authenticated and authorized secure LDAP channel to transfer data from the Mailbox server to the Edge Transport server.

To replicate data to AD LDS, the Mailbox server binds to a global catalog server to retrieve updated data. EdgeSync initiates a secure LDAP session between a Mailbox server and the subscribed Edge Transport server over the non-standard TCP port 50636.

When you first subscribe an Edge Transport server to an Active Directory site, the initial replication that populates AD LDS with data from Active Directory can take five minutes or more, depending on the quantity of data in the directory service. After initial replication, EdgeSync only synchronizes new and changed objects, and removes any deleted objects.

## Synchronization schedule

Different types of data synchronize on different schedules. The EdgeSync synchronization schedule specifies the maximum interval between EdgeSync synchronizations. EdgeSync synchronization occurs at the following intervals:

- Configuration data: 3 minutes.

- Recipient data: 5 minutes.

- Topology data: 5 minutes

If you want to change these intervals, use the **Set-EdgeSyncServiceConfig** cmdlet. Using the **Start-EdgeSynchronization** cmdlet on the Mailbox server to force Edge Subscription synchronization overrides the timer for the next scheduled EdgeSync synchronization, and starts EdgeSync immediately.

## Selection of Mailbox servers

Each subscribed Edge Transport server is associated with a particular Active Directory site. If more than one Mailbox server exists in the site, any of these Mailbox servers can replicate data to the subscribed Edge Transport servers. To avoid contention among Mailbox servers when synchronizing, the preferred Mailbox server is selected as follows:

1. The first Mailbox server in the Active Directory site to perform a topology scan and discover the new Edge Subscription performs the initial replication. Because this discovery is based on the timing of the topology scan, any Mailbox server in the site may perform the initial replication.

2. The Mailbox server performing the initial replication establishes an EdgeSync lease option and sets a lock on the Edge Subscription. The lease option establishes that particular Mailbox server as the preferred server providing synchronization services to that Edge Transport server. The lock prevents EdgeSync running on another Mailbox server from taking over the lease option.

3. The EdgeSync lease option lasts for one hour. During that hour, no other EdgeSync service can take over the option unless a manual synchronization is started before the end of the hour. If the preferred Mailbox server isn't available to provide EdgeSync service at the time manual synchronization is started, after a five-minute wait, the lock is released and another EdgeSync service can take over the lease option and perform synchronization.

4. Unless manual synchronization is started, synchronization occurs based on the EdgeSync synchronization schedule. If the preferred server isn't available when a scheduled synchronization occurs, after a five-minute wait, the lock is released and another EdgeSync service can take over the lease option and perform synchronization.

This method of locking and leasing prevents more than one instance of EdgeSync from pushing data to the same Edge Transport server at the same time.

> [!NOTE]
> If you also have Exchange 2010 or Exchange 2007 Mailbox servers in the subscribed Active Directory site, Exchange 2013 Mailbox servers will always take precedence and perform the replication.

> [!NOTE]
> When you subscribe an Edge Transport server to an Active Directory site, all Mailbox servers installed in that Active Directory site at that time can participate in the EdgeSync synchronization process. If one of those servers is removed, the EdgeSync service that's running on the remaining Mailbox servers will continue the data synchronization process. However, if you ;ater install new Mailbox servers in the Active Directory site, they won't automatically participate in EdgeSync synchronization. If you want to enable those new Mailbox servers to participate in EdgeSync synchronization, you will need to subscribe the Edge Transport server again.

The following table lists the EdgeSync properties related to locking and leasing. You can use the **Set-EdgeSyncServiceConfig** cmdlet to configure these properties.

### EdgeSync lease properties

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th>Parameter</th>
<th>Default value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><em>LockDuration</em></p></td>
<td><p><code>00:05:00</code> (5 minutes)</p></td>
<td><p>This setting determines how long a particular EdgeSync service will acquire a lock. If the EdgeSync service on the Mailbox server that's holding this lock doesn't respond, after five minutes the EdgeSync service on another Mailbox server will take over the lease. Forcing immediate EdgeSync synchronization doesn't override this setting.</p></td>
</tr>
<tr class="even">
<td><p><em>OptionDuration</em></p></td>
<td><p><code>01:00:00</code> (1 hour)</p></td>
<td><p>This setting determines how long an EdgeSync service can declare a lease option on an Edge Transport server. If the EdgeSync service holding the lease is unavailable and doesn't restart during this option period, no other Exchange EdgeSync service will take over the lease option unless you force EdgeSync synchronization.</p></td>
</tr>
<tr class="odd">
<td><p><em>LockRenewalDuration</em></p></td>
<td><p><code>00:01:00</code> (1 minute)</p></td>
<td><p>This setting determines how frequently the lock field is updated when an EdgeSync service has acquired a lock to an Edge Transport server.</p></td>
</tr>
</tbody>
</table>

## Preparing to run the EdgeSync service

Before you can subscribe your Edge Transport server to your Exchange organization, you need to make sure your infrastructure and your Mailbox servers are prepared for the EdgeSync service. To prepare for EdgeSync synchronization, you need to:

- Verify that the perimeter network firewall separating the Edge Transport server from the Exchange organization is configured to enable communications through the correct ports. The Edge Transport server uses non-standard LDAP ports. If your environment requires specific ports, you can modify the ports used by AD LDS using the ConfigureAdam.ps1 script provided with Exchange. For more information, see [Modify AD LDS configuration](modify-ad-lds-configuration-exchange-2013-help.md). Modify the ports before you create the Edge Subscription. If you modify the ports after you create the Edge Subscription, you will need to remove the Edge Subscription and then create a new Edge subscription. By default, the following LDAP ports are used to access AD LDS:

  - **LDAP**: Port 50389/TCP is used locally to bind to the AD LDS instance. This port doesn't have to be open on the perimeter network firewall.

  - **Secure LDAP**: Port 50636/TCP is used for directory synchronization from Mailbox servers to AD LDS. This port must be open on the firewall for successful EdgeSync synchronization.

- Verify that DNS host name resolution is successful from the Edge Transport server to the Mailbox servers and from the Mailbox servers to the Edge Transport server.

- License the Edge Transport server. The licensing information for the Edge Transport server is captured when the Edge Subscription is created. Subscribed Edge Transport servers need to be subscribed to the Exchange organization after the license key has been applied on the Edge Transport server. If the license key is applied on the Edge Transport server after you perform the Edge Subscription process, licensing information will not be updated in the Exchange organization, and you will need to resubscribe the Edge Transport server.

- Configure the following transport settings for propagation to the Edge Transport server:

  - **Internal SMTP servers**: Use the *InternalSMTPServers* parameter on the **Set-TransportConfig** cmdlet to specify a list of internal SMTP server IP addresses or IP address ranges to be ignored by the Sender ID and Connection Filtering agents on the Edge Transport server.

  - **Accepted domains**: Configure all authoritative domains, internal relay domains, and external relay domains.

  - **Remote domains**: Configure remote domain settings.

## Managing Edge Subscriptions

For step-by-step instructions on Edge Subscription management tasks, see [Manage Edge Subscriptions](manage-edge-subscriptions-exchange-2013-help.md).
