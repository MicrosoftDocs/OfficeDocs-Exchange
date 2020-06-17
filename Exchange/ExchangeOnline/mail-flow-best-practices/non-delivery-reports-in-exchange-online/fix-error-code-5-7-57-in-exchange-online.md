---
title: "Fix email delivery issues for error code 5.7.57 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Normal
f1.keywords:
- CSH
ms.custom: 
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 
description: "Learn how to fix email issues for error code 5.7.57 in Exchange Online (client was not authenticated to send anonymous mail during MAIL FROM)."
---

# Fix email delivery issues for error code 5.7.57 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.7.57 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|

## Why did I get this bounce message?

When you connect to the smtp.office365.com endpoint to submit (relay) messages through Office 365, you need to authenticate with the credentials of a user who has an Exchange Online mailbox. This bounce message indicates a problem in the configuration of the connecting application or device.

## I got this bounce message. How do I fix it?

- In the configuration of the connecting application or device, verify that the specified credentials are correct.

- Verify that the application or device is able to negotiate TLS, as TLS is required in order to authenticate. For more information, see [How to set up a multifunction device or application to send email using Office 365](../how-to-set-up-a-multifunction-device-or-application-to-send-email-using-office-3.md).

## I'm an email admin. How do I fix this?

The distinction between an end user and an admin is blurred for this bounce message, as the problem lies within the configuration of the application or device.

## Still need help with error code 550 5.7.57?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
