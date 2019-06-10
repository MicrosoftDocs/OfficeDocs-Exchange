---
title: 'Manually configure Edge Transport server mail flow: Exchange 2013 Help'
TOCTitle: Manually configure Edge Transport server mail flow
ms:assetid: cb4cc165-6c09-44ab-a95f-167ae8ed2485
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Dn606261(v=EXCHG.150)
ms:contentKeyID: 61200302
ms.date: 07/14/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Manually configure Edge Transport server mail flow

_**Applies to:** Exchange Server 2013_

This topic describes procedures for making manual configuration changes to how an Edge Transport Server manages mail flow. These procedures are intended to address specific scenarios; unless your organization has specific needs for making manual configuration changes, using the default configuration when subscribing Edge Transport servers is preferred.

**Contents**

Manually configure Send connectors

Intra-Organization Send Connectors

Create additional Send connectors after Edge subscription

Reasons to suppress automatic creation of Send connectors

Partition mail flow

Route outbound email to a smart host

Configure Send connectors for external relay domains

## Manually configure Send connectors

You can manually modify a Send connector's configuration. For example, if you need to route outbound email through a smart host, you can suppress automatic creation of a Send connector and manually configure a Send connector to the Internet.

## Intra-Organization Send connectors

The Intra-Organization Send connector is an implicit and hidden Send connector that's automatically computed by Exchange and enables the Transport service on Mailbox servers within the same organization to relay messages to each other without using explicit Send connectors. Because a configuration object with an Active Directory site association exists in Active Directory for an Edge Subscription, the intra-organization Send connector will also be used to relay messages to that Edge Transport server.

Only Mailbox servers located in the subscribed Active Directory site can transfer email directly to or from the subscribed Edge Transport server. If you have a multi-site Active Directory forest and Exchange is deployed in more than one site, the Mailbox servers in non-subscribed sites will route outbound email to the subscribed site. A Mailbox server in the subscribed site will route outbound email to the Edge Transport server.

## Create additional Send connectors after Edge subscription

After an Edge Transport server is subscribed to an Active Directory site, cmdlets for creating and modifying Send connectors on the Edge Transport server are disabled. If you want to create a Send connector whose source server is the Edge Transport server, you can create the Send connector inside the Exchange organization. You can specify one or more Edge Subscriptions as the source server for a Send connector. You can't specify both Mailbox servers and Edge Subscriptions as source servers for the same Send connector. The Send connector will be replicated to the AD LDS instance on the Edge Transport server that's configured as a source server the next time configuration data is synchronized by EdgeSync. If you list more than one Edge Subscription as a source server, connections to that Send connector will be load balanced between the subscribed Edge Transport servers. Edge Transport servers need to be subscribed to the same Active Directory site for load balancing to occur. If Edge Subscriptions in different Active Directory sites are configured as source servers on the same Send connector, Edge Transport servers will route only to the closest source server.

You will need to manually create Send connectors if:

- You suppressed automatic creation of the Internet or inbound Send connectors.

- You have accepted domains in your organization that are configured as external relay domains.

## Reasons to suppress automatic creation of Send connectors

Depending on the topology of your Exchange organization, you may decide to suppress automatic creation of Send connectors. The following examples describe scenarios that require you to suppress automatic creation of Send connectors.

## Partition mail flow

If you decide to partition the inbound and outbound mail processing between two Edge Transport servers, one Edge Transport server is responsible for processing outbound mail flow and a second Edge Transport server is responsible for processing inbound mail flow. To do this, configure the Edge Subscriptions as follows:

- For the outbound Edge Transport server, run the following command on the Mailbox server.

  ```powershell
  New-EdgeSubscription -FileData ([byte[]]$(Get-Content -Path "C:\EdgeServerSubscription.xml" -Encoding Byte -ReadCount 0)) -Site "Site-A" -CreateInboundSendConnector $false -CreateInternetSendConnector $true
  ```

- For the inbound Edge Transport server, run the following command on the Mailbox server.

  ```powershell
  New-EdgeSubscription -FileData ([byte[]]$(Get-Content -Path "C:\EdgeServerSubscription.xml" -Encoding Byte -ReadCount 0)) -Site "Site-A" -CreateInboundSendConnector $true -CreateInternetSendConnector $false
  ```

## Route outbound email to a smart host

If your Exchange organization routes all outbound email through a smart host, the automatically created Send connector won't have the correct configuration.

Run the following command on the Mailbox server to suppress automatic creation of the Send connector to the Internet.

```powershell
    New-EdgeSubscription -FileData ([byte[]]$(Get-Content -Path "C:\EdgeServerSubscription.xml" -Encoding Byte -ReadCount 0)) -Site "Site-A" -CreateInternetSendConnector $false
```

After the Edge Subscription process is complete, manually create a Send connector to the Internet. Create the Send connector inside the Exchange organization, and select the Edge Subscription as the source server for the connector. Select the `Custom` usage type and configure one or more smart hosts. This new Send connector will be replicated to the AD LDS instance on the Edge Transport server the next time EdgeSync synchronizes configuration data. You can force immediate EdgeSync synchronization by running the **Start-EdgeSynchronization** cmdlet on a Mailbox server.

Example: Using the Shell to configure a Send connector for a subscribed Edge Transport server to route messages for all Internet address spaces through a smart host. Run this task on a Mailbox server inside the Exchange organization, not on the Edge Transport server.

```powershell
New-SendConnector -Name "EdgeSync - Site-A to Internet" -Usage Custom -AddressSpaces SMTP:*;100 -DNSRoutingEnabled $false -SmartHosts 192.168.10.1 -SmartHostAuthMechanism None -SourceTransportServers EdgeSubscriptionName
```

> [!IMPORTANT]
> This example doesn't specify any smart host authentication mechanism. Make sure you configure the correct authentication mechanism and provide all necessary credentials when you create a smart host connector in your own Exchange organization.

## Configure Send connectors for external relay domains

If you have accepted domains in your Exchange organization that are configured as external relay domains, you need to manually create a Send connector for those address spaces. Messages being delivered to external relay domains are relayed by the Edge Transport server. The Edge Subscription process doesn't automatically create and configure Send connectors for external relay domains. Therefore, you need to configure Send connectors for those domains and specify one or more Edge Subscriptions as the source server for those Send connectors.

The DNS MX resource record for an external relay domain resolves to your Edge Transport server. You can configure a Send connector that relays email to an external relay domain to use a smart host for routing. Configuring the Send connector for an external relay domain to use DNS routing will create a routing loop. For more information about external relay domains, see [Accepted domains](accepted-domains-exchange-2013-help.md).
