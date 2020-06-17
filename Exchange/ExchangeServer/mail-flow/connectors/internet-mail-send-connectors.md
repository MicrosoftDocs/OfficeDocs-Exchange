---
localization_priority: Normal
description: Learn how to create the Send connector that's required to send mail to the internet in Exchange 2016 and Exchange 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 6deaefa8-1152-40d9-b1ba-9c19bdf8a928
ms.reviewer:
title: Create a Send connector in Exchange Server to send mail to the internet
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Create a Send connector in Exchange Server to send mail to the internet

When install your first Exchange Server 2016 or Exchange 2019 server, the server isn't able to send mail outside of your Exchange organization. To send mail outside your Exchange organization, you need to create a Send connector.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- See [Deploy a new installation of Exchange Server](../../plan-and-deploy/deploy-new-installations/deploy-new-installations.md) if you're beginning your installation. After the installation, you can use the steps in this topic to create an internet Send connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Create a Send connector to send mail to the internet

Until you create a Send connector, mail can't flow from your Exchange to the internet. The exception is if you install an Edge Transport in your perimeter network and subscribe the Edge Transport to your Exchange organization. For more information, see [Edge Transport servers](../../architecture/edge-transport-servers/edge-transport-servers.md).

See also [Send connectors](send-connectors.md) for more information about connectors and why you would may or may not want to use them instead of Edge Transport Servers.

### Use the EAC to create an internet Send connector

1. In the EAC, navigate to **Mail flow** \> **Send connectors**, and then click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). This starts the **New Send connector** wizard.

2. On the first page, enter the following information:

   - **Name**: Enter a descriptive name for the Send connector (for example, To internet).

   - **Type**: Select **Internet**.

   When you're finished, click **Next**.

3. On the next page, verify that **MX record associated with recipient domain** is selected. This means the connector uses DNS on the internet to route mail, as opposed to routing all outbound mail to a smart host. For information about creating a Send connector that uses smart host routing, see [Create a Send connector to route outbound mail through a smart host](outbound-smart-host-routing.md).

   When you're finished, click **Next**.

4. On the next page, enter the following information:

   - In the **Address space** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Add domain** dialog box that appears, in **Fully Qualified Domain Name (FQDN)**, enter an asterisk (\*), and then click **Save**. This value indicates that the Send connector applies to messages addressed to all external domains.

   - The **Scoped send connector** setting is important if your organization has Exchange servers installed in multiple Active Directory sites:

     - If you don't select **Scoped send connector**, the connector is usable by all transport servers (Exchange 2013 or later Mailbox servers and Exchange 2010 Hub Transport servers) in the entire Active Directory forest. This is the default value.

     - If you select **Scoped send connector**, the connector is only usable by other transport servers in the same Active Directory site.

   When you're finished, click **Next**.

5. On the next page, in the **Source server** section, click **Add** ![Add icon](../../media/ITPro_EAC_AddIcon.png). In the **Select a Server** dialog box that appears, select one or more Mailbox servers that you want to use to send mail to the internet. If you have multiple Mailbox servers in your environment, select the ones that can route mail to the internet. If you have only one Mailbox server, select that one. After you've selected at least one Mailbox server, click **Add**, click **OK**, and then click **Finish**.

After you create the Send connector, it appears in the Send connector list. To configure the Send connector to proxy outbound mail through the Front End Transport service, see [Configure Send connectors to proxy outbound mail](proxy-outbound-mail.md).

#### Use the Exchange Management Shell to create an internet Send connector

1. Open the Exchange Management Shell. For more information, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

2. Use the following syntax:

   ```PowerShell
   New-SendConnector -Name <Name> -AddressSpaces * -Internet [-SourceTransportServer <fqdn1>,<fqdn2>...]
   ```

   This example creates the internet Send connector named "To internet" with the following properties:

   - The usage type is Internet.

   - The Send connector uses DNS routing. We aren't using the _DNSRoutingEnabled_ parameter, and the default value is `$true`.

   - The Send connector is for all external domains (\*).

   - The local Exchange server is the source server. We aren't using the _SourceTransportServer_ parameter, and the default value is the local Exchange server.

   - The Send connector isn't scoped to the local Active Directory site. We aren't using the _IsScopedConnector_ parameter, and the default value is `$false`.

   ```PowerShell
   New-SendConnector -Name "To internet" -AddressSpaces * -Internet
   ```

   For information about other options, see [New-SendConnector](https://docs.microsoft.com/powershell/module/exchange/new-sendconnector).

> [!NOTE]
> To configure the Send connector to proxy outbound mail through the Front End Transport service, add `-FrontEndProxyEnabled $true` to the command. For more information, see [Configure Send connectors to proxy outbound mail](proxy-outbound-mail.md).

#### How do you know this worked?

To verify that you have successfully created a Send Connector that sends mail to the internet, create and send a message from an internal mailbox to an outside recipient, and verify the recipient receives the message.

You can also turn on protocol logging for the Send connector, and view the information in the log. For more information, see [Protocol logging](protocol-logging.md).
