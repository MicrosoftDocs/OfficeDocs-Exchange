---
title: 'Create a Receive connector to receive messages from an internal Exchange server'
TOCTitle: Create a Receive connector to receive messages from an internal Exchange server
ms:assetid: 546cead9-7a2d-4332-a5f6-35343d56c619
ms:mtpsurl: https://technet.microsoft.com/library/JJ657448(v=EXCHG.150)
ms:contentKeyID: 49289255
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a Receive connector to receive messages from an internal Exchange server

_**Applies to:** Exchange Server 2013_

You create a Receive connector of the **Internal** type when you want to receive mail from an Exchange server. Use this type of connector to control mail routing within your organization: for example, when you want to route mail from the Transport service on a Mailbox server to a specific Edge Transport server, or from one Mailbox server to another.

Interested in scenarios where this procedure is used? See the following topics:

- [Configure mail flow and client access](configure-mail-flow-and-client-access-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create your receive connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a Receive Connector to Receive Messages from an Internal Exchange Server

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to create a new Receive connector.

2. On the **New receive connector** page, specify a name for the Receive connector and then select **Hub transport** for the **Role**. In this case we assume you want to route mail within your network, not into and out of the organization.

3. Choose **Internal** for the type. The connector is configured with Exchange server authentication.

4. If the Remote network settings page lists 0.0.0.0-255.255.255.255, which means that the Receive connector receives connections from all IP addresses, click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") to remove it. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"), add the IP address for the server you want to receive mail from, such as 192.168.1.1, and click **Save**.

5. Click **Finish** to create the connector.

Once you have created the Receive connector, it appears in the Receive connector list. If you would like to see an example of how to create a Receive connector with a cmdlet, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).

## How do you know this worked?

To verify that you have successfully created a Receive connector to receive messages from an internal server, test that messages from the sending server travel successfully to the recipient server. One way to do this is to use the Exchange Management Shell to set the *ProtocolLoggingLevel* for the Receive connector you created to `Verbose`, using the [Set-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/Set-ReceiveConnector) cmdlet, and check the logs to ensure message delivery.

## For more information

[Create a Receive connector to receive email from the Internet](create-a-receive-connector-to-receive-email-from-the-internet-exchange-2013-help.md)

[Create a secure Receive connector to receive email from a partner](create-a-secure-receive-connector-to-receive-email-from-a-partner-exchange-2013-help.md)
