---
localization_priority: Normal
description: 'Fix issues with printers, scanners, and line of business applications that use Microsoft 365 or Office 365 to send email. '
ms.topic: troubleshooting
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c75542a8-c792-42c0-a8c5-291df987512d
ms.reviewer: 
title: Fix issues with printers, scanners, and LOB applications that send email using Microsoft 365 or Office 365
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- BCS160
- MET150
- MOE150
f1.keywords:
- CSH
ms.custom: MiniMaven
ms.service: exchange-online
manager: serdars

---

# Fix issues with printers, scanners, and LOB applications that send email using Microsoft 365 or Office 365

Email clients provide actionable error messages when something goes wrong. Sending email from devices and applications is less easy to fix, and you might not get clear information to help you. This article can help you troubleshoot, and it uses printer configurations as examples.

 **As a first step to fixing any problems, check your configuration**. See [How to set up a multifunction device or application to send email using Microsoft 365 or Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md) for detailed information about the configuration options.

## My printer is already configured for email, but I don't know which configuration option it uses

Below are the three configuration options to help you identify which one is in use:

 **1. SMTP client submission (recommended)**

- Your printer is connected to the server "smtp.office365.com."

- You entered an email address and password for the printer mailbox.

- The printer can send email to people inside and outside your organization.

![Shows how a multifunction printer connects to Microsoft 365 or Office 365 using SMTP client submission.](../media/d5c5a7fa-aba4-4bf4-976f-4c7128fcab2d.png)

 **2. Direct send**

- Your printer is connected to a Microsoft 365 or Office 365 server whose name ends with "mail.protection.outlook.com."

- There is no connector set up in Microsoft 365 or Office 365 for emails sent from your organization's network.

- The printer can send email only to people in your organization; email can't be sent to recipients outside your organization.

![Shows how a multifunction printer uses your Microsoft 365 or Office 365 MX endpoint to send email directly to recipients in your organization only.](../media/cb07aae7-ca31-43a7-a468-74c293b37a66.png)

 **3. Microsoft 365 or Office 365 SMTP relay**

- Your printer is connected to a Microsoft 365 or Office 365 server whose name ends with "mail.protection.outlook.com."

- There is a connector set up in Microsoft 365 or Office 365 for emails sent from your organization's network to Microsoft 365 or Office 365.

- The printer can send email to people inside and outside your organization.

![Shows how a multifunction printer connects to Microsoft 365 or Office 365 using SMTP relay.](../media/258cb8b1-752d-47b8-91e9-a0176dfcfad4.png)

## Fix issues with SMTP client submission
<a name="TroubleshootSMTP"> </a>

### I set up my printer for SMTP client submission, but it still can't send emails

1. Check the settings entered directly into the printer:

   |**Printer setting**|**Value**|
   |:-----|:-----|
   |Server/smart host|smtp.office365.com|
   |Port|Port 587 (recommended) or port 25|
   |TLS/ StartTLS|Enabled|
   |Username/email address and password|Login credentials of Microsoft 365 or Office 365 mailbox the printer uses|

2. If your printer didn't require a password for the email address you entered, your printer is trying to send emails without logging on to Microsoft 365 or Office 365. SMTP client submission requires your printer to log on to Microsoft 365 or Office 365. Direct send and Microsoft 365 or Office 365 SMTP relay do not require a logon; consider one of these options instead.

3. Your printer or application must send email from the same address that you entered logon credentials for during email setup. If the printer or application tries to send email from a different account, this results in an error similar to:

   > 5.7.60 SMTP; Client does not have permissions to send as this sender.

   For example, if you entered login credentials for sales@contoso.com in your application settings, but the application tries to send emails from salesperson1@contoso.com, this is not supported. For this scenario, use Microsoft 365 or Office 365 SMTP relay instead.

4. Test the user name and password by logging on to Outlook on the web, and try to send a test email to make sure the account is not blocked. If the user is blocked, you can find help in the article, [Remove blocked users from the Restricted Users portal](https://docs.microsoft.com/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam).

5. Next, test that you can connect to Microsoft 365 or Office 365 from your network by doing the following:

   1. Follow the instructions to [install the Telnet Client tool](https://go.microsoft.com/fwlink/p/?linkId=179054) on a computer on the same network as the device or application.

   2. Run the tool from the command line by typing **telnet**.

   3. Type **open smtp.office365.com 587** (or substitute **25** for **587** if you are using that port setting instead).

   4. If you connected successfully to an Office 365 server, expect to receive a response line similar to this:

      > 220 BY1PR10CA0041.outlook.office365.com Microsoft ESMTP MAIL Service ready at Mon, 1 Jun 2015 12:00:00 +0000

   5. If you can't connect to Microsoft 365 or Office 365, your network firewall or Internet Service Provider (ISP) might have blocked port 587 or 25. Correct this so you can send email from your printer.

6. If none of these issues applies to your device, it might not meet requirements for Transport Layer Security (TLS) encryption. Your device must support TLS version 1.0 or above. Update the firmware on the device to solve this, or try one of the other configuration options where TLS is optional.

   For more information about TLS, see [How Exchange Online uses TLS to secure email connections](https://docs.microsoft.com/microsoft-365/compliance/exchange-online-uses-tls-to-secure-email-connections) and for detailed technical information about how Exchange Online uses TLS with cipher suite ordering, see [Enhancing mail flow security for Exchange Online](https://go.microsoft.com/fwlink/?LinkId=620841).

### I receive an authentication error when my device tries to send email

This can be caused by a number of issues:

1. Make sure that you entered the correct username and password.

2. Try logging into OWA with the printer's username and password. Send an email to make sure that the mailbox is active and has not been blocked for sending spam.

3. Check that your device or application supports TLS version 1.0 or above. The best way to check is by upgrading the firmware on the device or updating the application you're sending email from to the latest version. Contact your device manufacturer to confirm that it supports TLS version 1.0 or above.

### Error: 5.7.60 SMTP; Client does not have permissions to send as this sender

This error indicates that the device is trying to send an email from an address that doesn't match the logon credentials. An example would be if your entered login credentials for sales@contoso.com in your application settings but the application tries to send emails from salesperson1@contoso.com. If your application or printer behaves this way, use Microsoft 365 or Office 365 SMTP relay because SMTP client submission does not support this scenario.

### Error: Client was not authenticated to send anonymous mail during MAIL FROM

This error indicates that your printer connects to the SMTP client submission endpoint (smtp.office365.com). However, your printer must also logon to a mailbox to send a message. This error occurs when you have not entered mailbox logon credentials in the printer's settings. If there is no option to enter credentials, this printer does not support SMTP client submission; use either direct send or Microsoft 365 or Office 365 SMTP relay instead. See [How to set up a multifunction device or application to send email using Microsoft 365 or Office 365](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md).

### Error: 550 5.1.8 Bad outbound sender

This error indicates that the device is trying to send an email from a Microsoft 365 or Office 365 mailbox that is on a spam block list. For help, see [Remove blocked users from the Restricted Users portal](https://docs.microsoft.com/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam).

## Fix issues with direct send
<a name="Troubleshootdirectsend"> </a>

### I set up my printer for direct send and it's not sending email - or - My device was sending email using direct send, but it stopped working

This can be caused by a number of issues.

1. A common reason for issues with direct send is a blocked IP address. If antispam tools detect outbound spam from your organization, your IP address can be blocked by a spam block list. Check whether your IP address is on a block list by using a third-party service, such as MXToolbox or WhatIsMyIPAddress. Follow up with the organization that added your IP address to their block list. Microsoft 365 and Office 365 use block lists to protect our service. For help, see [Remove blocked users from the Restricted Users portal](https://docs.microsoft.com/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam).

2. To rule out a problem with your device, send a test email to check your connection to Microsoft 365 or Office 365. To send a test email, follow these steps in the article, [Use Telnet to Test SMTP Communication](https://docs.microsoft.com/Exchange/mail-flow/test-smtp-with-telnet). If you can't connect to Microsoft 365 or Office 365, your network or ISP might have blocked communication using port 25. If you can't reverse this, use SMTP client submission instead.

### Error: Client was not authenticated to send anonymous mail during MAIL FROM

This indicates that you are connecting to the SMTP client submission endpoint (smtp.office365.com), which can't be used for direct send. For direct send, use the MX endpoint for your Microsoft 365 or Office 365 organization, which ends with "mail.protection.outlook.com." You can find your MX endpoint by following the steps in [Option 2: Send mail directly from your printer or application to Microsoft 365 or Office 365 (direct send)](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md#option-2-send-mail-directly-from-your-printer-or-application-to-microsoft-365-or-office-365-direct-send).

### My emails are not sent to recipients who are not in my organization

This is by design. Direct send allows email to be sent only to recipients in your organization that are hosted in Microsoft 365 or Office 365. If you need to send to external recipients, use SMTP client submission or Microsoft 365 or Office 365 SMTP relay.

### The MX endpoint is too long for the printer setting box. Can I use an IP address instead?

It's not possible to use an IP address in place of an MX endpoint. This could result in your not being able to send messages in the future. If the MX endpoint is too long, consider using SMTP client submission, which has a shorter endpoint (smtp.office365.com).

### Emails from my device are marked as junk by Microsoft 365 or Office 365

For direct send, we recommend using a device that sends from a static IP address. This allows you to set up a Sender Policy Framework (SPF) record to help prevent emails being marked as spam. Check that your SPF record is set up with your static IP address. A network or ISP change could change your static IP address. Update your SPF record to reflect this change. If you aren't sending from your own static IP address, consider SMTP client submission instead.

## Fix issues with Microsoft 365 or Office 365 SMTP relay

### I set up my printer for Microsoft 365 or Office 365 SMTP relay but it's not sending email -or- My device was sending email using SMTP relay, but it stopped working

This can be caused by a number of issues.

1. A common reason for issues with Microsoft 365 or Office 365 SMTP relay is a blocked IP address. If antispam tools detect outbound spam from your organization, your IP address can be blocked by a spam block list. Check whether your IP address is on a block list by using a third-party service, such as MXToolbox or WhatIsMyIPAddress. Follow up with the organization that added your IP address to their block list. Microsoft 365 and Office 365 use block lists to protect our service. For help, see [Remove blocked users from the Restricted Users portal](https://docs.microsoft.com/microsoft-365/security/office-365-security/removing-user-from-restricted-users-portal-after-spam).

2. To rule out a problem with your device, send a test email to check your connection to Microsoft 365 or Office 365. To send a test email, follow these steps in the article, [Use Telnet to Test SMTP Communication](https://docs.microsoft.com/Exchange/mail-flow/test-smtp-with-telnet). If you can't connect to Microsoft 365 or Office 365, your network or ISP might have blocked communication using port 25. If you can't reverse this, use SMTP client submission instead.

### Emails are no longer being sent to external recipients

Network or ISP changes might change your static IP address. This results in your connector not identifying and relaying your messages to external recipients. Update your connector and your SPF record with the new IP address. Follow the steps in [Option 3: Configure a connector to send mail using Microsoft 365 or Office 365 SMTP relay](how-to-set-up-a-multifunction-device-or-application-to-send-email-using-microsoft-365-or-office-365.md#option-3-configure-a-connector-to-send-mail-using-microsoft-365-or-office-365-smtp-relay) to edit your existing connector settings.

### Emails from my device are marked as junk by Microsoft 365 or Office 365

Microsoft 365 or Office 365 SMTP relay requires your device to send email from a static IP address. Check that your SPF record is set up with your static IP address. A network or ISP change could change your static IP address. Update your SPF record to reflect this change. If you aren't sending from your own static IP address, consider SMTP client submission instead.
