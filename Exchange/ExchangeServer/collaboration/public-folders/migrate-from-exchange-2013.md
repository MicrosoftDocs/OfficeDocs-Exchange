---
localization_priority: Normal
ms.author: v-mapenn
manager: serdars
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: 0f98801d-ad5c-4109-a021-63645e9c9ca2
ms.reviewer: 
ms.collection: exchange-server
description: Learn how to migrate on-premises public folders from Exchange 2013 to Exchange 2016 or Exchange 2019.
f1.keywords:
- NOCSH
audience: ITPro
title: Migrate public folders from Exchange 2013 to Exchange 2016 or Exchange 2019

---

# Migrate public folders from Exchange 2013 to Exchange 2016 or Exchange 2019

 To migrate your Exchange 2013 public folders to Exchange 2016 or Exchange 2019, you need to move all of your Exchange 2013 public folder mailboxes to an Exchange 2016 server or Exchange 2019 server.

Before you move your public folder mailboxes, here are some things you should consider:

- **Capacity**: The size of your public folder mailboxes might vary significantly depending on how many public folders and public folder mailboxes you have. Make sure the target Exchange servers where you'll move your public folder mailboxes have enough storage capacity.

- **Time**: It might take a while to move your public folder mailboxes. The following items could impact how long it takes:

- Public folder mailbox size

- The number of public folder mailboxes

- Network bandwith

The good news is that your public folders will remain available during the public folder mailbox move. There's only a brief time window where the public folders might no be available (as the move completes).

## What do you need to know before you begin?

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to move public folder mailboxes from Exchange 2013 to Exchange 2016 or Exchange 2019

1. Run the following command to get a list of all Exchange 2013 public folder mailboxes:

    ```PowerShell
    Get-ExchangeServer | Where {($_.AdminDisplayVersion -Like 'Version 15.0*') -And ($_.ServerRole -Like '*Mailbox*')} | Get-Mailbox -PublicFolder | Get-MailboxStatistics | Format-Table -Auto ServerName,DisplayName,TotalItemSize
    ```

2. Use the following syntax to list all mailbox databases on all Exchange 2016 or Exchange 2019 Mailbox servers:

    ```PowerShell
    Get-ExchangeServer | Where {($_.AdminDisplayVersion -like '<Version>') -and ($_.ServerRole -Like "*Mailbox*")} | Get-MailboxDatabase | Format-List Server,Name,EdbFilePath
    ```

    You can use the location information that's returned by this command to check the available free disk space for each mailbox database.

    This example returns the locations of all mailbox databases on all Exchange 2016 Mailbox servers.

    ```PowerShell
    Get-ExchangeServer | where {($_.AdminDisplayVersion -like 'Version 15.1*') -and ($_.ServerRole -Like '*Mailbox*')} | Get-MailboxDatabase | Format-List Server,Name,EdbFilePath
    ```

    This example returns the locations of all mailbox databases on all Exchange 2019 Mailbox servers.

    ```PowerShell
    Get-ExchangeServer | where {($_.AdminDisplayVersion -like 'Version 15.2*') -and ($_.ServerRole -Like '*Mailbox*')} | Get-MailboxDatabase | Format-List Server,Name,EdbFilePath
    ```

    This example returns the locations of all mailbox databases on all Exchange 2016 and Exchange 2019 Mailbox servers.

    ```PowerShell
    Get-ExchangeServer | where {(($_.AdminDisplayVersion -like 'Version 15.1*') -or ($_.AdminDisplayVersion -like 'Version 15.2*')) -and ($_.ServerRole -Like '*Mailbox*')} | Get-MailboxDatabase | Format-List Server,Name,EdbFilePath
    ```

3. Use the information from the previous steps to decide the target mailbox database and/or Mailbox server (if you have more than one) to move some or all of your public folder mailboxes to. For example, you might not want to move three large public folder mailboxes to a server with low available drive space.

    You can also decide whether you want to move all public folder mailboxes at once, all public folder mailboxes on a specific server, or a specific public folder mailbox.

    Choose the command that fits the kind of move you want to do. Be sure to replace the Exchange server names, database names, and public folder mailbox names with your own.

    - Move all Exchange 2013 public folder mailboxes at once.

      ```PowerShell
      Get-ExchangeServer | Where {($_.AdminDisplayVersion -Like "Version 15.0*") -And ($_.ServerRole -Like "*Mailbox*")} | Get-Mailbox -PublicFolder | New-MoveRequest -TargetDatabase Ex2016MbxDatabase
      ```

    - Move all public folder mailboxes on a specific Exchange 2013 server at once.

      ```PowerShell
      Get-Mailbox -PublicFolder -Server Ex2013Mbx | New-MoveRequest -TargetDatabase Ex2016MbxDatabase
      ```

    - Move a specific Exchange 2013 public folder mailbox.

      ```PowerShell
      New-MoveRequest "Sales Public Folder Mailbox" -TargetDatabase Ex2016MbxDatabase
      ```

4. To see the status of the move requests you created, run the following command:

    ```PowerShell
    Get-MoveRequest
    ```

    Depending on the size of the public folder mailboxes you're moving and your available network capacity, it could take several hours or days for the moves to complete.

    For a list of possible status values that can be returned, see the next section.

## How do you know this worked?

To verify that you've successfully migrated all of your Exchange 2013 public folders to Exchange 2016 or Exchange 2019, do the following steps:

- Check the status of the move requests you created by running the following command in the Exchange Management Shell on an Exchange 2016 or Exchange 2019 Mailbox server:

  ```PowerShell
  Get-MoveRequest
  ```

  The command will return each move request you created along with one of the following status values:

  - **Completed**: The public folder mailbox was successfully moved to the target mailbox database.

  - **CompletedWithWarning**: The public folder mailbox was moved to the target mailbox database, but one or more issues were encountered during the move. You can find more information by viewing the move report that was delivered to the Administrator mailbox.

  - **CompletionInProgress**: The public folder mailbox move to the target mailbox database is in its final stages. Public folders hosted in this mailbox may be unavailable for a brief period of time while the move is finalized.

  - **InProgress**: The public folder mailbox move to the target mailbox database is underway. Public folders hosted in this mailbox are available during this portion of the move.

  - **Failed**: The public folder mailbox move failed for one or more reasons. You can find more information by viewing the move report that was delivered to the Administrator mailbox.

  - **Queued**: The public folder mailbox move has been submitted but the move hasn't started yet.

  - **Retry**: The migration service is currently having trouble proceeding with the job, but it has not given up, and will continue trying.

  - **AutoSuspended**: The public folder mailbox move is ready to enter its final stages but won't proceed further until you manually resume the move.

    This option can be helpful if you want to choose the time a move will complete. You can automatically suspend a move when you create it by using the _SuspendWhenReadyToComplete_ switch on the **New-MoveRequest** cmdlet. To resume the move when you're ready, use the **Resume-MoveRequest** cmdlet.

  - **Suspended**: The public folder mailbox move has been manually suspended by **Suspend-MoveRequest** cmdlet and won't proceed until you manually resume the move. To resume the move when you're ready, use the **Resume-MoveRequest** cmdlet.

- View the location of your public folder mailboxes after their move request has completed by running the following command on an Exchange 2016 or Exchange 2019 server:

  ```PowerShell
  Get-Mailbox -PublicFolder | Get-MailboxStatistics | Format-Table ServerName,DisplayName,TotalItemSize
  ```

  In the list public folder mailboxes that are returned, verify that they've each been moved to an Exchange 2016 Mailbox server.
