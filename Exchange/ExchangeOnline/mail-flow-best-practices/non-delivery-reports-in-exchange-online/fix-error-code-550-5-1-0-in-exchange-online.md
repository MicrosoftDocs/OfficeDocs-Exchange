---
title: "Fix email delivery issues for error code 550 5.1.0 in Exchange Online"
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
ms.custom: MiniMaven
search.appverid:
- BCS160
- MOE150
- MET150
ms.assetid: 477289e6-9150-4627-afce-f61fc7c4605b
description: "Learn how to fix email issues for error code 550 5.5.0 or address rejected: SPF Permanent Error in Exchange Online (the destination email server won't accept messages from the sender or for the recipient)."
---

# Fix email delivery issues for error code 550 5.1.0 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.1.0 or 5.1.0 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN).

Use the information in the NDR to help you decide how to fix the problem.

## Why did I get this bounce message?

The destination email server that generated the 5.1.0 error won't accept messages from you (the sender) or messages for the recipient. This can happen if messages from you (your email address, your Exchange Online organization, or even all of Exchange Online) are being blocked by the recipient.

|||||||
|:-----|:-----|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|![Help symbol](../../media/5bf13e77-0400-4dda-a569-b99b8a918b48.png)|[Details for error code 5.1.0](#details-for-error-code-510)|

## I got this bounce message. How do I fix it?

This section contains steps that you can try to fix the problem yourself.

If these steps don't fix the problem for you, contact your email admin and refer them to this topic so they can try to resolve the issue for you.

### You're in the recipient's block list

Your email address could be in the recipient's personally-maintained block list. This is the likely cause if you can successfully send messages to other recipients in the same domain (for example, @fabrikam.com).

Contact the recipient (by phone, in person, etc.) to verify that your email address isn't in their block list.

### Remove bad entries from your Auto-Complete List

You might have an invalid entry in your Auto-Complete list (also known as the _nickname cache_) for the recipient. For example, the recipient might have been moved from an on-premises Exchange organization to Exchange Online, or vice-versa. Although the recipient's email address is the same, other internal identifiers for the recipient might have changed, thus breaking your cached entry for the recipient.

#### Fix your Auto-Complete list entries in Outlook

To remove invalid recipients or all recipients from your Auto-Complete list in Outlook 2010 later, see [Manage suggested recipients in the To, Cc, and Bcc boxes with Auto-Complete](https://support.office.com/article/dbe46e31-c098-4881-8cf7-66b037bce23e).

To resend the message in Outlook, see [Resend an email message](https://support.office.com/article/ACD16AC4-C881-477D-B4AA-36168FA96088).

#### Fix your Auto-Complete list entries in Outlook on the web

To remove recipients from your Auto-Complete list in Outlook on the web (formerly known as Outlook Web App), do one of the following procedures:

##### Remove a single recipient from your Outlook on the web Auto-Complete list

1. In Outlook on the web, click **New mail**.

2. Start typing the recipient's name or email address in the **To** field until the recipient appears in the drop-down list.

3. Use the Down Arrow and Up Arrow keys to select the recipient, and then press the Delete key.

##### Remove all recipients from your Outlook on the web Auto-Complete list

You can only clear your Auto-Complete list in the light version of Outlook on the web. To open your mailbox in the light version of Outlook on the web, do either of the following steps:

- Open the mailbox in an older web browser that only supports the light version of Outlook on the web (for example, Internet Explorer 9).

- Configure your Outlook on the web settings to only use the light version of Outlook on the web (the change takes effect the next time you open the mailbox):

   1. In Outlook on the web, click **Settings** ![Settings icon](../../media/f4b2e798-fff1-4a14-931f-5677a4543b58.png).

   2. In the **Search all settings** box, type **light** and select **Outlook on the web version** in the results.

   3. In the page that opens, select **Use the light version of Outlook on the web**, and then click **Save**.

   4. Log off, close your web browser, and open the mailbox again in Outlook on the web.

After you open your mailbox in the light version of Outlook on the web, do the following steps to clear all entries from your Auto-Complete list:

1. Choose **Options** and verify that **Messaging** is selected.

2. In the **E-Mail Name Resolution** section, click **Clear Most Recent Recipients list**, and then click **OK** in the confirmation dialog box.

3. While you're still in **Options**, to return your mailbox to the full version of Outlook on the web, go to **Outlook version**, clear the check box for **Use the light version**, and then click **Save**.

4. Log off and close your web browser. The next time you open your mailbox in a supported web browser, you'll use the full version of Outlook on the web.

## I'm an email admin. How do I fix this?

The Sender Policy Framework (SPF) record for your Exchange Online domain might be incomplete, and might not include all sources of mail for your domain. For more information, see [Set up SPF to help prevent spoofing](https://docs.microsoft.com/microsoft-365/security/office-365-security/set-up-spf-in-office-365-to-help-prevent-spoofing).

## Details for error code 5.1.0

The NDR from Exchange Online for this specific error might contain some or all of the following information:

- **User information section**: Address Rejected. A problem occurred during the delivery of this message to this email address.

- **Diagnostic information for administrators section**: Recipient address rejected: SPF Permanent Error.

## Still need help?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
