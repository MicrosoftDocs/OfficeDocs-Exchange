---
title: 'Move a mailbox database using database portability: Exchange 2013 Help'
TOCTitle: Move a mailbox database using database portability
ms:assetid: a765ead1-43bc-4786-ae93-1835cacfc8fc
ms:mtpsurl: https://technet.microsoft.com/library/Dd876926(v=EXCHG.150)
ms:contentKeyID: 50873806
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Move a mailbox database using database portability

_**Applies to:** Exchange Server 2013_

You can use database portability to move a Microsoft Exchange Server 2013 mailbox database between Exchange 2013 Mailbox servers in the same organization. This can help reduce overall recovery times for some failure scenarios. To learn more, see [Database portability](database-portability-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes, plus the time it takes to restore the data, move the database files, and wait for Active Directory replication to complete.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox recovery" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can't use the EAC to move user mailboxes to a recovered or dial tone database using database portability.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to move user mailboxes to a recovered or dial tone database using database portability

1. Verify that the database to be moved is in a clean shutdown state. If the database isn't in a clean shutdown state, perform a soft recovery.

    > [!NOTE]
    > When you perform a soft recovery, any uncommitted log files are committed to the database. If you don't have all of the required log files, you can't complete the soft recovery process. Proceed to step&nbsp;2.

    To commit all uncommitted log files to the database, from a command prompt, run the following command.

    ```powershell
    ESEUTIL /R <Enn>
    ```

    > [!NOTE]
    > &lt;E<EM>nn</EM>&gt; specifies the log file prefix for the database into which you intend to replay the log files. The log file prefix specified by &lt;E<EM>nn</EM>&gt; is a required parameter for Eseutil /r.

2. Create a database on a server using the following syntax:

    ```powershell
    New-MailboxDatabase -Name <DatabaseName> -Server <ServerName> -EdbFilePath <DatabaseFileNameandPath> -LogFolderPath <LogFilesPath>
    ```

3. Set the *This database can be over written by restore* attribute using the following syntax:

    ```powershell
    Set-MailboxDatabase <DatabaseName> -AllowFileRestore $true
    ```

4. Move the original database files (.edb file, log files, and Exchange Search catalog) to the database folder you specified when you created the new database above.

5. Mount the database using the following syntax:

    ```powershell
    Mount-Database <DatabaseName>
    ```

6. After the database is mounted, modify the user account settings with the [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox) cmdlet so that the account points to the mailbox on the new mailbox server. To move all of the users from the old database to the new database, use the following syntax.

    ```powershell
    Get-Mailbox -Database <SourceDatabase> |where {$_.ObjectClass -NotMatch '(SystemAttendantMailbox|ExOleDbSystemMailbox)'}| Set-Mailbox -Database <TargetDatabase>
    ```

7. Trigger delivery of any messages remaining in queues using the following syntax.

    ```powershell
    Get-Queue <QueueName> | Retry-Queue -Resubmit $true
    ```

After Active Directory replication is complete, all users can access their mailboxes on the new Exchange server. Most clients are redirected via Autodiscover. Outlook Web App users are also automatically redirected.

## How do you know this worked?

To verify that you've successfully moved a mailbox, do the following:

- Open the mailbox using Outlook Web App.

- Open the mailbox using Microsoft Outlook.
