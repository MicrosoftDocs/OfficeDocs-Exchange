---
localization_priority: Normal
description: Learn how administrators can enable or disable Exchange ActiveSync access to specific mailboxes in Exchange Server 2016 or Exchange Server 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: dcf7c05b-b1b9-4b0f-800d-fec9f2ddc9e4
ms.reviewer:
title: Enable or disable Exchange ActiveSync access to mailboxes in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Enable or disable Exchange ActiveSync access to mailboxes in Exchange Server

ActiveSync is a client protocol that lets users synchronize their Exchange mailbox with a mobile device. By default, ActiveSync is enabled on new user mailboxes. Disabling ActiveSync on a mailbox prevents the user from synchronizing their mailbox with a mobile device (by using ActiveSync).

Administrators can use the Exchange admin center (EAC) or the Exchange Management Shell to enable or disable Exchange ActiveSync access to a mailbox.

For more information about ActiveSync, see [Exchange ActiveSync](exchange-activesync.md).

For information about setting up email on your mobile device, see these topics:

- [Set up Office apps and email on iOS devices](https://go.microsoft.com/fwlink/p/?linkid=396655)

- [Set up Office apps and email on Android](https://go.microsoft.com/fwlink/p/?linkid=525632)

- [Set up Office apps and email on Windows Phone](https://support.microsoft.com/office/9bccc8b8-a321-4d0d-a45e-6e06a3438e43)

For additional management tasks related to user access to mailboxes, see these topics:

- [Enable or disable Outlook on the web access to mailboxes in Exchange Server](../../clients/outlook-on-the-web/mailbox-access.md)

- [Enable or disable POP3 or IMAP4 access to mailboxes in Exchange Server](../../clients/pop3-and-imap4/configure-mailbox-access.md)

- [Enable or disable MAPI access to mailboxes in Exchange Server](../../clients/mapi-mailbox-access.md)

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- For more information about accessing and using the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md).

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange ActiveSync settings" entry in the [Clients and mobile devices permissions](../../permissions/feature-permissions/client-and-mobile-device-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Enable or disable Exchange ActiveSync access to a single mailbox

### Use the EAC to enable or disable Exchange ActiveSync access to a mailbox

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, find the mailbox that you want to modify. You can:

   - Scroll through the list of mailboxes.

   - Click **Search** ![Search icon](../../media/ITPro_EAC_.png) and enter part of the user's name, email address, or alias.

   - Click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Advanced search** to find the mailbox.

     Once you've found the mailbox that you want to modify, select it, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the mailbox properties page that opens, click **Mailbox features**.

4. In the **Mobile Devices** section, configure one of these settings:

   - If ActiveSync is enabled on the mailbox, you'll see a **Disable Exchange ActiveSync** link. Click the link to disable ActiveSync, and then click **Yes** in the warning message that appears.

   - If ActiveSync is disabled on the mailbox, you'll see a **Enable Exchange ActiveSync** link. Click the link to enable ActiveSync.

     ![Mobile device settings for a mailbox in the EAC at Recipients \> select mailbox \> Edit \> Mailbox features \> Mobile Devices](../../media/52364427-9557-443e-8366-35a1339a0f15.png)

     When you're finished, click **Save**.

### Use the Exchange Management Shell to enable or disable Exchange ActiveSync access to a mailbox

To enable or disable ActiveSync access to a single mailbox, use this syntax:

```powershell
Set-CasMailbox -Identity <MailboxIdentity> -ActiveSyncEnabled <$true | $false>
```

This example disables ActiveSync access to the mailbox named Yan Li.

```powershell
Set-CasMailbox -Identity "Yan Li" -ActiveSyncEnabled $false
```

This example enables ActiveSync access to the mailbox named Elly Nkya.

```powershell
Set-CasMailbox -Identity "Elly Nkya" -ActiveSyncEnabled $true
```

For detailed syntax and parameter information, see [Set-CASMailbox](https://docs.microsoft.com/powershell/module/exchange/set-casmailbox).

## Enable or disable Exchange ActiveSync access to multiple mailboxes

### Use the EAC to enable or disable Exchange ActiveSync access to multiple mailboxes

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, find the mailboxes that you want to modify. You can:

   - Scroll through the list of mailboxes.

   - Click **Search** ![Search icon](../../media/ITPro_EAC_.png) and enter part of the user's name, email address, or alias.

   - Click **More options** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png) \> **Advanced search** to find the mailbox.

3. In the list of mailboxes, select multiple mailboxes of the same type (for example, **User**) from the list. For example:

   - Select a mailbox, hold down the Shift key, and select another mailbox that's farther down in the list.

   - Hold down the CTRL key as you select each mailbox.

   After you select multiple mailboxes of the same type, the title of the details pane changes to **Bulk Edit**.

4. In the details pane, scroll down to **Exchange ActiveSync**, click **Enable** or **Disable**, and then click **OK** in the warning message that appears.

   [Bulk select mailboxes in the EAC to enable or disable Exchange ActiveSync](../../media/b28f6467-f702-424b-b9ca-8f6edac109f2.png)

### Use the Exchange Management Shell to enable or disable Exchange ActiveSync access to multiple mailboxes

You can use the **Get-Mailbox**, **Get-User** or **Get-Content** cmdlets to identify the mailboxes that you want to modify. For example:

- Use the _OrganizationalUnit_ parameter to filter the mailboxes by organizational unit (OU).

- Use the _Filter_ parameter to create OPATH filters that identify the mailboxes. For more information, see [Filterable Properties for the -Filter Parameter](https://docs.microsoft.com/powershell/exchange/filter-properties).

- Use a text file to specify the mailboxes. The text file contains one mailbox (email address, name, or other unique identifier) on each line like this:

  > ebrunner@tailspintoys.com <br/> fapodaca@tailspintoys.com <br/> glaureano@tailspintoys.com <br/> hrim@tailspintoys.com

This example disables ActiveSync access to all user mailboxes in the North America\Finance OU.

```powershell
$NAFinance = Get-Mailbox -OrganizationalUnit "OU=Marketing,OU=North America,DC=contoso,DC=com" -Filter "RecipientTypeDetails -eq 'UserMailbox'" -ResultSize Unlimited; $NAFinance | foreach {Set-CasMailbox $_.Identity -ActiveSyncEnabled $false}
```

This example disables ActiveSync access to all user mailboxes in the Engineering department in Washington state.

```powershell
Get-User -Filter "RecipientType -eq 'UserMailbox' -and Department -like 'Engineering*' -and StateOrProvince -eq 'WA'" | Set-CasMailbox -ActiveSyncEnabled $false
```

This example uses the text file C:\My Documents\Accounts.txt to disable ActiveSync access to the specified mailboxes.

```powershell
Get-Content "C:\My Documents\Accounts.txt" | foreach {Set-CasMailbox $_ -ActiveSyncEnabled $false}
```

For detailed syntax and parameter information, see [Get-Mailbox](https://docs.microsoft.com/powershell/module/exchange/get-mailbox) and [Get-User](https://docs.microsoft.com/powershell/module/exchange/get-user).

## How do you know this worked?

To verify that you've successfully enabled or disabled Exchange ActiveSync access to a mailbox, do any of these steps:

- In the EAC, go to **Recipients** \> **Mailboxes** \> select the mailbox \> click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png) \> **Mailbox features** \> and verify the Exchange ActiveSync value in the **Mobile Devices** section.

  - If ActiveSync access is enabled for the mailbox, you'll see **Disable Exchange ActiveSync**.

  - If ActiveSync access is disabled for the mailbox, you'll see **Enable Exchange ActiveSync**.

  ![Mobile device settings for a mailbox in the EAC at Recipients \> select mailbox \> Edit \> Mailbox features \> Mobile Devices](../../media/52364427-9557-443e-8366-35a1339a0f15.png)

- In the Exchange Management Shell, replace _\<MailboxIdentity\>_ with the identity of the mailbox (for example, name, alias, or email address), and run this command:

  ```powershell
  Get-CasMailbox -Identity "<MailboxIdentity>"
  ```

- Use the same filter that you used to identify the mailboxes, but use the **Get-CasMailbox** cmdlet instead of **Set-CasMailbox**. For example:

  ```powershell
  Get-User -Filter "RecipientType -eq 'UserMailbox' -and Department -like 'Engineering*' -and StateOrProvince -eq 'WA'" | Get-CasMailbox
  ```

- In the Exchange Management Shell, run this command to show all mailboxes where ActiveSync access is disabled:

  ```powershell
  Get-CasMailbox -ResultSize unlimited -Filter "ActiveSyncEnabled -eq `$false"
  ```
