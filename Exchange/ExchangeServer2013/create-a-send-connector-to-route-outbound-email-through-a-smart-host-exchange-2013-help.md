---
title: 'Create a Send connector to route outbound email through a smart host'
TOCTitle: Create a Send connector to route outbound email through a smart host
ms:assetid: 4a9ef08e-bd62-4c6b-8790-d24fb0f8f24b
ms:mtpsurl: https://technet.microsoft.com/library/JJ673059(v=EXCHG.150)
ms:contentKeyID: 49289246
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a Send connector to route outbound email through a smart host

_**Applies to:** Exchange Server 2013_

In some situations you may want to route email through a third-party smart host, such as in an instance where you have a network appliance that you want to perform policy checks on outbound messages.

> [!NOTE]
> The third-party smart host must use SMTP for transport. If it does not, you should use a Foreign connector or Delivery Agent connector.

Interested in scenarios where this procedure is used? See the following topics:

- [Configure mail flow and client access](configure-mail-flow-and-client-access-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create your outbound connector.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to create a Send connector to route outbound email through a smart host

1. In the EAC, navigate to **Mail flow** \> **Send connectors**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **New send connector** wizard, specify a name for the send connector and then select **Custom** for the **Type**. You typically choose this selection when you want to route messages to computers not running Microsoft Exchange Server 2013. Click **Next**.

3. Choose **Route mail through smart hosts**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Add smart host** window, specify the IP address, such as 192.168.100.1, or the fully qualified domain name (FQDN), such as contoso.com. Click **Save**.

   For **Smart host authentication**, choose the type of authentication required by the smart host. If you choose **Basic authentication**, you must provide a username and password.

   > [!NOTE]
   > If you choose Basic authentication, we recommend that you use an encrypted connection because the username and password are sent in clear text.

4. Under **Address space**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Add domain** window, make sure SMTP is listed as the **Type**. For **Fully Qualified Domain Name (FQDN)**, enter \* to specify that this send connector applies to messages sent to any domain. Click **Save**.

5. For **Source server**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Select a server** window, choose a server and click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). Click **OK**.

6. Click **Finish**.

Once you have created the send connector, it appears in the Send connector list.

## How do you know this worked?

To verify that you have successfully created a Send connector to route outbound email through a smart host, send a message from a user in your organization (you can use the Outlook Web App) to the domain you specified for the **Address space**. If the recipient receives the message, you've successfully configured the send connector.

## For more information

[Create a Send connector for email sent to the Internet](create-a-send-connector-for-email-sent-to-the-internet-exchange-2013-help.md)

[Send connectors](send-connectors-exchange-2013-help.md)
