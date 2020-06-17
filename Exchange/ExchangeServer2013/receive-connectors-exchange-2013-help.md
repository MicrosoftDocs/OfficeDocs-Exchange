---
title: 'Receive connectors: Exchange 2013 Help'
TOCTitle: Receive connectors
ms:assetid: 17751a60-39fe-433f-84d2-bfc14ff4ba51
ms:mtpsurl: https://technet.microsoft.com/library/Aa996395(v=EXCHG.150)
ms:contentKeyID: 49289180
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Receive connectors

_**Applies to:** Exchange Server 2013_

Receive connectors control the flow of inbound messages to your Exchange organization. They are configured on computers running Microsoft Exchange Server 2013 with the Transport service, or in the Front End service on a Client Access server. They can be created in the Exchange admin center (EAC), or in the Exchange Management Shell.

By default, the Receive connectors that are required for internal mail flow are automatically created when a Client Access server or Mailbox server is installed.

Exchange 2013 servers running the Transport service require Receive connectors to receive messages from the Internet, from email clients, and from other email servers. A Receive connector controls inbound connections to the Exchange organization.

Each Receive connector listens for inbound connections that match the settings of the Receive connector. A Receive connector listens for connections that are received through a particular local IP address and port, and from a specified IP address range. You can create a Receive connector when you want to control which servers receive messages from a particular IP address or IP address range, and when you want to configure special connector properties for messages that are received from a particular IP address, such as allowing larger messages or more recipients per message. You can also scope the Receive connector using the *TlsCertificateName* parameter of the **Set-ReceiveConnector** cmdlet, which allows you to specify the certificate to use for the connector.

Receive connectors are scoped to a single server and determine how that specific server listens for connections. When you create a Receive connector on a Mailbox server running the Transport service, the Receive connector is stored in Active Directory as a child object of the server on which it's created.

If you need additional Receive connectors for specific scenarios, you can create them by using the EAC or the Exchange Management Shell. Each Receive connector must use a unique combination of IP address bindings, port number assignments, and remote IP address ranges from which mail is accepted.

For more information about how to create a Receive Connector, see [Receive connector procedures](receive-connector-procedures-exchange-2013-help.md).

## Default Receive connectors created during setup

Certain Receive connectors are created by default when you install the Mailbox server role.

## Default Receive connectors created on a Mailbox server running the Transport service

When you install a Mailbox server running the Transport service, two Receive connectors are created. No additional Receive connectors are needed for typical operation, and in most cases the default Receive connectors don't require a configuration change. These connectors are the following:

- **Default \<server name\>**: Accepts connections from Mailbox servers running the Transport service and from Edge servers.

- **Client Proxy \<server name\>**: Accepts connections from front-end servers. Typically, messages are sent to a front-end server over SMTP.

Each connector is assigned a *TransportRole* value. You can use it to determine the role the connector is running in. This can be helpful in cases where you are running multiple roles on a single server. In the case of each Receive connector previously mentioned, their *TransportRole* value is **HubTransport**.

To view the default Receive connectors and their parameter values, you can use the [Get-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/Get-ReceiveConnector) cmdlet.

## Default Receive connectors created on a Front End Transport server

During installation, three Receive connectors are created on the Front End transport, or Client Access server. The default Front End Receive connector is configured to accept SMTP communications from all IP address ranges. Additionally, there is a Receive connector that can act as an outbound proxy for messages sent to the front-end server from Mailbox servers. Finally, there is a secure Receive connector configured to accept messages encrypted with Transport Layer Security (TLS). These connectors are the following:

- **Default FrontEnd \<server name\>**: Accepts connections from SMTP senders over port 25. This is the common messaging entry point into your organization.

- **Outbound Proxy Frontend \<server name\>**: Accepts messages from a Send Connector on a back-end server, with front-end proxy enabled.

- **Client Frontend \<server name\>**: Accepts secure connections, with Transport Layer Security (TLS) applied.

In a typical installation, no additional Receive connectors are required.

## Receive connector types

The type determines the default security settings for each Receive connector.

The security settings for a Receive connector specify the permissions that are granted to sessions that connect to the Receive connector and the supported authentication mechanisms.

When you use the EAC to configure a Receive connector, the new receive connector page prompts you to select the type for the connector. Based on your selection, parameters are set to conform to the configuration you have chosen. Specific procedures contain more information about Receive connector type settings. Examples of these procedures are [Create a Receive connector to receive email from the Internet](create-a-receive-connector-to-receive-email-from-the-internet-exchange-2013-help.md) and [Create a secure Receive connector to receive email from a partner](create-a-secure-receive-connector-to-receive-email-from-a-partner-exchange-2013-help.md).

## Receive connector permission groups

A permission group is a predefined set of permissions that's granted to well-known security principals and assigned to a Receive connector. Security principals include users, computers, and security groups. The use of permission groups simplifies the configuration of permissions on Receive connectors. The **PermissionGroups** property defines the groups or roles that can submit messages to the Receive connector and the permissions that are assigned to those groups.

Permission groups include *Anonymous*, *ExchangeUsers*, *ExchangeServers*, *ExchangeLegacyServers*, and *Partner*.

## Receive connector type specifications

The type determines the default permission groups that are assigned to the Receive connector and the default authentication mechanisms that are available for session authentication. The following list describes the available types:

1. **Client**: Typically used to connect to clients not using Microsoft Outlook. It can use TLS authentication.

2. **Custom**: Typically used in a cross-forest scenario, or in a scenario where your organization receives messages from an SMTP message transfer agent.

3. **Internal**: Used for communication between servers running the Transport service, or between Mailbox servers running the Transport service and third-party transfer agents.

4. **Internet**: Used to receive SMTP mail from the Internet.

5. **Partner**: Use this type when you want to configure secure communication with a partner.

Each type is appropriate for a specific connection scenario. Select the type that has the default settings most applicable to the configuration that you want. You can modify permissions by using the **Add-ADPermission** and **Remove-ADPermission** cmdlets. For more information, see the following topics:

- [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/Add-ADPermission)

- [Remove-ADPermission](https://docs.microsoft.com/powershell/module/exchange/Remove-ADPermission)

## Receive connector permissions

Receive connector permissions are assigned to security principals when you specify the permission groups for the connector. When a security principal establishes a session with a Receive connector, the Receive connector permissions determine whether the session is accepted and how the received messages are processed. You can set Receive connector permissions by using the EAC or by using the *PermissionGroups* parameter with the **Set-ReceiveConnector** cmdlet in the Shell. To modify the default permissions for a Receive connector, you can also use the **Add-ADPermission** cmdlet.

[Receive connector permissions](receive-connector-permissions-exchange-2013-help.md) contains a table that lists security principals and permissions types in detail.

## Receive connector authentication settings

In the EAC, you use the authentication settings for a Receive connector to specify the authentication mechanisms that are supported by the Exchange 2013 transport server. In the Shell, you use the *AuthMechanisms* parameter to specify the supported authentication mechanisms. You can configure more than one authentication mechanism for a Receive connector. For more information, see [Receive connector authentication mechanisms](receive-connector-authentication-mechanisms-exchange-2013-help.md).

## New Receive connector features in Exchange 2013

The following features were added in Exchange 2013:

- With the *TlsCertificateName* parameter you can specify the local Certificate Authority (CA) issued certificate to use for secure mail. It helps minimize the risk of fraudulent certificates.

- The *TransportRole* parameter designates the server role associated with this connector. It is typically used to specify the server role when you host multiple server roles on a single computer.

See [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector) for more information about these parameters and other parameters for Receive connectors.
