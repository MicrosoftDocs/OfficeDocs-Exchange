---
localization_priority: Normal
description: "If a printer, scanner, or LOB application can't send email using Office 365, you might need to set up IIS to work as an intermediary. Learn how. "
ms.topic: article
author: supotter
ms.author: supotter
ms.assetid: eb57abd2-3859-4e79-b721-2ed1f0f579c9
ms.reviewer: 
title: How to configure IIS for relay with Office 365
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- BCS160
- MET150
- MOE150
audience: Admin
ms.custom: MiniMaven
ms.service: exchange-online
manager: scotv

---

# How to configure IIS for relay with Office 365

When you set up a multifunction device or application to send email through Office 365, there are some cases where the device or application can't connect directly to Office 365. In these cases, you need to set up Internet Information Services (IIS) to work as an intermediary.

You might want to do this in the following scenarios:

- You don't have an on-premises messaging system any longer

- You have line-of-business (LOB) programs or devices in an on-premises environment

- Your LOB programs and devices have to send email messages to remote domains and to your Exchange Online mailboxes

Before proceeding, review [How to set up a multifunction device or application to send email using Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3.md) as there may be an available option that doesn't require setting up an additional server to relay.

> [!NOTE]
> These instructions can be modified for other SMTP relays that you might have in your organization.

## What you need to know before you begin

- Estimated time to complete: 15 minutes

- Your on-premises domain must be added as an accepted domain in Office 365. For example, if the account you're relaying from is bob@tailspintoys.com, you have to add tailspintoys.com as an accepted domain in Office 365.

- Your on-premises account must also be either an Exchange Online-licensed user in Office 365 or an alternative email address of an Exchange Online-licensed user. For example, if the account that you're relaying from is printer@tailspintoys.com and you want to relay through bob@contoso.com (an Office 365 user), you have to add printer@tailspintoys.com as an alternate email address to bob@contoso.com.

## Set up Exchange Online as an SMTP Relay Using Windows Server 2012

### Step 1: Install Internet Information Services (IIS)

1. In Server Manager, select **Add Roles**.

2. On the Before you begin page in the Add Roles Wizard, select **Next**.

3. On the Select Installation Type page, select **Role-based or Feature-based installation**.

4. On the Select destination server page, choose **Select a server from the server pool**, and select the server that will be running SMTP services. Select **Next**.

5. On the Select Server Roles page, select **Web Server (IIS)**, and then select **Next**. If a page that requests additional features is displayed, select **Add Features** and then select **Next**.

6. On the Select Role Services page, make sure that Anonymous under Security is selected, and then select **Next**.

7. On the Confirm Installation Steps page, select **Install**.

### Step 2: Install SMTP

1. Open Server Manager and select **Add Roles and Features**.

2. Select **Server Selection** and make sure that the server that will be running the SMTP server is selected and then select Features.

3. On the Select Features screen, choose **SMTP Server**. You may be prompted to install additional components. If that's the case, select **Add Required Features** and select **Next**.

4. Select **Install**. After the installation is finished, you may have to start the SMTP service by using the Services snap-in for the Microsoft Management Console (MMC).

### Step 3: Set up SMTP

1. Open Server Manager, select **Tools**, and then select **Internet Information Services (IIS) 6.0**.

2. Expand the current server, right-click the **SMTP Virtual Server**, and then select **Properties**.

3. On the General tab, select **Advanced** \> **Add**.

4. In the IP Address box, specify the address of the server that's hosting the SMTP server.

5. In the Port box, enter **25** and select **OK**.

6. On the Access tab, do the following:

   1. Select **Authentication** and make sure that **Anonymous Access** is selected.

   2. Select **Connection** \> **Only the List Below**, and then specify the IP addresses of the devices that will be connecting to the SMTP server, such as printers.

   3. Select **Relay** \> **Only the List Below**, and then specify the IP address of the devices relaying through this SMTP server

7. On the Delivery tab, select **Outbound Security**, and then do the following:

   1. Select **Anonymous Authentication**.

   2. Select **TLS Encryption**.

   3. Select **Outbound Connections**, and in the TCP Port box, enter **25** and select **OK**.

   4. Select **Advanced** and specify **contoso-com.mail.protection.outlook.com** as the Smart Host.

### Step 4: Restart the IIS service and the SMTP service

## Set up Exchange Online as an SMTP Relay Using Windows Server 2008

### Step 1: Install Internet Information Services (IIS)

1. In Server Manager, select **Add Roles**.

2. On the Before you begin page in the Add Roles Wizard, select **Next**.

3. On the Select Server Roles page, select **Web Server (IIS)** and select **Install**.

4. Select Next until you get to the Select Role Services page.

5. In addition to what is already selected, make sure that **ODBC Logging, IIS Metabase Compatibility, and IIS 6 Management Console** are selected and then select **Next**.

6. When you're prompted to install IIS, select Install. You may need to restart the server after the installation is finished.

### Step 2: **Install SMTP**

1. Open Server Manager and select **Add Roles and Features**.

2. On the Select Features screen, choose **SMTP Server**. You may be prompted to install additional components. If that's the case, select **Add Required Features** and select **Next**.

3. Select **Install**. After the installation is finished, you may have to start the SMTP service by using the Services snap-in for the Microsoft Management Console (MMC).

### Step 3: Set up SMTP

1. Select **Start \> Administrative Tools \> Internet Information Services (IIS) 6.0**.

2. Expand the current server, right-click the **SMTP Virtual Server**, and then select **Properties**.

3. On the General tab, select **Advanced \> Add**.

4. In the IP Address box, specify the address of the server that's hosting the SMTP server.

5. In the Port box, enter **25** and select **OK**.

6. On the Access tab, do the following:

   1. Select Authentication and make sure that **Anonymous Access** is selected.

   2. Select **Connection \> Only the List Below**, and then specify the IP addresses of the devices that will be connecting to the SMTP server, such as printers.

   3. Select **Relay \> Only the List Below**, and then specify the IP address of the devices relaying through this SMTP server

7. On the Delivery tab, select **Outbound Security**, and then do the following:

   1. Select **Anonymous Authentication**.

   2. Select **TLS Encryption**.

   3. Select **Outbound Connections** and in the TCP Port box, enter **25** and select **OK**.

   4. Select **Advanced** and specify **contoso-com.mail.protection.outlook.com** as the Smart Host.

### Step 4 Restart the IIS service and the SMTP service

## How do you know this worked?

You can test SMTP relay services without using an separate LOB application or device.

To test SMTP relay services, use the following steps.

1. Create a text file using Notepad or another text editor. The file should contain the following code. Replace the source and destination email addresses with the addresses you will use to relay SMTP.

   ```
   FROM: <source email address>
   TO: <destination email address>
   SUBJECT: Test email
   This is a test email sent from my SMTP server
   ```

2. Save the text file as Email.txt.

3. Copy the Email.txt file into the following folder: C:\InetPub\MailRoot\Pickup.

4. After a short time, the file should automatically be moved to the C:\InetPub\MailRoot\Queue folder. When the SMTP server delivers the mail, the file is automatically deleted from the local folder.

    > [!CAUTION]
    > If the SMTP server can't deliver the message, a non-delivery report (NDR) is created in the C:\InetPub\MailRoot\BadMail folder. You can use this NDR to diagnose delivery issues.

## Related Topics

[Troubleshoot email sent from printers and business applications](fix-issues-with-printers-scanners-and-lob-applications-that-send-email-using-off.md)

[How to set up a multifunction device or application to send email using Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3.md)
