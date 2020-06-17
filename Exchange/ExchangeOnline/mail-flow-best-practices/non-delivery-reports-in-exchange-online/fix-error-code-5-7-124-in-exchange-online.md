---
title: "Fix email delivery issues for error code 5.7.124 in Exchange Online"
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
ms.assetid: 3269ef55-dabe-4710-9921-a3753c66b2c9
description: "Learn how to fix email issues for error code 5.7.124 in Exchange Online (the sender isn't in the recipient group's allowed senders list)."
---

# Fix email delivery issues for error code 5.7.124 in Exchange Online

It's frustrating when you get an error after sending an email message. This topic describes what you can do if you see status code 550 5.7.124 or 5.7.124 in a non-delivery report (also known as an NDR, bounce message, delivery status notification, or DSN). You'll see this automated notification when the sender sender isn't specified in the group's allowed senders list (directly or as a member of a group). Depending how the group is configured, even the group's owner might need to be in the group's allowed senders list in order to send messages to the group.

|||||
|:-----|:-----|:-----|:-----|
|![Email user icon](../../media/31425afd-41a9-435e-aa85-6886277c369b.png)|[I got this bounce message. How do fix I it?](#i-got-this-bounce-message-how-do-fix-i-it)|![Email admin icon](../../media/3d4c569e-b819-4a29-86b1-4b9619cf2acf.png)|[I'm the group owner or email admin. How do I fix this?](#im-the-group-owner-or-email-admin-how-do-i-fix-this)|

## I got this bounce message. How do fix I it?

Typically, members of a group can send messages to the group. If the group is in your Exchange Online organization, you can try to join the group in Outlook or Outlook on the web (formerly known as Outlook Web App). For instructions, see [Join a group in Outlook](https://support.microsoft.com/office/2e59e19c-b872-44c8-ae84-0acc4b79c45d).

You might have to wait for the group's owner to approve your request to join the group before you can successfully send messages to it. If the group isn't in your organization, or if the group doesn't allow requests to join, then you'll need to ask the group owner to add you to the allowed senders list. You'll find instructions for finding the group owner in the NDR.

## I'm the group owner or email admin. How do I fix this?

The two methods that will allow the sender to send messages to the group are described in the following sections.

To open the Exchange admin center (EAC), see [Exchange admin center in Exchange Online](https://docs.microsoft.com/Exchange/exchange-admin-center).

### Method 1: Add the sender to the group's existing allowed senders list

1. In the EAC, go to **Recipients** \> **Groups** \> select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

2. In the group properties dialog box that opens, go to **Delivery management** and then click **Add** ![Add icon](../../media/8ee52980-254b-440b-99a2-18d068de62d3.gif).

   ![Add an email address to the allowed senders list](../../media/bfa84c19-f972-4428-9001-47bebd8b9125.png)

3. In the **Select Allowed Senders** dialog box that opens, select and add the sender or a group that the sender is a member of. When you're finished, click **OK**.

4. Click **Save**.

> [!NOTE]
> To add an external sender to a group's allowed senders list, you must first create a [mail contact](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-contacts) or a [mail user](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-users) to represent the external sender in your organization.

### Method 2: Allow all internal and external senders to send messages to this group

If you decide that you don't need to restrict the message senders to this group, you can remove the restrictions so anyone can send messages to this group:

1. In the EAC, go to **Recipients** \> **Groups** \> select the group from the list, and then click **Edit** ![Edit icon](../../media/ebd260e4-3556-4fb0-b0bb-cc489773042c.gif).

2. In the group properties dialog box that opens, go to **Delivery management**.

3. In the **Distribution Group** box select **Delivery management** and configure the following settings:

   - Remove any entries in the allowed senders list by selecting one entry, pressing CTRL + A to select all entries, and then clicking **Remove** ![Remove icon](../../media/adf01106-cc79-475c-8673-065371c1897b.gif).

     ![Removing allowed senders from a distribution list](../../media/c135fe59-4c77-43e1-b514-da8dbe4b5fb6.png)

   - Select **Senders inside and outside of my organization**.

4. When you're finished, click **Save**.

## More information about groups

Groups with more than 5,000 members have the following restrictions automatically applied to them:

- Senders to the group must be members of the group.

- Messages sent to the group require the approval of a moderator. To configure moderation for a group, see [Configure a moderated recipient in Exchange Online](../../recipients-in-exchange-online/configure-a-moderated-recipient.md).

- Large messages can't be sent to the group. However, senders of large messages will receive a different NDR. For more information about large messages, see [Distribution group limits](https://docs.microsoft.com/office365/servicedescriptions/exchange-online-service-description/exchange-online-limits#distribution-group-limits).

## Still need help with error code 550 5.7.124?

[![Get help from the Office 365 community forums](../../media/12a746cc-184b-4288-908c-f718ce9c4ba5.png)](https://go.microsoft.com/fwlink/p/?LinkId=518605)

[![Admins: Sign in and create a service request](../../media/10862798-181d-47a5-ae4f-3f8d5a2874d4.png)](https://admin.microsoft.com/AdminPortal/Home#/support

[![Admins: Call Support](../../media/9f262e67-e8c9-4fc0-85c2-b3f4cfbc064e.png)](https://go.microsoft.com/fwlink/p/?LinkID=518322)

## See also

[Email non-delivery reports in Exchange Online](non-delivery-reports-in-exchange-online.md)

[Create and manage distribution groups in Exchange Online](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md).
