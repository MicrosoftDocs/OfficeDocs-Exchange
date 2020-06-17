---
localization_priority: Normal
description: Learn how to create a Send connector in Exchange 2016 and Exchange 2019 that's configured to route outbound mail through a smart host.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 4a9ef08e-bd62-4c6b-8790-d24fb0f8f24b
ms.reviewer:
title: Create a Send connector to route outbound mail through a smart host
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a Send connector to route outbound mail through a smart host

Instead of routing all outbound messages directly to the Internet, you may need to route your organization's outbound mail through a third-party smart host. For example, your organization may have an appliance that scans outbound mail for spam and malware.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- See [Deploy a new installation of Exchange Server](../../plan-and-deploy/deploy-new-installations/deploy-new-installations.md) if you're beginning your installation. After the installation you can use the steps in this topic to create your outbound connector.

- The smart host described in this topic needs to use SMTP to transmit messages. If it doesn't, you need to use a Delivery Agent connector or a Foreign connector. For more information, see[Delivery Agents and Delivery Agent Connectors](https://docs.microsoft.com/exchange/delivery-agents-and-delivery-agent-connectors-exchange-2013-help) and [Foreign Connectors](https://docs.microsoft.com/exchange/foreign-connectors-exchange-2013-help).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to create a Send connector that uses smart host routing

1. In the EAC, navigate to **Mail flow** \> **Send connectors**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). This starts the **New Send connector** wizard.

2. On the first page, enter the following information:

   - **Name**: Enter a descriptive name for the Send connector, for example, Smart host to Internet.

   - **Type**: Select a descriptive value. For example, **Internet** or **Custom**. For more information about Send connector usage types, see [Send connector usage types](send-connectors.md#send-connector-usage-types).

   When you're finished, click **Next**.

3. On the next page, select **Route mail through smart hosts**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Add smart host** dialog box that appears, identify the smart host by using one of the following values:

   - **IP address**: For example, 192.168.3.2.

   - **Fully qualified domain name (FQDN)**: For example, securitydevice01.contoso.com. Note that the Exchange source servers for the Send connector must be able to resolve the smart host in DNS by using this FQDN.

   When you're finished, click **Save**.

4. You can enter multiple smart hosts by repeating Step 3. When you're finished, click **Next**.

5. On the next page, in the **Route mail through smart hosts** section, select the authentication method that's required by the smart host. Valid values are:

   |**Authentication mechanism**|**Description**|
   |:-----|:-----|
   |**None**|No authentication. For example, when access to the smart host is restricted by the source IP address.|
   |**Basic authentication**|Basic authentication. Requires a username and password. The username and password are sent in clear text.|
   |**Offer basic authentication only after starting TLS**|Basic authentication that's encrypted with TLS. This requires a server certificate on the smart host that contains the exact FQDN of the smart host that's defined on the Send connector.|
   |**Exchange Server authentication**|Generic Security Services application programming interface (GSSAPI) and Mutual GSSAPI authentication.|
   |**Externally secured**|The connection is presumed to be secured by using a security mechanism that's external to Exchange. The connection may be an Internet Protocol security (IPsec) association or a virtual private network (VPN). Alternatively, the servers may reside in a trusted, physically controlled network.|

   When you're finished, click **Next**.

6. On the next page, in the **Address space** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Add domain** dialog box that appears, enter the following information:

   - **Type**: Verify SMTP is entered.

   - **Fully Qualified Domain Name (FQDN)**: Enter an asterisk (\*) to indicate the Send connector applies to messages addressed to all external domains. Alternatively, you can enter a specific external domain (for example, contoso.com), or a domain and all subdomains (for example, \*.contoso.com).

   - **Cost**: Verify 1 is entered. A lower value indicates a more preferred route for the domains you specified.

   When you're finished, click **Save**.

7. Back on the previous page, the **Scoped send connector** setting is important if your organization has Exchange servers installed in multiple Active Directory sites:

   - If you don't select **Scoped send connector**, the connector is usable by all transport servers (Exchange 2013 or later Mailbox servers and Exchange 2010 Hub Transport servers) in the entire Active Directory forest. This is the default value.

   - If you select **Scoped send connector**, the connector is only usable by other transport servers in the same Active Directory site.

   When you're finished, click **Next**.

8. On the next page, in the **Source server** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Select a Server** dialog box that appears, select one or more Mailbox servers that you want to use to send outbound mail to the smart host. If you have multiple Mailbox servers in your environment, select the ones that can route mail to the smart host. If you have only one Mailbox server, select that one. After you've selected at least one Mailbox server, click **Add**, click **OK**, and then click **Finish**.

After you create the Send connector, it appears in the Send connector list.

## Use the Exchange Management Shell to create a Send connector that uses smart host routing

1. Open the Exchange Management Shell. For more information, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

2. Use the following syntax:

   ```PowerShell
   New-SendConnector -Name <Name> -AddressSpaces * -Custom -DnsRoutingEnabled $false -SmartHosts <SmartHost1>[,<SmartHost2>...] [-SourceTransportServer <fqdn1>,<fqdn2>...]
   ```

   This example creates the Internet Send connector named "Smart host to Internet" with the following properties:

   - The usage type is Custom.

   - The Send connector uses smart host routing (the _DNSRoutingEnabled_ parameter is set to the value `$false`). The smart host's IP address is 192.168.3.2, and the authentication method is None, because the smart host is configured to listen for connections only from a restricted list of source servers.

   - The Send connector is for all external domains (\*). The value `*` is equivalent to the value `"SMTP:*;1"`, where the address space type is `SMTP`, and the address space cost value is `1`.

   - The local Exchange server is the source server. We aren't using the _SourceTransportServer_ parameter, and the default value is the local Exchange server.

   - The Send connector isn't scoped to the local Active Directory site. We aren't using the _IsScopedConnector_ parameter, and the default value is `$false`. The Send connector is useable by all Exchange transport servers in the Active Directory forest.

   ```PowerShell
   New-SendConnector -Name "Smart host to Internet" -AddressSpaces * -Custom -DNSRoutingEnabled $false -SmartHosts 192.168.3.2 -SmartHostAuthMechanism None
   ```

For information about other options, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector).

## How do you know this worked?

To verify that you have successfully created a Send connector to route outbound email through a smart host, send a message from a user in your organization to an external domain that's serviced by the Send connector.

You can also turn on protocol logging for the Send connector, and view the information in the log. For more information, see [Protocol logging](protocol-logging.md).
