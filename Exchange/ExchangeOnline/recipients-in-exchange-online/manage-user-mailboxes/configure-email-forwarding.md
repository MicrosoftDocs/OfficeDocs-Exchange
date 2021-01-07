---
localization_priority: Normal
description: Email forwarding lets you to set up a mailbox to forward email messages sent to that mailbox to another user's mailbox in or outside of your organization.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
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
> If you're using Microsoft 365 or Office 365 for business, you should configure email forwarding in the [Configure email forwarding](https://docs.microsoft.com/microsoft-365/admin/email/configure-email-forwarding)

## Use the new Exchange admin center (EAC) to configure email forwarding

You can use the new Exchange admin center (EAC) to set up email forwarding to a single internal recipient, a single external recipient (using a mail contact), or multiple recipients (using a distribution group).

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

1. In the new EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to configure mail forwarding for. A display pane is shown for the selected user mailbox.

3. Under **Mailbox** settings \> **Mail flow settings**, click the **Manage mail flow settings** link.

4. In the **Manage mail flow settings** display pane, you will see the **Email forwarding** option. Click the **Edit** button next to this option to view or change the setting for forwarding email messages.

5. The **Manage email forwarding** display pane is shown. By default the **Forward all emails sent to this mailbox** setting is OFF. Turn it ON.

6. Under **Forwarding address** text box, enter the forwarding email address. The text box allows a search option for searching email addresses by partially entering the    keyword.

7. You can turn ON the **Keep a copy of forwarded email in this mailbox** option if you wish to keep a copy of the forwarded email.

8. Click **Save** to save your changes. Click **Close** to exit from the **Manage mail flow settings** display pane.

## Use the Classic Exchange admin center to configure email forwarding

You can use the Classic Exchange admin center (EAC) to set up email forwarding to a single internal recipient, a single external recipient (using a mail contact), or multiple recipients (using a distribution group).

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

## Additional information

This topic is for admins. If you want to forward your own email to another recipient, check out the following topics:

- [Forward email to another email account](https://support.microsoft.com/office/ecafbc06-e812-4b9e-a7af-5074a9c7abd0)

- [Manage email messages by using rules](https://support.microsoft.com/office/c24f5dea-9465-4df4-ad17-a50704d66c59)

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).
