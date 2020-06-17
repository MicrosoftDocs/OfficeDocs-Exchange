---
title: 'Enable or disable email for a mail user: Exchange 2013 Help'
TOCTitle: Enable or disable email for a mail user
ms:assetid: 1e2571d4-ff84-4fda-bb1d-825e96e1bd26
ms:mtpsurl: https://technet.microsoft.com/library/Aa996598(v=EXCHG.150)
ms:contentKeyID: 50382995
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable email for a mail user

_**Applies to:** Exchange Server 2013_

You can disable email for an existing mail user in your Exchange organization. When you disable email for a mail user, it's removed from Exchange and your organization's address book. If the mail user is a member of a distribution group, the user no longer receives mail sent to the group. Also, the Exchange attributes are removed from the user object in Active Directory, but the user object and its non-Exchange attributes (such as contact and organization information) are retained in Active Directory.

After you disable email for a mail user, you can mail-enable the user again by using the **Enable-MailUser** cmdlet in the Shell. You can also use this cmdlet to mail-enable any Active Directory user.

> [!NOTE]
> Mail users (also called <EM>mail-enabled users</EM>) are different than users in your organization that have a mailbox. The primary difference is that mail users represent users outside your Exchange organization that have an external email address. They don't have a mailbox in your organization. For more information about the differences between users who have mailboxes in your organization and mail users, see <A href="recipients-exchange-2013-help.md">Recipients</A>.

For additional management tasks related to mail users, see [Manage mail users](https://docs.microsoft.com/exchange/recipients-in-exchange-online/manage-mail-users).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail users" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Disable email for a mail user

As previously stated, when you disable email for a mail user, the Exchange attributes are removed from the corresponding Active Directory mail user object, but the user is retained. The mail user is removed from the list of mail users in the EAC, but you can view and manage the corresponding Active Directory contact object by using Active Directory Users and Computers or by using the **Get-MailUser** and **Set-MailUser** cmdlets in the Shell.

## Use the EAC to disable email for a mail user

1. In the EAC, navigate to **Recipients**  \> **Contacts**.

2. In the list of contacts, click the mail user you want to disable email for.

3. Click **More** ![More Options Icon](images/JJ150550.5381819e-3b21-4873-8714-e9b956290b28(EXCHG.150).gif "More Options Icon") and then click **Disable**.

4. A warning will appear asking if you're sure you want to disable the selected mail user. Click **Yes** to disable it.

The mail user will be removed from the contacts list.

## Use the Shell to disable email for a mail user

This example disables email for the mail user Yan Li.

```powershell
Disable-MailUser -Identity "Yan Li"
```

For detailed syntax and parameter information, see [Disable-MailUser](https://docs.microsoft.com/powershell/module/exchange/Disable-MailUser).

## How do you know this worked?

To verify that you've successfully disabled email for a mail user, do one of the following:

1. In the EAC, navigate to **Recipients** \> **Contacts** and verify that the mail user is no longer listed.

2. In Active Directory Users and Computers, right-click the user, and then click **Properties**. On the **General** tab, notice that the **E-mail** box is blank. This verifies that the mail user isn't mail-enabled.

3. In the Shell, run the following command.

    ```powershell
    Get-MailUser
    ```

    The mail user that you disabled email for won't be returned in the results because this cmdlet only returns mail-enabled users.

4. In the Shell, run the following command.

    ```powershell
    Get-User
    ```

    The mail user that you disabled email for is returned in the results because this cmdlet returns all Active Directory user objects.

## Use the Shell to mail-enable users

You can use the **Enable-MailUser** cmdlet to mail-enable existing Active Directory users. You can mail-enable a single user or use a CSV file to mail-enable multiple users.

## Use the Shell to mail-enable a single user

This example mail-enables the user Sanjay Shah. You must provide an external email address.

```powershell
Enable-MailUser -Identity "Sanjay Shah" -ExternalEmailAddress renev@tailspintoys.com
```

## Use the Shell and a CSV file to mail-enable multiple users

When you're mail-enabling users in bulk, you first export the list of users that aren't mail-enabled to a CSV (comma-separated values) file, and then add the external email addresses to the CSV file by using a text editor such as Notepad, or a spreadsheet application such as Microsoft Excel. Then you use the updated CSV file in the Shell command to mail-enable the users listed in the CSV file.

1. Run the following command to export a list of existing users that aren't mail-enabled or don't have a mailbox in your organization to a file on the administrator's desktop named UsersToMailEnable.csv.

    ```powershell
    Get-User | Where {$_.RecipientType -eq "User"} | Out-File "C:\Users\Administrator\Desktop\UsersToMailEnable.csv"
    ```

    The resulting file will be similar to the following file.

    ```powershell
    Name            RecipientType
    ----            -------------
    Guest           User
    krbtgt          User
    RMS_SERVICE     User
    David Pelton    User
    Kim Akers       User
    Janet Schorr    User
    Jeffrey Zang    User
    Spencer Low     User
    Toni Poe        User
    ...
    ```

2. Make the following changes to the CSV file:

      - Delete any users that you don't want to mail-enable from the CSV file. For example, you would delete the first three entries in the previous example because they're default system accounts.

      - Delete the **RecipientType** column and all the instances of `User`.

      - Add a column heading named **EmailAddress** and then add an email address for each user in the file. The name and external email address for each user must be separated by a comma.

    The updated CSV file should look similar to the following file.

    ```powershell
    Name,EmailAddress
    David Pelton,davidp@contoso.com
    Kim Akers,kakers@tailspintoys.com
    Janet Schorr,janet.schorr@adatum.com
    Jeffrey Zang,jzang@tailspintoys.com
    Spencer Low,spencerl@fouthcoffee.com
    Toni Poe,tonip@contoso.com
    ...
    ```

3. Run the following command to use the data in the CSV file to mail-enable the users listed in the file.

    ```powershell
    Import-CSV "C:\Users\Administrator\Desktop\UsersToMailEnable.csv" | ForEach-Object {Enable-MailUser -Identity $_.Name -ExternalEmailAddress $_.EmailAddress}
    ```

    The command results display information about the new mail-enabled users.

## How do you know this worked?

To verify that you've successfully mail-enabled Active Directory users, do one of the following:

- In the EAC, navigate to **Recipients** \> **Contacts**. New mail users are displayed in the contact list. Under **Contact Type**, the type is **Mail user**.

  > [!NOTE]
  > You may have to click <STRONG>Refresh</STRONG> <IMG title="Refresh Icon" alt="Refresh Icon" src="images/Dn624163.85f271ca-32a4-426c-842a-d2172567099d(EXCHG.150).gif"> to display new mail users.

- In the Shell, run the following command to display information about new mail users.

  ```powershell
  Get-MailUser | Format-Table Name,RecipientTypeDetails,ExternalEmailAddress
  ```
