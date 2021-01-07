---
localization_priority: Normal
description: You can use the EAC or Exchange Online PowerShell to enable or disable Microsoft Exchange ActiveSync for a user mailbox. Exchange ActiveSync is a client protocol that lets users synchronize a mobile device with their Exchange mailbox. Exchange ActiveSync is enabled by default when a user mailbox is created. To learn more, see Exchange ActiveSync.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: dcf7c05b-b1b9-4b0f-800d-fec9f2ddc9e4
ms.reviewer: 
f1.keywords:
- NOCSH
title: Enable or disable Exchange ActiveSync for a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable or disable Exchange ActiveSync for a mailbox

You can use the EAC or Exchange Online PowerShell to enable or disable Microsoft Exchange ActiveSync for a user mailbox. Exchange ActiveSync is a client protocol that lets users synchronize a mobile device with their Exchange mailbox. Exchange ActiveSync is enabled by default when a user mailbox is created. To learn more, see [Exchange ActiveSync in Exchange Online](../../clients-and-mobile-in-exchange-online/exchange-activesync/exchange-activesync.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mobile devices" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the new Exchange admin center to enable or disable Exchange ActiveSync

1. In the new EAC, navigate to **Recipients** \> **Mailboxes**. 

2. In the list of user mailboxes, click the mailbox that you want to enable or disable Exchange ActiveSync for. A display pane is shown for the selected user mailbox.

3. Under **Mailbox** settings \> **Email apps**, click the **Manage email apps settings** link.

4. In the **Manage settings for email apps** display pane, do one of the following.

   - To disable Exchange ActiveSync, for the **Mobile (Exchange ActiveSync)** option, when the button is **Enabled**, set to **Disabled**. 

   - To enable Exchange ActiveSync, for the **Mobile (Exchange ActiveSync)** option, when the button is **Disabled**, set to **Enabled**. 

5. Click **Save** to save your change. A message **Email app settings updated successfully** is displayed. Click **Close** to exit.

## Use the Classic EAC to enable or disable Exchange ActiveSync

1. In the Classic EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to enable or disable Exchange ActiveSync for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Mobile Devices**, do one of the following:

   - To disable Exchange ActiveSync click **Disable Exchange ActiveSync**.

     A warning appears asking if you're sure you want to disable Exchange ActiveSync. Click **Yes**.

   - To enable Exchange ActiveSync, click **Enable Exchange ActiveSync**.

5. Click **Save** to save your change.

> [!NOTE]
> You can enable and disable Exchange ActiveSync for multiple user mailboxes by using the EAC bulk edit feature. For more information about how to do this, see the "Bulk edit user mailboxes" section in [Manage user mailboxes](manage-user-mailboxes.md).

## How do you know it worked?

To verify that you've successfully enabled or disabled Exchange ActiveSync for a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Mailbox Features**.

- Under **Mobile Devices**, verify whether Exchange ActiveSync is enabled or disabled.

## Use Exchange Online PowerShell to enable or disable Exchange ActiveSync

This example disables Exchange ActiveSync for the mailbox of Yan Li.

```PowerShell
Set-CASMailbox -Identity "Yan Li" -ActiveSyncEnabled $false
```

This example enables Exchange ActiveSync for the mailbox of Elly Nkya.

```PowerShell
Set-CASMailbox -Identity "Elly Nkya" -ActiveSyncEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## How do you know this worked?

To verify that you've successfully enabled or disabled Exchange ActiveSync for a user mailbox using PowerShell, do one of the following:

- Run the following command in Exchange Online PowerShell.

  ```PowerShell
  Get-CASMailbox -Identity <MailboxIdentity>
  ```
  If Exchange ActiveSync is enabled, the value for the _ActiveSyncEnabled_ property is `True`. If Exchange ActiveSync is disabled, the value is `False`.

