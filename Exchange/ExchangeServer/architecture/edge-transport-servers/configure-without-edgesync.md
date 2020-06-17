---
localization_priority: Normal
description: 'Summary: Learn how you can configure mail flow between your Exchange organization and an Edge Transport server without using an Edge Subscription.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6bb98d10-6f12-4b08-a58e-36375f605d65
ms.reviewer:
title: Configure internet mail flow through Edge Transport servers without using EdgeSync
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure internet mail flow through Edge Transport servers without using EdgeSync

**We recommend that you use the Edge Subscription process to establish mail flow between your Exchange organization and an Edge Transport server as described in [Edge Subscriptions](edge-subscriptions.md)**. However, certain situations may prevent you from subscribing the Edge Transport server to your Exchange organization. To manually establish mail flow between your Exchange organization and an unsubscribed Edge Transport server, you need to manually create and/or modify the following Send connectors and Receive connectors:

On the Edge Transport server:

- Create a dedicated Send connector to only send messages to the internet.

- Create a dedicated Send connector to only send messages to Mailbox servers in the Exchange organization.<sup>1<sup/>

- Create a dedicated Receive connector to only receive messages from Mailbox servers in the Exchange organization<sup>2<sup/>

- Modify the default Receive connector to only accept messages only from the internet.

On a Mailbox server:

- Create a dedicated Send connector to relay outgoing messages to the Edge Transport server

<sup>1</sup>The Send connector that's created by an EdgeSync subscription for delivering email into the Exchange organization is configured to use Exchange Server (GSSAPI) authentication. The EdgeSync subscription identifies the Edge Transport server as an Exchange server to the internal Active Directory forest, which also allows Exchange Server authentication. By definition, there is no EdgeSync subscription in this scenario, so you'll need to improvise:

- You can configure Basic authentication over TLS to provide authentication and encryption for email traffic between the Edge Transport server and the internal Exchange organization. This method has the following issues:

   - You need to configure an Active Directory account that belongs to the Exchange Servers universal security group for authentication on the Send connector that relays messages from the Edge Transport server to the internal Exchange organization. Be sure to safeguard the account credentials, and you can configure the account to allow logon only to specific computers. You also need a local account on the Edge Transport server for authentication on the Send connector that relays messages from the internal Exchange organization to the Edge Transport server.

   - Messages coming from these Send connectors will be seen as authenticated SMTP by the destination Mailbox server. This means the default Receive connector named Client Frontend \<ServerName\> in the Front End Transport service will accept the messages on port 587, and the messages are accepted in the backend Transport service using the default Receive connector named Client Proxy \<ServerName\> on port 465.

   - To provide encryption, you need to use a certificate. The self-signed certificate on the Edge Transport server won't be recognized by the internal Exchange Organization (again, the EdgeSync subscription usually takes care of this). You'll need to manually import the self-signed certificate on each Mailbox or use a certificate from a trusted third-party certification authority.

- If you don't want the messages coming from the Edge Transport server to be identified as authenticated SMTP and therefore using the corresponding client Receive connectors, you can use Externally Secured as the authentication method, which means email traffic between the Edge Transport server and the internal Exchange organization isn't authenticated or encrypted _by Exchange_. If you use this method, you **must** configure and use an external encryption method (for example, IPsec or a VPN).

<sup>2</sup>Instead of a dedicated Receive connector, you can configure and use the default Receive connector on the Edge Transport server for both incoming internet messages and incoming messages from internal Mailbox servers (an EdgeSync subscription uses this Receive connector for both connections).

For more information about Send connectors, see [Send connectors](../../mail-flow/connectors/send-connectors.md). For more information about Receive connectors, see [Receive connectors](../../mail-flow/connectors/receive-connectors.md).

## Before you begin

- Estimated time to complete this task: 30 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry, the "Send connectors - Edge Transport" entry, and the "Receive connectors - Edge Transport" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- On Edge Transport servers, you can only use the Exchange Management Shell to create Send connectors and Receive connectors. On Mailbox servers, you can use the Exchange admin center (EAC) or the Exchange Management Shell to create Send connectors.

   To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

   For information about opening and using the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md).

- The basic configuration of an Edge Transport server in the perimeter network must allow for resolving public domains for internet email and internal host names for internal email. There are different ways to do this, but you can configure the network adapter that's connected to the external (public) network segment to use a public DNS server, and configure the network adapter that's connected to the internal (private) network segment to use a DNS server in the perimeter network.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Edge Transport Server Procedures

### Step 1: Create a dedicated Send connector to only send messages to the internet

This Send connector requires the following configuration:

- **Name**: To Internet (or any descriptive name)

- **Usage type**: Internet

- **Address spaces**: "\*" (all domains)

- **Network settings**: Use DNS MX records to route mail automatically. Depending on your network configuration, you can also route mail through a smart host. The smart host then routes mail to the internet.

To create a Send connector that's configured to send messages to the internet, run this command:

```PowerShell
New-SendConnector -Name "To Internet" -AddressSpaces * -Usage Internet -DNSRoutingEnabled $true
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector).

### Step 2: Create a dedicated Send connector to only send messages to the Exchange organization

This Send connector requires the following configuration:

- **Name**: To Internal Org (or any descriptive name)

- **Usage type**: Internal

- **Address spaces**: `--` (indicates all accepted domains for the Exchange organization)

- DNS routing disabled (smart host routing enabled)

- **Smart hosts**: FQDN of one or more Mailbox servers as smart hosts. For example, mbxserver01.contoso.com and mbxserver02.contoso.com.

- **Smart host authentication methods**: Basic authentication over TLS

- **Smart host authentication credentials**: Credentials for the user account in the internal domain that's a member of the Exchange Servers universal security group. You need to use the **Get-Credential** cmdlet to store the credentials. Use the format _\<Domain\>_\ _\<UserName\>_ or the user principal name (UPN; for example, chris@contoso.com) to enter the username.

To create a Send connector configured to send messages to the Exchange organization, replace the smart host values with the Mailbox servers in your organization, and run this command:

```PowerShell
New-SendConnector -Name "To Internal Org" -Usage Internal -AddressSpaces "--" -DNSRoutingEnabled $false -SmartHosts mbxserver01.contoso.com,mbxserver02.contoso.com -SmartHostAuthMechanism BasicAuthRequireTLS -AuthenticationCredential (Get-Credential)
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector).

### Step 3: Modify the default Receive connector to only accept messages from the internet

Make the following configuration changes to the default Receive connector:

- Modify the name to indicate that the connector will be used solely to receive email from the internet (the default name is Default internal receive connector _\<ServerName\>_).

- Change the network bindings to accept messages only from the network adapter that is accessible from the internet (for example, 10.1.1.1 and the standard SMTP TCP port value of 25).

To modify the default Receive connector to only accept messages from the internet, replace \< _ServerName\>_ and bindings ith the name of your Edge Transport server and external network adapter configuration, and run this command:

```PowerShell
Set-ReceiveConnector -Identity "Default internal Receive connector ServerName>" -Name "From Internet" -Bindings 10.1.1.1:25
```

For detailed syntax and parameter information, see [Set-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/set-receiveconnector).

### Step 4: Create a dedicated Receive connector to only accept messages from the Exchange organization

This Receive connector requires the following configuration:

- **Name**: From Internal Org (or any descriptive name)

- **Usage type**: Internal

- **Local network bindings**: Internal network-facing network adapter (for example, 10.1.1.2 and the standard SMTP TCP port value of 25).

- **Remote network settings**: IP address of one or more Mailbox servers in the Exchange organization. For example, 192.168.5.10 and 192.168.5.20.

- **Authentication methods**: TLS, Basic authentication, Basic authentication over TLS, and Exchange Server authentication.

To create a Receive connector configured to only accept messages from the Exchange organization, replace the bindings and remote IP ranges with your values, and run this command.

```PowerShell
New-ReceiveConnector -Name "From Internal Org" -Usage Internal -AuthMechanism TLS,BasicAuth,BasicAuthRequireTLS,ExchangeServer -Bindings 10.1.1.2:25 -RemoteIPRanges 192.168.5.10,192.168.5.20
```

For detailed syntax and parameter information, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/new-receiveconnector).

### How do you know this worked?

To verify that you have successfully configured the required Send connectors and Receive connectors on the Edge Transport server, run this command on the Edge Transport server and verify the property values:

```PowerShell
Get-SendConnector | Format-List Name,Usage,AddressSpaces,SourceTransportServers,DSNRoutingEnabled,SmartHosts,SmartHostAuthMechanism; Get-ReceiveConnector | Format-List Name,Usage,AuthMechanism,Bindings,RemoteIPRanges
```

## Mailbox server procedures

You don't need to modify the default Receive connectors on Mailbox servers. For more information about default Receive connectors on Mailbox servers, see [Default Receive connectors created during setup](../../mail-flow/connectors/receive-connectors.md#DefaultConnectors).

### Step 5: Create a dedicated Send connector to send outgoing messages to the Edge Transport server

This Send connector requires the following configuration:

- **Name**: To Edge (or any descriptive name)

- **Usage type**: Internal

- **Address spaces**: "\*" (all external domains)

- DNS routing disabled (smart host routing enabled)

- **Smart hosts**: IP address or FQDN of the Edge Transport server. For example, edge01.contoso.net.

- **Source servers**: FQDN of one or more Mailbox servers. For example, mbxserver01.contoso.com and mbxserver02.contoso.com.

- **Smart host authentication methods**: Basic authentication over TLS.

- **Smart host authentication credentials**: Credentials for the user account on the Edge Transport server.

#### Use the EAC to create a Send connector to send outgoing messages to the Edge Transport server

1. In the EAC, go to **Mail flow** \> **Send connectors**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). This starts the **New Send connector** wizard.

2. On the first page, configure these settings:

   - **Name**: Enter To Edge.

   - **Type**: Select **Internal**.

   Click **Next**.

3. On the next page, select **Route mail through smart hosts**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Add smart host** dialog box that appears, identify the Edge Transport server by using one of these values:

   - **IP address**: For example, 10.1.1.2.

   - **Fully qualified domain name (FQDN)**: For example, edge01.contoso.net. Note that the source Mailbox servers for the Send connector must be able to resolve the Edge Transport server in DNS by using this FQDN. If they can't, use the IP address instead.

   Click **Save**.

4. On the next page, in the **Smart host authentication** section, select **Basic authentication**, and then configure these additional settings:

   - Select **Offer basic authentication only after starting TLS**

   - In the **User name** and **Password** fields, enter the credentials for the local user account on the Edge Transport server.

   Click **Next**.

5. On the next page, in the **Address space** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Add domain** dialog box that appears, enter the following information:

   - **Type**: Verify SMTP is selected.

   - **Fully Qualified Domain Name (FQDN)**: Enter an asterisk (\*) to indicate the Send connector is used for all external domains.

   - **Cost**: Verify 1 is entered. A lower value indicates a more preferred route.

   Click **Save**.

6. Back on the previous page, the **Scoped send connector** setting is important if your organization has Exchange servers installed in multiple Active Directory sites:

   - If you don't select **Scoped send connector**, the connector is usable by all transport servers (Exchange 2019 Mailbox servers, Exchange 2016 Mailbox servers, Exchange 2013 Mailbox servers, and Exchange 2010 Hub Transport servers) in the entire Active Directory forest. This is the default value.

   - If you select **Scoped send connector**, the connector is only usable by other transport servers in the same Active Directory site.

   Click **Next**.

7. On the next page, in the **Source server** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Select a Server** dialog box that appears, select one or more Mailbox servers that you want to use to send outgoing mail through the Edge Transport server. Select a Mailbox server and click **Add -\>** (repeat as many times a necessary), click **OK**, and then click **Finish**.

#### Use the Exchange Management Shell to create a Send connector to send outgoing messages to the Edge Transport server

To create a Send connector to send outgoing messages to the Edge Transport server, replace the smart hosts and source Mailbox servers with your values, and run this command:

```PowerShell
New-SendConnector -Name "To Edge" -Usage Internal -AddressSpaces * -DNSRoutingEnabled $false -SmartHosts edge01.contoso.com -SourceTransportServers mbxserver01.contoso.com,mbxserver02.contoso.com -SmartHostAuthMechanism BasicAuthRequireTLS -AuthenticationCredential (Get-Credential)
```

For detailed syntax and parameter information, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector).

#### How do you know this worked?

To verify that you've successfully created a Send connector to send outgoing messages to the Edge Transport server, use either of these steps:

- In the EAC, go to **Mail flow** \> **Send connectors**, select the Send connector named To Edge \> click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png), and verify the property values.

- In the Exchange Management Shell, run this command on a Mailbox server to verify the property values:

   ```PowerShell
   Get-SendConnector -Identity "To Edge" | Format-List Usage,AddressSpaces,DSNRoutingEnabled,SmartHosts,SourceTransportServers,SmartHostAuthMechanism
   ```
