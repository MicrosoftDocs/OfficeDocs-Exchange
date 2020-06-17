---
title: 'Enable or disable MAPI for a mailbox: Exchange 2013 Help'
TOCTitle: Enable or disable MAPI for a mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c2c6718c-a2c0-4ed2-b4ed-364c3cb1f592
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable MAPI for a mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the Exchange admin center or the Exchange Management Shell to enable or disable MAPI for a user mailbox. When MAPI is enabled, a user's mailbox can be accessed by Outlook or other MAPI email clients. When MAPI is disabled, it can't be accessed by Outlook or other MAPI clients. However, the mailbox will continue to receive email messages, and, assuming that the mailbox is enabled to support access by those clients, a user can access the mailbox to send and receive email by using Outlook Web App, a POP email client, or an IMAP client.

> [!NOTE]
> Support for Outlook Web App and MAPI, POP3, and IMAP4 email clients is enabled by default when a user mailbox is created.

For additional management tasks related to managing email client access to a mailbox, see the following topics:

- [Enable or disable Outlook Web App for a mailbox](enable-or-disable-outlook-web-app-exchange-2013-help.md)

- [Enable or disable IMAP4 access for a user](enable-or-disable-imap4-access-for-a-user-exchange-2013-help.md)

- [Enable or disable POP3 access for a user](enable-or-disable-pop3-access-for-a-user-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Client Access user settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable MAPI

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable MAPI, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Email Connectivity**, do one of the following.

   - To disable MAPI, under **MAPI: Enabled**, click **Disable**.

     A warning appears asking if you're sure you want to disable MAPI. Click **Yes**.

   - To enable MAPI, under **MAPI: Disabled**, click **Enable**.

5. Click **Save** to save your change.

## Use the Exchange Management Shell to enable or disable MAPI

This example disables MAPI for the mailbox of Ken Sanchez.

```powershell
Set-CASMailbox -Identity "Ken Sanchez" -MAPIEnabled $false
```

This example enables MAPI for the mailbox of Esther Valle.

```powershell
Set-CASMailbox -Identity "Esther Valle" -MAPIEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## How do you know this worked?

To verify that you've successfully enabled or disabled MAPI for a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Mailbox Features**.

- Under **Email Connectivity**, verify whether MAPI is enabled or disabled.

Or

- Run the following command in the Exchange Management Shell.

  ```powershell
  Get-CASMailbox <identity>
  ```

    If MAPI is enabled, the value for the _MapiEnabled_ property is `True`. If MAPI is disabled, the value is `False`.
