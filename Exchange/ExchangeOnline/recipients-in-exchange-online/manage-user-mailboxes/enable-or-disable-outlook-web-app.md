---
localization_priority: Normal
description: You can use the EAC or Exchange Online PowerShell to enable or disable Outlook on the web for a user mailbox. When Outlook on the web is enabled, a user can use Outlook on the web to send and receive email. When Outlook on the web is disabled, the mailbox will continue to receive email messages, and a user can access it to send and receive email by using a MAPI client, such as Microsoft Outlook, or with a POP or IMAP email client, assuming that the mailbox is enabled to support access by those clients.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: abc19646-6211-4f18-a060-e347452dcc53
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable Outlook on the web for a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable Outlook on the web for a mailbox

You can use the EAC or Exchange Online PowerShell to enable or disable Outlook on the web (formerly known as Outlook Web App) for a user mailbox. When Outlook on the web is enabled, a user can use Outlook on the web to send and receive email. When Outlook on the web is disabled, the mailbox will continue to receive email messages, and a user can access it to send and receive email by using a MAPI client, such as Microsoft Outlook, or with a POP or IMAP email client, assuming that the mailbox is enabled to support access by those clients.

> [!NOTE]
> Support for Outlook on the web and MAPI, POP3, and IMAP4 email clients is enabled by default when a user mailbox is created.

For additional management tasks related to managing email client access to a mailbox, see the following topics:

- [Enable or disable MAPI for a mailbox](enable-or-disable-mapi.md)

- [Enable or Disable POP3 or IMAP4 access for a user](../../clients-and-mobile-in-exchange-online/pop3-and-imap4/enable-or-disable-pop3-or-imap4-access.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access user settings" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the new EAC to enable or disable Outlook on the web

1. In the new EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable Outlook on the web. A display pane is shown for the selected user mailbox.

3. Under **Mailbox** settings > **Email apps**, click the **Manage email apps settings** link.

4. In the **Manage settings for email apps** display pane, do one of the following.

   -  To disable Outlook on the web, for the **Outlook on the web** option, when the button is **Enabled**, set to **Disabled**.

   -  To enable Outlook on the web, for the **Outlook on the web** option, when the button is **Disabled**, set to **Enabled**.
   
5. Click **Save** to save your change. A message **Email app settings updated successfully** is displayed. Click **Close** to exit.

## Use the Classic EAC to enable or disable Outlook on the web

1. In the Classic EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable Outlook on the web for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Email Connectivity**, do one of the following:

   - To disable Outlook on the web, under **Outlook Web App: Enabled**, click **Disable**.

    A warning appears asking if you're sure you want to disable Outlook on the web. Click **Yes**.

   - To enable Outlook on the web, under **Outlook Web App: Disabled**, click **Enable**.

5. Click **Save** to save your changes.

> [!NOTE]
> You can enable and disable Outlook on the web for multiple user mailboxes by using the EAC bulk edit feature. For more information about how to do this, see the "Bulk edit user mailboxes" section in [Manage user mailboxes](manage-user-mailboxes.md).

## How do you know it worked?

To verify that you've successfully enabled or disabled Outlook on the web for a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Mailbox Features**.

- Under **Email Connectivity**, verify whether Outlook on the web is enabled or disabled.

## Use Exchange Online PowerShell to enable or disable Outlook on the web

This example disables Outlook on the web for the mailbox of Yan Li.

```PowerShell
Set-CASMailbox -Identity "Yan Li" -OWAEnabled $false
```

This example enables Outlook on the web for the mailbox of Elly Nkya.

```PowerShell
Set-CASMailbox -Identity "Elly Nkya" -OWAEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## How do you know this worked?

To verify that you've successfully enabled or disabled Outlook on the web for a user mailbox, do one of the following:

- Run the following command in Exchange Online PowerShell.

  ```PowerShell
  Get-CASMailbox -Identity <MailboxIdentity>
  ```

  If Outlook on the web is enabled, the value for the _OWAEnabled_ property is `True`. If Outlook on the web is disabled, the value is `False`.

