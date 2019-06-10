---
localization_priority: Normal
description: 'Summary: Learn about subscribing an Edge Transport server to your internal Exchange Server 2016 or Exchange Server 2019 organization, which provides end-to-end mail flow, recipient look-up, and safelist aggregation.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 3addd71a-4165-401f-a009-002bcd8baba6
ms.date: 7/9/2018
ms.reviewer: 
title: Edge Subscriptions
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Edge Subscriptions

Edge Subscriptions are used to populate the Active Directory Lightweight Directory Services (AD LDS) instance on the Edge Transport server with Active Directory data. Although creating an Edge Subscription is optional, subscribing an Edge Transport server to the Exchange organization provides a simpler management experience and enhances antispam features. You need to create an Edge Subscription if you plan to use recipient lookup or safelist aggregation, or if you plan to help secure SMTP communications with partner domains by using Mutual Transport Layer Security (MTLS).

## Edge Subscription process

An Edge Transport server doesn't have direct access to Active Directory. The configuration and recipient information the Edge Transport server uses to process messages is stored locally in AD LDS. Creating an Edge Subscription establishes secure, automatic replication of information from Active Directory to AD LDS. The Edge Subscription process provisions the credentials used to establish a secure LDAP connection between the internal Exchange Mailbox servers and a subscribed Edge Transport server. The Microsoft Exchange EdgeSync service (EdgeSync) that runs on Mailbox servers performs periodic one-way synchronization to transfer up-to-date data to AD LDS. This reduces the administration tasks you perform in the perimeter network by letting you configure the Mailbox server and then synchronize that information to the Edge Transport server.

You subscribe an Edge Transport server to the Active Directory site that contains the Mailbox servers responsible for transferring messages to and from your Edge Transport servers. The Edge Subscription process creates an Active Directory site membership affiliation for the Edge Transport server. The site affiliation enables Mailbox servers in the Exchange organization to relay messages to the Edge Transport server for delivery to the Internet without having to configure explicit Send connectors.

One or more Edge Transport servers can be subscribed to a single Active Directory site. However, an Edge Transport server can't be subscribed to more than one Active Directory site. If you have more than one Edge Transport server deployed, each server can be subscribed to a different Active Directory site. Each Edge Transport server requires an individual Edge Subscription.

To deploy an Edge Transport server and subscribe it to an Active Directory site, follow these steps:

1. Install the Edge Transport server role.

2. Prepare for the Edge Subscription:

   - License the Edge Transport server.

   - Open ports in the firewall for mail flow and EdgeSync synchronization.

   - Verify that the Mailbox servers and the Edge Transport server can locate one another using DNS name resolution.

   - On the Mailbox Server, configure the transport settings to be replicated to the Edge Transport server.

3. On the Edge Transport server, create and export an Edge Subscription file by running the **New-EdgeSubscription** cmdlet.

4. Copy the Edge Subscription file to a Mailbox server or a file share that's accessible from the Active Directory site containing your Mailbox servers.

5. Import the Edge Subscription file to the Active Directory site by running the **New-EdgeSubscription** cmdlet on the Mailbox server.

### Prepare for the Edge Subscription

Before you can subscribe your Edge Transport server to your Exchange organization, you need to make sure your infrastructure and your Mailbox servers are prepared for the EdgeSync synchronization. To prepare for EdgeSync, you need to:

- **License the Edge Transport server**: The licensing information for the Edge Transport server is captured when the Edge Subscription is created. Subscribed Edge Transport servers need to be subscribed to the Exchange organization after the license key has been applied on the Edge Transport server. If the license key is applied on the Edge Transport server after you perform the Edge Subscription process, licensing information will not be updated in the Exchange organization, and you will need to resubscribe the Edge Transport server.

- **Verify that the required ports are open in the firewall**: The following ports are used by subscribed Edge Transport servers:

   - **SMTP**: Port 25/TCP must be open for inbound and outbound mail flow between the Internet and the Edge Transport server, and between the Edge Transport server and the internal Exchange organization.

   - **Secure LDAP**: Non-standard port 50636/TCP is used for directory synchronization from Mailbox servers to AD LDS on the Edge Transport server. This port is required for successful EdgeSync synchronization.

   > [!NOTE]
   > Port 50389/TCP is used locally by LDAP to bind to the AD LDS instance. This port doesn't have to be open on the firewall; it's used locally on the Edge Transport server.

   If your environment requires specific ports, you can modify the ports used by AD LDS using the `ConfigureAdam.ps1` script provided with Exchange. Modify the ports before you create the Edge Subscription. If you modify the ports after you create the Edge Subscription, you need to remove the Edge Subscription and create another one.

- **Verify that DNS host name resolution is successful from the Edge Transport server to the Mailbox servers and from the Mailbox servers to the Edge Transport server**

- **Configure the following transport settings for propagation to the Edge Transport server**

   - **Internal SMTP servers**: Use the _InternalSMTPServers_ parameter on the **Set-TransportConfig** cmdlet to specify a list of internal SMTP server IP addresses or IP address ranges to be ignored by the Sender ID and Connection Filtering agents on the Edge Transport server.

   - **Accepted domains**: Configure all authoritative domains, internal relay domains, and external relay domains.

   - **Remote domains**: Configure the settings for the default remote domain object (used for recipients in all remote domains), and configure remote domain objects as required for recipients in specific remote domains.

### Create and export an Edge Subscription file on the Edge Transport server

When you create an Edge Subscription file by running the **New-EdgeSubscription** cmdlet on the Edge Transport server, the following actions occur:

- An AD LDS account called the EdgeSync bootstrap replication account (ESBRA) is created. These ESBRA credentials are used to authenticate the first EdgeSync connection to the Edge Transport server. This account is configured to expire 24 hours after being created. Therefore, you need to complete the five-step subscription process described in the previous section within 24 hours. If the ESBRA expires before the Edge Subscription process is complete, you will need to run the **New-EdgeSubscription** cmdlet again to create a new Edge Subscription file.

- The ESBRA credentials are retrieved from AD LDS and written to the Edge Subscription file. The public key for the Edge Transport server's self-signed certificate is also exported to the Edge Subscription file. The credentials written to the Edge Subscription file are specific to the server that exported the file.

- Any previously created configuration objects on the Edge Transport server that will now be replicated to AD LDS from Active Directory are deleted from AD LDS, and the Exchange Management Shell cmdlets used to configure those objects are disabled. However, you can still use the **Get-\*** cmdlets to view those objects. Running the **New-EdgeSubscription** cmdlet disables the following cmdlets on the Edge Transport server:

   - **Set-SendConnector**

   - **New-SendConnector**

   - **Remove-SendConnector**

   - **New-AcceptedDomain**

   - **Set-AcceptedDomain**

   - **Remove-AcceptedDomain**

   - **New-RemoteDomain**

   - **Set-RemoteDomain**

   - **Remove-RemoteDomain**

This example creates and exports the Edge Subscription file on the Edge Transport server.

```
New-EdgeSubscription -FileName "C:\Data\EdgeSubscriptionInfo.xml"
```

> [!NOTE]
> When you run the **New-EdgeSubscription** cmdlet on the Edge Transport server, you receive a prompt to acknowledge the commands that will be disabled and the configuration that will be overwritten on the Edge Transport server. To bypass this confirmation, you need to use the _Force_ parameter. This parameter is useful when you script the **New-EdgeSubscription** cmdlet. You can also use the _Force_ parameter to overwrite an existing file when you resubscribe an Edge Transport server.

### Import the Edge Subscription file on a Mailbox server

When you import the Edge Subscription file to the Active Directory site by running the **New-EdgeSubscription** cmdlet on a Mailbox server, the following actions occur:

- The Edge Subscription is created, joining the Edge Transport server to the Exchange organization. EdgeSync will propagate configuration data to this Edge Transport Server, creating an Edge configuration object in Active Directory.

- Each Mailbox server in the Active Directory site receives notification from Active Directory that a new Edge Transport server has been subscribed. The Mailbox server retrieves the ESBRA from the Edge Subscription file. The Mailbox server then encrypts the ESBRA by using the public key of the Edge Transport server's self-signed certificate. The encrypted credentials are then written to the Edge configuration object.

- Each Mailbox server also encrypts the ESBRA using its own public key and then stores the credentials in its own configuration object.

- EdgeSync replication accounts (ESRAs) are created in Active Directory for each Edge Transport-Mailbox server pair. Each Mailbox server stores its ESRA credentials as an attribute of the Mailbox server configuration object.

- Send connectors are automatically created to relay messages outbound from the Edge Transport server to the Internet, and inbound from the Edge Transport server to the Exchange organization. For more information, see the [Send connectors created automatically by the Edge Subscription](#send-connectors-created-automatically-by-the-edge-subscription) section in this topic.

- The Microsoft Exchange EdgeSync service that runs on Mailbox servers uses the ESBRA credentials to establish a secure LDAP connection between a Mailbox server and the Edge Transport server, and performs the initial replication of data. The following data is replicated to AD LDS:

   - Topology data

   - Configuration data

   - Recipient data

   - ESRA credentials

- The Microsoft Exchange Credential Service that runs on the Edge Transport server installs the ESRA credentials. These credentials are used to authenticate and secure later synchronization connections.

- The EdgeSync synchronization schedule is established.

- The Microsoft Exchange EdgeSync service running on the Mailbox servers in the subscribed Active Directory site then performs one-way replication of data from Active Directory to AD LDS on a regular schedule. You can also use the **Start-EdgeSynchronization** cmdlet to override the EdgeSync synchronization schedule and immediately start synchronization.

This example subscribes an Edge Transport server to the specified site and automatically creates the Internet Send connector and the Send connector from the Edge Transport server to the Mailbox servers.

```
New-EdgeSubscription -FileData ([byte[]]$(Get-Content -Path "C:\Data\EdgeSubscriptionInfo.xml" -Encoding Byte -ReadCount 0)) -Site "Default-First-Site-Name"
```

> [!NOTE]
> The default values of the _CreateInternetSendConnector_ and _CreateInboundSendConnector_ parameters are both `$true`, so you don't need to use them in this command.

## Send connectors created automatically by the Edge Subscription

By default, when you import the Edge Subscription file to a Mailbox server, the Send connectors required to enable end-to-end mail flow between the Internet and the Exchange organization are created automatically, and any existing Send connectors on the Edge Transport server are deleted.

The Edge Subscription creates the following Send connectors:

- A Send connector named EdgeSync - Inbound to \<_Site Name_\> that's configured to relay messages from the Edge Transport server to the Exchange organization.

- A Send connector named EdgeSync - \<_Site Name_\> to Internet that's configured to relay messages from the Exchange organization to the Internet.

Also, subscribing an Edge Transport server to the Exchange organization allows the Mailbox servers in the subscribed Active Directory site to use the invisible and implicit intra-organization Send connector to relay messages to the Edge Transport server.

### Inbound Send connector to receive messages from the Internet

When you run the **New-EdgeSubscription** cmdlet on the Mailbox server, the _CreateInboundSendConnector_ parameter is set to the value `$true`. This creates the Send connector needed to send messages from the Edge Transport server to the Exchange organization. The following table shows the configuration of this Send connector.

**Automatic inbound Send connector configuration**

|**Property**|**Value**|
|:-----|:-----|
|_Name_|EdgeSync - Inbound to \< _Site Name_\>|
|_AddressSpaces_|`SMTP:--;1` <br/> The `--` value in the address space represents all authoritative and internal relay accepted domains for the Exchange organization. Any messages the Edge Transport server receives for these accepted domains are routed to this Send connector and relayed to the smart hosts.|
|_SourceTransportServers_|\< _Edge Subscription name_\>|
|_Enabled_|True|
|_DNSRoutingEnabled_|False|
|_SmartHosts_|`--` <br/> The `--` value in the list of smart hosts represents all Mailbox servers in the subscribed Active Directory site. Any Mailbox servers you add to the subscribed Active Directory site after you establish the Edge Subscription don't participate in the EdgeSync synchronization process. However, they are automatically added to the list of smart hosts for the automatically created inbound Send connector. If more than one Mailbox server is located in the subscribed Active Directory site, inbound connections will be load balanced across the smart hosts.|

You can't modify the address space or list of smart hosts at creation time for the automatically created inbound Send connector. However, you can set the _CreateInboundSendConnector_ parameter to the value `$false` when you create an Edge Subscription. This allows you to manually configure a Send connector from the Edge Transport server to the Exchange organization.

### Outbound Send connector to send messages to the Internet

When you run the **New-EdgeSubscription** cmdlet on the Mailbox server, the _CreateInternetSendConnector_ parameter is set to the value `$true`. This creates the Send connector needed to send messages from the Exchange organization to the Internet. The following table shows the default configuration of this Send connector.

**Automatic Internet Send connector configuration**

|**Property**|**Value**|
|:-----|:-----|
|_Name_|EdgeSync - \< _Site Name_\> to Internet|
|_AddressSpaces_|`SMTP:*;100`|
|_SourceTransportServers_|\< _Edge Subscription name_\> <br/> The name of the Edge Subscription is the same as the name of the subscribed Edge Transport server.|
|_Enabled_|True|
|_DNSRoutingEnabled_|True|
|_DomainSecureEnabled_|True|

If more than one Edge Transport server is subscribed to the same Active Directory site, no additional Send connectors to the Internet are created. Instead, all Edge Subscriptions are added to the same Send connector as the source server. This load balances outbound connections to the Internet across the subscribed Edge Transport servers.

The outbound Send connector is configured to send email messages from the Exchange organization to all remote SMTP domains, using DNS routing to resolve domain names to MX resource records.

## Details about the EdgeSync service

After you subscribe an Edge Transport server to an Active Directory site, EdgeSync will replicate configuration and recipient data to the Edge Transport servers. The service replicates the following data from Active Directory to AD LDS:

- Send connector configuration

- Accepted domains

- Remote domains

- Safe Senders Lists

- Blocked Senders Lists

- Recipients

- List of send and receive domains used in domain secure communications with partners

- List of SMTP servers listed as internal in your organization's transport configuration

- List of Mailbox servers in the subscribed Active Directory site

 EdgeSync uses a mutually authenticated and authorized secure LDAP channel to transfer data from the Mailbox server to the Edge Transport server.

To replicate data to AD LDS, the Mailbox server binds to a global catalog server to retrieve updated data. EdgeSync initiates a secure LDAP session between a Mailbox server and the subscribed Edge Transport server over the non-standard TCP port 50636.

When you first subscribe an Edge Transport server to an Active Directory site, the initial replication that populates AD LDS with data from Active Directory can take five minutes or more, depending on the quantity of data in the directory service. After initial replication, EdgeSync only synchronizes new and changed objects, and removes any deleted objects.

### Synchronization schedule

Different types of data synchronize on different schedules. The EdgeSync synchronization schedule specifies the maximum interval between EdgeSync synchronizations. EdgeSync synchronization occurs at the following intervals:

- Configuration data: 3 minutes.

- Recipient data: 5 minutes.

- Topology data: 5 minutes

If you want to change these intervals, use the **Set-EdgeSyncServiceConfig** cmdlet. Using the **Start-EdgeSynchronization** cmdlet on the Mailbox server to force Edge Subscription synchronization overrides the timer for the next scheduled EdgeSync synchronization, and starts EdgeSync immediately.

### Selection of Mailbox servers

Each subscribed Edge Transport server is associated with a particular Active Directory site. If more than one Mailbox server exists in the site, any of these Mailbox servers can replicate data to the subscribed Edge Transport servers. To avoid contention among Mailbox servers when synchronizing, the preferred Mailbox server is selected as follows:

1. The first Mailbox server in the Active Directory site to perform a topology scan and discover the new Edge Subscription performs the initial replication. Because this discovery is based on the timing of the topology scan, any Mailbox server in the site may perform the initial replication.

2. The Mailbox server performing the initial replication establishes an EdgeSync lease option and sets a lock on the Edge Subscription. The lease option establishes that particular Mailbox server as the preferred server providing synchronization services to that Edge Transport server. The lock prevents EdgeSync running on another Mailbox server from taking over the lease option.

3. The EdgeSync lease option lasts for one hour. During that hour, no other EdgeSync service can take over the option unless a manual synchronization is started before the end of the hour. If the preferred Mailbox server isn't available to provide EdgeSync service at the time manual synchronization is started, after a five-minute wait, the lock is released and another EdgeSync service can take over the lease option and perform synchronization.

4. Unless manual synchronization is started, synchronization occurs based on the EdgeSync synchronization schedule. If the preferred server isn't available when a scheduled synchronization occurs, after a five-minute wait, the lock is released and another EdgeSync service can take over the lease option and perform synchronization.

This method of locking and leasing prevents more than one instance of EdgeSync from pushing data to the same Edge Transport server at the same time.

**Notes**:

- In Exchange 2016 organizations, if you also have Exchange 2010 Hub Transport servers in the subscribed Active Directory site, Exchange 2016 Mailbox servers will always take precedence and perform the replication.

- When you subscribe an Edge Transport server to an Active Directory site, all Mailbox servers installed in that Active Directory site at that time can participate in the EdgeSync synchronization process. If one of those servers is removed, the EdgeSync service that's running on the remaining Mailbox servers will continue the data synchronization process. However, if you later install new Mailbox servers in the Active Directory site, they won't automatically participate in EdgeSync synchronization. If you want to enable those new Mailbox servers to participate in EdgeSync synchronization, you will need to subscribe the Edge Transport server again.

The following table lists the EdgeSync properties related to locking and leasing. You can use the **Set-EdgeSyncServiceConfig** cmdlet to configure these properties.

**EdgeSync lease properties**

|**Parameter**|**Default value**|**Description**|
|:-----|:-----|:-----|
|_LockDuration_|`00:05:00` (5 minutes)|This setting determines how long a particular EdgeSync service will acquire a lock. If the EdgeSync service on the Mailbox server that's holding this lock doesn't respond, after five minutes the EdgeSync service on another Mailbox server will take over the lease. Forcing immediate EdgeSync synchronization doesn't override this setting.|
|_OptionDuration_|`01:00:00` (1 hour)|This setting determines how long an EdgeSync service can declare a lease option on an Edge Transport server. If the EdgeSync service holding the lease is unavailable and doesn't restart during this option period, no other Exchange EdgeSync service will take over the lease option unless you force EdgeSync synchronization.|
|_LockRenewalDuration_|`00:01:00` (1 minute)|This setting determines how frequently the lock field is updated when an EdgeSync service has acquired a lock to an Edge Transport server.|
