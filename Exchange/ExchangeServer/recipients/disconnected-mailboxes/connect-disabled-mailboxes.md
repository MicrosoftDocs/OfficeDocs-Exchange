---
localization_priority: Normal
description: 'Summary: Learn how you can use the Exchange admin center(EAC) or the Exchange Management Shell in Exchange to connect a disabled mailbox to an Active Directory user account.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: a8abd399-75fd-4ee2-b2e4-634b55e4f79f
ms.date: 7/5/2018
ms.reviewer: 
title: Connect a disabled mailbox
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Connect a disabled mailbox

When you disable a mailbox, Exchange retains the mailbox in the mailbox database and switches the mailbox to a disabled state. The Exchange attributes are also removed from the corresponding Active Directory user account, but the user account is retained. The mailbox is retained until the deleted mailbox retention period expires, which is 30 days by default, before it's then deleted permanently (or *purged*) from the mailbox database.

Until a disabled mailbox is permanently deleted from the Exchange mailbox database, you can use the EAC or the Exchange Management Shell to reconnect it to the original Active Directory user account.

To learn more about disconnected mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes.md)

- [Disable or delete a mailbox in Exchange Server](disable-or-delete-mailboxes.md)

- [Connect or restore a deleted mailbox](restore-deleted-mailboxes.md)

- [Permanently delete a mailbox](permanently-delete-mailboxes.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](http://technet.microsoft.com/library/63976059-25f8-4b4f-b597-633e78b803c0.aspx).

- Run the **Get-User** cmdlet in theExchange Management Shell to verify that the Active Directory user account that you want to connect the disabled mailbox to exists and that it isn't already associated with another mailbox. To connect a disabled mailbox to a user account, the account must exist and the value for the _RecipientType_ property has to be `User`, which indicates that the account isn't already mailbox-enabled.

  You can also verify this information in Active Directory Users and Computers.

- Replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands in the Exchange Management Shell to verify that the disabled mailbox that you want to connect to a user account exists and isn't a soft-deleted mailbox.

  ```
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisplayName,Database,DisconnectReason
  ```

  To be able to connect a disabled mailbox, the mailbox has to exist in the mailbox database and the value for the _DisconnectReason_ property has to be `Disabled`. If the mailbox has been purged from the database, the command won't return any results.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351)..

## Use the EAC to connect a disabled mailbox

The following procedure shows how to connect a disabled user mailbox. You can also reconnect disabled linked mailboxes and disabled shared mailboxes to the corresponding user account.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. Click **More** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png), and then click **Connect a mailbox**.

    A list of mailboxes that are disconnected on the selected Exchange server in your Exchange organization will be displayed.

    > [!NOTE]
    > This list of disconnected mailboxes includes disabled mailboxes, deleted mailboxes, and soft-deleted mailboxes.

3. Click the disabled mailbox that you want to reconnect, and then click **Connect**.

4. In the window that asks if you're sure that you want to reconnect the mailbox, click **Yes**.

    Exchange will reconnect the disabled mailbox to the corresponding user account.

## Use the Exchange Management Shell to connect a disabled mailbox

Use the **Connect-Mailbox** cmdlet in the Exchange Management Shell to connect a user account to a disabled mailbox. You have to specify the type of mailbox that you're connecting. The following examples show the syntax for reconnecting user, linked, and shared mailboxes.

This example connects a user mailbox. The _Identity_ parameter specifies the disconnected mailbox in the Exchange database. The _User_ parameter specifies the Active Directory user account to reconnect the mailbox to.

```
Connect-Mailbox -Identity "Jeffrey Zeng" -Database MBXDB01 -User "Jeffrey Zeng"
```

This example connects a linked mailbox. The _Identity_ parameter specifies the disconnected mailbox in the Exchange database. The _LinkedMasterAccount_ parameter specifies the Active Directory user account in the account forest that you want to reconnect the mailbox to. The _Alias_ parameter specifies the alias, which is the portion of the email address on the left side of the at (@) symbol, for the reconnected mailbox.

```
Connect-Mailbox -Identity "Kai Axford" -Database MBXDB02 -LinkedDomainController FabrikamDC01 -LinkedMasterAccount kai.axford@fabrikam.com -Alias kaia
```

This example connects a shared mailbox.

```
Connect-Mailbox -Identity "Corporate Shared Mailbox" -Database "Mailbox Database 03" -User "Corporate Shared Mailbox" -Alias corpshared -Shared
```

> [!NOTE]
> If you don't include the _Alias_ parameter when you run the **Connect-Mailbox** cmdlet, the value specified in the _User_ or _LinkedMasterAccount_ parameter is used to create the email address alias for the reconnected mailbox.

For detailed syntax and parameter information, see [Connect-Mailbox](http://technet.microsoft.com/library/48757062-abe5-4c61-acc5-5884569c1d8b.aspx).

## How do you know this worked?

To verify that you've successfully connected a disabled mailbox to a user account, do one of the following:

- In the EAC, click **Recipients**, navigate to the appropriate page for the mailbox type that you reconnected, click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png), and verify that the mailbox is listed.

- In Active Directory Users and Computers, right-click the user account whose mailbox you disabled, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is populated with the email address for the reconnected mailbox.

- In theExchange Management Shell,replace \<Identity\> with the name of the user account and run the following command:

  ```
  Get-User "<Identity>"
  ```

    The **UserMailbox** value for the _RecipientType_ property indicates that the user account and the mailbox are connected. You can also run the **Get-Mailbox** cmdlet to verify that the mailbox exists.
