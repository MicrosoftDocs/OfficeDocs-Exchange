---
localization_priority: Normal
ms.topic: conceptual
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e6e4b0d0-4c3d-4826-a818-8aeab06b9b76
ms.reviewer: 
description: When you undertake an Internet Message Access Protocol (IMAP) migration from an on-premises Exchange Server to Office 365, you have a few choices for optimizing the migration performance.
title: Tips for optimizing IMAP migrations
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

# Tips for optimizing IMAP migrations

When you undertake an Internet Message Access Protocol (IMAP) migration from an on-premises Exchange Server to Office 365, you have a few choices for optimizing the migration performance.

## Optimize IMAP migrations

Here are some tips for optimizing an IMAP migration:

- **Increase the connection limits to your IMAP server**: Many firewalls and email servers have per-user limits, per-IP address limits, and overall connection limits. Before you migrate mailboxes, make sure that your firewall and IMAP server are configured to allow a large, or maximum, number of connections for the following settings:

  - The total number of connections to the IMAP server.

  - The number of connections by a particular user. This is important if you use an administrator account in the comma-separated value (CSV) migration file because all connections to the IMAP server are made by this user account.

  - The number of connections from a single IP address. This limit is typically enforced by the firewall or the email server.

    If your IMAP server is running Microsoft Exchange Server 2010 or Exchange 2007, the default settings for connection limits are low. Be sure to increase these limits before you migrate email. By default, Exchange 2003 doesn't limit the number of connections.

    For more information, see:

  - Exchange 2013: [Set connection limits for IMAP4](https://docs.microsoft.com/exchange/set-connection-limits-for-imap4-exchange-2013-help)

  - Exchange 2010: [View or Configure IMAP4 Properties](https://go.microsoft.com/fwlink/p/?LinkId=183037)

  - Exchange 2007: [How to Set Connection Limits for IMAP4](https://go.microsoft.com/fwlink/p/?LinkId=183038)

  - Exchange 2003: [How to Set Connection Limits](https://go.microsoft.com/fwlink/p/?LinkId=183039)

- **Change the DNS Time-to-Live (TTL) setting on your MX record**: Before you start migrating mailboxes, change the Domain Name System (DNS) TTL setting on your current MX record to a shorter interval, such as 3,600 seconds (one hour). Then, when you change the MX record to point to your Microsoft 365 or Office 365 email organization after all mailboxes are migrated, the updated MX record should propagate more quickly because of the shortened TTL interval.

- **Run one or more test migration batches**: Run a few small IMAP migration batches before you migrate larger numbers of users. In a test migration, you can do the following:

  - Verify the format of the CSV file.

  - Test the migration endpoint used to connect to the IMAP server.

  - Verify that you can successfully migrate email by using administrator credentials, if applicable.

  - Determine the optimal number of simultaneous connections to the IMAP server that minimize the impact on your internet bandwidth.

  - Verify that folders you exclude aren't migrated to Office 365 mailboxes.

  - Determine how long it takes to migrate a batch of users.

  - Use CSV files with the same number of rows and run the batches at similar times during the day. Then compare the total running time for each test batch. This comparison will help you estimate how long it will take to migrate all your mailboxes, how large each migration batch should be, and how many simultaneous connections to the IMAP server you should use to balance migration speed and internet bandwidth.

- **Use administrator credentials in the CSV file to migrate email**: This method is the least disruptive and inconvenient for users, and it will help minimize synchronization errors caused when users change the password on their on-premises account. It also saves you from having to obtain or change user passwords. If you use this method, be sure to verify that the administrator account you use has the necessary permissions to access the mailboxes you're migrating.

    > [!NOTE]
    > If you decide to use user credentials in the CSV file, consider globally changing users' passwords, and then preventing users from changing their password on their on-premises account before you migrate their mailboxes. If users change their password before their mailbox is migrated to the cloud-based mailbox, the migration will fail. If they change their password after the mailbox is migrated, new email sent to their mailbox on the IMAP server won't be migrated to their Office 365 mailbox.

- **Don't delete mailboxes or change their SMTP addresses during migration**: The migration system will report an error when it can't find a mailbox that's been migrated. Be sure to complete the migration and delete the migration batch before you delete or change the SMTP address of an Office 365 or on-premises mailbox that's been migrated.

- **Communicate with your users**: Let users know ahead of time that you'll be migrating the content of their on-premises mailboxes to your Microsoft 365 or Office 365 organization. Consider the following:

  - Tell users that email messages larger than 35 MB won't be migrated. Ask users to save very large messages and attachments to their local computer or to a removable USB drive.

  - Ask users to delete old or unnecessary email messages from their on-premises mailboxes before migration. This helps reduce the amount of data that has to be migrated and can help reduce the overall migration time. Or you can clean up their mailboxes yourself.

  - Suggest that users back up their Inboxes.

  - Tell users which folders won't be migrated, if applicable.

  - Folders with a forward slash ( / ) in the folder name aren't migrated. If users want to migrate folders that contain forward slashes in their names, they have to rename the folders or replace the forward slashes with a different character, such as an underscore character ( _ ) or a dash ( - ).
