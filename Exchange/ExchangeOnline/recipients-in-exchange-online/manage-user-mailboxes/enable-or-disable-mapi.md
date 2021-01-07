---
localization_priority: Normal
description: You can use the Exchange admin center or Exchange Online PowerShell to enable or disable MAPI for a user mailbox. When MAPI is enabled, a user's mailbox can be accessed by Outlook or other MAPI email clients. When MAPI is disabled, it can't be accessed by Outlook or other MAPI clients. However, the mailbox will continue to receive email messages, and, assuming that the mailbox is enabled to support access by those clients, a user can access the mailbox to send and receive email by using Outlook on the web, a POP email client, or an IMAP client.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: c2c6718c-a2c0-4ed2-b4ed-364c3cb1f592
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable MAPI for a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable MAPI for a mailbox

You can use the Exchange admin center or Exchange Online PowerShell to enable or disable MAPI for a user mailbox. When MAPI is enabled, a user's mailbox can be accessed by Outlook or other MAPI email clients. When MAPI is disabled, it can't be accessed by Outlook or other MAPI clients. However, the mailbox will continue to receive email messages, and, assuming that the mailbox is enabled to support access by those clients, a user can access the mailbox to send and receive email by using Outlook on the web (formerly known as Outlook Web App), a POP email client, or an IMAP client.

> [!NOTE]
> Support for Outlook on the web and MAPI, POP3, and IMAP4 email clients is enabled by default when a user mailbox is created.

For additional management tasks related to managing email client access to a mailbox, see the following topics:

- [Enable or disable Outlook on the web for a mailbox](enable-or-disable-outlook-web-app.md)

- [Enable or Disable POP3 or IMAP4 access for a user](../../clients-and-mobile-in-exchange-online/pop3-and-imap4/enable-or-disable-pop3-or-imap4-access.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access user settings" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://docs.microsoft.com/answers/topics/office-exchange-server-itpro.html) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the new Exchange admin center to enable or disable MAPI

1. In the new EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable MAPI. A display pane is shown for the selected user mailbox.

3. Under **Mailbox** settings \> **Email apps**, click the **Manage email apps settings** link.

4. In the **Manage settings for email apps** display pane, do one of the following.

   - To disable MAPI, for the **Outlook desktop (MAPI)** option, when the button is **Enabled**, set to **Disabled**.
 
   - To enable MAPI, for the **Outlook desktop (MAPI)** option, when the button is **Disabled**, set to **Enabled**.

5. Click **Save** to save your change. A message **Email app settings updated successfully** is displayed. Click **Close** to exit.

## Use the Classic EAC to enable or disable MAPI

1. In the Classic EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable MAPI, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Email Connectivity**, do one of the following.

   - To disable MAPI, under **MAPI: Enabled**, click **Disable**.

     A warning appears asking if you're sure you want to disable MAPI. Click **Yes**.

   - To enable MAPI, under **MAPI: Disabled**, click **Enable**.

5. Click **Save** to save your change.

## How do you know this worked?

To verify that you've successfully enabled or disabled MAPI for a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Mailbox Features**.

- Under **Email Connectivity**, verify whether MAPI is enabled or disabled.

## Use Exchange Online PowerShell to enable or disable MAPI

This example disables MAPI for the mailbox of Ken Sanchez.

```PowerShell
Set-CASMailbox -Identity "Ken Sanchez" -MAPIEnabled $false
```

This example enables MAPI for the mailbox of Esther Valle.

```PowerShell
Set-CASMailbox -Identity "Esther Valle" -MAPIEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## How do you know this worked?

To verify that you've successfully enabled or disabled MAPI for a user mailbox, do one of the following:

- Run the following command in Exchange Online PowerShell.

  ```PowerShell
  Get-CASMailbox -Identity <MailboxIdentity>
  ```

