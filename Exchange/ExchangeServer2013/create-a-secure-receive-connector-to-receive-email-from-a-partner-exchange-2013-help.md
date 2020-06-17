---
title: 'Create a secure Receive connector to receive email from a partner'
TOCTitle: Create a secure Receive connector to receive email from a partner
ms:assetid: 06aa692c-7940-4a14-a722-058c47440f85
ms:mtpsurl: https://technet.microsoft.com/library/JJ673037(v=EXCHG.150)
ms:contentKeyID: 49289153
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a secure Receive connector to receive email from a partner

_**Applies to:** Exchange Server 2013_

This procedure shows you how to configure a Receive connector to receive secure email from a partner. Use this procedure when you are required to encrypt communication between you and a trusted partner. The connector is configured to accept connections only from servers that authenticate with Transport Layer Security (TLS).

Interested in scenarios where this procedure is used? See the following topics:

- [Configure mail flow and client access](configure-mail-flow-and-client-access-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create your receive connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to Create a Receive Connector to Receive Secure Messages from a Partner

1. In the EAC, navigate to **Mail flow** \> **Receive connectors**. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon") to create a new Receive connector.

2. On the **New receive connector** page, specify a name for the Receive connector and then select **Frontend Transport** for the **Role**. Since you are receiving mail from a partner in this case, we recommend that you initially route mail to your front end server to simplify and consolidate your mail flow.

3. Choose **Partner** for the type. The Receive connector will receive mail from a trusted third party.

4. For the **Network adapter bindings**, observe that **All available IPV4** is listed in the **IP addresses** list and the **Port** is 25. (Simple Mail Transfer Protocol uses port 25.) This indicates that the connector listens for connections on all IP addresses assigned to network adapters on the local server. Click **Next**.

5. If the Remote network settings page lists 0.0.0.0-255.255.255.255, which means that the Receive connector receives connections from all IP addresses, click **Remove** ![Remove icon](images/Dd362328.479b6ced-8d64-4277-a725-f17fea202b28(EXCHG.150).gif "Remove icon") to remove it. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"), add the IP address for your partner's server, and click **Save**.

   > [!NOTE]
   > You can also specify an IP address range with CIDR notation, such as 64.4.6.100/24.

6. Click **Finish** to create the connector.

Once you have created the Receive connector, it appears in the Receive connector list. If you would like to see an example of how to create a Receive connector with a cmdlet, see [New-ReceiveConnector](https://docs.microsoft.com/powershell/module/exchange/New-ReceiveConnector).

## How do you know this worked?

To verify that you have successfully created a Receive connector to receive messages from a partner, test that the partner can send mail to one of your users and that the user successfully receives it. If you can receive encrypted mail (you can verify that TLS is used by checking the message header), you know that the configuration worked successfully.

## For more information

[Receive connectors](receive-connectors-exchange-2013-help.md)
