---
localization_priority: Normal
description: 'Summary: Learn how to use the Exchange admin center (EAC) to set up email forwarding.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c7a7afaf-577e-49d6-8cee-bb4c4a5d570b
ms.reviewer:
title: Configure email forwarding for a mailbox
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure email forwarding for a mailbox

Email forwarding lets you to set up a mailbox to forward email messages sent to a user's mailbox to another user's mailbox in or outside of your organization.

## Use the Exchange admin center and the Exchange Management Shell

You can use either the Exchange admin center (EAC) or Exchange Management Shell to set up email forwarding.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" entry in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

### Use the Exchange admin center to set up email forwarding

1. In the Exchange admin center, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click or tap the mailbox that you want to set up mail forwarding for, and then click or tap **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Mail Flow**, select **View details** to view or change the setting for forwarding email messages.

    On this page, you can set the maximum number of recipients that the user can send a message to. The recipient limit is unlimited by default. If you want to specify a limit, click the **Maximum recipients** check box and then type the limit in the text box beneath the check box.

5. Check the **Enable forwarding** check box, and then click or tap **Browse**.

6. On the **Select Recipient** page, select a user you want to forward all email to. Select the **Deliver message to both forwarding address and mailbox** check box if you want both the recipient and the forwarding email address to get copies of the emails sent. Click or tap **OK**, and then click or tap **Save**.

> [!NOTE]
> What if you want to forward emails to an email address outside your organization? You can use the Exchange Management Shell to do this. See the following example in "Use the Exchange Management Shell to configure mail forwarding".

### Use the Exchange Management Shell to set up mail forwarding

Haven't used Exchange Management Shell much? Check out the [Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/exchange-management-shell) topic to learn more. Take a look at the [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) and [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox) topics for more details on the cmdlets used here.

This example delivers email to the mailbox of Douglas Kohn and also forwards all mail sent to Douglas Kohn to an external email address, douglaskohn.parents@fineartschool.net.

```PowerShell
Set-Mailbox -Identity "Douglas Kohn" -DeliverToMailboxAndForward $true -ForwardingSMTPAddress "douglaskohn.parents@fineartschool.net"
```

This example forwards all email sent to the mailbox of Ken Sanchez, an employee of Contoso Suites, to one of his coworkers, pilarp@contoso.com.

```PowerShell
Set-Mailbox -Identity "Ken Sanchez" -ForwardingAddress "pilarp@contoso.com"
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

## How do you know this worked?

To make sure that you've successfully set up email forwarding, do one of the following:

1. In the Exchange admin center, go to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click or tap the mailbox that you configured email forwarding for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page, click or tap **Mailbox Features**.

4. Under **Mail Flow**, click or tap **View details** to view the mail forwarding settings.

Or

Run the following command in the Exchange Management Shell.

```PowerShell
Get-Mailbox <identity> | Format-List ForwardingSMTPAddress,DeliverToMailboxandForward
```

Make sure that the forwarding address is listed in the _ForwardingSMTPAddress_ parameter. Also, if the _DeliverToMailboxAndForward_ parameter is set to `$true`, messages will be delivered to the mailbox and to the forwarding address. If the parameter is set to `$false`, messages are delivered only to the forwarding address.

## End users

Check out the following topics on how to forward your email to another email address by using Outlook and Outlook Web App.

- [Forward email to another email account](https://support.microsoft.com/office/ecafbc06-e812-4b9e-a7af-5074a9c7abd0)

- [Manage email messages by using rules](https://go.microsoft.com/fwlink/p/?LinkId=510869)

## Additional information

For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).
