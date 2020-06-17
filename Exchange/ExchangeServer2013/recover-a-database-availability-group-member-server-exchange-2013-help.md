---
title: 'Recover a database availability group member server: Exchange 2013 Help'
TOCTitle: Recover a database availability group member server
ms:assetid: eccd8f61-9706-4bb7-a62a-ec7c166f8019
ms:mtpsurl: https://technet.microsoft.com/library/Dd638206(v=EXCHG.150)
ms:contentKeyID: 48385683
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Recover a database availability group member server

_**Applies to:** Exchange Server 2013_

If a Mailbox server that's a member of a database availability group (DAG) is lost or otherwise fails and is unrecoverable and needs replacement, you can perform a server recovery operation. Microsoft Exchange Server 2013 Setup includes the switch */m:RecoverServer* that can be used to perform the server recovery operation. Running Setup with the */m:RecoverServer* switch causes Setup to read the server's configuration information from Active Directory for a server with the same name as the server from which you're running Setup. After the server's configuration information is gathered from Active Directory, the original Exchange files and services are then installed on the server, and the roles and settings that were stored in Active Directory are then applied to the server.

Looking for other management tasks related to DAGs? Check out [Managing database availability groups](managing-database-availability-groups-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](high-availability-and-site-resilience-permissions-exchange-2013-help.md) topic.

- If Exchange is installed in a location other than the default location, you must use the */TargetDir* Setup switch to specify the location of the Exchange program files. If you don't use the */TargetDir* switch, the Exchange program files will be installed in the default location (%programfiles%\\Microsoft\\Exchange Server\\V15).

  To determine the install location, follow these steps:

  1. Open ADSIEDIT.MSC or LDP.EXE.

  2. Navigate to the following location: **CN=ExServerName,CN=Servers,CN=First Administrative Group,CN=Administrative Groups,CN=ExOrg Name,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=DomainName,CN=Com**

  3. Right-click the Exchange server object, and then click **Properties**.

  4. Locate the **msExchInstallPath** attribute. This attribute stores the current installation path.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use Setup /m:RecoverServer to recover a server

1. Retrieve any replay lag or truncation lag settings for any mailbox database copies that exist on the server being recovered by using the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxDatabase) cmdlet:

    ```powershell
    Get-MailboxDatabase DB1 | Format-List *lag*
    ```

2. Remove any mailbox database copies that exist on the server being recovered by using the [Remove-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Remove-MailboxDatabaseCopy) cmdlet:

    ```powershell
    Remove-MailboxDatabaseCopy DB1\MBX1
    ```

3. Remove the failed server's configuration from the DAG by using the [Remove-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Remove-DatabaseAvailabilityGroupServer) cmdlet:

    ```powershell
    Remove-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
    ```

    > [!NOTE]
    > If the DAG member being removed is offline and can't be brought online, you must add the <EM>ConfigurationOnly</EM> parameter to the preceding command. If you use the <EM>ConfigurationOnly</EM> switch, you must also manually evict the node from the cluster.

4. Reset the server's computer account in Active Directory. For detailed steps, see [Reset a Computer Account](https://go.microsoft.com/fwlink/p/?linkid=167188).

5. Open a Command Prompt window. Using the original Setup media, run the following command:

    ```powershell
    Setup /m:RecoverServer
    ```

6. When the Setup recovery process is complete, add the recovered server to the DAG by using the [Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/Add-DatabaseAvailabilityGroupServer) cmdlet:

    ```powershell
    Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
    ```

7. After the server has been added back to the DAG, you can reconfigure mailbox database copies by using the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Add-MailboxDatabaseCopy) cmdlet. If any of the database copies being added previously had replay lag or truncation lag times greater than 0, you can use the *ReplayLagTime* and *TruncationLagTime* parameters of the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/Add-MailboxDatabaseCopy) cmdlet to reconfigure those settings:

    ```powershell
    Add-MailboxDatabaseCopy -Identity DB1 -MailboxServer MBX1
    Add-MailboxDatabaseCopy -Identity DB2 -MailboxServer MBX1 -ReplayLagTime 3.00:00:00
    Add-MailboxDatabaseCopy -Identity DB3 -MailboxServer MBX1 -ReplayLagTime 3.00:00:00 -TruncationLagTime 3.00:00:00
    ```

## How do you know this worked?

To verify that you've successfully recovered the DAG member, do the following:

- In the Shell, run the following command to verify the health and status of the recovered DAG member.

    ```powershell
    Test-ReplicationHealth <ServerName>
    ```

    ```powershell
    Get-MailboxDatabaseCopyStatus -Server <ServerName>
    ```

    All of the replication health tests should pass successfully, and the status of databases and their content indexes should be healthy.
