---
localization_priority: Normal
description: 'Summary: Learn how and when to create custom Receive connectors in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 86f7d6e7-a869-4c90-a570-0239fd0e5872
ms.reviewer:
title: Scenarios for custom Receive connectors in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Scenarios for custom Receive connectors in Exchange Server

By default, Exchange Server comes with many different Receive connectors that are configured for most common mail flow scenarios. For more information about these connectors, see [Default Receive connectors created during setup](receive-connectors.md#DefaultConnectors).

However, you might need to process messages from another messaging system that's not running Exchange. Or, if you have a network appliance that performs policy checks and then routes messages to your Exchange server, you'll need to manually configure a Receive connector.

![Custom Receive connector options in Exchange Server](../../media/ac5ceef7-5051-45a3-a1ed-d27aed240301.png)

If you need to create a custom Receive connector, consider these issues:

- You can create custom Receive connectors in the following services on Exchange servers:

  - **Mailbox servers**: The Transport (Hub) service and the Front End Transport service.

  - **Edge Transport servers**: The Transport service.

- Each Receive connector on an Exchange server requires a unique combination of network adapter bindings (the combination of **local IP address** and **TCP port**) and **remote network settings** (remote IP addresses).

  - A default Receive connector that listens on port 25 on all available local IP addresses from all remote IP addresses already exists on all Mailbox servers and Edge Transport servers.

  - If you create a custom Receive connector that listens on port 25 on all available local IP addresses, but the connector is restricted to a limited range of *remote* IP addresses, the new connector won't conflict with any of the default Receive connectors on the server. For a detailed explanation, see [Receive connector remote addresses](receive-connectors.md#RemoteAddresses).

    If you can't restrict the remote IP addresses of the custom Receive connector, your only other option is to restrict the local IP address that the connector uses for port 25. You'll need to modify the local IP address of the conflicting default Receive connector, and then use a different local IP address when you create custom Receive connector.

- For Mailbox servers, you need to create custom Receive connectors that use port 25 in the Front End Transport service, not the Transport (Hub) service. Receive connectors in the Transport service on Mailbox servers accept authenticated and encrypted SMTP connections from other transport services on the local server or other Mailbox servers in your organization. Clients don't directly connect to these connectors. This is different than Exchange 2010, because you could only create Receive connectors on Hub Transport servers (not Client Access servers).

Read more about Receive connectors in Exchange Server see, [Receive connectors](receive-connectors.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- The Exchange admin center (EAC) procedures are only available on Mailbox servers. For more information about the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md).

- The Exchange Management Shell procedures are available on Mailbox servers and Edge Transport servers. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Scenario 1: Receive email from the Internet

For this scenario, the Receive connector listens for anonymous SMTP connections on port 25 from all remote IP addresses. Typically, you don't need to manually configure a Receive connector to receive mail from the Internet. A Receive connector with these settings is automatically created by the installation of a Mailbox server or an Edge Transport server:

- The Receive connector named Default Frontend _\<ServerName\>_ in the Front End Transport service on Mailbox servers.

- The Receive connector named Default internal receive connector _\<ServerName\>_ on Edge Transport servers.

If one of these connectors exists, and you try to create a custom Receive connector on the server that also listens for anonymous SMTP connections on port 25 from all remote IP addresses, you'll get an error. You'll need to change the network adapter binding on the conflicting Receive connector to a specific local IP address. When you create the custom Internet Receive connector, you'll need to specify a different network adapter binding.

### Use the EAC to create an Internet Receive connector on Mailbox servers

1. In the EAC, go to **Mail flow** \> **Receive connectors**, and then click **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)).

2. The **New receive connector** wizard opens. On the first page, configure these settings:

   - **Name**: Type something descriptive. For example, Internet Receive Connector.

   - **Role**: Select **Frontend Transport**.

   - **Type**: Select **Internet**.

   When you're finished, click **Next**.

3. On the last page of the wizard, do one of these steps in the **Network adapter bindings** section:

   - If you're recreating an Internet Receive connector to replace the missing default Receive connector named Default Frontend _\<ServerName\>_ on the Mailbox server, leave the default values of **IP addresses**: **(All available IPv4)** and **Port**: **25** (when you click **Finish**, you won't receive an error message).

   - If you're creating an Internet Receive connector while the default Receive connector named Default Frontend _\<ServerName\>_ still exists on the Mailbox server, do these steps:

     1. Select the default entry **IP addresses**: **(All available IPv4)** and **Port**: **25**, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

     2. In the **Edit IP address** dialog that opens, configure these settings:

        - **Address**: Select **Specify an IPv4 address or an IPv6 address**, and type in a valid local IP address to use for the connector.

        - **Port**: Leave the default value **25** selected.

        When you're finished, click **Save**.

   > [!NOTE]
   > After you've created the new Internet Receive connector on the Mailbox server, be sure to modify the local IP address settings in the properties of the default Receive connector named Default Frontend _\<ServerName\>_. You'll need to go to **Scoping** \> **Network adapter bindings** in the properties of the connector, and then select a different local IP address to replace the default **IP addresses**: **(All available IPv4)** and **Port**: **25** entry.

   When you're finished, click **Finish**.

### Use the Exchange Management Shell to create an Internet Receive connector

To create an Internet Receive connector, use this syntax:

```PowerShell
New-ReceiveConnector -Name <UniqueName> [-TransportRole Frontend] -Internet -Bindings <UniqueValidLocalIPAddress>
```

This example creates a new Receive connector named Internet Receive Connector on a Mailbox server that listens on port 25 on the local IP address 10.1.15 from all remote IP addresses:

```PowerShell
New-ReceiveConnector -Name "Internet Receive Connector" -TransportRole Frontend -Internet -Bindings 10.10.1.1:25
```

**Notes**:

- To run this command on an Edge Transport server, omit the _TransportRole_ parameter.

- If another Receive connector is configured to listen on port 25 using all available local IP addresses on the server, you'll need to use the _Bindings_ parameter on the **Set-ReceiveConnector** cmdlet to specify a unique local IP address for the other connector after you create the new Internet Receive connector.

This example creates a new Receive connector named Internet Receive Connector that listens on port 25 from all remote IP addresses, but on all available local IP addresses. You can only run this command if the server has no other Receive connectors that are configured to listen on port 25 using all available local IP addresses.

```PowerShell
New-ReceiveConnector -Name "Internet Receive Connector" -TransportRole Frontend -Internet -Bindings "0.0.0.0","[::]:"
```

**Note**: To run this command on an Edge Transport server, omit the _TransportRole_ parameter.

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/new-receiveconnector).

### How do you know this worked?

To verify that you've successfully created a Receive connector to receive messages from the Internet, do any of these steps:

- In the EAC, go to **Mail flow** \> **Receive connectors**, select the Receive connector, select **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)), and verify the property values

- In the Exchange Management Shell, run this command on the server, and verify the property values:

  ```PowerShell
  Get-ReceiveConnector | where {$_.Bindings -like '*25' -AND $_.PermissionGroups -like '*AnonymousUsers*'} | Format-List Identity,Bindings,RemoteIPRanges,PermissionGroups
  ```

- Enable protocol logging for the Receive connector. For more information, see [Configure protocol logging](configure-protocol-logging.md).

- From an external client, send a test message to someone in your organization. You can also connect to the Receive connector by using Telnet. For more information, see [Use Telnet to test SMTP communication on Exchange servers](../../mail-flow/test-smtp-with-telnet.md).

## Scenario 2: Receive email from a partner

For this scenario, the Receive connector listens for TLS authenticated SMTP connections on port 25, but only from the specific IP addresses of the partner organization. No default Receive connector is suitable for this scenario; you need to create a custom Receive connector.

 **Note**: Creating a dedicated Receive connector is only one step in TLS encrypting communication between your organization a trusted partner (for example, creating and installing certificates).

### Use the EAC to create a Receive connector to encrypt messages from a partner on Mailbox servers

1. In the EAC, go to **Mail flow** \> **Receive connectors**, and then click **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)).

2. The **New receive connector** wizard opens. On the first page, configure these settings:

   - **Name**: Type something descriptive. For example, TLS Encrypted Messages from Fabrikam.com.

   - **Role**: Select **Frontend Transport**.

   - **Type**: Select **Partner**.

   When you're finished, click **Next**.

3. On the second page of the wizard, do one of these steps in the **Network adapter bindings** section:

   - Leave the default values of **IP addresses**: **(All available IPv4)** and **Port**: **25**.

   - If it's required for your scenario, you can restrict the Receive connector to a valid local IP address on the server:

     1. Select the default entry **IP addresses**: **(All available IPv4)** and **Port**: **25**, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

     2. In the **Edit IP address** dialog that opens, configure these settings:

        - **Address**: Select **Specify an IPv4 address or an IPv6 address**, and type in a valid local IP address to use for the connector.

        - **Port**: Leave the default value **25** selected.

        When you're finished, click **Save**.

   When you're finished, click **Next**.

4. On the last page of the wizard, configure these settings in the **Remote network settings** section:

   1. Select the default entry **0.0.0.0-255.255.255.255**, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

   2. In the **Edit IP address** dialog that opens, enter the IP address or IP address range of the remote partner organization.

      When you're finished, click **Save**.

   When you're finished, click **Finish**.

### Use the Exchange Management Shell to create a Receive connector to encrypt messages from a partner

To create a Receive connector that uses TLS to encrypt messages from a partner, use this syntax:

```PowerShell
New-ReceiveConnector -Name <UniqueName> [-TransportRole Frontend] -Partner  -Bindings <0.0.0.0:25 | LocalIPAddress:25> -RemoteIPRanges <RemoteIPAddresses>
```

This example creates a Receive connector named Fabrikam.com TLS on a Mailbox server that only accepts messages from the IP addresses 17.17.17.1/24 using all available local IP addresses.

```PowerShell
New-ReceiveConnector -Name "Fabrikam.com TLS" -TransportRole Frontend -Partner -RemoteIPRanges 17.17.17.1/24 -Bindings 0.0.0.0:25
```

 **Note**: To run this command on an Edge Transport server, omit the _TransportRole_ parameter.

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/new-receiveconnector).

### How do you know this worked?

To verify that you've successfully created a Receive connector to receive TLS encrypted messages from a partner, do any of these steps:

- In the EAC, go to **Mail flow** \> **Receive connectors**, select the Receive connector, select **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)), and verify the property values

- In the Exchange Management Shell, run this command on the server, and verify the property values:

  ```PowerShell
  Get-ReceiveConnector | where {$_.Bindings -like '*25' -AND $_.PermissionGroups -like '*Partners*'} | Format-List Identity,Bindings,RemoteIPRanges,PermissionGroups
  ```

- Enable protocol logging for the Receive connector. For more information, see [Configure protocol logging](configure-protocol-logging.md).

- Have someone in the partner organization send a test message to someone in your organization. Verify that the message is encrypted (you can verify that TLS is used by checking the message header).

## Scenario 3: Receive messages from a server, service, or device that doesn't use Exchange

For this scenario, the Receive connector listens for connections on port 25, but only from the specific IP address of the service, or device. It's also likely that this scenario requires some type of authentication (consult the documentation for the service or device).

### Use the EAC to create a Receive connector that only accepts messages from a specific service or device on Mailbox servers

1. In the EAC, go to **Mail flow** \> **Receive connectors**, and then click **Add** (![Add icon](../../media/ITPro_EAC_AddIcon.png)).

2. The **New receive connector** wizard opens. On the first page, configure these settings:

   - **Name**: Type something descriptive. For example, Inbound mail from security appliance.

   - **Role**: Select **Frontend Transport**.

   - **Type**: Select **Custom**.

   When you're finished, click **Next**.

3. On the second page of the wizard, do one of these steps in the **Network adapter bindings** section:

   - Leave the default values of **IP addresses**: **(All available IPv4)** and **Port**: **25**.

   - If it's required for your scenario, you can restrict the Receive connector to a valid local IP address on the server:

     1. Select the default entry **IP addresses**: **(All available IPv4)** and **Port**: **25**, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

     2. In the **Edit IP address** dialog that opens, configure these settings:

        - **Address**: Select **Specify an IPv4 address or an IPv6 address**, and type in a valid local IP address to use for the connector.

        - **Port**: Leave the default value **25** selected.

        When you're finished, click **Save**.

   When you're finished, click **Next**.

4. On the last page of the wizard, configure these settings in the **Remote network settings** section:

   1. Select the default entry **0.0.0.0-255.255.255.255**, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

   2. In the **Edit IP address** dialog that opens, enter the IP address or IP address range of the service or device.

      When you're finished, click **Save**.

   When you're finished, click **Finish**.

5. Back at **Mail flow** \> **Receive connectors**, select the connector you just created, and then click **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)).

6. On the **Security** tab, configure the combination of authentication mechanisms and permission groups that are required for the service or device. For example:

   - Leave **Transport Layer Security (TLS)** selected, select **Basic authentication**, and then select the **Anonymous users** permission group.

   - Clear **Transport Layer Security (TLS)**, select **Basic authentication** and **Exchange server authentication**, and then select the **Exchange users** and **Legacy Exchange servers** permission group.

     For more information about permission groups, see [Receive connector permission groups](receive-connectors.md#PermissionGroups).

     > [!CAUTION]
     > Be very careful using the authentication mechanism **Externally secured** with the permission group **Exchange servers**. This combination allows the remote IP addresses specified in the **Remote network settings** section on the **Scoping** tab to anonymously relay messages through the Exchange server. For more information, see [Allow anonymous relay on Exchange servers](allow-anonymous-relay.md).
     
     > [!WARNING]
     > When using the authentication mechanism **Basic authentication** or **Offer basic authentication only after starting TLS** without the permission group **Anonymous users** as an authenticated relay connector, the routing of mail will always try to select the authenticated user or the organization's arbitration mailbox active mailbox server. 

   When you're finished, click **Save**.

### Use the Exchange Management Shell to create a Receive connector that only accepts messages from a specific service or device

To create a Receive connector that only accepts messages from a specific service or device, use this syntax:

```PowerShell
New-ReceiveConnector -Name <UniqueName> [-TransportRole Frontend] -Custom -Bindings <0.0.0.0:25 | LocalIPAddress:25> -RemoteIPRanges <RemoteIPAddresses> -AuthMechanism <AuthMechanism1>,<AuthMechanism2>... - PermissionGroups <PermissionGroup1>,<PermissionGroup2>...
```

This example creates a Receive connector named Inbound From Service on a Mailbox server:

- **Bindings**: All available local IP addresses.

- **Remote IP address ranges**: 192.168.5.1/24.

- **Authentication mechanisms**: Basic authentication.

- **Permission groups**: Anonymous users.

```PowerShell
New-ReceiveConnector -Name "Inbound From Service" -TransportRole Frontend -Custom -Bindings 0.0.0.0:25 -RemoteIPRanges 192.168.10.5 -AuthMechanism BasicAuth -PermissionGroups AnonymousUsers
```

**Note**: To run this command on an Edge Transport server, omit the _TransportRole_ parameter.

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/new-receiveconnector).

### How do you know this worked?

To verify that you've successfully created a Receive connector that only accepts messages from a specific service or device, do any of these steps:

- In the EAC, go to **Mail flow** \> **Receive connectors**, select the Receive connector, select **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)), and verify the property values.

- In the Exchange Management Shell, run this command on the server, and verify the property values:

  ```PowerShell
  Get-ReceiveConnector | where {$_.Bindings -like '*25'} | Format-List Identity,RemoteIPRanges,PermissionGroups,AuthMechanism
  ```

- Enable protocol logging for the Receive connector. For more information, see [Configure protocol logging](configure-protocol-logging.md).

- Send a test message or connect to the Receive connector by using Telnet from the server or device. For more information, see [Use Telnet to test SMTP communication on Exchange servers](../../mail-flow/test-smtp-with-telnet.md).

## Scenario 4: Receive messages from internal Exchange servers

You don't need to configure custom Receive connectors for internal mail flow between Mailbox servers. However, you might need to create a custom Receive connector on an unsubscribed Edge Transport server to receive messages from Mailbox servers. For this scenario, the Edge Transport server listens on port 25, but only from the IP address of the specified Mailbox servers.

### Use the Exchange Management Shell to create a Receive connector that only accepts messages from an internal Exchange server

To create a Receive connector that only accepts messages from an internal Exchange server, use this syntax:

```PowerShell
New-ReceiveConnector -Name <UniqueName> [-TransportRole Frontend] -Internal -RemoteIPRanges <RemoteIPAddress>
```

This example creates a Receive connector named Inbound From Organization on an unsubscribed Edge Transport server that listens for inbound messages from the internal Mailbox servers at IP addresses 10.1.2.10, 10.1.2.15, and 10.1.2.20.

```PowerShell
New-ReceiveConnector -Name "Inbound From Organization" -Internal -RemoteIPRanges 10.1.2.10,10.1.2.15,10.1.2.20
```

**Note**: If your Edge Transport server uses different network adapters for internal and external networks, be sure to use the _Bindings_ parameter on the **Set-ReceiveConnector** cmdlet after you create the connector to specify the correct local IP address for the connector.

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/new-receiveconnector).

### How do you know this worked?

To verify that you've successfully created a Receive connector that only accepts messages from an internal Exchange server, do any of these steps:

- In the EAC, go to **Mail flow** \> **Receive connectors**, select the Receive connector, select **Edit** (![Edit icon](../../media/ITPro_EAC_EditIcon.png)), and verify the property values.

- In the Exchange Management Shell, run this command on the server, and verify the property values:

  ```PowerShell
  Get-ReceiveConnector | where {$_.Bindings -like '*25'} | Format-List Identity,RemoteIPRanges,PermissionGroups,AuthMechanism
  ```

- Enable protocol logging for the Receive connector. For more information, see [Configure protocol logging](configure-protocol-logging.md).

- Send a test message or connect to the Receive connector by using Telnet from the remote Exchange server. For more information, see [Use Telnet to test SMTP communication on Exchange servers](../../mail-flow/test-smtp-with-telnet.md).
