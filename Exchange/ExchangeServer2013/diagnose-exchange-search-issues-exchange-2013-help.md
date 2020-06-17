---
title: 'Diagnose Exchange Search issues: Exchange 2013 Help'
TOCTitle: Diagnose Exchange Search issues
ms:assetid: 8cfa26f4-ccf0-42dd-8570-67018188b4e8
ms:mtpsurl: https://technet.microsoft.com/library/Bb123701(v=EXCHG.150)
ms:contentKeyID: 51407264
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Diagnose Exchange Search issues

_**Applies to:** Exchange Server 2013_

Exchange Search indexes mailboxes and supported attachments in Exchange mailboxes. With increasing volumes of email, increasing mailbox sizes and storage quotas, provisioning archive mailboxes for users, and In-Place eDiscovery for performing discovery searches, Exchange Search is a critical component of the Mailbox servers in your Microsoft Exchange Server 2013 organization. Issues with Exchange Search can affect user productivity and impact In-Place eDiscovery functionality.

To learn more about Exchange Search, see [Exchange Search](exchange-search-exchange-2013-help.md).

Looking for management tasks related to managing Exchange Search? See [Exchange Search procedures](exchange-search-procedures-exchange-2013-help.md).

## Using the Test-ExchangeSearch Cmdlet

Step 5 of the procedure in this topic describes running the **Test-ExchangeSearch** cmdlet to help diagnose Exchange Search issues. You can use the **Test-ExchangeSearch** cmdlet to test Exchange Search functionality for a Mailbox server, a mailbox database, or a specific mailbox. The cmdlet delivers a test message to the specified mailbox (or to a database's system mailbox if a mailbox isn't specified), and then performs a search to determine whether the message is indexed, including the time taken to index it. Under normal conditions, Exchange Search indexes a message within about 10 seconds of the message being created or delivered to a mailbox. The test message is automatically deleted after the test.

For detailed syntax and parameter information, see [Test-ExchangeSearch](https://docs.microsoft.com/powershell/module/exchange/Test-ExchangeSearch).

## Retrieving unsearchable Items

You can use the [Get-FailedContentIndexDocuments](https://docs.microsoft.com/powershell/module/exchange/Get-FailedContentIndexDocuments) cmdlet to retrieve a list of unsearchable mailbox items that couldn't be successfully indexed by Exchange Search. You can run the cmdlet against a Mailbox server, a mailbox database, or a specific mailbox. The cmdlet returns details about each item that couldn't be searched. There are several reasons why a mailbox item can't be searched; for example, an email message might contain an attachment file type that can't be indexed for search or because a search filter isn't installed or is disabled. If a search filter for that file type is available, you can install it on your Exchange servers.

> [!IMPORTANT]
> Search filters provided by Microsoft are tested and supported by Microsoft. We recommend that you test any third-party search filters in a test environment before installing them on Exchange servers in a production environment.

For more information about unsearchable items, see :

- [File formats indexed by Exchange Search](file-formats-indexed-by-exchange-search-exchange-2013-help.md)

- [Unsearchable items in Exchange eDiscovery](unsearchable-items-in-exchange-ediscovery-exchange-2013-help.md)

## Diagnose Exchange Search issues

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange Search" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

1. **Check service state**: Is the Microsoft Exchange Search (MSExchangeFastSearch) service started on the Mailbox server? If yes, go to Step 2. If no, use the Services MMC snap-in to verify that the MSExchangeFastSearch service is running as follows:

    1. Click **Start**, point to **Administrative Tools**, and then click **Services**.

    2. In **Services**, verify that the **Status** for the **Microsoft Exchange Search** service is listed as **Started**.

2. **Check mailbox database configuration**: Is the *IndexEnabled* parameter set to true for the user's mailbox database? If yes, go to Step 3. If no, run the following command in the Shell to verify that the *IndexEnabled* flag is set to true.

    ```powershell
    Get-MailboxDatabase | Format-Table Name,IndexEnabled
    ```

    For detailed syntax and parameter information, see [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabase).

3. **Check mailbox database crawl state**: Has the Exchange database been crawled? If yes, go to Step 4. If no, use Reliability and Performance Monitor to check the **Crawler: Mailboxes Remaining** counter of the **MSExchange Search Indexes** performance object. Perform the following steps:

    1. Open **Performance Monitor** (perfmon.exe).

    2. In the console tree, under **Monitoring Tools**, click **Performance Monitor**.

    3. In the Performance Monitor pane, click **Add** (green plus sign).

    4. In **Add Counters**, in the **Select counters from computer** list, select the server on which the mailbox database you want to monitor is located.

    5. In the unlabeled box below the **Select counters from computer** list, select the **MSExchange Search Indexes** performance object.

    6. In the **Instances of selected object** box, select the instance for the user's mailbox database.

    7. Click **Add**, and then click **OK**.

        In the Performance Monitor pane, the **MSExchange Search Indexes** performance object is listed in the **Object** column, and its various counters are listed in the **Counter** column.

    8. View the **Crawler: Mailboxes Remaining** counter. Any value of 1 or higher indicates that mailboxes in the database are still being crawled. When the crawl is complete, the value is **0**.

    For information about using Performance Monitor, see [Performance and Reliability Monitoring Getting Started Guide for Windows Server 2008](https://go.microsoft.com/fwlink/p/?linkid=178005)

4. **Check the database copy indexing health**: Is the content index healthy? Use the **Get-MailboxDatabaseCopyStatus** cmdlet to check the content indexing health for a database copy.

    ```powershell
    Get-MailboxDatabaseCopyStatus -Server $env:ComputerName | Format-Table Name,Status,ContentIndex* -Auto
    ```

    For detailed syntax and parameter information, see [Get-MailboxDatabaseCopyStatus](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabaseCopyStatus).

5. **Run the Test-ExchangeSearch cmdlet**: If the mailbox database has already been crawled, you can run the **Test-ExchangeSearch** cmdlet for the mailbox database or for a specific mailbox.

    ```powershell
    Test-ExchangeSearch -Identity AlanBrewer@contoso.com
    ```

    For detailed syntax and parameter information, see [Test-ExchangeSearch](https://docs.microsoft.com/powershell/module/exchange/Test-ExchangeSearch).

6. **Check the Application event log**: Using Event Viewer or the Shell, check the Application event log for search-related error messages. Check for following event sources.

      - **MSExchangeFastSearch**

      - **MSExchangeIS**

    For more information, follow the link in the event log entry.

7. **Restart the Microsoft Exchange Search service**: Use the Services MMC snap-in or the Shell to stop and then restart the Microsoft Exchange Search (MSExchangeFastSearch) service:

    1. Click **Start**, point to **Administrative Tools**, and then click **Services**.

    2. In **Services**, right-click **Microsoft Exchange Search**, and then click **Stop**. After the service is stopped, right-click the service again, and then click **Start**.

8. **Reseed the search catalog**: In some cases, such as when the search catalog is corrupted, you may need to reseed the catalog. When a search catalog needs to be reseeded, Exchange Search notifies you by logging entries in the Application event log. For more information about reseeding the Search catalog, see [Reseed the search catalog](reseed-the-search-catalog-exchange-2013-help.md).
