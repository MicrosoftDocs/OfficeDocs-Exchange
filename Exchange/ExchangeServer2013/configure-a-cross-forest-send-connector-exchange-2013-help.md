---
title: 'Configure a cross-forest Send connector: Exchange 2013 Help'
TOCTitle: Configure a cross-forest Send connector
ms:assetid: 7840d172-071e-4f13-9379-2fe1eee1a7cc
ms:mtpsurl: https://technet.microsoft.com/library/JJ945053(v=EXCHG.150)
ms:contentKeyID: 51423387
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure a cross-forest Send connector

_**Applies to:** Exchange Server 2013_

In Active Directory, the *forest* represents the outer boundary of your directory service. You can create Send connectors to enable communication between forests. In this example, the connectors use Basic authentication.

For additional management tasks related to configuring connectors, see [Connectors](connectors-exchange-2013-help.md).

Interested in scenarios where this procedure is used? See the following topics:

- [Deploy Exchange 2013 in a cross-forest topology](deploy-exchange-2013-in-a-cross-forest-topology-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 20 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Send connectors" entry and "Receive connectors" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- See [Deploy a new installation of Exchange 2013](deploy-a-new-installation-of-exchange-2013-exchange-2013-help.md) if you are beginning your installation. After the installation you can use the steps in this topic to create connectors to configure a cross-forest topology.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a user account in each forest

You must create a user account in each forest to use for Basic authentication. Create an account in each forest and add each to the universal security group of the Exchange Server used for communication. This account is used by the Send connector to authenticate to the server receiving mail in the other forest. For example, provide a user account that has the user principal name (UPN) FourthCoffee@Contoso.com as the credentials that must be used for authentication by the Exchange server in the Fourth Coffee domain when mail is sent to the Exchange server in the Contoso domain.

## Use the EAC to create a send connector to route email to another Exchange 2013 forest

Establish cross-forest mail flow using Basic authentication.

1. In the EAC, navigate to **mail flow** \> **send connectors**. Click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon").

2. In the **new send connector** wizard, specify a name for the send connector and then select **Internal** for the **Type**. Click **next**.

3. Choose **Route mail through smart hosts**, and then click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **add smart host** window, specify the IP address of the target server in the second forest, such as 64.4.6.100. Click **save** and then **next**.

    For **Smart host authentication**, choose **Basic authentication** and provide a username and password. Here you can choose **Offer basic authentication only after starting TLS** for secure communication over TLS.

    > [!NOTE]
    > If you use Basic authentication over TLS, the target server must be configured to use an X.509 certificate.

4. Under **Address space**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **add domain** window, make sure SMTP is listed as the **Type**. For **Fully Qualified Domain Name (FQDN)**, enter the receiving domain, such as fourthcoffee.com. Click **save** and then **next**.

5. For **Source server**, click **Add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). In the **Select a server** window, choose the server to use and click **add** ![Add Icon](images/JJ218640.c1e75329-d6d7-4073-a27d-498590bbb558(EXCHG.150).gif "Add Icon"). Click **ok**.

6. Click **finish**. The connector appears in the list of Send connectors.

After you create your Send connector, create a Send connector in the second forest that sends mail to the original forest. In this case, the Fully Qualified Domain Name (FQDN) you specify will be the domain name of the first forest. For example, contoso.com.

## Use the Shell to set permissions on the Send connector

This example uses the Enable-CrossForestConnector.ps1 script in the Shell to set permissions on the Send connector for use in a cross-forest topology.

```powershell
.\Enable-CrossForestConnector.ps1 -Connector "Cross-Forest" -user "ANONYMOUS LOGON"
```

## How do you know this worked?

To verify that you have successfully created Send connectors to route email to a second forest, send a message from a user in your organization (you can use the Outlook Web App) to the domain you specified for the **Address space**. If the recipient receives the message, you've successfully configured the send connector.

## For more information

[Create a Send connector for email sent to the Internet](create-a-send-connector-for-email-sent-to-the-internet-exchange-2013-help.md)

[Send connectors](send-connectors-exchange-2013-help.md)
