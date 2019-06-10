---
title: 'Enable and configure priority queuing: Exchange 2013 Help'
TOCTitle: Enable and configure priority queuing
ms:assetid: 1975d85d-2f1d-4852-8d19-e74ba4ba3853
ms:mtpsurl: https://technet.microsoft.com/en-us/library/JJ891104(v=EXCHG.150)
ms:contentKeyID: 50646230
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Enable and configure priority queuing

_**Applies to:** Exchange Server 2013_

*Priority queuing* is a feature of Microsoft Exchange Server 2013 that enables the message priority that's configured by the sender in Microsoft Outlook or Outlook Web Access to influence the processing of the message by the Transport service on the Mailbox server. When priority queuing is enabled, High priority messages are transmitted to their destinations before Normal priority messages, and Normal priority messages are transmitted to their destinations before Low priority messages. For more information, see [Priority queuing](priority-queuing-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- Exchange permissions don't apply to the procedures in this topic. These procedures are performed in the operating system of the Exchange Server.

- Changes you save to the EdgeTransport.exe.config application configuration file are applied after you restart the Microsoft Exchange Transport service.

- When you restart the Microsoft Exchange Transport service, mail flow on the server is temporarily interrupted.

- Any customized per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update (CU). Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange CU.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Command Prompt to enable and configure priority queuing in the EdgeTransport.exe.config file

1. In a Command prompt window, open the EdgeTransport.exe.config application configuration file in Notepad by running the following command:

    ```powershell
    Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
    ```

2. Find the following keys in the `<appSettings>` section.

    ```powershell
    <add key="PriorityQueuingEnabled" value="false" />
    <add key="MaxPerDomainHighPriorityConnections" value="3" />
    <add key="MaxPerDomainNormalPriorityConnections" value="15" />
    <add key="MaxPerDomainLowPriorityConnections" value="2" />
    <add key="HighPriorityMessageExpirationTimeout" value="8:00:00" />
    <add key="NormalPriorityMessageExpirationTimeout" value="2.00:00:00" />
    <add key="LowPriorityMessageExpirationTimeout" value="2.00:00:00" />
    <add key="HighPriorityDelayNotificationTimeout" value="00:30:00" />
    <add key="NormalPriorityDelayNotificationTimeout" value="4:00:00" />
    <add key="LowPriorityDelayNotificationTimeout" value="8:00:00" />
    <add key="MaxHighPriorityMessageSize" value="250KB" />
    ```

    To enable priority queuing in the Transport service on the Mailbox server, use the following value:

    ```powershell
    <add key="PriorityQueuingEnabled" value="true" />
    ```

    Configure the remaining priority queuing values, or leave them at their default values.

3. When you are finished, save and close the EdgeTransport.exe.config file.

4. Restart the Microsoft Exchange Transport service by running the following command:

    ```powershell
    net stop MSExchangeTransport && net start MSExchangeTransport
    ```

## How do you know this worked?

To verify that you have successfully enabled and configured priority queuing, do the following:

1. Verify the **PriorityQueueinEnabled** key in the EdgeTransport.exe.config file has the value `"true"`.

2. Use Outlook to create a high priority test message that's larger than the value specified by the **MaxHighPriorityMessageSize** key, and verify the message arrives as a normal priority message.

3. Try to verify that higher priority messages arrive before lower priority messages sent to the same recipient or destination. You can try to use multiple mailboxes to send multiple, similar test messages with different priority values to the same recipient simultaneously.
