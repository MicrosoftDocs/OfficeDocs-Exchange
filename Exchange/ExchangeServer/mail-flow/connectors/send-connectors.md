---
localization_priority: Normal
description: Learn about Send connectors in Exchange 2016 and Exchange 2019, and how they control mail flow from your Exchange organization.
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6aa19a12-c7b2-4eac-a8dc-9a4d26919ac5
ms.reviewer:
title: Send connectors in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Send connectors

Exchange uses Send connectors for outbound SMTP connections from source Exchange servers to destination email servers. The Send connector that's used to route messages to a recipient is selected during the routing resolution phase of message categorization. For more information, see [Mail routing](../../mail-flow/mail-routing/mail-routing.md).

You can create Send connectors in the Transport service on Mailbox servers and on Edge Transport servers. Send connectors are stored in Active Directory and are (by default) visible to all Mailbox servers in the organization.

> [!IMPORTANT]
> By default, no Send connectors exist for external mail flow when you install Exchange. To enable outbound internet mail flow, you need to create a Send connector, or subscribe an Edge Transport server to your Exchange organization. For more information, see [Create a Send connector to send mail to the Internet](internet-mail-send-connectors.md) and [Edge Transport servers](../../architecture/edge-transport-servers/edge-transport-servers.md).

You don't need to configure Send connectors to send mail between Exchange servers in the same Active Directory forest. Implicit and invisible Send connectors that are fully aware of the Exchange server topology are available for sending mail to internal Exchange servers. These connectors are described in the [Implicit Send connectors](#implicit-send-connectors) section.

These are the important settings on Send connectors:

- **Usage type**

- **Network settings**: Configure how the Send connector routes mail: by using DNS or by automatically forward all mail to a smart host.

- **Address spaces**: Configure the destination domains that the Send connector is responsible for.

- **Scope**: Configures the visibility of the Send connector to other Exchange servers in the organization.

- **Source servers**: Configure the Exchange servers where the Send connector is hosted. Mail that needs to be delivered by using the Send connector is routed to one of the source servers.

On Mailbox servers, you can create and manage Send connectors in the Exchange admin center or in the Exchange Management Shell. On Edge Transport servers, you can only use the Exchange Management Shell.

## Send connector changes in Exchange Server

These are the notable changes to Send connectors in Exchange 2016 or Exchange 2019 compared to Exchange 2010:

- You can configure Send connectors to redirect or *proxy* outbound mail through the Front End Transport service. For more information, see [Configure Send connectors to proxy outbound mail](proxy-outbound-mail.md).

- The _IsCoexistenceConnector_ parameter is no longer available.

- The _LinkedReceiveConnector_ parameter is no longer available.

- The default maximum message size is increased to 35 MB (approximately 25 MB due to Base64 encoding). For more information, see [Message size and recipient limits in Exchange Server](../../mail-flow/message-size-limits.md).

- The _TlsCertificateName_ parameter allows you to specify the certificate issuer and the certificate subject. This helps minimize the risk of fraudulent certificates.

## Implicit Send connectors

Although no Send connectors are created during the installation of Exchange servers, a special *implicit Send connector* named the intra-organization Send connector is present. This implicit Send connector is automatically available, invisible, and requires no management. The intra-organization Send connector exists in the transport services to send mail, either internally between services on the local Exchange server, or to services on remote Exchange servers in the organization. For example:

- Front End Transport service to the Transport service.

- Transport service to the Transport service on other servers.

- Transport service to subscribed Edge Transport servers.

- Transport service to the Mailbox Transport Delivery service.

- Mailbox Transport Submission service to the Transport service.

For more information, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md).

## Send connector usage types

For Send connectors, the usage type is basically a descriptive label that identifies what the Send connector is used for. All usage type values receive the same permissions.

 You can specify the connector usage type only when you create Send connectors. When you use the EAC, you must select a **Type** value. But when you use the **New-SendConnector** cmdlet in the Exchange Management Shell, the usage type isn't required (either by using `-Usage <UsageType>` or `-<UsageType>`).

Specifying a usage type does configure a default maximum message size, which you can change after you create the connector.

The available usage type values are described in the following table.

|**Usage type**|**Maximum message size**|**Comments**|
|:-----|:-----|:-----|
|Custom|35 MB|None|
|Internal|unlimited|When you create a Send connector of this usage type in the EAC, you can't select **MX record associated with recipient domain**. After you create the connector, you can go to the **Delivery** tab in the properties of the Send connector and select **MX record associated with recipient domain**. <br/><br/> This same restriction doesn't exist in the Exchange Management Shell. You can use the _Internal_ switch and set the _DNSRoutingEnabled_ to `$true` on the **New-SendConnector** cmdlet.|
|Internet|35 MB|None|
|Partner|35 MB|When you create a Send connector of this usage type in the EAC, you can't select **Route mail through smart hosts** or a smart host authentication mechanism. After you create the connector, you can go to the **Delivery** tab in the properties of the Send connector and select **Route mail through smart hosts** and the smart host authentication mechanism. <br/><br/> This same restriction doesn't exist in the Exchange Management Shell. You can use the _Partner_ switch and set the _DNSRoutingEnabled_ to `$false` and use the _SmartHosts_ and _SmartHostAuthMechanism_ parameters on the **New-SendConnector** cmdlet.|

## Send connector network settings

Every Send connector needs to be configured with one of these options:

- Use DNS to route mail.

- Use one or more smart hosts to route mail.

### Use DNS to route mail

When you select DNS resolution to deliver mail, the source Exchange server for the Send connector must be able to resolve the MX records for the address spaces that are configured on the connector. Depending on the nature of the connector, and how many network adapters are in the server, the Send connector could require access to an internal DNS server, or an external (public) DNS server. You can configure the server to use specific DNS servers for internal and external DNS lookups:

- In the EAC at **Servers** \> **Server** \> select the server and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **DNS lookups** tab.

- In the Exchange Management Shell, you use the _ExternalDNS\*_ and _InternalDNS\*_ parameters on the **Set-TransportService** cmdlet.

If you've already configured the Exchange server with separate DNS settings to use for internal and external DNS lookups, and the Send connector routes mail to an external address space, you need to configure the Send connector to use the external DNS server:

- In the EAC, select **Use the external DNS lookup setting on servers with transport roles** (in the new Send connector wizard, or on the **Delivery** tab in the properties of existing connectors).

- In the Exchange Management Shell, use the _UseExternalDNSServersEnabled_ parameter on the **New-SendConnector** and **Set-SendConnector** cmdlets.

### Use smart hosts to route mail

When you route mail through a smart host, the Send connector forwards mail to the smart host, and the smart host is responsible for routing mail to next hop on its way to the ultimate destination. A common use for smart host routing is to send outgoing mail through an antispam service or device.

You identify one or more smart hosts to use for the Send connector by an individual IP address (for example 10.1.1.1), a fully qualified domain name (FQDN) (for example spamservice.contoso.com), or combinations of both types of values. If you use an FQDN, the source Exchange server for the Send connector must be able to resolve the FQDN (which could be an MX record or an A record) by using DNS.

An important part of smart host routing is the authentication mechanism that the smart hosts uses. The available authentication mechanisms are described in the following table.

|**Authentication mechanism**|**Description**|
|:-----|:-----|
|**None** (`None`)|No authentication. For example, when access to the smart host is restricted by the source IP address.|
|**Basic authentication** (`BasicAuth`)|Basic authentication. Requires a username and password. The username and password are sent in clear text.|
|**Offer basic authentication only after starting TLS** (`BasicAuthRequireTLS`)|Basic authentication that's encrypted with TLS. This requires a server certificate on the smart host that contains the exact FQDN of the smart host that's defined on the Send connector. <br/><br/>  The Send connector attempts to establish the TLS session by sending the **STARTTLS** command to the smart host, and only performs Basic authentication after the TLS session is established.  <br/> A client certificate is also required to support mutual TLS authentication.|
|**Exchange Server authentication** (`ExchangeServer`)|Generic Security Services application programming interface (GSSAPI) and Mutual GSSAPI authentication.|
|**Externally secured** (`ExternalAuthoritative`)|The connection is presumed to be secured by using a security mechanism that's external to Exchange. The connection may be an Internet Protocol security (IPsec) association or a virtual private network (VPN). Alternatively, the servers may reside in a trusted, physically controlled network.|

## Send connector address spaces

The address space specifies the destination domains that are serviced by the Send connector. Mail is routed through a Send connector based on the domain of the recipient's email address.

The available SMTP address space values are described in the following table.

|**Address space**|**Explanation**|
|:-----|:-----|
| `*`|The Send connector routes mail to recipients in all domains.|
|Domain (for example, `contoso.com`)|The Send connector routes mail to recipients in the specified domain, but not in any subdomains.|
|Domain and subdomains (for example, `*.contoso.com`)|The Send connector routes mail to recipients in the specified domain, and in all subdomains.|
| `--`| The Send connector routes mail to recipients in all accepted domains in the Exchange organization. This value is only available on Send connectors on Edge Transport servers that send mail to the internal Exchange organization.|

An address space also has **Type** and **Cost** values that you can configure.

On Edge Transport servers, the **Type** value must be `SMTP`. On Mailbox servers, you can also use non-SMTP address space types like `X400` or any other text string. X.400 addresses need to be RFC 1685 compliant (for example, `o=MySite;p=MyOrg;a=adatum;c=us`), but other **Type** values accept any text value for the address space. If you specify a non-SMTP address space type, the Send connector must use smart host routing, and SMTP is used to send messages to the smart host. Delivery Agent connectors and Foreign connectors send non-SMTP messages to non-SMTP servers without using SMTP. For more information, see [Delivery Agents and Delivery Agent Connectors](https://docs.microsoft.com/exchange/delivery-agents-and-delivery-agent-connectors-exchange-2013-help) and [Foreign Connectors](https://docs.microsoft.com/exchange/foreign-connectors-exchange-2013-help).

The **Cost** value on the address space is used for mail flow optimization and fault tolerance when you have the same address spaces configured on multiple Send connectors on different source servers. A lower priority value indicates a preferred Send connector.

The Send connector that's used to route messages to a recipient is selected during the routing resolution phase of message categorization. The Send connector whose address space most closely matches the recipient's email address, and whose priority value is lowest is selected.

For example, suppose the recipient is julia@marketing.contoso.com. If a Send connector is configured for \*.contoso.com, the message is routed through that connector. If no Send connector is configured for \*.contoso.com, the message is routed through the connector that's configured for \*. If multiple Send connectors in the same Active Directory site are configured for \*.contoso.com, the connector with the lower priority value is selected.

## Send connector scope

The source servers for a Send connector determine the destination Exchange server for mail that needs to be routed through the Send connector. The Send connector scope controls the visibility of the connector within the Exchange organization.

 By default, Send connectors are visible to all the Exchange servers in the entire Active Directory forest, and are used in routing decisions. However, you can limit the scope of a Send connector so that it's only visible to other Exchange servers in the same Active Directory site. The Send connector is invisible to Exchange servers in other Active Directory sites, and isn't used in their routing decisions. A Send connector that's restricted in this way is said to be *scoped*.

To configure scoped Send connectors in the EAC, you select **Scoped send connector** in the **Address space** section of the new Send connector wizard, or on the **Scoping** tab in the properties of existing Send connectors. In the Exchange Management Shell, you use the _IsScopedConnector_ parameter on the **New-SendConnector** and **Set-SendConnector** cmdlets.

## Send connector permissions

When the Send connector establishes a connection with the destination email server, the Send connector permissions determine the types of headers that can be sent in messages. If a message includes headers that aren't allowed by the permissions, those headers are removed from messages.

Permissions are assigned to Send connectors by well-known security principals. Security principals include user accounts, computer accounts, and security groups (objects that are identifiable by a security identifier or SID that can have permissions assigned to them). By default, the same security principals with the same permissions are assigned on all Send connectors, regardless of the usage type that you selected when you created the connector. To modify the default permissions for a Send connector, you need to use the **Add-ADPermission** and **Remove-ADPermission** cmdlets in the Exchange Management Shell.

The available Send connector permissions are described in the following table.

****

|**Permission**|**Assigned to**|**Description**|
|:-----|:-----|:-----|
|`ms-Exch-Send-Headers-Forest`|`<Domain>\Exchange Servers` <br/><br/> `MS Exchange\Edge Transport Servers` <br/><br/> `MS Exchange\Hub Transport Servers`|Controls the preservation of Exchange forest headers in messages. Forest header names start with **X-MS-Exchange-Forest-**. If this permission isn't granted, all forest headers are removed from messages.|
|`ms-Exch-Send-Headers-Organization`|`<Domain>\Exchange Servers` <br/><br/> `MS Exchange\Edge Transport Servers` <br/><br/> `MS Exchange\Hub Transport Servers`|Controls the preservation of Exchange organization headers in messages. Organization header names start with **X-MS-Exchange-Organization-**. If this permission isn't granted, all organization headers are removed from messages.|
|`ms-Exch-Send-Headers-Routing`|`NT AUTHORITY\ANONYMOUS LOGON` <br/><br/> `<Domain>\Exchange Servers` <br/><br/> `MS Exchange\Edge Transport Servers` <br/><br/> `MS Exchange\Externally Secured Servers` <br/><br/> `MS Exchange\Hub Transport Servers` <br/><br/> `MS Exchange\Legacy Exchange Servers` <br/><br/> `MS Exchange\Partner Servers`|Controls the preservation of **RECEIVED** headers in messages. If this permission isn't granted, all received headers are removed from messages.|
|`ms-Exch-SMTP-Send-Exch50`|`<Domain>\Exchange Servers` <br/><br/> `MS Exchange\Edge Transport Servers` <br/><br/> `MS Exchange\Externally Secured Servers` <br/><br/> `MS Exchange\Hub Transport Servers` <br/><br/> `MS Exchange\Legacy Exchange Servers`|Allows the source Exchange server to submit **XEXCH50** commands on the Send connector. The **X-EXCH50** binary large object (BLOB) was used by older versions of Exchange (Exchange 2003 and earlier) to store Exchange data in messages (for example, the spam confidence level or SCL). <br/><br/> If this permission isn't granted, and messages contain the **X-EXCH50** BLOB, the Exchange server sends the message without the **X-EXCH50** BLOB.|
|`ms-Exch-SMTP-Send-XShadow`|`<Domain>\Exchange Servers` <br/><br/> `MS Exchange\Edge Transport Servers` <br/><br/> `MS Exchange\Hub Transport Servers`|This permission is reserved for internal Microsoft use, and is presented here for reference purposes only.|

**Note**: Permissions names that contain `ms-Exch-Send-Headers-` are part of the *header firewall* feature. For more information, see [Header firewall](https://docs.microsoft.com/exchange/header-firewall-exchange-2013-help).

### Send connector permission procedures

To see the permissions that are assigned to security principals on a Send connector, use the following syntax in the Exchange Management Shell:

```PowerShell
Get-ADPermission -Identity <SendConnector> [-User <SecurityPrincipal>] | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

For example, to see the permissions that are assigned to all security principals on the Send connector named To Fabrikam.com, run the following command:

```PowerShell
Get-ADPermission -Identity "To Fabrikam.com" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

To see the permissions that are assigned only to the security principal `NT AUTHORITY\ANONYMOUS LOGON` on the Send connector named To Fabrikam, run the following command:

```PowerShell
Get-ADPermission -Identity "To Fabrikam.com" -User "NT AUTHORITY\ANONYMOUS LOGON" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
```

To add permissions to a security principal on a Send connector, use the following syntax:

```PowerShell
Add-ADPermission -Identity <SendConnector> -User <SecurityPrincipal> -ExtendedRights "<Permission1>","<Permission2>"...
```

To remove permissions from a security principal on a Send connector, use the following syntax:

```PowerShell
Remove-ADPermission -Identity <SendConnector> -User <SecurityPrincipal> -ExtendedRights "<Permission1>","<Permission2>"...
```
