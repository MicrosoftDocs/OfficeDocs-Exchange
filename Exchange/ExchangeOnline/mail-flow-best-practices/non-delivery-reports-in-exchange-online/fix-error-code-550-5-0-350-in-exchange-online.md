---
title: "Fix email delivery issues for error code 550 5.0.350 in Exchange Online"
ms.author: v-mapenn
author: mattpennathe3rd
manager: dansimp
ms.reviewer: 
audience: Admin
ms.topic: troubleshooting
ms.service: o365-administration
localization_priority: Normal
f1.keywords:
- CSH
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid:
description: "Learn how to fix email issues for error code 550 5.0.350 or 550 x-dg-ref header is too long."
---

# Fix email delivery issues for error code 550 5.0.350 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.0.350 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

Use the information in the NDR to help you decide how to fix the problem.

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|

## Why did I get this bounce message?

5.0.350 is a generic wrapper that's used by Exchange Online for a wide variety of non-specific errors that are typically returned by the recipient's email organization.

But, if the NDR also contains `x-dg-ref header is too long`, that's a specific problem with a specific solution. This issue occurs if you use Rich Text formatting in Outlook messages. The message likely contains at least one attachment, and one of the attachments is likely an email message that also contains at least one attached email message.

## I got this bounce message. How do I fix it?

If the NDR contains `x-dg-ref header is too long`, use **HTML** formatting for messages in Outlook instead of **Rich Text Format**. For more information, see [Change the message format to HTML, Rich Text Format, or plain text](https://support.office.com/article/338A389D-11DA-47FE-B693-CF41F792FEFA).

Otherwise, forward the NDR to your admin for help.

## I'm an email admin. How do I fix this?

### x-dg-ref header is too long

In Rich Text formatted messages, the attachment's binary large object (BLOB) becomes part of the header stream in the `X-MS-TNEF-Correlator` header field. If the attachment is too big, the line length of the header field is too long, so the receiving email server will reject the message.

In Exchange Online, you can control TNEF (also known as the Transport Neutral Encapsulation Format, Outlook Rich Text Format, or Exchange Rich Text Format) settings in remote domains, and in the properties of mail contacts or mail users. For more information, see [Message format and transmission in Exchange Online](../message-format-and-transmission.md).

### Other 5.0.350 errors

Typically, there's nothing that Office 365 support can do for you, since the problem lies with the recipient's email system.

The **Diagnostic information for administrators** section in the bounce message will contain the original error message when Office 365 tried to send the message to the external email server or service. Use this information to help identify the issue, and to see if there's anything you can do to fix the problem.

## Still need help with error code 550 5.0.350?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
