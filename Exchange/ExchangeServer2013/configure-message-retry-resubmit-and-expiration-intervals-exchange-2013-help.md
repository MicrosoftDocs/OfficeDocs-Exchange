---
title: 'Configure message retry, resubmit, and expiration intervals'
TOCTitle: Configure message retry, resubmit, and expiration intervals
ms:assetid: 5420124f-aa4c-4702-b493-40a9a7edb786
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Aa998043(v=EXCHG.150)
ms:contentKeyID: 50646232
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Configure message retry, resubmit, and expiration intervals

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can configure message retry, resubmit, and expiration intervals in the Transport service on Mailbox servers and on Edge Transport servers. For descriptions of these settings, see [Message retry, resubmit, and expiration intervals](message-retry-resubmit-and-expiration-intervals-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" and "Edge Transport server" entries in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- Any customized per-server settings you make in Exchange XML application configuration files, for example, web.config files on Client Access servers or the EdgeTransport.exe.config file on Mailbox servers, will be overwritten when you install an Exchange Cumulative Update (CU). Make sure that you save this information so you can easily re-configure your server after the install. You must re-configure these settings after you install an Exchange CU.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use EdgeTransport.exe.config to configure the queue glitch retry count, the queue glitch retry interval, the mailbox delivery queue retry interval, and the maximum idle time before resubmit interval.

To configure the queue glitch retry count, the queue glitch retry interval, the mailbox delivery queue retry interval, and the maximum idle time before resubmit interval you modify keys in the %ExchangeInstallPath%Bin\\EdgeTransport.exe.config XML application configuration file on the Mailbox server or Edge Transport server. Changes you save to this file are applied after you restart the Microsoft Exchange Transport service. When you restart this service, mail flow on the server is temporarily interrupted.

1. In a Command prompt window on the Mailbox server or Edge Transport server, open the EdgeTransport.exe.config file in Notepad by running the following command:

    ```powershell
    Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
    ```

2. Locate the following keys in the `<appSettings>` section.

    ```powershell
    <add key="QueueGlitchRetryCount" value="<Integer>" />
    <add key="QueueGlitchRetryInterval" value="<hh:mm:ss>" />
    <add key="MailboxDeliveryQueueRetryInterval" value="<hh:mm:ss>" />
    <add key="MaxIdleTimeBeforeResubmit" value="<hh:mm:ss>" />
    ```

    This example changes the queue glitch retry count to 6, the queue glitch retry interval to 30 seconds, the mailbox delivery queue retry interval to 3 minutes, and the maximum idle time before resubmit interval to 6 hours.

    ```powershell
    <add key="QueueGlitchRetryCount" value="6" />
    <add key="QueueGlitchRetryInterval" value="00:00:30" />
    <add key="MailboxDeliveryQueueRetryInterval" value="00:03:00" />
    <add key="MaxIdleTimeBeforeResubmit" value="6:00:00" />
    ```

3. When you are finished save and close the EdgeTransport.exe.config file.

4. Restart the Microsoft Exchange Transport service by running the following command:

    ```powershell
    net stop MSExchangeTransport && net start MSExchangeTransport
    ```

## Configure the transient failure retry attempts, the transient failure retry interval, and the outbound connection failure retry interval

The transient failure retry attempts specifies the number of connection attempts that are tried after the connection attempts controlled by the `QueueGlitchRetryCount` and `QueueGlitchRetryInterval` keys have failed. The default number of transient failure retry attempts is 6. The valid input range for this parameter is from 0 through 15. If you set the number of transient failure retry attempts to 0, the next connection attempt is controlled by the *outbound connection failure retry interval*.

The transient failure retry interval specifies the interval between each connection attempt that's specified by the number of transient failure retry attempts. In the Transport service on a Mailbox server, the default transient failure retry interval is 5 minutes. On an Edge Transport server, the default transient failure retry interval is 10 minutes.

The outbound connection failure retry interval specifies the retry interval for outgoing connection attempts that have previously failed. The previously failed connection attempts are controlled by the transient failure retry attempts and the transient failure retry interval. The default value for the outbound connection failure retry interval in the Transport service on a Mailbox server is 10 minutes. The default value on an Edge Transport server is 30 minutes.

## Use the EAC to configure the transient failure retry attempts, the transient failure retry interval, or the outbound connection failure retry interval

1. In the Exchange admin center (EAC), click **Servers** \> **Servers**, select the server, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"), and then click **Transport limits**.

2. In the **Retries** section, enter a value for **Outbound connection failure retry interval (seconds)**, the **Transient failure retry interval (minutes)**, or the **Transient failure retry attempts**.

3. When you are finished, click **Save**.

## Use the Shell to configure the transient failure retry attempts, the transient failure retry interval, and the outbound connection failure retry interval

Use the following syntax to configure the transient failure retry attempts, the transient failure retry interval, and the outbound connection failure retry interval in the Transport service on a Mailbox server or on an Edge Transport server.

```powershell
Set-TransportService <ServerIdentity> -TransientFailureRetryCount <Integer> -TransientFailureRetryInterval <hh:mm:ss> -OutboundConnectionFailureRetryInterval <dd.hh:mm:ss>
```

This example changes the following values on the Mailbox server named Mailbox01: on the Edge Transport server Exchange01.

- The number of transient failure retry attempts is set to 8.

- The transient failure retry interval is set to 1 minute.

- The outbound connection failure retry interval is set to 45 minutes.

```powershell
Set-TransportService Mailbox01 -TransientFailureRetryCount 8 -TransientFailureRetryInterval 00:01:00 -OutboundConnectionFailureRetryInterval 00:45:00
```

> [!NOTE]
> The <EM>TransientFailureRetryCount</EM> and <EM>TransientFailureRetryInterval</EM> parameters are also available on the <STRONG>Set-FrontEndTransportService</STRONG> cmdlet for the Front End Transport service on Client Access servers.

## Use the Shell to configure the message retry interval

By default, the message retry interval is `00:15:00` or 15 minutes. We recommend that you don't modify the default value unless Microsoft Customer Service and Support advises you to do this.

Use the following syntax to set the message retry interval.

```powershell
Set-TransportService <ServerIdentity> -MessageRetryInterval <dd.hh:mm:ss>
```

This example changes the message retry interval to 20 minutes on the Mailbox server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -MessageRetryInterval 00:20:00
```

## Configure the delay DSN timeout settings

You can use the EAC or the Shell to configure the delay DSN notification timeout interval. This setting is applied to the local transport server only. You can only use the Shell to enable or disable the sending of delay DSN messages to internal and external senders. These setting are applied to all transport servers in your organization.

> [!NOTE]
> On Exchange 2007 Hub Transport servers, all <EM>ExternalDSN*</EM> and <EM>InternalDSN*</EM> parameters are available on the <STRONG>Set-TransportServer</STRONG> cmdlet, not the <STRONG>Set-TransportConfig</STRONG> cmdlet. If you have any Exchange 2007 Hub Transport servers in your organization, you need to make changes to these values using the <STRONG>Set-TransportServer</STRONG> cmdlet on each Exchange 2007 Hub Transport server.

## Use the EAC to configure the delay DSN message notification timeout interval

1. In the EAC, click **Servers** \> **Servers**, select the server, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"), and then click **Transport limits**.

2. In the **Notifications** section, enter a value for **Notify sender when message is delayed after (hours)**.

3. When you are finished, click **Save**.

## Use the Shell to configure the delay DSN message notification timeout interval

Use the following syntax to set the message retry interval.

```powershell
Set-TransportService <ServerIdentity> -DelayNotificationTimeout <dd.hh:mm:ss>
```

This example changes the delay DSN message notification timeout interval to 6 hours on the Mailbox server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -DelayNotificationTimeout 06:00:00
```

## Use the Shell to enable or disable the sending of delay DSN notifications to external or internal message senders

Use the following syntax to configure the delay DSN notification settings.

```powershell
Set-TransportConfig -ExternalDelayDSNEnabled <$true | $false> -InternalDelayDSNEnabled <$true |$false>
```

This example prevents the sending of delay DSN notification messages to external senders.

```powershell
Set-TransportConfig -ExternalDelayDSNEnabled $false
```

This example prevents the sending of delay DSN notification messages to internal senders.

```powershell
Set-TransportConfig -InternalDelayDSNEnabled $false
```

## Configure the message expiration timeout interval

## Use the EAC to configure the message expiration timeout interval

1. In the EAC, click **Servers** \> **Servers**, select the server, click **Edit** ![Edit icon](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon"), and then click **Transport limits**.

2. In the **Message expiration** section, enter a value for **Maximum time since submission (days)**.

3. When you are finished, click **Save**.

## Use the Shell to configure the message expiration timeout interval

To configure the message expiration timeout interval, use the following syntax.

```powershell
Set-TransportService <ServerIdentity> -MessageExpirationTimeout <dd.hh:mm:ss>
```

This example changes the message expiration timeout interval to 4 days on the Exchange server named Mailbox01.

```powershell
Set-TransportService Mailbox01 -MessageExpirationTimeout 4.00:00:00
```
