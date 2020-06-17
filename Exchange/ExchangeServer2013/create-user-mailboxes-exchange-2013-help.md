---
title: 'Create user mailboxes: Exchange 2013 Help'
TOCTitle: Create user mailboxes
ms:assetid: 51a8b4c6-a53e-41c5-8bb1-ea4c0eaa0174
ms:mtpsurl: https://technet.microsoft.com/library/JJ991919(v=EXCHG.150)
ms:contentKeyID: 51588093
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create user mailboxes

_**Applies to:** Exchange Server 2013_

Mailboxes are the most common recipient type used by information workers in an Exchange organization. Each mailbox is associated with an Active Directory user account. The user can use the mailbox to send and receive messages, and to store messages, appointments, tasks, notes, and documents. Use the EAC or the Shell to create user mailboxes.

You can also create user mailboxes for existing users that have an Active Directory user account but don't have a corresponding mailbox. This is known as *mailbox-enabling* existing users.

## What do you need to know before you begin?

- Estimated time to complete each user mailbox task: 2 to 5 minutes.

- When you create a new user mailbox, you can't use an apostrophe (') or a quotation mark (") in the alias or the user logon name because these characters aren't supported. Although you might not receive an error if you create a new mailbox using unsupported characters, these characters can cause problems later. For example, users that have been assigned access permissions to a mailbox that was created using an unsupported character may experience problems or unexpected behavior.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Create a user mailbox

## Use the EAC to create a user mailbox

1. In the EAC, navigate to **Recipients**  \> **Mailboxes**.

2. Click **New** \> **User mailbox**.

3. On the **New user mailbox** page, in the **Alias** box, type the user's alias, which specifies the email alias for the user. The user's alias is the portion of the email address on the left side of the at (@) symbol. It must be unique in the forest.

    > [!NOTE]
    > If you leave this box blank, the value from the username portion of the <STRONG>User logon name</STRONG> is used for the email alias.

4. Select one of the following options:

   - **Existing user**: Select to mail-enable and create a mailbox for an existing user.

        Click **Browse** to open the **Select User - Entire Forest** dialog box. This dialog box displays a list of Active Directory user accounts in the forest that aren't mail-enabled or don't have Exchange mailboxes. Select the user account you want to mail-enable, and then click **OK**. If you select this option, you don't have to provide user account information because this information already exists in Active Directory.

   - **New user**: Select to create a new user account in Active Directory and create a mailbox for this user. If you select this option, you'll have to provide the required user account information.

    > [!NOTE]
    > The Active Directory account that is associated with user mailboxes must reside in the same forest as the Exchange server. To create a mailbox for a user account that resides in a trusted forest, you have to create a linked mailbox. See <A href="manage-linked-mailboxes-exchange-2013-help.md">Manage linked mailboxes</A>.

5. If you selected **New user** in Step 4, complete the following boxes on the **New user mailbox** page. Otherwise skip to Step 7.

   - **First name**: Use this box to type the first name of the user.

   - **Initials**: Use this box to type the initials of the user.

   - **Last name**: Use this box to type the last name of the user.

   - **\* Display name**: Use this box to type a display name for the user. This is the name that's listed in the mailbox list in the EAC and in the shared address book. By default, this box is populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name in this box because it's required. The name can't exceed 64 characters.

   - **\* Name**: Use this box to type a name for the user. This is the name that's listed in Active Directory. This box is also populated with the names you enter in the **First name**, **Initials**, and **Last name** boxes. If you didn't use those boxes, you must still type a name because this box is required. This name also can't exceed 64 characters.

   - **Organizational unit**: You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the Active Directory domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

        To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the desired OU, and then click **OK**.

   - **\* User logon name**    Use this box to type the name that the user will use to sign in to the mailbox and to log on to the domain. The user logon name consists of a username on the left side of the at (@) symbol and a suffix on the right side. Typically, the suffix is the domain name in which the user account resides. Note that you can't use an apostrophe (') or a quotation mark (") in the user logon name because these characters aren't supported.

        > [!NOTE]
        > If the value for the username is different than the value used in the <STRONG>Alias</STRONG> box, then the user's email address and the user logon name will be different.

   - **\* New Password**: Use this box to type the password that the user must use to sign in to his or her mailbox.

        > [!NOTE]
        > Make sure that the password you supply complies with the password length, complexity, and history requirements of the domain in which you are creating the user account.

   - **\* Confirm password**: Use this box to confirm the password that you typed in the **Password** box.

   - **Require password change on next logon**: Select this check box if you want the user to reset the password when they first sign in to the mailbox.

        If you select this check box, at first sign-in, the new user will be prompted with a dialog box in which to change the password. The user won't be allowed to perform any tasks until the password is successfully changed.

6. Click **More options** to configure the following boxes. Otherwise, skip to Step 7 to save the new user mailbox.

   - **Specify the mailbox database**: Use this option to specify a mailbox database instead of allowing Exchange to select a database for you. Click **Browse** to open the **Select Mailbox Database** dialog box. This dialog box lists all the mailbox databases in your Exchange organization. By default, the mailbox databases are sorted by name. You can also click the title of the corresponding column to sort the databases by server name or version. Select the mailbox database you want to use, and then click **OK**.

   - **Create local archive storage for this user**: Select this check box to create an archive mailbox for the mailbox. If you create an archive mailbox, mailbox items will be moved automatically from the primary mailbox to the archive, based on the default retention policy settings or those you define.

        Click **Browse** to select a database that resides in the local forest to store the archive mailbox.

        To learn more, see [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md).

   - **Address book policy**: Use this option to specify an address book policy (ABP) for the mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. When assigned to mailbox users, an ABP provides them with access to a customized GAL in Outlook and Outlook Web App. To learn more, see [Address book policies](https://docs.microsoft.com/exchange/address-books/address-book-policies/address-book-policies).

        In the drop-down list, select the policy that you want associated with this mailbox.

7. When you're finished, click **Save** to create the mailbox.

## Use the Shell to create a user mailbox

This example creates a new user account and mailbox for Pilar Pinilla with the following details:

- The alias is pilarp

- The first name is Pilar and the last name is Pinilla

- The name and display name is Pilar Pinilla

- The user logon name is pilarp@contoso.com

- The password is Pa$$word1

- The mailbox will be created in the default OU. To specify a different OU, you can use the *OrganizationalUnit* parameter.

```powershell
New-Mailbox -Alias pilarp -Name "Pilar Pinilla" -FirstName Pilar -LastName Pinilla -DisplayName "Pilar Pinilla" -UserPrincipalName pilarp@contoso.com -Password (ConvertTo-SecureString -String 'Pa$$word1' -AsPlainText -Force)
```

For syntax and parameter information, see [New-Mailbox](https://docs.microsoft.com/powershell/module/exchange/New-Mailbox).

## How do you know this worked?

To verify that you've successfully created a user mailbox, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes**. The new user mailbox is displayed in the mailbox list. Under **Mailbox Type**, the type is **User**.

- In the Shell, run the following command to display information about the new user mailbox.

    ```powershell
    Get-Mailbox <Name> | FL Name,RecipientTypeDetails,PrimarySmtpAddress
    ```

## Create a mailbox for an existing user

You can also create user mailboxes for existing users that have an Active Directory user account but don't have a corresponding mailbox. This is known as *mailbox-enabling* existing users. After you mailbox-enable an existing user, the user can send and receive email messages.

## Use the EAC to create a mailbox for an existing user

1. In the EAC, navigate to **Recipients**  \> **Mailboxes**.

2. Click **New** \> **User mailbox**.

3. On the **New user mailbox** page, in the **Alias** box, type the user's alias, which specifies the email alias for the user. The user's alias is the portion of the email address on the left side of the at (@) symbol. It must be unique in the forest.

    > [!NOTE]
    > If you leave this box blank, the value from the username portion of the <STRONG>User logon name</STRONG> is used for the email alias.

4. Click **Existing user**.

5. Click **Browse** to open the **Select User - Entire Forest** dialog box. This dialog box displays a list of Active Directory user accounts in the forest that aren't mail-enabled or don't have Exchange mailboxes. Select the user account you want to mail-enable, and then click **OK**.

    When you create a mailbox for an existing user, you don't have to provide account information because this information already exists in Active Directory.

    > [!NOTE]
    > The Active Directory account that is associated with user mailboxes must reside in the same forest as the Exchange server. To create a mailbox for a user account that resides in a trusted forest, you have to create a linked mailbox. See <A href="manage-linked-mailboxes-exchange-2013-help.md">Manage linked mailboxes</A>.

6. Click **More options** to configure the following boxes. Otherwise, skip to Step 7 to save the new user mailbox.

   - **Specify the mailbox database**: Use this option to specify a mailbox database instead of allowing Exchange to select a database for you. Click **Browse** to open the **Select Mailbox Database** dialog box. This dialog box lists all the mailbox databases in your Exchange organization. By default, the mailbox databases are sorted by name. You can also click the title of the corresponding column to sort the databases by server name or version. Select the mailbox database you want to use, and then click **OK**.

   - **Create local archive storage for this user**: Select this check box to create an archive mailbox for the mailbox. If you create an archive mailbox, mailbox items will be moved automatically from the primary mailbox to the archive, based on the default retention policy settings or those you define.

        Click **Browse** to select a database that resides in the local forest to store the archive mailbox.

        To learn more, see [In-Place Archiving in Exchange 2013](in-place-archiving-in-exchange-2013-exchange-2013-help.md).

   - **Address book policy**: Use this option to specify an address book policy (ABP) for the mailbox. ABPs contain a global address list (GAL), an offline address book (OAB), a room list, and a set of address lists. When assigned to mailbox users, an ABP provides them with access to a customized GAL in Outlook and Outlook Web App. To learn more, see [Address book policies](https://docs.microsoft.com/exchange/address-books/address-book-policies/address-book-policies).

        In the drop-down list, select the policy that you want associated with this mailbox.

7. When you're finished, click **Save** to create the mailbox.

## Use the Shell to create a mailbox for an existing user

This example creates a mailbox for the existing user estherv@contoso.com on the Exchange database named UsersMailboxDatabase.

```powershell
Enable-Mailbox estherv@contoso.com -Database UsersMailboxDatabase
```

You can also use the **Enable-Mailbox** cmdlet to mail-enable multiple users. You can do this by piping the results of the **Get-User** cmdlet to the **Enable-Mailbox** cmdlet. When you run the **Get-User** cmdlet, you must return only users that aren't already mail-enabled. To do this, you need to specify the value User with the *RecipientTypeDetails* parameter. You can also limit the results returned by using the *Filter* parameter to request only users that meet the criteria you specify. You then pipe the results to the **Enable-Mailbox** cmdlet.

For example, the following command mailbox-enables users who aren't already mail-enabled and that have a value in the **UserPrincipalName** property, which helps ensure that you don't inadvertently convert a system account to a mailbox.

```powershell
Get-User -RecipientTypeDetails User -Filter "UserPrincipalName -ne `$Null" | Enable-Mailbox
```

For syntax and parameter information, see [Enable-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Enable-Mailbox) and [Get-User](https://docs.microsoft.com/powershell/module/exchange/Get-User).

For more information about pipelining, see [about_Pipelines](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines).

## How do you know this worked?

To verify that you've successfully created a mailbox for an existing user, do one of the following:

- In the EAC, navigate to **Recipients**  \> **Mailboxes**. The new mailbox-enabled user is displayed in the mailbox list. Under **Mailbox Type**, the type is **User**.

- In the Shell, run the following command to display information about the new mailbox-enabled user.

    ```powershell
    Get-Mailbox <Name> | FL Name,RecipientTypeDetails,PrimarySmtpAddress
    ```

    Note that value for the *RecipientTypeDetails* property is `UserMailbox`.
