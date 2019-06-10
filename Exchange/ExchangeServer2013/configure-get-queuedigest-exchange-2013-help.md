---
title: 'Configure Get-QueueDigest: Exchange 2013 Help'
TOCTitle: Configure Get-QueueDigest
ms:assetid: f730c520-4ba5-4a15-8846-132bff500bb8
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Dn505733(v=EXCHG.150)
ms:contentKeyID: 59603968
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure Get-QueueDigest

_**Applies to:** Exchange Server 2013_

The **Get-QueueDigest** cmdlet allows you to view information about some or all of the queues in your Exchange organization by using a single command.

By default, the results returned by the **Get-QueueDigest** cmdlet are between one and two minutes old. These values are controlled by the following settings:

- **QueueLoggingInterval key in EdgeTransport.exe.config**: This key specifies how frequently queue data is logged and is available to **Get-QueueDigest**. The default value is `00:01:00` (one minute). To specify a value, enter it as a time span: *hh:mm:ss* where *h* = hours, *m* = minutes, and *s* = seconds. By default, this key isn't present in the EdgeTransport.exe.config file.

- **QueueDiagnosticsAggregationInterval parameter on Set-TransportConfig**: This parameter specifies how frequently queue data is shared between Mailbox servers. The default value is `00:01:00` (one minute). To specify a value, enter it as a time span: *hh:mm:ss* where *h* = hours, *m* = minutes, and *s* = seconds.

The sum of the **QueueLoggingInterval** key and *QueueDiagnosticsAggregationInterval* parameter values determine the maximum age of the results returned by **Get-QueueDigest**.

Also, **Get-QueueDigest** returns results differently based on the type of queue and the status of the queue. For example, the following queues are displayed in the results as long as they contain at least one message:

- The Submission queue, the Unreachable queue, and the poison message queue (persistent queues).

- Delivery queues in the Suspended state (queues manually suspended by an administrator).

By default, delivery queues that have the status Active, Connecting, Ready, or Retry are returned in the results only if the queue contains 10 or more messages. This value is controlled by the **QueueLoggingThreshold** key in the EdgeTransport.exe.config file. You can specify a smaller or larger integer value. By default, this key isn't present in the EdgeTransport.exe.config file.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- To see the Exchange permissions you need to run **Set-TransportConfig** in the Exchange Management Shell, see the "Transport configuration" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- Exchange permissions don't apply to modifying the EdgeTransport.exe.config file and restarting the Microsoft Exchange Transport service. These procedures are performed in the operating system of the Exchange Server.

- Changes you save to the EdgeTransport.exe.config file are applied after you restart the Microsoft Exchange Transport service. When you restart the service, mail flow on the server is temporarily interrupted.

- Any customized per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update (CU). Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange CU.

- Changes you make using **Set-TransportConfig** affect all Mailbox servers in your organization. Changes you make in the EdgeTransport.exe.config file affect the local Mailbox server only.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Configure Get-QueueDigest

1. In a Command Prompt window, open the EdgeTransport.exe.config file in Notepad by running the following command:

    ```powershell
    Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
    ```

2. Add one or both of the following keys in the `<appSettings>` section.

    ```Command&nbsp;Line
    <add key="QueueLoggingThreshold" value="<integer>" />
    <add key="QueueLoggingInterval" value="<hh:mm:ss>" />
    ```

    For example, to set the **QueueLoggingThreshold** value to 1 and the **QueueLoggingInterval** value to 30 seconds, use the following values:

    ```Command&nbsp;Line
    <add key="QueueLoggingThreshold" value="1" />
    <add key="QueueLoggingInterval" value="00:00:30" />
    ```

3. When you are finished, save and close the EdgeTransport.exe.config file.

4. Restart the Microsoft Exchange Transport service by running the following command:

    ```powershell
    net stop MSExchangeTransport && net start MSExchangeTransport
    ```

5. To change the value of the *QueueDiagnosticsAggregationInterval* parameter in the Exchange Management Shell, use the following syntax:

    ```powershell
    Set-TransportConfig -QueueDiagnosticsAggregationInterval <hh:mm:ss>
    ```

    For example, to change the value to 30 seconds, run the following command:

    ```powershell
    Set-TransportConfig -QueueDiagnosticsAggregationInterval 00:00:30
    ```

## How do you know this worked?

To verify that you have successfully configured **Get-QueueDigest**, do the following:

1. Verify the values of the **QueueLoggingThreshold** and **QueueLoggingInterval** keys in the EdgeTransport.exe.config file. If the keys aren't present, the default values are used.

2. Verify the value of the *QueueDiagnosticsAggregationInterval* parameter by running the following command:

    ```powershell
    Get-TransportConfig | Format-List *queue*
    ```
