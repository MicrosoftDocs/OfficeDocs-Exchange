---
title: 'Create a Receive connector to receive email from the Internet'
TOCTitle: Create a Receive connector to receive email from the Internet
ms:assetid: 534bbd32-a0db-4d50-9579-4933b156d7b3
ms:mtpsurl: https://technet.microsoft.com/library/JJ657447(v=EXCHG.150)
ms:contentKeyID: 49289253
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a Receive connector to receive email from the Internet

_**Applies to:** Exchange Server 2013_

This procedure shows you how to configure a Receive connector to receive email from the Internet.

> [!NOTE]
> In most cases, you won't need to explicitly set up a Receive connector to receive mail from the Internet, because a Receive connector to accept mail from the Internet is implicitly created upon installation of Exchange. See <A href="receive-connectors-exchange-2013-help.md">Receive connectors</A> for more information.

Interested in scenarios where this procedure is used? See the following topics:

- [Configure mail flow and client access](configure-mail-flow-and-client-access-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create your receive connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to Create a Receive Connector to Receive Messages from the Internet

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to create a Receive connector.

2. On the **New receive connector** page, specify a name for the Receive connector and then select **Frontend transport** for the **Role**. Since you are receiving mail from the Internet in this case, we recommend that you initially route mail to your Front End server or servers, to simplify and consolidate your mail flow.

3. Choose **Internet** for the type. The Receive connector will receive mail from Internet senders.

4. For the **Network adapter bindings**, observe that **All available IPV4** is listed in the **IP addresses** list and the **Port** is 25. (Simple Mail Transer Protocol (SMTP) uses port 25.) This indicates that the connector listens for connections on all IP addresses assigned to network adapters on the local server.

    > [!NOTE]
    > If you have multiple network adapters, on this page you can add an IP address that is assigned to a specific network adapter on the local server, but this isn't required.

5. Click the **Finish** button to create your connector.

Once you have created the Receive connector, it appears in the Receive connector list. If you would like to see an example of how to create a Receive connector with a cmdlet, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).

## How do you know this worked?

To verify that you have successfully created a Receive connector to receive messages from the Internet, test that you can send mail from an outside source and one of your users can receive it. If you can receive mail, you know that the configuration worked successfully.

## For more information

[Create a secure Receive connector to receive email from a partner](create-a-secure-receive-connector-to-receive-email-from-a-partner-exchange-2013-help.md)

[Create a Receive connector to receive email from a system not running Exchange](create-a-receive-connector-to-receive-email-from-a-system-not-running-exchange-exchange-2013-help.md)
