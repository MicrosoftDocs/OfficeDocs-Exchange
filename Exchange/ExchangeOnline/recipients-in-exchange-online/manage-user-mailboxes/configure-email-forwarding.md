---
localization_priority: Normal
description: Email forwarding lets you to set up a mailbox to forward email messages sent to that mailbox to another user's mailbox in or outside of your organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c7a7afaf-577e-49d6-8cee-bb4c4a5d570b
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure email forwarding for a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure email forwarding for a mailbox

Email forwarding lets you to set up a mailbox to forward email messages sent to that mailbox to another user's mailbox in or outside of your organization.

> [!IMPORTANT]
> If you're using Office 365 for business, you should configure email forwarding in the [Microsoft 365 admin center: Configure email forwarding in Office 365](https://docs.microsoft.com/microsoft-365/admin/email/configure-email-forwarding)

If your organization uses an on-premises Exchange or hybrid Exchange environment, you should use the on-premises Exchange admin center (EAC) to create and manage shared mailboxes.

## Use the Exchange admin center to configure email forwarding

You can use the Exchange admin center (EAC) set up email forwarding to a single internal recipient, a single external recipient (using a mail contact), or multiple recipients (using a distribution group).

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click or tap the mailbox that you want to configure mail forwarding for, and then click or tap **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Mail Flow**, select **View details** to view or change the setting for forwarding email messages.

    On this page, you can set the maximum number of recipients that the user can send a message to. For on-premises Exchange organizations, the recipient limit is unlimited. For Exchange Online organizations, the limit is 500 recipients.

5. Check the **Enable forwarding** check box, and then click or tap **Browse**.

6. On the **Select Recipient** page, select a user you want to forward all email to. Select the **Deliver message to both forwarding address and mailbox** check box if you want both the recipient and the forwarding email address to get copies of the emails sent. Click or tap **OK**, and then click or tap **Save**.

What if you want to forward mail to an address outside your organization? Or forward mail to multiple recipients? You can do that, too!

- **External addresses**: Create a mail contact and then, in the steps above, select the mail contact on the **Select Recipient** page. Need to know how to create a mail contact? Check out [Manage mail contacts](../../recipients-in-exchange-online/manage-mail-contacts.md).

- **Multiple recipients**: Create a distribution group, add recipients to it, and then in the steps above, select the mail contact on the **Select Recipient** page. Need to know how to create a mail contact? Check out [Create and manage distribution groups](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md).

## How do you know this worked?

To make sure that you've successfully configured email forwarding, do one of the following:

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click or tap the mailbox that you configured email forwarding for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click or tap **Mailbox Features**.

4. Under **Mail Flow**, click or tap **View details** to view the mail forwarding settings.

## Additional information

This topic is for admins. If you want to forward your own email to another recipient, check out the following topics:

- [Forward email to another email account](https://support.microsoft.com/office/ecafbc06-e812-4b9e-a7af-5074a9c7abd0)

- [Manage email messages by using rules](https://go.microsoft.com/fwlink/p/?LinkId=510869)

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
