---
title: 'Add or remove email addresses for a mailbox: Exchange 2013 Help'
TOCTitle: Add or remove email addresses for a mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 93e2d9a4-7558-4509-8641-8381a7eb674f
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add or remove email addresses for a mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can configure more than one email address for the same mailbox. The additional addresses are called proxy addresses. A proxy address lets a user receive email that's sent to a different email address. Any email message sent to the user's proxy address is delivered to their primary email address, which is also known as the primary SMTP address or the default reply address.

For additional management tasks related to managing recipients, see the "Recipients documentation" table in [Recipients](recipients-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

- The procedures in this topic show how to add or remove email addresses for a user mailbox. You can use similar procedures to add or remove email addresses for other recipient types.

## Add an email address to a user mailbox

### Use the EAC to add an email address

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to add an email address to, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Email Address**.

   > [!NOTE]
   > On the **Email Address** page, the primary SMTP address is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

4. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif), and then click **SMTP** to add an SMTP email address to this mailbox.

   > [!NOTE]
   > SMTP is the default email address type. You can also add Unified Messaging (UM) addresses or custom addresses to a mailbox. For more information, see "Change user mailbox properties" in the [Manage user mailboxes](manage-user-mailboxes-exchange-2013-help.md) topic.

5. Type the new SMTP address in the **Email address** box, and then click **OK**.

   The new address is displayed in the list of email addresses for the selected mailbox.

6. Click **Save** to save the change.

### Use the Shell to add an email address

The email addresses associated with a mailbox are contained in the _EmailAddresses_ property for the mailbox. Because it can contain more than one email address, the _EmailAddresses_ property is known as a multivalued property. The following examples show different ways to modify a multivalued property.

This example shows how to add an SMTP address to the mailbox of Dan Jump.

```powershell
Set-Mailbox "Dan Jump" -EmailAddresses @{add="dan.jump@northamerica.contoso.com"}
```

This example shows how to add multiple SMTP addresses to a mailbox.

```powershell
Set-Mailbox "Dan Jump" -EmailAddresses @{add="dan.jump@northamerica.contoso.com","danj@tailspintoys.com"}
```

For more information about how to use this method of adding and removing values for multivalued properties, see [Modifying multivalued properties](modifying-multivalued-properties-exchange-2013-help.md).

This example shows another way to add email addresses to a mailbox by specifying all addresses associated with the mailbox. In this example, danj@tailspintoys.com is the new email address that you want to add. The other two email addresses are existing addresses. The address with the case-sensitive qualifier `SMTP` is the primary SMTP address. You have to include all email addresses for the mailbox when you use this command syntax. If you don't, the addresses specified in the command will overwrite the existing addresses.

```powershell
Set-Mailbox "Dan Jump" -EmailAddresses SMTP:dan.jump@contoso.com,dan.jump@northamerica.contoso.com,danj@tailspintoys.com
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

### How do you know this worked?

To verify that you've successfully added an email address to a mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Email Address**.

- In the list of email addresses for the mailbox, verify that the new email address is included.

Or

- Run the following command in the Shell.

  ```powershell
  Get-Mailbox <identity> | Format-List EmailAddresses
  ```

- Verify that the new email address is included in the results.

## Remove an email address from a user mailbox

### Use the EAC to remove an email address

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to remove an email address from, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Email Address**.

4. In the list of email addresses, select the address you want to remove, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

5. Click **Save** to save the change.

### Use the Shell to remove an email address

This example shows how to remove an email address from the mailbox of Janet Schorr.

```powershell
Set-Mailbox "Janet Schorr" -EmailAddresses @{remove="janets@corp.contoso.com"}
```

This example shows how to remove multiple addresses from a mailbox.

```powershell
Set-Mailbox "Janet Schorr" -EmailAddresses @{remove="janet.schorr@corp.contoso.com","janets@tailspintoys.com"}
```

For more information about how to use this method of adding and removing values for multivalued properties, see [Modifying multivalued properties](modifying-multivalued-properties-exchange-2013-help.md).

You can also remove an email address by omitting it from the command to set email addresses for a mailbox. For example, let's say Janet Schorr's mailbox has three email addresses: janets@contoso.com (the primary SMTP address), janets@corp.contoso.com, and janets@tailspintoys.com. To remove the address janets@corp.contoso.com, you would run the following command.

```powershell
Set-Mailbox "Janet Schorr" -EmailAddresses SMTP:janets@contoso.com,janets@tailspintoys.com
```

Because janets@corp.contoso.com was omitted in the previous command, it's removed from the mailbox.

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

### How do you know this worked?

To verify that you've successfully removed an email address from a mailbox, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click the mailbox, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Email Address**.

- In the list of email addresses for the mailbox, verify that the email address isn't included.

Or

- Run the following command in the Shell.

  ```powershell
  Get-Mailbox <identity> | Format-List EmailAddresses
  ```

- Verify that the email address isn't included in the results.

## Use the Shell to add email addresses to multiple mailboxes

You can add a new email address to multiple mailboxes at one time by using the Shell and a comma separated values (CSV) file.

This example imports data from C:\Users\Administrator\Desktop\AddEmailAddress.csv, which has the following format.

> Mailbox,NewEmailAddress <br/> Dan Jump,danj@northamerica.contoso.com <br/> David Pelton,davidp@northamerica.contoso.com <br/> Kim Akers,kima@northamerica.contoso.com <br/> Janet Schorr,janets@northamerica.contoso.com <br/> Jeffrey Zeng,jeffreyz@northamerica.contoso.com <br/> Spencer Low,spencerl@northamerica.contoso.com <br/> Toni Poe,tonip@northamerica.contoso.com <br/> ...

Run the following command to use the data in the CSV file to add the email address to each mailbox specified in the CSV file.

```powershell
Import-CSV "C:\Users\Administrator\Desktop\AddEmailAddress.csv" | ForEach {Set-Mailbox $_.Mailbox -EmailAddresses @{add=$_.NewEmailAddress}}
```

> [!NOTE]
> The column names in the first row of this CSV file ( `Mailbox,NewEmailAddress`) are arbitrary. Whatever you use for column names, make sure you use the same column names in the Shell command.

### How do you know this worked?

To verify that you've successfully added an email address to multiple mailboxes, do one of the following:

- In the EAC, navigate to **Recipients** \> **Mailboxes**, click a mailbox that you added the address to, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

- On the mailbox properties page, click **Email Address**.

- In the list of email addresses for the mailbox, verify that the new email address is included.

Or

- Run the following command in the Shell, using the same CSV file that you used to add the new email address.

  ```powershell
  Import-CSV "C:\Users\Administrator\Desktop\AddEmailAddress.csv" | ForEach {Get-Mailbox $_.Mailbox | Format-List Name,EmailAddresses}
  ```

- Verify that the new email address is included in the results for each mailbox.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).
