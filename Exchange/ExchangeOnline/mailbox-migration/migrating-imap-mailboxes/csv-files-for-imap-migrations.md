---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 187ce085-9a83-4612-80a2-f562b3049fc2
ms.reviewer: 
description: The comma-separated values (CSV) file that you use to migrate the contents of users' mailboxes in an IMAP migration contains a row for each user. Each row contains information about the user's Office 365 mailbox and IMAP mailbox, and Office 365 uses this information to process the migration.
title: CSV files for IMAP migration batches
ms.collection: 
- exchange-online
- M365-email-calendar
search.appverid:
- MET150
- MOE150
- MED150
- BCS160
audience: Admin
f1.keywords:
- CSH
ms.custom: Adm_O365
ms.service: exchange-online
manager: serdars

---

# CSV files for IMAP migration batches

The comma-separated values (CSV) file that you use to migrate the contents of users' mailboxes in an IMAP migration contains a row for each user. Each row contains information about the user's Office 365 mailbox and IMAP mailbox, and Office 365 uses this information to process the migration.

## Required attributes
<a name="bk_attributes"> </a>

Here are the required attributes for each user:

- **EmailAddress** specifies the user ID for the user's Office 365 mailbox.

- **UserName** specifies the user logon name for the user's mailbox on the IMAP server. You can use either the username or domain\username format. For example, `hollyh` or `contoso\hollyh`.

- **Password** is the password for the user's account in the IMAP messaging system.

The migration will fail if any one of these attributes isn't included in the header row of the CSV file. Also, be sure to type the attributes exactly as they're shown. Attributes can't contain spaces. They must be a single word. For example, **Email Address** is invalid. You must use **EmailAddress**.

## CSV file format
<a name="bk_format"> </a>

Here's an example of the format for the CSV file. In this example, user credentials are used to migrate three mailboxes:

```CSV
EmailAddress,UserName,Password
terrya@contoso.edu,contoso\terry.adams,1091990
annb@contoso.edu,contoso\ann.beebe,2111991
paulc@contoso.edu,contoso\paul.cannon,3281986
```

The first row, or header row, of the CSV file lists the names of the attributes, or fields, specified in the rows that follow. Each attribute name is separated by a comma.

Each row under the header row represents one user and supplies the information that will be used to migrate the user's mailbox. The attribute values in each row must be in the same order as the attribute names in the header row. Each attribute value is separated by a comma.

Use any text editor, or an application like Microsoft Excel, to create the CSV file. Save the file as a .csv or .txt file.

> [!TIP]
> If the CSV file contains non-ASCII or special characters, save the CSV file with UTF-8 or other Unicode encoding. Depending on the application, saving the CSV file with UTF-8 or other Unicode encoding might be easier when the system locale of the computer matches the language used in the CSV file.

## Divide a large migration into several batches
<a name="BK_dividebatches"> </a>

The CSV file can contain up to 50,000 rows, one row for each user, and can be as large as 10 MB. But it's a good idea to migrate users in several smaller batches.

If you plan to migrate lots of users, decide which ones to include in each batch. For example, if you have 10,000 accounts to migrate, you could run four batches with 2,500 users each. You could also divide the batches alphabetically; by user type, such as faculty, students, and alumni; by class, such as freshman, sophomore, junior, and senior; or in other ways that meet your organization's needs.

> [!TIP]
> One strategy is to create Office 365 mailboxes and migrate email for the same group of users. For example, if you import 100 new users to your Microsoft 365 or Office 365 organization, create a migration batch for those same 100 users. This is an effective way to organize and manage your migration from an on-premises messaging system to Office 365.

## Provide user or administrator credentials
<a name="BK_Creds"> </a>

In the CSV file, you have to provide the username and password for the user's on-premises account. This enables the migration process to access the account. There are two ways to do this:

- **Use user credentials**: This requires that you obtain users' passwords or that you change their passwords to a value that you know so you can include it in the CSV file.

    > [!TIP]
    > If you use this option, prevent users from changing the passwords of their on-premises accounts. If users change their passwords after the initial migration, subsequent synchronizations between the mailboxes on the IMAP server and Office 365 mailboxes will fail.

- **Use super-user or administrator credentials**: This requires that you use an account in your IMAP messaging system that has the necessary rights to access all user mailboxes. In the CSV file, you use the credentials for this account for each row. To learn whether your IMAP server supports this approach and how to enable it, see the documentation for your IMAP server.

    > [!NOTE]
    > It's a good idea to use administrator credentials because it doesn't affect or inconvenience users. For example, it won't matter if users change their passwords after the initial migration.

## Format for the administrator credentials for different IMAP servers
<a name="BK_AdminCreds"> </a>

You can use the username and password of an administrator account in the **UserName** and **Password** fields for each row of the CSV file. The username for administrator credentials is a combination of the username for the person whose email is being migrated and the username for an administrator account that has permission to access all user mailboxes. The supported format for administrator credentials is different depending on the IMAP server you're migrating email from. For more information about how to use administrator credentials, see the documentation for your IMAP server.

> [!NOTE]
> When you submit a new migration request, the CSV file is uploaded to the Microsoft datacenter over a Secure Sockets Layer (SSL) connection. The information from the CSV file is encrypted and stored on the Microsoft Exchange servers at the Microsoft datacenter.

The following sections explain how to format the administrator credentials in the CSV file that you use to migrate email from different types of IMAP servers.

### Microsoft Exchange
<a name="exchange"> </a>

If you're migrating email from the IMAP implementation for Microsoft Exchange, use the format **Domain/Admin_UserName/User_UserName** for the **UserName** attribute in the CSV file. Let's say you're migrating email from Exchange for Terry Adams, Ann Beebe, and Paul Cannon. You have a mail administrator account, where the username is mailadmin and the password is P@ssw0rd. Here's what your CSV file would look like:

```CSV
EmailAddress,UserName,Password
terrya@contoso.edu,contoso-students/mailadmin/terry.adams,P@ssw0rd
annb@contoso.edu,contoso-students/mailadmin/ann.beebe,P@ssw0rd
paulc@contoso.edu,contoso-students/mailadmin/paul.cannon,P@ssw0rd
```

### Dovecot
<a name="dovecot"> </a>

For IMAP servers that support Simple Authentication and Security Layer (SASL), such as a Dovecot IMAP server, use the format **User_UserName\*Admin_UserName**, where the asterisk ( **\*** ) is a configurable separator character. Let's say you're migrating those same users' email from a Dovecot IMAP server using the administrator credentials mailadmin and P@ssw0rd. Here's what your CSV file would look like:

```CSV
EmailAddress,UserName,Password
terrya@contoso.edu,terry.adams*mailadmin,P@ssw0rd
annb@contoso.edu,ann.beebe*mailadmin,P@ssw0rd
paulc@contoso.edu,paul.cannon*mailadmin,P@ssw0rd
```

### Mirapoint
<a name="mirapoint"> </a>

If you're migrating email from Mirapoint Message Server, use the format **#user@domain#Admin_UserName#** for the administrator credentials. To migrate email from Mirapoint using the administrator credentials mailadmin and P@ssw0rd, your CSV file would look like this:

```CSV
EmailAddress,UserName,Password
terrya@contoso.edu,#terry.adams@contoso-students.edu#mailadmin#,P@ssw0rd
annb@contoso.edu,#ann.beebe@contoso-students.edu#mailadmin#,P@ssw0rd
paulc@contoso.edu,#paul.cannon@contoso-students.edu#mailadmin#,P@ssw0rd
```

## Use the optional UserRoot attribute
<a name="bi_Root"> </a>

Some IMAP servers, such as Courier IMAP, don't support using administrator credentials to migrate mailboxes to Office 365. To use administrator credentials to migrate mailboxes, you can configure your IMAP server to use virtual shared folders. Virtual shared folders allow administrators to use the administrator's logon credentials to access user mailboxes on the IMAP server. For more information about how to configure virtual shared folders for Courier IMAP, see [Shared Folders](migrate-other-types-of-imap-mailboxes.md).

To migrate mailboxes after you set up virtual shared folders on your IMAP server, you have to include the optional attribute **UserRoot** in the CSV file. This attribute specifies the location of each user's mailbox in the virtual shared folder structure on the IMAP server.

Here's an example of a CSV file that contains the **UserRoot** attribute:

```
EmailAddress,UserName,Password,UserRoot
terrya@contoso.edu,mailadmin,P@ssw0rd,/users/terry.adams
annb@contoso.edu,mailadmin,P@ssw0rd,/users/ann.beebe
paulc@contoso.edu,mailadmin,P@ssw0rd,/users/paul.cannon
```
