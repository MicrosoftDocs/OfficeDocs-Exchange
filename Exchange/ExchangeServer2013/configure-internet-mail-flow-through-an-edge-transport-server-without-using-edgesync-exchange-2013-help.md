---
title: 'Configure internet mail flow through an Edge Transport server without using EdgeSync'
TOCTitle: Configure Internet mail flow through an Edge Transport server without using EdgeSync
ms:assetid: 6bb98d10-6f12-4b08-a58e-36375f605d65
ms:mtpsurl: https://technet.microsoft.com/library/Bb232082(v=EXCHG.150)
ms:contentKeyID: 61200290
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure Internet mail flow through an Edge Transport server without using EdgeSync

_**Applies to:** Exchange Server 2013_

We recommend you use the Edge Subscription process to establish mail flow between your Exchange organization and an Edge Transport server. However, certain situations may prevent you from subscribing the Edge Transport server to your Exchange organization using the Edge Subscription process. To manually establish mail flow between your Exchange organization and an Edge Transport server, you must create and configure the Send connectors and Receive connectors on the Edge Transport server and on the Mailbox servers in your Exchange organization.

## Before you begin

- Estimated time to complete this task: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry, the "Send connectors - Edge Transport" entry and the "Receive connectors - Edge Transport" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- This procedure uses Basic authentication over Transport Layer Security (TLS) to provide encryption and authentication. When you use Basic authentication over TLS, the receiving server must have an X.509 Secure Sockets Layer (SSL) server certificate installed. The fully qualified domain name (FQDN) value configured on the Receive connector must match the FQDN in the SSL server certificate. By default, the value of the FQDN on the Receive connector is the FQDN of the server that contains the Receive connector.

- You can also use the Externally Secured authentication method. However, if you do so, the communication between the Edge Transport server and Mailbox server isn't authenticated or encrypted by Exchange. We recommend you use the Externally Secured authentication method only when an additional encryption method is also used. The encryption method can be an Internet Protocol security (IPsec) association or a virtual private network (VPN).

- An Edge Transport server is typically *multihomed*. This means that the Edge Transport server has network adapters connected to multiple network segments. Each of these network adapters has a unique IP configuration. The network adapter that's connected to the external, or public, network segment should be configured to use a public Domain Name System (DNS) server for name resolution. This enables the server to resolve SMTP domain names to MX resource records and route mail to the Internet. The network adapter that's connected to the internal, or private, network segment should be configured to use a DNS server in the perimeter network or should have a Hosts file available.

- You need to create a user account in Active Directory and add the account to the universal security group on the Exchange Server computer. This account is used by the Send connector on the Edge Transport server to authenticate to the destination Mailbox server in the Exchange organization.

  > [!IMPORTANT]
  > This account is granted the permissions associated with the computers running Exchange Server. Make sure you safeguard the account credentials to prevent misuse of the account. You can configure the account to allow logon to specific computers only.

## Edge Transport Server Procedures

The following connectors are required on the Edge Transport server:

- A Send connector configured to send messages to the Internet

- A Send connector configured to send messages to the Mailbox servers in the Exchange organization

- A Receive connector configured to receive messages only from Mailbox servers in the Exchange organization

- A Receive connector configured to accept messages only from the Internet

By default, a single Receive connector is created during the installation of the Edge Transport server role. This connector can be used for both incoming Internet messages and incoming messages from the Mailbox servers. Typically, the Edge Subscription process automatically configures the correct permissions and authentication on the default Receive connector. When you don't use the Edge Subscription process, we recommend you modify the default Receive connector on the Edge Transport server to only accept messages from the Internet. You should then create a Receive connector on the Edge Transport server that's configured to only accept messages from internal Mailbox servers.

The following sections walk you through all the configuration steps required to prepare your Edge Transport server to communicate with your Exchange organization.

> [!NOTE]
> You can only use the Shell to perform these procedures on Edge Transport servers.

## Step 1: Create a Send connector configured to send messages to the Internet

This Send connector requires the following configuration:

- **Name**: To Internet (or any descriptive name)

- **Usage type**: Internet

- **Address spaces**: "\*" (all domains)

- **Network settings**: Use DNS MX records to route mail automatically. Depending on your network configuration, you can also route mail through a smart host. The smart host then routes mail to the Internet.

To create a Send connector that's configured to send messages to the Internet, run the following command.

```powershell
New-SendConnector -Name "To Internet" -AddressSpaces * -Usage Internet -DNSRoutingEnabled $true
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/New-SendConnector).

## Step 2: Create a Send connector configured to send messages to the Exchange organization

Use the **New-SendConnector** cmdlet to create a Send connector.

> [!NOTE]
> Before you create the Send connector, you first need to run the <STRONG>Get-Credential</STRONG> command to save the username and password you will use in a temporary variable. You need to do this because the <STRONG>New-SendConnector</STRONG> cmdlet will not accept user credentials in plain text.

This Send connector requires the following configuration:

- **Name**: To Internal Org (or any descriptive name)

- **Usage type**: Internal

- **Address spaces**: All accepted domains for the Exchange organization. For example, \*.contoso.com.

- DNS routing disabled (smart host routing enabled)

- **Smart hosts**: FQDN of one or more Mailbox servers as smart hosts. For example, mbxserver01.contoso.com and mbxserver02.contoso.com.

- **Smart host authentication methods**: Basic authentication over TLS

- **Smart host authentication credentials**: Credentials for the user account in the internal domain. You first need to save the username and password in a temporary variable, because the **New-SendConnector** cmdlet will not accept user credentials in plain text.

To create a Send connector configured to send messages to the Exchange organization, run the following commands.

```powershell
$MailboxCredentials = Get-Credential
New-SendConnector -Name "To Internal Org" -Usage Internal -AddressSpaces *.contoso.com -DNSRoutingEnabled $false -SmartHosts mbxserver01.contoso.com,mbxserver02.contoso.com -SmartHostAuthMechanism BasicAuthRequireTLS -AuthenticationCredential $MailboxCredentials
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/New-SendConnector).

## Step 3: Modify the default Receive connector to only accept messages from the Internet

You should make the following configuration changes to the default Receive connector:

- Modify the name to reflect that the connector will be used solely to receive email from the Internet. The name of the default Receive connector is "Default internal Receive connector \<Edge Transport server name\>".

- Change the network bindings to accept messages only from the network adapter that is accessible from the Internet. For example, 10.1.1.1 and the standard SMTP TCP port value of 25.

To modify the default Receive connector to only accept messages from the Internet, run the following command.

```powershell
Set-ReceiveConnector "Default internal Receive connector Edge01" -Name "From Internet" -Bindings 10.1.1.1:25
```

For detailed syntax and parameter information, see [Set-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/Set-ReceiveConnector).

## Step 4: Create a Receive connector configured to only accept messages from the Exchange organization

This Receive connector requires the following configuration:

- **Name**: From Internal Org (or any descriptive name)

- **Usage type**: Internal

- **Local network bindings**: Internal network-facing network adapter. For example, 10.1.1.2 and the standard SMTP TCP port value of 25.

- **Remote network settings**: IP address of one or more Mailbox servers in the Exchange organization. For example, 192.168.5.10 and 192.168.5.20.

- **Authentication methods**: TLS, Basic authentication, Basic authentication over TLS, and Exchange Server authentication.

To create a Receive connector configured to only accept messages from the Exchange organization, run the following command.

```powershell
New-ReceiveConnector -Name "From Internal Org" -Usage Internal -AuthMechanism TLS,BasicAuth,BasicAuthRequireTLS,ExchangeServer -Bindings 10.1.1.2:25 -RemoteIPRanges 192.168.5.10,192.168.5.20
```

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).

## How do you know these steps worked?

To verify that you have successfully configured the required Send connectors and Receive connectors, run the following commands on the Edge Transport server and verify the values displayed are the values you configured.

```powershell
Get-SendConnector | Format-List Name,Usage,AddressSpaces,SourceTransportServers,DSNRoutingEnabled,SmartHosts,SmartHostAuthMechanism
Get-ReceiveConnector | Format-List Name,Usage,AuthMechanism,Bindings,RemoteIPRanges
```

## Mailbox server procedures

Mailbox servers in your organization require a Send connector configured to send messages to the Edge Transport server for relay to the Internet.

By default, two Receive connectors are created during the installation of the Mailbox server role. The connector named Client *ServerName* is configured to accept messages from all POP3 and IMAP messaging clients. The connector named Default *ServerName* is configured to accept messages from an Edge Transport server. No modifications to these connectors are required.

## Step 5: Create a Send connector configured to send outgoing messages to the Edge Transport server

This Send connector requires the following configuration:

- **Name**: To Edge (or any descriptive name)

- **Usage type**: Internal

- **Address spaces**: "\*" (all domains)

- DNS routing disabled (smart host routing enabled)

- **Smart hosts**: IP address or FQDN of the Edge Transport server. For example, edge01.contoso.net.

- **Source Mailbox servers**: FQDN of one or more Mailbox servers. For example, mbxserver01.contoso.com and mbxserver02.contoso.com.

- **Smart host authentication method**: Basic authentication over TLS.

- **Smart host authentication credentials**: Credentials for the user account on the Edge Transport server. You first need to save the username and password in a temporary variable, because the **New-SendConnector** cmdlet will not accept user credentials in plain text.

To create a Send connector configured to send outgoing messages to the Edge Transport server, run the following commands.

```powershell
$EdgeCredentials = Get-Credential
New-SendConnector -Name "To Edge" -Usage Internal -AddressSpaces * -DNSRoutingEnabled $false -SmartHosts edge01.contoso.com -SourceTransportServers mbxserver01.contoso.com,mbxserver02.contoso.com -SmartHostAuthMechanism BasicAuthRequireTLS -AuthenticationCredential $EdgeCredentials
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/New-SendConnector).

## How do you know this step worked?

To verify that you have successfully created a Send connector configured to send outgoing messages to the Edge Transport server, run the following command on a Mailbox server and verify the values displayed are the values you configured.

```powershell
Get-SendConnector | Format-List Name,Usage,AddressSpaces,DSNRoutingEnabled,SmartHosts,SourceTransportServers,SmartHostAuthMechanism
```
