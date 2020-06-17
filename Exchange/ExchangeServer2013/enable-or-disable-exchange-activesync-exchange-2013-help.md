---
title: 'Enable or disable Exchange ActiveSync for a mailbox: Exchange 2013 Help'
TOCTitle: Enable or disable Exchange ActiveSync for a mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: dcf7c05b-b1b9-4b0f-800d-fec9f2ddc9e4
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable Exchange ActiveSync for a mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to enable or disable Microsoft Exchange ActiveSync for a user mailbox. Exchange ActiveSync is a client protocol that lets users synchronize a mobile device with their Exchange mailbox. Exchange ActiveSync is enabled by default when a user mailbox is created. To learn more, see [Exchange ActiveSync](exchange-activesync-exchange-2013-help.md)).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange ActiveSync settings" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable Exchange ActiveSync

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable Exchange ActiveSync for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Mobile Devices**, do one of the following:

   - To disable Exchange ActiveSync click **Disable Exchange ActiveSync**.

     A warning appears asking if you're sure you want to disable Exchange ActiveSync. Click **Yes**.

   - To enable Exchange ActiveSync, click **Enable Exchange ActiveSync**.

5. Click **Save** to save your change.

> [!NOTE]
> You can enable and disable Exchange ActiveSync for multiple user mailboxes by using the EAC bulk edit feature. For more information about how to do this, see the "Bulk edit user mailboxes" section in [Manage user mailboxes](manage-user-mailboxes-exchange-2013-help.md).

## Use the Shell to enable or disable Exchange ActiveSync

This example disables Exchange ActiveSync for the mailbox of Yan Li.

```powershell
Set-CASMailbox -Identity "Yan Li" -ActiveSyncEnabled $false
```

This example enables Exchange ActiveSync for the mailbox of Elly Nkya.

```powershell
Set-CASMailbox -Identity Ellyn@contoso.com -ActiveSyncEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## How do you know this worked?

To verify that you've successfully enabled or disabled Exchange ActiveSync for a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Mailbox Features**.

- Under **Mobile Devices**, verify whether Exchange ActiveSync is enabled or disabled.

Or

- Run the following command in the Shell.

  ```powershell
  Get-CASMailbox <identity>
  ```

    If Exchange ActiveSync is enabled, the value for the _ActiveSyncEnabled_ property is `True`. If Exchange ActiveSync is disabled, the value is `False`.
