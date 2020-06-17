---
title: 'Create a Receive connector to receive email from a system not running Exchange'
TOCTitle: Create a Receive connector to receive email from a system not running Exchange
ms:assetid: 85f0864a-6502-49db-8804-16755a7292b4
ms:mtpsurl: https://technet.microsoft.com/library/JJ657467(v=EXCHG.150)
ms:contentKeyID: 49289337
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a Receive connector to receive email from a system not running Exchange

_**Applies to:** Exchange Server 2013_

You may have a situation where you want to receive messages from a system not running Exchange. For instance, if you have a network appliance that performs policy checks and then routes messages to your Exchange server. We assume in this case that the appliance uses SMTP. If not, you should use a Foreign connector or a Delivery Agent connector.

Interested in scenarios where this procedure is used? See the following topics:

- [Configure mail flow and client access](configure-mail-flow-and-client-access-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create your receive connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to Create a Receive Connector to Receive Messages from a Messaging Appliance

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to create a Receive connector.

2. On the **New receive connector** page, specify a name for the Receive connector and then select **Hub Transport** for the **Role**. In this case, you want your Mailbox server running the Transport service to receive messages from the appliance.

3. Choose **Custom** for the type, since the Receive connector will receive mail from an appliance not running Microsoft Exchange Server 2013.

4. For the **Network adapter bindings**, observe that **All available IPV4** is listed in the **IP addresses** list. Click **Next**.

5. For **Remote network settings**, click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") to remove **0.0.0.0-255.255.255.255** from the **IP addresses** list, since you want to specify that the connector accepts mail from a specific appliance. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to add a new IP address, and in the **Add IP address** window, add the IP address of your appliance. Click **Save**.

6. Click the **Finish** button to create your connector.

Once you have created the Receive connector, it appears in the Receive connector list. If you would like to see an example of how to create a Receive connector with a cmdlet, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).

## How do you know this worked?

To verify that you have successfully created a Receive connector to receive messages from a messaging appliance, test that you can receive mail from the appliance. If you can receive mail, you know that the configuration worked successfully.

## For more information

[Create a Receive connector to receive email from the Internet](create-a-receive-connector-to-receive-email-from-the-internet-exchange-2013-help.md)
