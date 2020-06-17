---
localization_priority: Normal
description: Admins can learn how to use connectors and mail flow rules to route mail in Exchange Online
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 82d105e2-e955-4e03-99c3-3314a5d21a4c
ms.reviewer: 
f1.keywords:
- NOCSH
title: Scenario Conditional mail routing in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Scenario: Conditional mail routing in Exchange Online

There might be times you need to route mail differently depending on who the mail is sent to or from, where it's being sent, the contents of the message, and so on. For example, if you have multiple sites around the world, you might want to route mail to a specific site. You can do this using connectors and mail flow rules (also known as transport rules).

When the steps below are completed, a mail flow rule will redirect messages addressed to users whose City property is set to New Orleans to the IP address specified by the Outbound connector.

## Step 1: Use the Exchange admin center to create the connector

The first thing we need to do is create an Outbound connector. This connector will be used by the mail flow rule that we'll set up in Step 2. In this connector, you'll select where messages it receives originate (such as a mailbox in your Microsoft 365 or Office 365 organization), the type of organization where the messages will be sent (such as your on-premises servers), the security that should be applied to the connection, and name or IP address of the target server. If you want to learn more about how to create connectors, check out [Configure mail flow using connectors in Office 365](use-connectors-to-configure-mail-flow.md).

1. In the EAC, go to **Mail flow** \> **Connectors**. click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to create a new connector.

2. In the **From:** drop-down box, choose Office 365.

3. In the **To:** drop-down box, choose either Your organization's email server or Partner organization if you want to connect to a server other than your organizations.

   ![route to and from options](../../media/eaa1aabd-31fa-4598-921b-7803182f7b5f.png)

4. Name the connector and add a description. If you want to turn the connector on immediately, check **Turn it on**. Click **Next**.

   ![name connector](../../media/f4b47d74-3251-4d04-83aa-7978ba5cd0e4.png)

5. Choose **Only when I have a transport rule...** and click **Next**.

   ![transport rule option](../../media/5aab8ee0-7244-41ea-b504-71ff3e5d1f18.png)

6. Specify one or more smart hosts to which Office 365 will deliver email messages.

   ![add host](../../media/d41ef961-224c-4c7e-8ac8-756b785a73fc.png)

7. Define your Transport Layer Security (TLS) settings depending on your security needs.

   ![define TLS settings](../../media/728b161e-7780-4686-a169-df37a7f96531.png)

8. Review your new connector configurations and click **Next** to validate the connector.

## Step 2: Use the EAC to create a mail flow rule

Now that we've created a connector, we need to create a mail flow rule that'll send mail to it based on the criteria you define. There are many conditions you can select from to control when messages should be sent to the connector.

1. In the EAC, navigate to **Mail flow** \> **Rules**. Click **New** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) and choose **Create a new rule...**.

2. In the **New rule** window, name the rule. To see all the options available for the rule, click **More options...** at the bottom of the page.

   ![click more options](../../media/96c2ca71-32b2-423c-99f6-1ee35c63af5c.png)

3. For **\*Apply this rule if...**, select **The recipient...** and **has specific properties including any of these words**. The **select user properties** box appears. Click ![Add Icon](../../media/ITPro_EAC_AddIcon.gif), and under **User properties:**, choose **City**. **City** is an Active Directory attribute made available for use by the rule. Specify the name of the city, such as New Orleans. Click **OK**, and then click **OK** again to close the **select user properties** box.

   ![apply rule if](../../media/98b9ea9b-ca67-44bd-99ff-a8e3ca0493bc.png)

   > [!IMPORTANT]
   > Check the accuracy of user attributes in Active Directory to ensure that the mail flow rule works as intended. > Note that outbound connector changes may take time to replicate.

4. For **\*Do the following...**, choose **Redirect the message to...** and then specify **the following connector**. The **select connector** box appears. Choose the Outbound connector you created previously.

   You can choose additional properties for the rule, such as the test mode and when to activate the rule.

5. To save the connector, click **Save**.
