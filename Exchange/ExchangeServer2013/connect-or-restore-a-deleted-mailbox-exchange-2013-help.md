---
title: 'Connect or restore a deleted mailbox: Exchange 2013 Help'
TOCTitle: Connect or restore a deleted mailbox
ms:assetid: a5e6ac44-5901-4eab-9017-c6fae80a0f83
ms:mtpsurl: https://technet.microsoft.com/library/JJ863438(v=EXCHG.150)
ms:contentKeyID: 50387719
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Connect or restore a deleted mailbox

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to connect a deleted mailbox to an Active Directory user account. When you delete a mailbox, Exchange retains the mailbox in the mailbox database and switches the mailbox to a disabled state. The associated Active Directory user account is also deleted. The mailbox is retained until the deleted mailbox retention period expires, which is 30 days by default, and then it's permanently deleted (or *purged*) from the mailbox database.

Until a deleted mailbox is permanently deleted from the Exchange mailbox database, you can use the EAC or the Shell to connect it to an Active Directory user account. You can also use the Shell to restore the contents of the deleted mailbox to an existing mailbox.

To learn more about disconnected mailboxes and perform other related management tasks, see the following topics:

- [Disconnected mailboxes](disconnected-mailboxes-exchange-2013-help.md)

- [Disable or delete a mailbox](disable-or-delete-a-mailbox-exchange-2013-help.md)

- [Connect a disabled mailbox](connect-a-disabled-mailbox-exchange-2013-help.md)

- [Permanently delete a mailbox](permanently-delete-a-mailbox-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- Create a new user account in Active Directory to connect the deleted mailbox to. Or use the **Get-User** cmdlet in the Shell to verify that the Active Directory user account that you want to connect the deleted mailbox to exists and that it isn't already associated with another mailbox. To connect a deleted mailbox to a user account, the account must exist and the value for the *RecipientType* property has to be `User`, which indicates that the account isn't already mailbox-enabled.

  For on-premises Exchange organizations, you can also verify this information in Active Directory Users and Computers.

  > [!IMPORTANT]
  > When you connect deleted linked mailboxes, resource mailboxes, or shared mailboxes, the Active Directory user account that you're connecting the mailbox to must be disabled.

- To verify that the deleted mailbox that you want to connect a user account to exists in the mailbox database and isn't a soft-deleted mailbox, replace _\<DisplayName\>_ with the display name of the mailbox, and run the following commands.

  ```powershell
  $dbs = Get-MailboxDatabase
  $dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisplayName -eq "<DisplayName>"} | Format-List DisplayName,Database,DisconnectReason
   ```

  The deleted mailbox has to exist in the mailbox database and the value for the *DisconnectReason* property has to be `Disabled`. If the mailbox has been purged from the database, the command won't return any results.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Connect a deleted mailbox

When you connect a deleted mailbox, you associate the mailbox with a user account that isn't mail-enabled, which means that it doesn't have an existing mailbox. To connect a deleted mailbox to a user account that has a mailbox, you have to restore the deleted mailbox. For more information, see Restore a deleted mailbox later in this topic.

## Use the EAC to connect a deleted mailbox

The following procedure shows how to connect a deleted user mailbox to a user account. You can also use this procedure to connect linked mailboxes, resource mailboxes, and shared mailboxes that have been deleted to a user account.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. Click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon"), and then click **Connect a mailbox**.

   A list of mailboxes that are disconnected on the selected Exchange server in your Exchange organization will be displayed.

   > [!NOTE]
   > This list of disconnected mailboxes includes disabled mailboxes, deleted mailboxes, and soft-deleted mailboxes.

3. Click the deleted mailbox that you want to connect a user to, and then click **Connect**.

4. In the window that asks if you're sure that you want to connect the mailbox, click **Yes**.

   A list of user accounts that aren't mail-enabled is displayed.

5. Click the user that you want to connect the deleted mailbox to, and then click **OK**.

   Exchange will connect the deleted mailbox to the user account that you selected.

## Use the Shell to connect a deleted mailbox

Use the **Connect-Mailbox** cmdlet in the Shell to connect a deleted mailbox to a user account that isn't mail enabled. You have to specify the type of mailbox that you're connecting. The following examples show the syntax for reconnecting user, linked, room, equipment, and shared mailboxes. In all examples, the optional *Alias* parameter is used to specify the email alias, which is the portion of the email address on the left side of the at (@) symbol. If you don't include the *Alias* parameter, the value specified in the *User* or *LinkedMasterAccount* parameter is used to create the alias for the email address for the reconnected mailbox.

> [!NOTE]
> As previously stated, when you connect linked, resource, or shared mailboxes, the Active Directory user account that you're linking the mailbox to must be disabled.

This example connects a user mailbox. The *Identity* parameter specifies the display name of the deleted mailbox retained in the mailbox database named MBXDB01. The *User* parameter specifies the Active Directory user account to connect the mailbox to.

```powershell
Connect-Mailbox -Identity "Paul Cannon" -Database MBXDB01 -User "Robin Wood" -Alias robinw
```

> [!NOTE]
> You can also use the values for the <CODE>LegacyDN</CODE> or <CODE>MailboxGuid</CODE> properties to identify the deleted mailbox.

This example connects a linked mailbox. The *Identity* parameter specifies the deleted mailbox on the mailbox database named MBXDB02. The *LinkedMasterAccount* parameter specifies the Active Directory user account in the account forest that you want to connect the mailbox to. The *LinkedDomainController* parameter specifies a domain controller in the account forest.

```powershell
Connect-Mailbox -Identity "Temp User" -Database MBXDB02 -LinkedDomainController FabrikamDC01 -LinkedMasterAccount danpark@fabrikam.com -Alias dpark
```

This example connects a room mailbox.

```powershell
Connect-Mailbox -Identity "rm2121" -Database "MBXResourceDB" -User "Conference Room 2121" -Alias ConfRm2121 -Room
```

This example connects an equipment mailbox.

```powershell
Connect-Mailbox -Identity "MotorPool01" -Database "MBXResourceDB" -User "Van01 (12 passengers)" -Alias van01 -Equipment
```

This example connects a shared mailbox.

```powershell
Connect-Mailbox -Identity "Printer Support" -Database MBXDB01 -User "Corp Printer Support" -Alias corpprint -Shared
```

> [!NOTE]
> You can also use the <CODE>LegacyDN</CODE> or <CODE>MailboxGuid</CODE> values to identify the deleted mailbox.

For detailed syntax and parameter information, see [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Connect-Mailbox).

## How do you know this worked?

To verify that you've successfully connected a deleted mailbox to a user account, do one of the following:

- In the EAC, click **Recipients**, navigate to the appropriate page for the mailbox type that you connected, click **Refresh** ![Refresh Icon](images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif "Refresh Icon"), and verify that the mailbox is listed.

- In Active Directory Users and Computers, right-click the user account that you connected to the mailbox, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is populated with the email address for the connected mailbox.

- In the Shell, run the following command.

  ```powershell
  Get-User <identity>
  ```

  The **UserMailbox** value for the *RecipientType* property indicates that the user account and the mailbox are connected. You can also run the **Get-Mailbox \<identity\>** command to verify that the mailbox was connected.

## Restore a deleted mailbox

You can use the Shell to restore a deleted mailbox to an existing mailbox using the **New-MailboxRestoreRequest** cmdlet. When you restore a deleted mailbox, its contents are copied to an existing mailbox, which is referred to as the *target mailbox*. After a deleted mailbox is restored, it's still retained in the mailbox database until it's permanently deleted by an administrator or purged after the deleted mailbox retention period expires.

After a mailbox restore request is successfully completed, it's retained for 30 days, by default, before it's removed. You can remove it sooner by using the **Remove-StoreMailbox** cmdlet.

> [!NOTE]
> You can't use the EAC to restore a deleted mailbox.

## Use the Shell to restore a deleted mailbox

To create a mailbox restore request, you have to use the display name, legacy distinguished name (DN), or mailbox GUID of the deleted mailbox. Use the **Get-MailboxStatistics** cmdlet to display the values of the `DisplayName`, `MailboxGuid`, and `LegacyDN` properties for the deleted mailbox that you want to restore. For example, run the following command to return this information for all disabled and deleted mailboxes in your organization.

```powershell
$dbs = Get-MailboxDatabase
$dbs | foreach {Get-MailboxStatistics -Database $_.DistinguishedName} | where {$_.DisconnectReason -eq "Disabled"} | Format-List DisplayName,MailboxGuid,LegacyDN,Database
```

This example restores the deleted mailbox, which is identified by the *SourceStoreMailbox* parameter and is located on the MBXDB01 mailbox database, to the target mailbox Debra Garcia. The *AllowLegacyDNMismatch* parameter is used so the source mailbox can be restored to a different mailbox, one that doesn't have the same legacy DN value.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox e4890ee7-79a2-4f94-9569-91e61eac372b -SourceDatabase MBXDB01 -TargetMailbox "Debra Garcia" -AllowLegacyDNMismatch
```

This example restores Pilar Pinilla's deleted archive mailbox to her current archive mailbox. The *AllowLegacyDNMismatch* parameter isn't necessary because a primary mailbox and its corresponding archive mailbox have the same legacy DN.

```powershell
New-MailboxRestoreRequest -SourceStoreMailbox "Personal Archive - Pilar Pinilla" -SourceDatabase "MDB01" -TargetMailbox pilarp@contoso.com -TargetIsArchive
```

For detailed syntax and parameter information, see [New-MailboxRestoreRequest](https://docs.microsoft.com/powershell/module/exchange/New-MailboxRestoreRequest).

## Use the Shell to restore a deleted public folder mailbox

If you hard deleted a public folder mailbox that you now want to restore, and the mailbox is within the Deleted Item Retention limit (see [Configure Deleted Item retention and Recoverable Items quotas](configure-deleted-item-retention-and-recoverable-items-quotas-exchange-2013-help.md)) you can use the `Connect-Mailbox` cmdlet, followed by the `Update-StoreMailboxState` cmdlet. For detailed syntax and parameter information, see [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Connect-Mailbox) and [Update-StoreMailboxState](https://docs.microsoft.com/powershell/module/exchange/Update-StoreMailboxState).

You will need the GUID of the deleted public folder mailbox, as well as the GUID or name of the mailbox database that contained the public folder mailbox. If you don't have this information, you can take the following steps:

1. Get the Active Directory forest and domain controller fully-qualified domain name (FQDN) by running the following cmdlet:

   ```powershell
   Get-OrganizationConfig | fl OriginatingServer
   ```

2. With the information returned by Step 1, search the Deleted Objects container in Active Directory for the GUID of the public folder mailbox and for the GUID or name of the mailbox database that the deleted public folder mailbox was contained in.

   > [!TIP]
   > You can search Deleted Objects using a custom script or by using the Ldp utility, which can be opened by typing <STRONG>ldp.exe</STRONG> at a Powershell prompt.

When you know the deleted public folder mailbox GUID and the name or GUID of the mailbox database that contained the public folder mailbox, run the following commands to restore the public folder mailbox.

1. Create a new Active Directory object by running the following commands (you may be prompted to provide appropriate credentials):

   ```powershell
   New-MailUser <mailUserName> -ExternalEmailAddress <emailAddress>
   ```

   ```powershell
   Get-MailUser <mailUserName> | Disable-MailUser
   ```

   Where `<mailUserName>`, `<emailAddress>`, and `<mailUserName>` are values you choose. You will need to use the same `<mailUserName>` value in the next step.

2. Connect the deleted public folder mailbox to the Active Directory object you just created by running the following command:

   ```powershell
   Connect-Mailbox -Identity <public folder mailbox GUID> -Database <database name or GUID> -User <mailUserName>
   ```

   > [!NOTE]
   > The <CODE>Identity</CODE> parameter specifies the mailbox object in the Exchange database to connect to an Active Directory user object. The above example specifies the GUID for the public folder mailbox, but you can also use the Display name value or the LegacyExchangeDN value.

3. Run `Update-StoreMailboxState` on the public folder mailbox, based on the following example:

   ```powershell
   Update-StoreMailboxState -Identity <public folder mailbox GUID> -Database <database name or GUID>
   ```

   As in Step 2, the `Identity` parameter will accept GUID, Display Name, or LegacyExchangeDN values for the public folder mailbox.

## How do you know this worked?

To verify that you've successfully restored a deleted public folder mailbox, run the **Get-PublicFolder -GetChildren -\<public folder mailbox GUID\>** cmdlet. If the restore was successful, this cmdlet will work.

For more information, see:

- [Connect-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Connect-Mailbox)

- [Update-StoreMailboxState](https://docs.microsoft.com/powershell/module/exchange/Update-StoreMailboxState)
