---
title: "Fix email delivery issues for error code 5.7.13 or 5.7.135 in Exchange Online"
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
ms.assetid: fc1ea7e0-3680-4778-ad96-4328c60d517c
description: "Learn how to fix email issues for error code 550 5.7.13 or 5.7.135 in Exchange Online (the public folder recipient is configured to reject messages from external or unauthenticated senders)."
---

# Fix email delivery issues for error code 5.7.13 or 5.7.135 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see error code 550 5.7.13 or 550 5.7.135 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN). You'll see this automated notification when the recipient is a public folder that's configured to reject messages from external senders (senders from outside the organization).

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do I fix it?](#i-got-this-bounce-message-how-do-i-fix-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm an email admin. How do I fix this?](#im-an-email-admin-how-do-i-fix-this)|

## I got this bounce message. How do I fix it?

Only an email admin in the recipient's organization can fix this issue. Contact the email admin and refer them to this information so they can try to resolve the issue for you.

## I'm an email admin. How do I fix this?

The two methods that will allow the external sender to send messages to the public folder in your organization are described in the following sections.

To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](https://docs.microsoft.com/Exchange/exchange-admin-center).

### Method 1: Allow all internal and external senders to send messages to this public folder

1. In the EAC, go to **Public folders** \> **Public folders** \> select the public folder from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif)

   ![View public folders to help fix DSN 5.7.135](../../media/fcffe06b-0f7d-4370-b4be-519982aaf5b3.png)

2. In the public folder properties dialog box that opens, go to **Mail flow settings**, and configure the following settings in the **Accept messages from** section:

    - Clear the check box for **Require that all senders are authenticated**.

    - Select **All senders**.

   ![Public folder delivery restrictions to help fix DSN 5.7.135](../../media/0b1eed9e-0da4-4c75-a0e5-17ce93ae0663.png)

3. Click **Save**.

### Method 2: Use the public folder's allowed senders list

Instead of allowing all external senders to send messages to this public folder, you can use the public folder's allowed senders list to selectively allow messages from all internal senders and the specified external senders.

**Notes**:

- To add an external sender to a public folder's allowed senders list, you must first create a [mail contact](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-contacts) or a [mail user](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-users) to represent the external sender in your organization.

- To add everyone in your organization to a public folder's allowed sender's list, you can create a [distribution group](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups) or a [dynamic distribution group](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-dynamic-distribution-groups/manage-dynamic-distribution-groups) that contains everyone in your organization. After you create this group, you can add it to the public folder's allowed senders list.

- The public folder's allowed senders list is completely different from the organization's allowed senders list for anti-spam that you manage in the EAC at **Protection** \> **Spam filter**.

To configure the public folder's allowed senders list, open the EAC do the following steps:

1. In the EAC, go to **Public folders** \> **Public folders** \> select the public folder from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif)

   ![View public folders to help fix DSN 5.7.135](../../media/fcffe06b-0f7d-4370-b4be-519982aaf5b3.png)

2. In the public folder properties dialog box that opens, go to **Mail flow settings**, and configure the following settings in the **Accept messages from** section:

   - Clear the check box for **Require that all senders are authenticated**.

   - Select **Only senders in the following list**, and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif). In the **Select Members** dialog box that opens, select and add the external senders and the all internal users group. When you're finished, click **OK**.

     ![Custom allowed sender list for a public folder to help fix DSN 5.7.135](../../media/792bf991-356e-48e8-b356-c669cc9b084e.png)

3. Click **Save**.

## Still need help with error code 550 5.7.13 or 550 5.7.135?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)
