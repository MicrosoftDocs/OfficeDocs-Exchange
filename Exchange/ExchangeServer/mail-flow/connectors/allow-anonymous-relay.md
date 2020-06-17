---
localization_priority: Normal
description: 'Summary: Learn how to configure anonymous relay in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5b675b4e-3a33-4191-91ce-44e1c0923517
ms.reviewer: 
title: Allow anonymous relay on Exchange servers
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Allow anonymous relay on Exchange servers

 *Open relay* is a very bad thing for messaging servers on the Internet. Messaging servers that are accidentally or intentionally configured as open relays allow mail from any source to be transparently re-routed through the open relay server. This behavior masks the original source of the messages, and makes it look like the mail originated from the open relay server. Open relay servers are eagerly sought out and used by spammers, so you never want your messaging servers to be configured for open relay.

On the other hand, *anonymous relay* is a common requirement for many businesses that have internal web servers, database servers, monitoring applications, or other network devices that generate email messages, but are incapable of actually sending those messages.

In Exchange Server, you can create a dedicated Receive connector in the Front End Transport service on a Mailbox server that allows anonymous relay from a specific list of internal network hosts. Here are some key considerations for the anonymous relay Receive connector:

- You need to create a dedicated Receive connector to specify the network hosts that are allowed to anonymously relay messages, so you can exclude anyone or anything else from using the connector. Don't attempt to add anonymous relay capability to the default Receive connectors that are created by Exchange. Restricting access to the Receive connector is critical, because you don't want to configure the server as an open relay.

- You need to create the dedicated Receive connector in the Front End Transport service, not in the Transport service. In Exchange Server, the Front End Transport service and the Transport service are always located together on Mailbox servers. The Front End Transport service has a default Receive connector named Default Frontend _\<ServerName\>_ that's configured to listen for inbound SMTP connections from any source on TCP port 25. You can create another Receive connector in the Front End Transport service that also listens for incoming SMTP connections on TCP port 25, but you need to specify the IP addresses that are allowed to use the connector. The dedicated Receive connector will always be used for incoming connections from those specific network hosts (the Receive connector that's configured with the most specific match to the connecting server's IP address wins).

  In contrast, the Transport service has a Default receive connector named Default _\<ServerName\>_ that's also configured to listed for inbound SMTP connections from any source, but this connector listens on TCP port 2525 so that it doesn't conflict with the Receive connector in the Front End Transport service. Furthermore, only other transport services and Exchange servers in your organization are expected to use this Receive connector, so the authentication and encryption methods are set accordingly.

  For more information, see [Mail flow and the transport pipeline](../../mail-flow/mail-flow.md) and [Default Receive connectors created during setup](receive-connectors.md#DefaultConnectors).

- After you create the dedicated Receive connector, you need to modify its permissions to allow anonymous relay only by the specified network hosts as identified by their IP addresses. At a minimum, the network hosts need the following permissions on the Receive connector to anonymously relay messages:

  - `ms-Exch-Accept-Headers-Routing`

  - `ms-Exch-SMTP-Accept-Any-Recipient`

  - `ms-Exch-SMTP-Accept-Any-Sender`

  - `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender`

  - `ms-Exch-SMTP-Submit`

    For more information about permissions on Receive connectors, see [Receive connector permission groups](receive-connectors.md#PermissionGroups) and [Receive connector permissions](receive-connectors.md#Permissions).

    There are two different methods that you can use to configure the permissions that are required for anonymous relay on a Receive connector. These methods are described in the following table.

|**Method**|**Permissions granted**|**Pros**|**Cons**|
|:-----|:-----|:-----|:-----|
|Add the **Anonymous users** (`Anonymous`) permission group to the Receive connector and add the `Ms-Exch-SMTP-Accept-Any-Recipient` permission to the `NT AUTHORITY\ANONYMOUS LOGON` security principal on the Receive connector.|Connections use the `NT AUTHORITY\ANONYMOUS LOGON` security principal with the following permissions: <br/>• `ms-Exch-Accept-Headers-Routing` <br/>• `ms-Exch-SMTP-Accept-Any-Recipient` <br/>• `ms-Exch-SMTP-Accept-Any-Sender` <br/>• `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/>• `ms-Exch-SMTP-Submit`|Grants the minimum required permissions to allow anonymous relay.|More difficult to configure (must use the Exchange Management Shell). <br/><br/> The network hosts are considered anonymous senders. Messages don't bypass antispam or message size limit checks, and the sender's email address can't be resolved to the corresponding display name (if any) in the global address list.|
|Add the **Exchange servers** (`ExchangeServers`) permission group and the **Externally secured** (`ExternalAuthoritative`) authentication mechanism to the Receive connector.|Connections use the `MS Exchange\Externally Secured Servers` security principal with the following permissions: <br/>• `ms-Exch-Accept-Headers-Routing` <br/>• `ms-Exch-Bypass-Anti-Spam` <br/>• `ms-Exch-Bypass-Message-Size-Limit` <br/>• `ms-Exch-SMTP-Accept-Any-Recipient` <br/>• `ms-Exch-SMTP-Accept-Any-Sender` <br/>• `ms-Exch-SMTP-Accept-Authentication-Flag` <br/>• `ms-Exch-SMTP-Accept-Authoritative-Domain-Sender` <br/>• `ms-Exch-SMTP-Accept-Exch50` <br/>• `ms-Exch-SMTP-Submit`|Easier to configure (can do everything in the Exchange admin center). <br/><br/> The network hosts are considered authenticated senders. Messages bypass antispam and message size limit checks, and the sender's email address can be resolved to a corresponding display name in the global address list.|Grants the permissions to submit messages as if they originated from internal senders within your Exchange organization. The network hosts are considered completely trustworthy, regardless of the volume, size, or content of the messages that they send.|

Ultimately, you need to decide on the approach that best fits the needs of your organization. We'll show you how to configure both methods. Just remember that it's one method or the other, and not both at the same time.

## What do you need to know before you begin?

- Estimated time to complete this task: 10 minutes.

- Some of these procedures require the Exchange Management Shell. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create a dedicated Receive connector for anonymous relay

You can create the Receive connector in the EAC or in the Exchange Management Shell.

### Use the EAC to create a dedicated Receive connector for anonymous relay

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). This starts the **New Receive connector** wizard.

2. On the first page, enter the following information:

   - **Name**: Enter a descriptive name for the Receive connector, for example, Anonymous Relay.

   - **Role**: Select **Frontend Transport**.

   - **Type**: Select **Custom**.

     When you're finished, click **Next**.

3. On the next page, in the **Network adapter bindings** section, do one of the following:

   - If the Exchange server has one network adapter, and doesn't segregate internal and external traffic by using different subnets, accept the existing **(All available IPv4)** entry on port 25.

   - If the Exchange server has an internal network adapter and an external network adapter, and segregates internal and external network traffic by using different subnets, you can further enhance security for the connector by limiting the use of the connector to requests that originate on the internal network adapter. To do this:

     1. Select the existing **(All available IPv4)** entry, click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png), and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

     2. In the resulting **Network Adapter Bindings** dialog, select **Specify an IPv4 address or an IPv6 address**, and enter a valid and available IP address that's configured on the internal network adapter, and then click **Save**.

   When you're finished, click **Next**.

4. On the next page, in the **Remote network settings** section, do the following steps:

   1. Select the existing **0.0.0.0-255.255.255.255** entry, and then click **Remove** ![Remove icon](../../media/ITPro_EAC_RemoveIcon.png), and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png).

   2. In the resulting **Remote Address Settings** dialog, enter an IP address or IP address range that identifies the network hosts that are allowed use this connector, and then click **Save**. You can repeat this step to add multiple IP addresses or IP address ranges. Err on the side of being too specific instead of too general to clearly identify the network hosts that are allowed to use this connector.

   When you're finished, click **Finish**.

### Use the Exchange Management Shell to create a dedicated Receive connector for anonymous relay

To create the same Receive connector in the Exchange Management Shell, use the following syntax:

```PowerShell
New-ReceiveConnector -Name <ConnectorName> -TransportRole FrontendTransport -Custom -Bindings <LocalIPAddresses>:25 -RemoteIpRanges <RemoteIPAddresses>
```

This example creates a new Receive connector with the following configuration options:

- **Name**: Anonymous Relay

- **Transport role**: `FrontEndTransport`

- **Usage type**: Custom

- **Bindings**: `0.0.0.0:25` (listen for inbound messages on all IP addresses that are configured on all network adapters in the Exchange server on TCP port 25.)

- **Remote IP addresses that are allowed to use this connector**: 192.168.5.10 and 192.168.5.11

```PowerShell
New-ReceiveConnector -Name "Anonymous Relay" -TransportRole FrontendTransport -Custom -Bindings 0.0.0.0:25 -RemoteIpRanges 192.168.5.10,192.168.5.11
```

**Notes:**

- The _Bindings_ parameter is required when you specify the Custom usage type.

- The _RemoteIpRanges_ parameter accepts an individual IP address, an IP address range (for example, `192.168.5.10-192.168.5.20`), or Classless InterDomain Routing (CIDR) (for example, `192.168.5.1/24`). You can specify multiple values separated by commas.

## Step 2: Configure the permissions for anonymous relay on the dedicated Receive connector

As described in the introduction, there are two different methods you can use to configure the required permissions on the Receive connector:

- Configure the connections as anonymous.

- Configure the connections as externally secured.

Choose one method or the other. The examples use the Receive connector named Anonymous Relay that you created in Step 1.

### Configure the connections as anonymous

Run the following commands in the Exchange Management Shell:

1.

  ```PowerShell
  Set-ReceiveConnector "Anonymous Relay" -PermissionGroups AnonymousUsers
  ```

2.

  ```PowerShell
  Get-ReceiveConnector "Anonymous Relay" | Add-ADPermission -User "NT AUTHORITY\ANONYMOUS LOGON" -ExtendedRights "Ms-Exch-SMTP-Accept-Any-Recipient"
  ```

### Configure the connections as externally secured

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**, select the Anonymous Relay connector, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. In the properties of the connector, click **Security** and make the following selections:

   - **Authentication**: Deselect **Transport Layer Security (TLS)** and select **Externally secured (for example, with IPsec)**.

   - **Permission groups**: Select **Exchange servers**.

   When you're finished, click **Save**.

To perform these same steps in the Exchange Management Shell, run the following command:

```PowerShell
Set-ReceiveConnector "Anonymous Relay" -AuthMechanism ExternalAuthoritative -PermissionGroups ExchangeServers
```

## How do you know this worked?

To verify that you've successfully configured anonymous relay, do the following steps:

- Verify the configuration of the dedicated Receive connector.

  ```PowerShell
  Get-ReceiveConnector "Anonymous Relay" | Format-List Enabled,TransportRole,Bindings,RemoteIPRanges
  ```

- Verify the permissions on the dedicated Receive connector.

  ```PowerShell
  Get-ADPermission "Anonymous Relay" -User "NT AUTHORITY\ANONYMOUS LOGON" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
  ```

  Or

  ```PowerShell
  Get-ADPermission "Anonymous Relay" -User "MS Exchange\Externally Secured Servers" | where {($_.Deny -eq $false) -and ($_.IsInherited -eq $false)} | Format-Table User,ExtendedRights
  ```

- Use Telnet to test if one or more of the specified network hosts can connect to the dedicated Receive connector, and can anonymously relay mail through the connector. By default, the Telnet Client isn't installed in most client or server versions of Microsoft Windows. To install it, see [Install Telnet Client](https://go.microsoft.com/fwlink/p/?linkId=179054).

  For more information, see [Use Telnet to test SMTP communication on Exchange servers](../../mail-flow/test-smtp-with-telnet.md).

  If the network host is a device that doesn't have Telnet, you could temporarily add the IP address of a computer to the Receive connector, and then remove the IP address from the Receive connector when you're finished testing.

  For the test, the you'll need the following values:

  - **Destination**: This is the IP address or FQDN that you use to connect to the dedicated Receive connector. This is likely the IP address of the Mailbox server where the Receive connector is defined. This relates to the **Network adapter bindings** property (or the _Bindings_ parameter) value that you configured on the connector. You'll need to use the valid value for your environment. In this example, we'll use 10.1.1.1.

  - **Sender's email address**: You'll probably configure the servers or devices that are anonymously relaying mail to use a sending email address that's in an authoritative domain for your organization. In this example, we'll use chris@contoso.com.

  - **Recipient's email address**: Use a valid email address. In this example, we'll use kate@fabrikam.com.

  - **Message subject**: Test

  - **Message body**: This is a test message

  1. Open a Command Prompt window, type telnet, and then press Enter.

  2. Type set localecho, and then press Enter.

  3. Type OPEN 10.1.1.1 25, and then press Enter.

  4. Type EHLO, and then press Enter.

  5. Type MAIL FROM:chris@contoso.com, and then press Enter.

  6. Type RCPT TO:kate@fabrikam.com, and then press Enter.

     - If you receive the response `250 2.1.5 Recipient OK`, the Receive connector allows anonymous relay from the network host. Continue to the next step to finish sending the test message.

     - If you receive the response `550 5.7.1 Unable to relay`, the Receive connector doesn't allow anonymous relay from the network host. If this happens, do the following:

       - Verify that you're connecting to the correct IP address or FQDN for the dedicated Receive connector.

       - Verify that the computer where you're running Telnet is allowed to use the Receive connector.

       - Verify the permissions on the Receive connector.

  7. Type DATA, and then press Enter.

     You should receive a response that looks like this:

     `354 Start mail input; end with <CLRF>.<CLRF>`

  8. Type Subject: Test, and then press Enter.

  9. Press Enter again.

  10. Type This is a test message, and then press Enter.

  11. Press Enter, type a period ( . ), and then press Enter.

      You should receive a response that looks like this:

      `250 2.6.0 <GUID> Queued mail for delivery`

  12. To disconnect from the SMTP server, type QUIT, and then press Enter.

      You should receive a response that looks like this:

      `221 2.0.0 Service closing transmission channel`

  13. To close the Telnet session, type quit, and then press Enter.

- If anonymous relay works intermittently, you may need to modify the default message rate and throttling limits on the Receive connector. For more information, see [Message throttling on Receive connectors](../message-rate-limits.md#message-throttling-on-receive-connectors).
