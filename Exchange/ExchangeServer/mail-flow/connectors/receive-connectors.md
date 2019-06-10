---
localization_priority: Normal
description: 'Summary: Learn about Receive connectors in Exchange Server 2016 or Exchange Server 2019, and how they control mail flow into your Exchange organization.'
ms.topic: overview
author: chrisda
ms.author: chrisda
ms.assetid: 17751a60-39fe-433f-84d2-bfc14ff4ba51
ms.date: 7/6/2018
ms.reviewer: 
title: Receive connectors
ms.collection:
- Strat_EX_Admin
- exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Receive connectors

Exchange servers use Receive connectors to control inbound SMTP connections from:

- Messaging servers that are external to the Exchange organization.

- Services in the transport pipeline on the local Exchange server or on remote Exchange servers.

- Email clients that need to use authenticated SMTP to send messages.

You can create Receive connectors in the Transport service on Mailbox servers, the Front End Transport service on Mailbox servers, and on Edge Transport servers. By default, the Receive connectors that are required for inbound mail flow are created automatically when you install an Exchange Mailbox server, and when you subscribe an Edge Transport server to your Exchange organization.

A Receive connector is associated with the Mailbox server or Edge Transport server where it's created, and determines how that specific server listens for SMTP connections. On Mailbox servers, the Receive connector is stored in Active Directory as a child object of the server. On Edge Transport servers, the Receive connector is stored in Active Directory Lightweight Directory Services (AD LDS).

These are the important settings on Receive connectors:

- **Local adapter bindings**: Configure the combination of local IP addresses and TCP ports that the Receive connector uses to accept connections.

- **Remote network settings**: Configure the source IP addresses that the Receive connector listens to for connections.

- **Usage type**: Configure the default permission groups and smart host authentication mechanisms for the Receive connector.

- **Permission groups**: Configure who's allowed to use the Receive connector, and the permissions that they receive.

A Receive connector listens for inbound connections that match the configuration settings of the connector. Each Receive connector on the Exchange server uses a unique combination of local IP address bindings, TCP ports, and remote IP address ranges that define if and how connections from SMTP clients or servers are accepted.

Although the default Receive connectors are adequate in most cases, you can create custom Receive connectors for specific scenarios. For example:

- To apply special properties to an email source, for example, a larger maximum message size, more recipients per message or more simultaneous inbound connections.

- To accept encrypted mail by using a specific TLS certificate.

On Mailbox servers, you can create and manage Receive connectors in the Exchange admin center (EAC) or in the Exchange Management Shell. On Edge Transport servers, you can only use the Exchange Management Shell.

## Receive connector changes in Exchange Server
<a name="WhatsNew"> </a>

These are the notable changes to Receive connectors in Exchange 2016 and Exchange 2019 compared to Exchange 2010:

- The _TlsCertificateName_ parameter allows you to specify the certificate issuer and the certificate subject. This helps minimize the risk of fraudulent certificates.

- The _TransportRole_ parameter allows you to distinguish between frontend (Client Access) and backend connectors on Mailbox servers.

## Default Receive connectors created during setup
<a name="DefaultConnectors"> </a>

Several different Receive connectors are created by default when you install Exchange. By default, these connectors are enabled, and protocol logging is disabled for most of them. For more information about protocol logging on Receive connectors, see [Protocol logging](protocol-logging.md).

### Default Receive connectors in the Front End Transport service on Mailbox servers

The primary function of Receive connectors in the Front End Transport service is to accept anonymous and authenticated SMTP connections into your Exchange organization. The **TransportRole** property value for these connectors is `FrontendTransport`. The Front End Transport service relays or *proxies* these connections to the Transport service for categorization and routing to the final destination.

The default Receive connectors that are created in the Front End Transport service on Mailbox servers are described in the following table.

|**Name**|**Description**|**Protocol logging**|**TCP Port**|**Local IP address bindings**|**Remote IP address ranges**|**Authentication mechanisms**|**Permission groups**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Client Frontend _\<ServerName\>_|Accepts connections from authenticated SMTP clients.|None|587|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| `TLS` <br/> `BasicAuth` <br/> ` BasicAuthRequireTLS ` <br/> `Integrated`| `ExchangeUsers`|
|Default Frontend _\<ServerName\>_|Accepts anonymous connections from external SMTP servers. This is the common messaging entry point into your Exchange organization.|Verbose|25|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| `TLS` <br/> `BasicAuth` <br/> ` BasicAuthRequireTLS ` <br/> ` ExchangeServer ` <br/> `Integrated`| `AnonymousUsers` <br/> `ExchangeLegacyServers` <br/> `ExchangeServers`|
|Outbound Proxy Frontend _\<ServerName\>_|Accepts authenticated connections from the Transport service on Mailbox servers. The connections are encrypted with the Exchange server's self-signed certificate.  <br/> This connector is used only if the Send connector is configured to use outbound proxy. For more information, see [Configure Send connectors to proxy outbound mail](proxy-outbound-mail.md).|None|717|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| `TLS` <br/> `BasicAuth` <br/> ` BasicAuthRequireTLS ` <br/> ` ExchangeServer ` <br/> `Integrated`| `ExchangeServers`|

### Default Receive connectors in the Transport service on Mailbox servers

The primary function of Receive connectors in the Transport service is to accept authenticated and encrypted SMTP connections from other transport services on the local Mailbox server or remote Mailbox servers in your organization. The **TransportRole** property value on theses connectors is `HubTransport`. Clients don't directly connect to these connectors.

The default Receive connectors that are created in the Transport service on Mailbox servers are described in the following table.

|**Name**|**Description**|**Protocol logging**|**TCP Port**|**Local IP address bindings**|**Remote IP address ranges**|**Authentication mechanisms**|**Permission groups**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Client Proxy _\<ServerName\>_|Accepts authenticated client connections that are proxied from the Front End Transport service.|None|465|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| `TLS` <br/> `BasicAuth` <br/> ` BasicAuthRequireTLS ` <br/> ` ExchangeServer ` <br/> `Integrated`| `ExchangeServers` <br/> `ExchangeUsers`|
|Default _\<ServerName\>_|Accepts authenticated connections from:  <br/> • The Front End Transport service on the local or remote Mailbox servers  <br/> • The Transport service on remote Mailbox servers  <br/> • The Mailbox Transport service on the local or remote Mailbox servers  <br/> • Edge Transport servers  <br/> The connections are encrypted with the Exchange server's self-signed certificate.|None|2525|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| `TLS` <br/> `BasicAuth` <br/> `ExchangeServer` <br/> `Integrated`| `ExchangeLegacyServers` <br/> `ExchangeServers` <br/> `ExchangeUsers`|

### Default Receive connectors in the Transport service on Edge Transport servers

The primary function of the Receive connector on Edge Transport servers is to accept mail from the Internet. Subscribing the Edge Transport server to your Exchange organization automatically configures the connector permissions and authentication mechanisms that are required for Internet mail flow to and from your organization. For more information, see [Edge Transport servers](../../architecture/edge-transport-servers/edge-transport-servers.md).

The default Receive connector that's created in the Transport service on Edge Transport servers is described in the following table.

|**Name**|**Description**|**Protocol logging**|**TCP Port**|**Local IP address bindings**|**Remote IP address ranges**|**Authentication mechanisms**|**Permission groups**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Default internal receive connector _\<ServerName\>_|Accepts anonymous connections from external SMTP servers.|None|25|All available IPv4 addresses (`0.0.0.0`)| `{0.0.0.0-255.255.255.255}` (all IPv4 addresses)| `TLS` <br/> `ExchangeServer`| `AnonymousUsers` <br/> `ExchangeServers` <br/> `Partners`|

### Implicit Receive connectors in the Mailbox Transport Delivery service on Mailbox servers
<a name="ImplicitReceiveConnectors"> </a>

In addition to the Receive connectors are created during the installation of Exchange servers, there's a special *implicit Receive connector* in the Mailbox Transport Delivery service on Mailbox servers. This implicit Receive connector is automatically available, invisible, and requires no management. The primary function of this connector is to accept mail from the Transport service on the local Mailbox server or remote Mailbox servers in your organization.

The implicit Receive connector that exists in the Mailbox Transport Delivery service on Mailbox servers is described in the following table.

|**Name**|**Description**|**Protocol logging**|**TCP Port**|**Local IP address bindings**|**Remote IP address ranges**|**Authentication mechanisms**|**Permission groups**|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|Mailbox delivery Receive connector|Accepts authenticated connections from the Transport service on the local or remote Mailbox servers.|None|475|All available IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`)| `{::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff, 0.0.0.0-255.255.255.255}` (all IPv4 and IPv6 addresses)| ` ExchangeServer `| `ExchangeServers`|

## Receive connector local address bindings
<a name="Bindings"> </a>

Local address bindings restrict the Receive connector to listen for SMTP connections on a specific local IP address (network adapter) and TCP port. Typically, the combination of local IP address and TCP port is unique for every Receive connector on a server. However, multiple Receive connectors on a server can have the same local IP addresses and TCP ports if the remote IP address ranges are different. For more information, see the [Receive connector remote addresses](receive-connectors.md#RemoteAddresses) section.

By default, a Receive connector listens for connections on all available local IPv4 and IPv6 addresses (`0.0.0.0` and `[::]:`). If the server has multiple network adapters, you can configure Receive connectors to accept connections only from IP addresses that are configured for a specific network adapter. For example, on an Internet-facing Exchange server, you can have a Receive connector that's bound to the IP address of the external network adapter to listen for anonymous Internet connections. You can have a separate Receive connector that's bound to the IP address of the internal network adapter to listen for authenticated connections from internal Exchange servers.

> [!NOTE]
> If you bind a Receive connector to a specific IP address, make sure that the address is configured on a local network adapter. If you specify an invalid local IP address, the Microsoft Exchange Transport service may fail to start when the server or service is restarted.

In the EAC, you use the **Network adapter bindings** field to configure the local address bindings in the new Receive connector wizard, or on the **Scoping** tab in the properties of existing Receive connectors. In the Exchange Management Shell, you use the _Bindings_ parameter on the **New-ReceiveConnector** and **Set-ReceiveConnector** cmdlets. Depending on the usage type that you select, you might not be able to configure the local address bindings when you create the Receive connector, but you can modify them after you create the Receive connector. The affected usage types are identified in the [Receive connector usage types](receive-connectors.md#UsageTypes) section.

## Receive connector remote addresses
<a name="RemoteAddresses"> </a>

Remote addresses define from where the Receive connector receives SMTP connections. By default, Receive connectors listen for connections from all IPv4 and IPv6 addresses (0.0.0.0-255.255.255.255 and ::-ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff). If you create a custom Receive connector to receive mail from a specific source, configure the connector to listen for connections only from the specific IP address or address ranges.

 Multiple Receive connectors on the server can have overlapping remote IP address ranges as long as one range is completely overlapped by another. When remote IP address ranges overlap, the remote IP address range that has the most specific match to the connecting server's IP address is used.

For example, consider the following Receive connectors in the Front End Transport service on the server named Exchange01:

- **Connector name**: Client Frontend Exchange01

  - **Network adapter bindings**: All available IPv4 on port 25.

  - **Remote network settings**: 0.0.0.0-255.255.255.255

- **Connector name**: Custom Connector A

  - **Network adapter bindings**: All available IPv4 on port 25.

  - **Remote network settings**: 192.168.1.0-192.168.1.255

- **Connector name**: Custom Connector B

  - **Network adapter bindings**: All available IPv4 on port 25.

  - **Remote network settings**: 192.168.1.75

SMTP connections from 192.168.1.75 are accepted by Custom Connector B, because that connector has the most specific IP address match.

SMTP connections from 192.168.1.100 are accepted by Custom Connector A, because that connector has the most specific IP address match.

In the EAC, you use the **Remote network settings** field to configure the remote IP addresses in the new Receive connector wizard, or on the **Scoping** tab in the properties of existing Receive connectors. In the Exchange Management Shell, you use the _RemoteIPRanges_ parameter on the **New-ReceiveConnector** and **Set-ReceiveConnector** cmdlets.

## Receive connector usage types
<a name="UsageTypes"> </a>

The usage type determines the default security settings for the Receive connector. The usage type specifies who is authorized to use the connector, the permissions they get, and the authentication methods that are supported.

When you use the EAC to create Receive connectors, the wizard prompts you to select the **Type** value for the connector. When you use the **New-ReceiveConnector** cmdlet in the Exchange Management Shell, you use the _Usage_ parameter with one of the available values (for example, `-Usage Custom`), or the designated switch for the usage type (for example, `-Custom`).

You can specify the connector usage type only when you create Receive connectors. After you create a connector, you can modify the available authentication mechanisms and permission groups in the EAC, or by using the **Set-ReceiveConnector** cmdlet in the Exchange Management Shell.

The available usage types are described in the following table.

|**Usage type**|**Permission groups assigned**|**Authentication mechanisms available**|**Comments**|
|:-----|:-----|:-----|:-----|
|Client|**Exchange users** (`ExchangeUsers`)|**Transport Layer Security** (`TLS`)  <br/> **Basic authentication** (`BasicAuth`)  <br/> **Offer basic authentication only after starting TLS** (`BasicAuthRequireTLS`)  <br/> **Integrated Windows authentication** (`Integrated`)|Used by POP3 and IMAP4 clients that need to submit email messages by using authenticated SMTP.  <br/> When you create a Receive connector of this usage type in the EAC or in the Exchange Management Shell, you can't select the local IP address bindings or TCP port. By default, this usage type is bound to all local IPv4 and IPv6 addresses on TCP port 587. You can change these bindings after you create the connector.  <br/> This usage type isn't available on Edge Transport servers.|
|Custom|None selected (`None`)|**Transport Layer Security** (`TLS`)|Used in cross-forest scenarios, for receiving mail from third-party messaging servers, and for external relay.  <br/> After you create a Receive connector of this usage type, you need to add permissions groups in the EAC or in the Exchange Management Shell.|
|Internal|**Legacy Exchange servers** (`ExchangeLegacyServers`)  <br/> **Exchange servers** (`ExchangeServers`)|**Transport Layer Security** (`TLS`)  <br/> **Exchange Server authentication** (`ExchangeServers`)|Used in cross-forest scenarios, for receiving mail from previous versions of Exchange, for receiving mail from third-party messaging servers, or on Edge Transport servers to receive outbound mail from the internal Exchange organization.  <br/>  When you create a Receive connector of this usage type in the EAC or in the Exchange Management Shell, you can't select the local IP address bindings or TCP port. By default, the connector is bound to all local IPv4 and IPv6 addresses on TCP port 25. You can change these bindings after you create the connector.  <br/> The `ExchangeLegacyServers` permission group isn't available on Edge Transport servers.|
|Internet|**Anonymous users** (`AnonymousUsers`)|**Transport Layer Security** (`TLS`)|Used to receive mail from the Internet.  <br/>  When you create a Receive connector of this usage type in the EAC or in the Exchange Management Shell, you can't select the remote IP addresses. By default, the connector accepts remote connections from all IPv4 addresses (0.0.0.0-255.255.255.255). You can change these bindings after you create the connector.|
|Partner|**Partners** (`Partners`)|**Transport Layer Security** (`TLS`)|Used to configure secure communication with an external partner (mutual TLS authentication, also known as domain secure).|

## Receive connector authentication mechanisms
<a name="AuthMechanisms"> </a>

Authentication mechanisms specify the logon and encryption settings that are used for incoming SMTP connections. You can configure multiple authentication mechanisms for a Receive connector. In the EAC, authentication mechanisms are available in the **Security** tab in the properties of the Receive connector. In the Exchange Management Shell, permission groups are available in the _AuthMechanisms_ parameter on the **New-ReceiveConnector** and **Set-ReceiveConnector** cmdlets.

The available authentication mechanisms are described in the following table.

|**Authentication mechanism**|**Description**|
|:-----|:-----|
|None selected (`None`)|No authentication.|
|**Transport Layer Security (TLS)** (`TLS`)|Advertise **STARTTLS** in the EHLO response. TLS encrypted connections require a server certificate that includes the name that the Receive connector advertises in the EHLO response. For more information, see [Modify the SMTP banner on Receive connectors](modify-smtp-banners.md). Other Exchange servers in your organization trust the server's self-signed certificate, but clients and external servers typically use a trusted third-party certificate.|
|**Basic authentication** (`BasicAuth`)|Basic authentication (clear text).|
|**Offer basic authentication only after starting TLS** (`BasicAuthRequireTLS`)|Basic authentication that's encrypted with TLS.|
|**Integrated Windows authentication** (`Integrated`)|NTLM and Kerberos authentication.|
|**Exchange Server authentication** (`ExchangeServer`)|Generic Security Services application programming interface (GSSAPI) and Mutual GSSAPI authentication.|
|**Externally secured** (`ExternalAuthoritative`)|The connection is presumed to be secured by using a security mechanism that's external to Exchange. The connection may be an Internet Protocol security (IPsec) association or a virtual private network (VPN). Alternatively, the servers may reside in a trusted, physically controlled network.  <br/> This authentication mechanism requires the `ExchangeServers` permission group. This combination of authentication mechanism and security group permits the resolution of anonymous sender email addresses for messages that are received through the connector.|

## Receive connector permission groups
<a name="PermissionGroups"> </a>

A *permission group* is a predefined set of permissions that's granted to well-known security principals and assigned to a Receive connector. Security principals include user accounts, computer accounts, and security groups (objects that are identifiable by a security identifier or SID that can have permissions assigned to them). Permission groups define who can use the Receive connector, and the permissions that they get. You can't create permission groups, nor can you modify the permission group members or the default permissions of the permission group.

In the EAC, permission groups are available in the **Security** tab in the properties of the Receive connector. In the Exchange Management Shell, permission groups are available in the _PermissionGroups_ parameter in the **New-ReceiveConnector** and **Set-ReceiveConnector** cmdlets.

The available permission groups are described in the following table.

|**Permission group**|**Associated security principals**|**Permissions granted**|
|:-----|:-----|:-----|
|**Anonymous users** (`Anonymous`)| `NT AUTHORITY\ANONYMOUS LOGON`| `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-SMTP-Accept-Any-Sender` <br/> `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/> `ms-Exch-SMTP-Submit`|
|**Exchange users** (`ExchangeUsers`)| `NT AUTHORITY\Authenticated Users`| `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-Bypass-Anti-Spam` <br/> `ms-Exch-SMTP-Accept-Any-Recipient` <br/> `ms-Exch-SMTP-Submit`|
|**Exchange servers** (`ExchangeServers`)| `<Domain>\Exchange Servers` <br/> `MS Exchange\Edge Transport Servers` <br/> `MS Exchange\Hub Transport Servers` <br/> **Note:** These security principals also have other internal permissions assigned to them. For more information, see the end of the [Receive connector permissions](receive-connectors.md#Permissions) section.| `ms-Exch-Accept-Headers-Forest` <br/> `ms-Exch-Accept-Headers-Organization` <br/> `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-Bypass-Anti-Spam` <br/> `ms-Exch-Bypass-Message-Size-Limit` <br/> `ms-Exch-SMTP-Accept-Any-Recipient` <br/> `ms-Exch-SMTP-Accept-Any-Sender` <br/> `ms-Exch-SMTP-Accept-Authentication-Flag` <br/> `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/> `ms-Exch-SMTP-Accept-Exch50` <br/> `ms-Exch-SMTP-Submit`|
|**Exchange servers** (`ExchangeServers`)| `MS Exchange\Externally Secured Servers`| `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-Bypass-Anti-Spam` <br/> `ms-Exch-Bypass-Message-Size-Limit` <br/> `s-Exch-SMTP-Accept-Any-Recipient` <br/> `ms-Exch-SMTP-Accept-Any-Sender` <br/> `ms-Exch-SMTP-Accept-Authentication-Flag` <br/> `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/> `ms-Exch-SMTP-Accept-Exch50` <br/> `ms-Exch-SMTP-Submit`|
|**Legacy Exchange servers** (`ExchangeLegacyServers`)| `<Domain>\ExchangeLegacyInterop`| `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-Bypass-Anti-Spam` <br/> `ms-Exch-Bypass-Message-Size-Limit` <br/> `ms-Exch-SMTP-Accept-Any-Recipient` <br/> `ms-Exch-SMTP-Accept-Any-Sender` <br/> `ms-Exch-SMTP-Accept-Authentication-Flag` <br/> `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/> `ms-Exch-SMTP-Accept-Exch50` <br/> `ms-Exch-SMTP-Submit`|
|**Partners** (`Partner`)| `MS Exchange\Partner Servers`| `ms-Exch-Accept-Headers-Routing` <br/> `ms-Exch-SMTP-Submit`|

The permissions are explained in the [Receive connector permissions](receive-connectors.md#Permissions) section later in this topic.

## Receive connector permissions
<a name="Permissions"> </a>

Typically, you apply permissions to Receive connectors by using permission groups. However, you can configure granular permissions on a Receive connector by using the **Add-ADPermission** and **Remove-ADPermission** cmdlets.

Receive connector permissions are assigned to security principals by the permission groups for the connector. When an SMTP server or client establishes a connection to a Receive connector, the Receive connector permissions determine whether the connection is accepted, and how messages are processed.

The available Receive connector permissions are described in the following table.

|**Receive connector permission**|**Description**|
|:-----|:-----|
| `ms-Exch-Accept-Headers-Forest`|Controls the preservation of Exchange forest headers in messages. Forest header names start with **X-MS-Exchange-Forest-**. If this permission isn't granted, all forest headers are removed from messages.|
| `ms-Exch-Accept-Headers-Organization`|Controls the preservation of Exchange organization headers in messages. Organization header names start with **X-MS-Exchange-Organization-**. If this permission isn't granted, all organization headers are removed from messages.|
| `ms-Exch-Accept-Headers-Routing`|Controls the preservation of **Received** and **Resent-\*** headers in messages. If this permission isn't granted, all of these headers are removed from messages.|
| `ms-Exch-Bypass-Anti-Spam`|Allows SMTP clients or servers to bypass antispam filtering.|
| `ms-Exch-Bypass-Message-Size-Limit`|Allows SMTP clients or servers to submit messages that exceed the maximum message size that's configured for the Receive connector.|
| `ms-Exch-SMTP-Accept-Any-Recipient`|Allows SMTP clients or servers to relay messages through the Receive connector. If this permission isn't granted, only messages that are sent to recipients in accepted domains that are configured for the Exchange organization are accepted by the Receive connector.|
| `ms-Exch-SMTP-Accept-Any-Sender`|Allows SMTP clients or servers to bypass the sender address spoofing check that normally requires the sender's email address to be in an accepted domain that's configured for Exchange organization.|
| `ms-Exch-SMTP-Accept-Authentication-Flag`|Controls whether messages from SMTP clients or servers are treated as authenticated. If this permission isn't granted, messages from theses sources are identified as external (unauthenticated). This setting is important for distribution groups that are configured to accept mail only from internal recipients (for example, the _RequireSenderAuthenticationEnabled_ parameter value for the group is `$true`).|
| `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender`|Allows access to the Receive connector by senders that have email addresses in authoritative domains that are configured for the Exchange organization.|
| `ms-Exch-SMTP-Accept-Exch50`|Allows SMTP clients or servers to submit **XEXCH50** commands on the Receive connector. The **X-EXCH50** binary large object (BLOB) was used by older versions of Exchange (Exchange 2003 and earlier) to store Exchange data in messages (for example, the spam confidence level or SCL).|
| `ms-Exch-SMTP-Submit`|This permission is required to submit messages to Receive connectors. If this permission isn't granted, the **MAIL FROM** and **AUTH** commands will fail.|

 **Notes**:

- In addition to the documented permissions, there are permissions that are assigned to all of the security principals in the **Exchange servers** (`ExchangeServers`) permission group except `MS Exchange\Externally Secured Servers`. These permissions are reserved for internal Microsoft use, and are presented here for reference purposes only.

  - `ms-Exch-SMTP-Accept-Xattr`

  - `ms-Exch-SMTP-Accept-XProxyFrom`

  - `ms-Exch-SMTP-Accept-XSessionParams`

  - `ms-Exch-SMTP-Accept-XShadow`

  - `ms-Exch-SMTP-Accept-XSysProbe`

  - `ms-Exch-SMTP-Send-XMessageContext-ADRecipientCache`

  - `ms-Exch-SMTP-Send-XMessageContext-ExtendedProperties`

  - `ms-Exch-SMTP-Send-XMessageContext-FastIndex`

- Permissions names that contain `ms-Exch-Accept-Headers-` are part of the *header firewall* feature. For more information, see [Header firewall](https://technet.microsoft.com/library/bb232136.aspx).

### Receive connector permission procedures

To see the permissions that are assigned to security principals on a Receive connector, use the following syntax in the Exchange Management Shell:

```
Get-ADPermission -Identity <ReceiveConnector> [-User <SecurityPrincipal>] | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

For example, to see the permissions that are assigned to all security principals on the Receive connector named Client Frontend Mailbox01, run the following command:

```
Get-ADPermission -Identity "Client Frontend Mailbox01" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

To see the permissions that are assigned only to the security principal `NT AUTHORITY\Authenticated Users` on the Receive connector named Default Mailbox01, run the following command:

```
Get-ADPermission -Identity "Default Mailbox01" -User "NT AUTHORITY\Authenticated Users" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

To add permissions to a security principal on a Receive connector, use the following syntax:

```
Add-ADPermission -Identity <ReceiveConnector> -User <SecurityPrincipal> -ExtendedRights "<Permission1>","<Permission2>"...
```

To remove permissions from a security principal on a Receive connector, use the following syntax:

```
Remove-ADPermission -Identity <ReceiveConnector> -User <SecurityPrincipal> -ExtendedRights "<Permission1>","<Permission2>"...
```
