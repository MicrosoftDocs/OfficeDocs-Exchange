---
title: 'Connect a disabled mailbox: Exchange 2013 Help'
TOCTitle: Connect a disabled mailbox
ms:assetid: a8abd399-75fd-4ee2-b2e4-634b55e4f79f
ms:mtpsurl: https://technet.microsoft.com/library/JJ863439(v=EXCHG.150)
ms:contentKeyID: 50387720
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Connect a disabled mailbox

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to connect a disabled mailbox to an Active Directory user account. When you disable a mailbox, Exchange retains the mailbox in the mailbox database and switches the mailbox to a disabled state. The Exchange attributes are also removed from the corresponding Active Directory user account, but the user account is retained. The mailbox is retained until the deleted mailbox retention period expires, which is 30 days by default, and then it's permanently deleted (or *purged*) from the mailbox database.

Until a disabled mailbox is permanently deleted from the Exchange mailbox database, you can use the EAC or the Shell to reconnect it to the original Active Directory user account.

To learn more about disconnected mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md)

- [Disable or delete a mailbox](disable-or-delete-a-mailbox-exchange-2013-help.md)

- [Connect or restore a deleted mailbox](connect-or-restore-a-deleted-mailbox-exchange-2013-help.md)

- [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- Run the **Get-User** cmdlet in the Shell to verify that the Active Directory user account that you want to connect the disabled mailbox to exists and that it isn't already associated with another mailbox. To connect a disabled mailbox to a user account, the account must exist and the value for the *RecipientType* property has to be `User`, which indicates that the account isn't already mailbox-enabled.

  For on-premises Exchange organizations, you can also verify this information in Active Directory Users and Computers.

- Replace \<DisplayName\> with the display name of the mailbox, and run the following command to verify that the disabled mailbox that you want to connect a user account to exists in the mailbox database and isn't a soft-deleted mailbox.

  ```powershell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisplayName,Database,DisconnectReason
  ```

  To be able to connect a disabled mailbox, the mailbox has to exist in the mailbox database and the value for the *DisconnectReason* property has to be `Disabled`. If the mailbox has been purged from the database, the command won't return any results.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the EAC to connect a disabled mailbox

The following procedure shows how to connect a disabled user mailbox. You can also reconnect disabled linked mailboxes and disabled shared mailboxes to the corresponding user account.

1. In the EAC, navigate to **Recipients**  \> **Mailboxes**.

2. Click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon"), and then click **Connect a mailbox**.

   A list of mailboxes that are disconnected on the selected Exchange server in your Exchange organization will be displayed.

   > [!NOTE]
   > This list of disconnected mailboxes includes disabled mailboxes, deleted mailboxes, and soft-deleted mailboxes.

3. Click the disabled mailbox that you want to reconnect, and then click **Connect**.

4. In the window that asks if you're sure that you want to reconnect the mailbox, click **Yes**.

   Exchange will reconnect the disabled mailbox to the corresponding user account.

## Use the Shell to connect a disabled mailbox

Use the **Connect-Mailbox** cmdlet in the Shell to connect a user account to a disabled mailbox. You have to specify the type of mailbox that you're connecting. The following examples show the syntax for reconnecting user, linked, and shared mailboxes.

This example connects a user mailbox. The *Identity* parameter specifies the disconnected mailbox in the Exchange database. The *User* parameter specifies the Active Directory user account to reconnect the mailbox to.

```powershell
Connect-Mailbox -Identity "Jeffrey Zeng" -Database MBXDB01 -User "Jeffrey Zeng"
```

This example connects a linked mailbox. The *Identity* parameter specifies the disconnected mailbox in the Exchange database. The *LinkedMasterAccount* parameter specifies the Active Directory user account in the account forest that you want to reconnect the mailbox to. The *Alias* parameter specifies the alias, which is the portion of the email address on the left side of the at (@) symbol, for the reconnected mailbox.

```powershell
Connect-Mailbox -Identity "Kai Axford" -Database MBXDB02 -LinkedDomainController FabrikamDC01 -LinkedMasterAccount kai.axford@fabrikam.com -Alias kaia
```

This example connects a shared mailbox.

```powershell
Connect-Mailbox -Identity "Corporate Shared Mailbox" -Database "Mailbox Database 03" -User "Corporate Shared Mailbox" -Alias corpshared -Shared
```

> [!NOTE]
> If you don't include the <EM>Alias</EM> parameter when you run the <STRONG>Connect-Mailbox</STRONG> cmdlet, the value specified in the <EM>User</EM> or <EM>LinkedMasterAccount</EM> parameter is used to create the email address alias for the reconnected mailbox.

For detailed syntax and parameter information, see [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Connect-Mailbox).

## How do you know this worked?

To verify that you've successfully connected a disabled mailbox to a user account, do one of the following:

- In the EAC, click **Recipients**, navigate to the appropriate page for the mailbox type that you reconnected, click **Refresh** ![Refresh Icon](images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif "Refresh Icon"), and verify that the mailbox is listed.

- In Active Directory Users and Computers, right-click the user account whose mailbox you disabled, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is populated with the email address for the reconnected mailbox.

- In the Shell, run the following command.

  ```powershell
  Get-User <identity>
  ```

  The **UserMailbox** value for the *RecipientType* property indicates that the user account and the mailbox are connected. You can also run the **Get-Mailbox** cmdlet to verify that the mailbox exists.
