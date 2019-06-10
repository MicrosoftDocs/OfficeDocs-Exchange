---
localization_priority: Normal
description: 'Summary: Learn how to move or recreate the message queue database in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: f170cb0c-04a9-4fa7-b594-206e3a787e14
ms.date: 7/6/2018
ms.reviewer: 
title: Change the location of the queue database
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Change the location of the queue database

Exchange Server uses an Extensible Storage Engine (ESE) database for queue message storage. All the different queues are stored in a single ESE database. Queues exist on Exchange Mailbox servers and Edge Transport servers. For more information about queues, see [Queues and messages in queues](queues.md).

The location of the queue database and the queue database transaction logs is controlled by keys in the `%ExchangeInstallPath%Bin\EdgeTransport.exe.config` XML application configuration file. This file is associated with the Exchange Transport service. The following table explains each key in more detail.

|**Key**|**Description**|
|:-----|:-----|
| _QueueDatabasePath_|Specifies the location of the queue database files. The files are:  <br/> • Mail.que  <br/> • Trn.chk  <br/> The default location is `%ExchangeInstallPath%TransportRoles\data\Queue`.|
| _QueueDatabaseLoggingPath_|Specifies the location of the queue database transaction log files. The files are:  <br/> • Trn.log  <br/> • Trntmp.log  <br/> • Trn _nnn_.log  <br/> • Trnres00001.jrs  <br/> • Trnres00002.jrs  <br/> • Temp.edb  <br/> Note that Temp.edb is used to verify the queue database schema when the Exchange Transport service starts. Although Temp.edb isn't a transaction log file, it's kept in the same location as the transaction log files.  <br/> The default location is `%ExchangeInstallPath%TransportRoles\data\Queue`.|

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange server.

- When you stop or restart the Exchange Transport service, mail flow on the server is interrupted.

- When you change the location of the queue database or the transaction logs, the existing queue database and transaction log files aren't moved. A new queue database and new transaction logs are created at the new location. The old files are left at the old location, but they're no longer used. If you want to reuse the old queue database or transaction log files at the new location, you need to move the files to the new location while the Exchange Transport service is stopped.

- The folder for the queue database and transaction logs needs the following permissions:

  - Network Service: Full Control

  - System: Full Control

  - Administrators: Full Control

    If the folder doesn't exist, but the parent folder has these permissions, the new folder is created automatically.

- Any customized Exchange or Internet Information Server (IIS) settings that you made in Exchange XML application configuration files on the Exchange server (for example, web.config files or the EdgeTransport.exe.config file) **will be overwritten** when you install an Exchange CU. Be sure save this information so you can easily re-apply the settings after the install. After you install the Exchange CU, you need to re-configure these settings.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

- Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Command Prompt to create a new queue database and transaction logs in a new location
<a name="New"> </a>

1. Create the folder where you want to keep the queue database and transaction logs. Make sure that the correct permissions are applied to the folder.

2. In a Command prompt window, open the EdgeTransport.exe.config file in Notepad by running the following command:

  ```
  Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
  ```

3. Find and modify the following keys in the `<appSettings>` section.

  ```
  <add key="QueueDatabasePath" value="<LocalPath>" />
  <add key="QueueDatabaseLoggingPath" value="<LocalPath>" />
  ```

    For example, to create a new queue database and transaction logs in D:\Queue\QueueDB, use the following values:

  ```
  <add key="QueueDatabasePath" value="D:\Queue\QueueDB" />
  <add key="QueueDatabaseLoggingPath" value="D:\Queue\QueueDB" />
  ```

    When you're finished, save and close the EdgeTransport.exe.config file.

4. Restart the Exchange Transport service by running the following command:

  ```
  net stop MSExchangeTransport && net start MSExchangeTransport
  ```

### How do you know this worked?

To verify that you've successfully created a new queue database and new transaction logs in the new location, do these steps:

1. Verify the new database files Mail.que and Trn.chk exist at the new location.

2. Verify the new transaction log files Trn.log, Trntmp.log, Trnres00001.jrs, Trnres00002.jrs, and Temp.edb files exist at the new location.

3. If you can delete the old queue database and transaction log files from the old location after the Exchange Transport service has started, the old queue database is no longer being used.

## Use the Command Prompt to move the existing queue database and transaction logs to a new location
<a name="Existing"> </a>

> [!NOTE]
> There is also a script to move the queue database and transaction logs, it can be found in the %ExchangeInstallPath%Scripts folder and it's called Move-TransportDatabase.ps1. You have to specify the following parameters: queueDatabasePath, queueDatabaseLoggingPath, iPFilterDatabasePath, iPFilterDatabaseLoggingPath and temporaryStoragePath.

Although you'll need to move the existing queue database to preserve any undelivered messages in it, you typically don't need to move the existing transaction logs because:

- An ordinary shutdown of the Exchange Transport service writes all uncommitted transaction log entries to the queue database.

- Circular logging is used, so transaction logs that contain previously committed database changes aren't preserved.

1. Create the folder where you want to keep the queue database and transaction logs. Make sure that the correct permissions are applied to the folder.

2. In a Command prompt window, open the EdgeTransport.exe.config file in Notepad by running the following command:

  ```
  Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
  ```

3. Find and modify the following keys in the `<appSettings>` section:

  ```
  <add key="QueueDatabasePath" value="<LocalPath>" />
  <add key="QueueDatabaseLoggingPath" value="<LocalPath>" />
  ```

    For example, to change the location of the queue database and transaction logs to D:\Queue\QueueDB, use the following values:

  ```
  <add key="QueueDatabasePath" value="D:\Queue\QueueDB" />
  <add key="QueueDatabaseLoggingPath" value="D:\Queue\QueueDB" />
  ```

    When you're finished, save and close the EdgeTransport.exe.config file.

4. Stop the Exchange Transport service by running the following command:

  ```
  net stop MSExchangeTransport
  ```

5. Move the existing database files Mail.que and Trn.chk from the old location to the new location.

6. Move the existing transaction log files Trn.log, Trntmp.log, Trn _nnnnn_.log, Trnres00001.jrs, Trnres00002.jrs, and Temp.edb from the old location to the new location.

7. Start the Exchange Transport service by running the following command:

  ```
  net start MSExchangeTransport
  ```

### How do you know this worked?

 To verify that you've successfully moved the existing queue database and transaction logs to the new location, do these steps:

1. Verify the queue database files Mail.que and Trn.chk exist in the new location.

2. Verify the transaction log files Trn.log, Trntmp.log, Trnres00001.jrs, Trnres00002.jrs, and Temp.edb files exist in the new location.

3. Verify there are no queue database or transaction log files in the old location.
