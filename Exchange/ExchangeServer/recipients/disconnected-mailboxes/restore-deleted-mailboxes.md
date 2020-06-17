---
localization_priority: Normal
description: 'Summary: Learn how to use the Exchange admin center (EAC) or the Exchange Management Shell to connect a deleted mailbox to an Active Directory user account in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a5e6ac44-5901-4eab-9017-c6fae80a0f83
ms.reviewer:
title: Connect or restore a deleted mailbox
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Connect or restore a deleted mailbox

When you delete a mailbox, Exchange retains the mailbox in the mailbox database and switches the mailbox to a disabled state. The associated Active Directory user account is also deleted. The mailbox is retained until the deleted mailbox retention period expires, which is 30 days by default, and then it's permanently deleted (or *purged*) from the mailbox database.

Until a deleted mailbox is permanently deleted from the Exchange mailbox database, you can use the EAC or the Exchange Management Shell to connect it to an Active Directory user account. You can also use the Exchange Management Shell to restore the contents of the deleted mailbox to an existing mailbox.

To learn more about disconnected mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes.md)

- [Disable or delete a mailbox in Exchange Server](disable-or-delete-mailboxes.md)

- [Connect a disabled mailbox](connect-disabled-mailboxes.md)

- [Permanently delete a mailbox](permanently-delete-mailboxes.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- To open the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To open the Exchange Management Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- Create a new user account in Active Directory to connect the deleted mailbox to. Or use the **Get-User** cmdlet in the Exchange Management Shell to verify that the Active Directory user account that you want to connect the deleted mailbox to exists and that it isn't already associated with another mailbox. To connect a deleted mailbox to a user account, the account must exist and the value for the _RecipientType_ property has to be `User`, which indicates that the account isn't already mailbox-enabled.

   For on-premises Exchange organizations, you can also verify this information in Active Directory Users and Computers.

   > [!IMPORTANT]
   > When you connect deleted linked mailboxes, resource mailboxes, or shared mailboxes, the Active Directory user account that you're connecting the mailbox to must be disabled.

- To verify that the deleted mailbox that you want to connect a user account to exists in the mailbox database and isn't a soft-deleted mailbox, run the following command:

   ```PowerShell
   Get-MailboxDatabase | foreach {Get-MailboxStatistics -Database $_.name} | where {$_.DisplayName -eq "<display name>"} | Format-List DisplayName,Database,DisconnectReason
   ```

   The deleted mailbox has to exist in the mailbox database and the value for the _DisconnectReason_ property has to be `Disabled`. If the mailbox has been purged from the database, the command won't return any results.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](../../permissions/feature-permissions/recipient-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Connect a deleted mailbox

When you connect a deleted mailbox, you associate the mailbox with a user account that isn't mail-enabled, which means that it doesn't have an existing mailbox. To connect a deleted mailbox to a user account that has a mailbox, you have to restore the deleted mailbox. For more information, see [Restore a deleted mailbox](#restore-a-deleted-mailbox) later in this topic.

### Use the EAC to connect a deleted mailbox

The following procedure shows how to connect a deleted user mailbox to a user account. You can also use this procedure to connect linked mailboxes, resource mailboxes, and shared mailboxes that have been deleted to a user account.

1. In the EAC, go to **Recipients** \> **Mailboxes**.

2. Click **More** ![More Options icon](../../media/ITPro_EAC_MoreOptionsIcon.png), and then click **Connect a mailbox**.

   A list of mailboxes that are disconnected on the selected Exchange server in your Exchange organization will be displayed.

   > [!NOTE]
   > This list of disconnected mailboxes includes disabled mailboxes, deleted mailboxes, and soft-deleted mailboxes.

3. Click the deleted mailbox that you want to connect a user to, and then click **Connect**.

4. In the window that asks if you're sure that you want to connect the mailbox, click **Yes**.

   A list of user accounts that aren't mail-enabled is displayed.

5. Click the user that you want to connect the deleted mailbox to, and then click **OK**.

   Exchange will connect the deleted mailbox to the user account that you selected.

### Use the Exchange Management Shell to connect a deleted mailbox

Use the **Connect-Mailbox** cmdlet in the Exchange Management Shell to connect a deleted mailbox to a user account that isn't mail enabled. You have to specify the type of mailbox that you're connecting. The following examples show the syntax for reconnecting user, linked, room, equipment, and shared mailboxes. In all examples, the optional _Alias_ parameter is used to specify the email alias, which is the portion of the email address on the left side of the at (@) symbol. If you don't include the _Alias_ parameter, the value specified in the _User_ or _LinkedMasterAccount_ parameter is used to create the alias for the email address for the reconnected mailbox.

> [!NOTE]
> As previously stated, when you connect linked, resource, or shared mailboxes, the Active Directory user account that you're linking the mailbox to must be disabled.

This example connects a deleted user mailbox to a user account that isn't mail enabled. The _Identity_ parameter specifies the display name of the deleted mailbox retained in the mailbox database named MBXDB01. The _User_ parameter specifies the Active Directory user account to connect the mailbox to.

```PowerShell
Connect-Mailbox -Identity "Paul Cannon" -Database MBXDB01 -User "Robin Wood" -Alias robinw
```

> [!NOTE]
> You can also use the values for the `LegacyDN` or `MailboxGuid` properties to identify the deleted mailbox.

This example connects a linked mailbox. The _Identity_ parameter specifies the deleted mailbox on the mailbox database named MBXDB02. The _LinkedMasterAccount_ parameter specifies the Active Directory user account in the account forest that you want to connect the mailbox to. The _LinkedDomainController_ parameter specifies a domain controller in the account forest.

```PowerShell
Connect-Mailbox -Identity "Temp User" -Database MBXDB02 -LinkedDomainController FabrikamDC01 -LinkedMasterAccount danpark@fabrikam.com -Alias dpark
```

This example connects a room mailbox.

```PowerShell
Connect-Mailbox -Identity "rm2121" -Database "MBXResourceDB" -User "Conference Room 2121" -Alias ConfRm2121 -Room
```

This example connects an equipment mailbox.

```PowerShell
Connect-Mailbox -Identity "MotorPool01" -Database "MBXResourceDB" -User "Van01 (12 passengers)" -Alias van01 -Equipment
```

This example connects a shared mailbox.

```PowerShell
Connect-Mailbox -Identity "Printer Support" -Database MBXDB01 -User "Corp Printer Support" -Alias corpprint -Shared
```

> [!NOTE]
> You can also use the `LegacyDN` or `MailboxGuid` values to identify the deleted mailbox.

For detailed syntax and parameter information, see [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/connect-mailbox).

### How do you know this worked?

To verify that you've successfully connected a deleted mailbox to a user account, do one of the following steps:

- In the EAC, click **Recipients**, go to the appropriate page for the mailbox type that you connected, click **Refresh** ![Refresh icon](../../media/ITPro_EAC_RefreshIcon.png), and verify that the mailbox is listed.

- In Active Directory Users and Computers, right-click the user account that you connected to the mailbox, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is populated with the email address for the connected mailbox.

- In the Exchange Management Shell, run the following command.

   ```PowerShell
   Get-User <identity>
   ```

   The **UserMailbox** value for the _RecipientType_ property indicates that the user account and the mailbox are connected. You can also run the **Get-Mailbox \<identity\>** command to verify that the mailbox was connected.

## Restore a deleted mailbox
<a name="restore"> </a>

You can use the Exchange Management Shell to restore a deleted mailbox to an existing mailbox using the **New-MailboxRestoreRequest** cmdlet. When you restore a deleted mailbox, its contents are copied to an existing mailbox, which is referred to as the *target mailbox*. After a deleted mailbox is restored, it's still retained in the mailbox database until it's permanently deleted by an administrator or purged after the deleted mailbox retention period expires.

After a mailbox restore request is successfully completed, it's retained for 30 days, by default, before it's removed. You can remove the mailbox sooner by using the **Remove-StoreMailbox** cmdlet.

> [!NOTE]
> You can't use the EAC to restore a deleted mailbox.

### Use the Exchange Management Shell to restore a deleted mailbox

To create a mailbox restore request, you have to use the display name, legacy distinguished name (DN), or mailbox GUID of the deleted mailbox. Use the **Get-MailboxStatistics** cmdlet to display the values of the `DisplayName`, `MailboxGuid`, and `LegacyDN` properties for the deleted mailbox that you want to restore. For example, run the following commands to return this information for all disabled and deleted mailboxes in your organization.

```PowerShell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "Disabled"} | Format-Table DisplayName,MailboxGuid,Database,DisconnectDate
```

This example restores the deleted mailbox, which is identified by the _SourceStoreMailbox_ parameter and is located on the MBXDB01 mailbox database, to the target mailbox Debra Garcia. The _AllowLegacyDNMismatch_ parameter is used so the source mailbox can be restored to a different mailbox, one that doesn't have the same legacy DN value.

```PowerShell
New-MailboxRestoreRequest -SourceStoreMailbox e4890ee7-79a2-4f94-9569-91e61eac372b -SourceDatabase MBXDB01 -TargetMailbox "Debra Garcia" -AllowLegacyDNMismatch
```

This example restores Pilar Pinilla's deleted archive mailbox to her current archive mailbox. The _AllowLegacyDNMismatch_ parameter isn't necessary because a primary mailbox and its corresponding archive mailbox have the same legacy DN.

```PowerShell
New-MailboxRestoreRequest -SourceStoreMailbox "Personal Archive - Pilar Pinilla" -SourceDatabase "MDB01" -TargetMailbox pilarp@contoso.com -TargetIsArchive
```

For detailed syntax and parameter information, see [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/new-mailboxrestorerequest).

### How do you know this worked?

To verify that you've successfully restored a deleted mailbox to the target mailbox, run the **Get-MailboxRestoreRequest** cmdlet to display information about the restore request. If the restore request was successfully created, the _Status_ property will have a value of `Queued`, `InProgress`, or `Completed`. After the restore request is completed, the contents from the deleted mailbox will appear in the target mailbox.

For more information, see:

- [Manage Mailbox Restore Requests](https://docs.microsoft.com/exchange/manage-mailbox-restore-requests-exchange-2013-help)

- [Get-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/get-mailboxrestorerequest)

- [Get-MailboxRestoreRequestStatistics](https://docs.microsoft.com/powershell/module/exchange/get-mailboxrestorerequeststatistics)
