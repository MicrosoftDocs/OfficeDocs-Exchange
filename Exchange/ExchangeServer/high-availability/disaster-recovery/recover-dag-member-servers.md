---
localization_priority: Normal
description: 'Summary: How to recover an Exchange DAG member after a failure in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: eccd8f61-9706-4bb7-a62a-ec7c166f8019
ms.reviewer:
title: Recover a database availability group member server, recover Exchange DAG member, Exchange DAG server recovery, DAG server recovery, Exchange DAG failover
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Recover a database availability group member server

If a Mailbox server that's a member of a database availability group (DAG) is lost or fails, and is unrecoverable and needs replacement, you can perform a server recovery operation.

Microsoft Exchange Server Setup includes the switch _/m:RecoverServer_ that can be used to perform the server recovery operation. Running Setup with the _/m:RecoverServer_ switch causes Setup to read the server's configuration information from Active Directory for a server with the same name as the server from which you're running Setup.

After the server's configuration information is gathered from Active Directory, the original Exchange files and services are then installed on the server, and the roles and settings that were stored in Active Directory are then applied to the server.

Looking for other management tasks related to DAGs? Check out [Manage database availability groups](../manage-ha/manage-dags.md).

## What do you need to know before you begin?

- Estimated time to complete: 30 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mailbox database copies" entry in the [High availability and site resilience permissions](../../permissions/feature-permissions/ha-permissions.md) topic.

- If Exchange is installed in a location other than the default location, you must use the _/TargetDir_ Setup switch to specify the location of the Exchange program files. If you don't use the _/TargetDir_ switch, the Exchange program files will be installed in the default location (%programfiles%\Microsoft\Exchange Server\V15).

  To determine the install location, follow these steps:

  1. Open ADSIEDIT.MSC or LDP.EXE.

  2. Navigate to the following location: **CN=ExServerName,CN=Servers,CN=First Administrative Group,CN=Administrative Groups,CN=ExOrg Name,CN=Microsoft Exchange,CN=Services,CN=Configuration,DC=DomainName,CN=Com**

  3. Right-click the Exchange server object, and then click **Properties**.

  4. Locate the **msExchInstallPath** attribute. This attribute stores the current installation path.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Setup /m:RecoverServer to recover a server

1. Retrieve any replay lag or truncation lag settings for any mailbox database copies that exist on the server being recovered by using the [Get-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/get-mailboxdatabase) cmdlet:

   ```powershell
   Get-MailboxDatabase DB1 | Format-List *lag*
   ```

2. Remove any mailbox database copies that exist on the server being recovered by using the [Remove-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxdatabasecopy) cmdlet:

   ```powershell
   Remove-MailboxDatabaseCopy DB1\MBX1
   ```

3. Remove the failed server's configuration from the DAG by using the [Remove-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/remove-databaseavailabilitygroupserver) cmdlet:

   ```powershell
   Remove-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
   ```

   > [!NOTE]
   > If the DAG member being removed is offline and can't be brought online, you must add the `-ConfigurationOnly` parameter to the preceding command. If you use the `-ConfigurationOnly` switch, you must also manually evict the node from the cluster.

4. Reset the server's computer account in Active Directory. For detailed steps, see [Reset a Computer Account](https://go.microsoft.com/fwlink/p/?linkId=167188).

5. Open a Command Prompt window. Using the original Setup media, run the following command:

   ```powershell
   Setup /m:RecoverServer
   ```

6. When the Setup recovery process is complete, add the recovered server to the DAG by using the [Add-DatabaseAvailabilityGroupServer](https://docs.microsoft.com/powershell/module/exchange/add-databaseavailabilitygroupserver) cmdlet:

   ```powershell
   Add-DatabaseAvailabilityGroupServer -Identity DAG1 -MailboxServer MBX1
   ```

7. After the server has been added back to the DAG, you can reconfigure mailbox database copies by using the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) cmdlet. If any of the database copies being added previously had replay lag or truncation lag times greater than 0, you can use the _ReplayLagTime_ and _TruncationLagTime_ parameters of the [Add-MailboxDatabaseCopy](https://docs.microsoft.com/powershell/module/exchange/add-mailboxdatabasecopy) cmdlet to reconfigure those settings:

   ```powershell
   Add-MailboxDatabaseCopy -Identity DB1 -MailboxServer MBX1
   Add-MailboxDatabaseCopy -Identity DB2 -MailboxServer MBX1 -ReplayLagTime 3.00:00:00
   Add-MailboxDatabaseCopy -Identity DB3 -MailboxServer MBX1 -ReplayLagTime 3.00:00:00 -TruncationLagTime 3.00:00:00
   ```

## How do you know this worked?

To verify that you've successfully recovered the DAG member, do the following:

- In the Exchange Management Shell, run the following command to verify the health and status of the recovered DAG member.

  ```powershell
  Test-ReplicationHealth <ServerName>
  ```

  ```powershell
  Get-MailboxDatabaseCopyStatus -Server <ServerName>
  ```

  All of the replication health tests should pass successfully, and the status of databases and their content indexes should be healthy.
