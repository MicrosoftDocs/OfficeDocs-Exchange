---
localization_priority: Normal
description: 'Summary: Learn how to configure message expiration intervals, message retries, and message resubmissions in the Transport service on a Mailbox server or on an Edge Transport server in Exchange 2016 and 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 5420124f-aa4c-4702-b493-40a9a7edb786
ms.reviewer: 
title: Configure message retry, resubmit, and expiration intervals
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure message retry, resubmit, and expiration intervals

In Exchange Server, you can configure message retry, resubmit, and expiration intervals in the Transport service on Mailbox servers and Edge Transport servers. For detailed descriptions of these settings, see [Message retry, resubmit, and expiration intervals](message-intervals.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes

- You can only use the Exchange admin center (EAC) on Mailbox servers. For more information about the EAC, see [Exchange admin center in Exchange Server](../../architecture/client-access/exchange-admin-center.md). To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" and "Edge Transport severs" entries in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use EdgeTransport.exe.config to configure the queue glitch retry count, the queue glitch retry interval, the mailbox delivery queue retry interval, and the maximum idle time before resubmit interval

- **Queue glitch retry count**: The number of connection attempts that are immediately tried when the Transport service has trouble connecting to the destination server. Typically, you don't need to modify this key unless the network is unreliable and continues to experience many accidentally dropped connections.

- **Queue glitch retry interval**: The interval between each queue glitch retry. Typically, you don't need to modify this key unless the network is unreliable and continues to experience many accidentally dropped connections.

- **Mailbox delivery queue retry interval**: How frequently a queue try to connect to the Mailbox Transport Delivery service for a destination mailbox database that can't be successfully reached.

- **Max idle time before resubmit**: How long undelivered messages in delivery queues the status of Retry wait before they're resubmitted.

To configure these intervals, you modify keys in the %ExchangeInstallPath%Bin\EdgeTransport.exe.config XML application configuration file on Mailbox servers or Edge Transport servers. Changes you save to this file are applied after you restart the Exchange Transport service. When you restart this service, mail flow on the server is temporarily interrupted.

> [!NOTE]
> Any customized Exchange or Internet Information Server (IIS) settings that you made in Exchange XML application configuration files on the Exchange server (for example, web.config files or the EdgeTransport.exe.config file) **will be overwritten** when you install an Exchange CU. Be sure save this information so you can easily re-apply the settings after the install. After you install the Exchange CU, you need to re-configure these settings.

1. In a Command prompt window on the Mailbox server or Edge Transport server, open the EdgeTransport.exe.config file in Notepad by running this command:

   ```console
   Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
   ```

2. Locate the following keys in the `<appSettings>` section.

   ```xml
   <add key="QueueGlitchRetryCount" value="<Integer>" />
   <add key="QueueGlitchRetryInterval" value="<hh:mm:ss>" />
   <add key="MailboxDeliveryQueueRetryInterval" value="<hh:mm:ss>" />
   <add key="MaxIdleTimeBeforeResubmit" value="<hh:mm:ss>" />
   ```

   This example changes the queue glitch retry count to 6, the queue glitch retry interval to 30 seconds, the mailbox delivery queue retry interval to 3 minutes, and the maximum idle time before resubmit interval to 6 hours.

   ```xml
   <add key="QueueGlitchRetryCount" value="6" />
   <add key="QueueGlitchRetryInterval" value="00:00:30" />
   <add key="MailboxDeliveryQueueRetryInterval" value="00:03:00" />
   <add key="MaxIdleTimeBeforeResubmit" value="6:00:00" />
   ```

3. When you're finished, save and close the EdgeTransport.exe.config file.

4. Restart the Exchange Transport service by running this command:

   ```console
   net stop MSExchangeTransport && net start MSExchangeTransport
   ```

### How do you know this worked?

To verify that you've configured these intervals, do these steps:

1. Open the EdgeTransport.exe.config file in Notepad by running this command:

   ```console
   Notepad %ExchangeInstallPath%Bin\EdgeTransport.exe.config
   ```

2. Verify the values of the following keys in the `<appSettings>` section.

   ```xml
   <add key="QueueGlitchRetryCount" value="<Integer>" />
   <add key="QueueGlitchRetryInterval" value="<hh:mm:ss>" />
   <add key="MailboxDeliveryQueueRetryInterval" value="<hh:mm:ss>" />
   <add key="MaxIdleTimeBeforeResubmit" value="<hh:mm:ss>" />
   ```

## Configure the transient failure retry attempts, the transient failure retry interval, and the outbound connection failure retry interval

- **Transient failure retry attempts**: The number of connection attempts that are tried after the connection attempts controlled by the _QueueGlitchRetryCount_ and _QueueGlitchRetryInterval_ keys have failed. A valid value is 0 through 15, and the default value is 6. If you set the value to 0, the next connection attempt is controlled by the outbound connection failure retry interval.

- **Transient failure retry interval**: The interval between each transient failure retry attempt. On Mailbox servers, the default value is 5 minutes. On Edge Tranport Servers, the default value is 10 minutes.

- **Outbound connection failure retry interval**: The retry interval for outgoing connection attempts that have previously failed (the transient failure retry attempts and the transient failure retry interval). On Mailbox servers, the default value is 10 minutes. On Edge Tranport Servers, the default value is 30 minutes.

### Use the EAC to configure the transient failure retry attempts, the transient failure retry interval, or the outbound connection failure retry interval on Mailbox servers

1. In the EAC, go to **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. In the server properties window that opens, click **Transport limits**.

3. In the **Retries** section, enter a value for any of these settings:

   - **Outbound connection failure retry interval (seconds)**

   - **Transient failure retry interval (minutes)**

   - **Transient failure retry attempts**

   When you're finished, click **Save**.

### Use the Exchange Management Shell to configure the transient failure retry attempts, the transient failure retry interval, and the outbound connection failure retry interval on Mailbox severs or Edge Transport servers

To configure the intervals in the Transport service on Mailbox servers or Edge Transport servers, use this syntax:

```powershell
Set-TransportService -Identity <ServerIdentity> -TransientFailureRetryCount <Integer> -TransientFailureRetryInterval <hh:mm:ss> -OutboundConnectionFailureRetryInterval <dd.hh:mm:ss>
```

To configure the intervals in the Front End Transport service on Mailbox servers, use this syntax:

```powershell
Set-FrontEndTransportService -Identity <ServerIdentity> -TransientFailureRetryCount <Integer> -TransientFailureRetryInterval <hh:mm:ss>
```

This example changes the following values on the Mailbox server named Mailbox01:

- The number of transient failure retry attempts is set to 8.

- The transient failure retry interval is set to 1 minute.

- The outbound connection failure retry interval is set to 45 minutes.

```powershell
Set-TransportService -Identity Mailbox01 -TransientFailureRetryCount 8 -TransientFailureRetryInterval 00:01:00 -OutboundConnectionFailureRetryInterval 00:45:00
```

### How do you know this worked?

To verify that you've configured these intervals, do any of these steps:

- On a Mailbox server, open the EAC and go to **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the server properties window that opens, click **Transport limits**, and verify the values in the **Retries** section.

- In the Exchange Management Shell on a Mailbox server or Edge Transport server, run this command to verify the property values:

  ```powershell
  Get-TransportService | Format-List Name,TransientFailureRetry*,OutboundConnectionFailureRetryInterval
  ```

- In the Exchange Management Shell on a Mailbox serve, run this command to verify the property values:

  ```powershell
  Get-FrontEndTransportService | Format-List Name,TransientFailureRetry*
  ```

## Use the Exchange Management Shell to configure the message retry interval

The message retry interval specifies how long to wait between sending attempts for individual messages in queues that have a status of Retry. The default value is 15 minutes, and we recommend that you don't change the default value unless you're directed to do so by Microsoft Customer Service and Support, or specific product documentation.

To configure the message retry interval, use this syntax:

```powershell
Set-TransportService -Identity <ServerIdentity> -MessageRetryInterval <dd.hh:mm:ss>
```

This example changes the message retry interval to 20 minutes on the Mailbox server named Mailbox01.

```powershell
Set-TransportService -Identity Mailbox01 -MessageRetryInterval 00:20:00
```

### How do you know this worked?

To verify that you've configured the message retry interval on a Mailbox server or Edget Transport server, run this command in the Exchange Management Shell to verify the **MessageRetryInterval** property value:

```powershell
Get-TransportService | Format-List Name,MessageRetryInterval
```

## Configure the delay DSN timeout settings

- **Delay DSN message notification timeout interval**: How long to wait before sending delay DSN messages to senders. This setting applies to the Transport service on a Mailbox server or an Edge Transport server.

 **Note**: This value should always be greater than the transient failure retry count multiplied by the transient failure retry interval (the default total is 30 minutes on a Mailbox server, and one hour on an Edge Transport server).

- **Internal and external delay DSN settings**: Specifies whether delay DSN messages can be sent to internal or external message senders (senders who are inside or outside the Exchange organization). This setting applies to the Transport service on all Mailbox servers in the organization.

### Use the EAC to configure the delay DSN message notification timeout interval on Mailbox servers

1. In the EAC, click **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. In the server properties window that opens, click **Transport limits**.

3. In the **Notifications** section, enter a value for **Notify sender when message is delayed after (hours)**, and then click **Save**.

### Use the Exchange Management Shell to configure the delay DSN message notification timeout interval on Mailbox servers or Edge Transport servers

To configure the delay DSN message notification timeout interval, use this syntax:

```powershell
Set-TransportService -Identity <ServerIdentity> -DelayNotificationTimeout <dd.hh:mm:ss>
```

This example changes the delay DSN message notification timeout interval to 6 hours on the Mailbox server named Mailbox01.

```powershell
Set-TransportService -Identity Mailbox01 -DelayNotificationTimeout 06:00:00
```

### Use the Exchange Management Shell to enable or disable the sending of delay DSN notifications to external or internal message senders

To configure the delay DSN notification settings, use this syntax:

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

### How do you know this worked?

To verify that you've configured the delay DSN timeout settings, do any of these steps:

- On a Mailbox server, open the EAC and go to **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the server properties window that opens, click **Transport limits**, and verify the **Notify sender when message is delayed after (hours)** value in the **Notifications** section.

- In the Exchange Management Shell on a Mailbox server or Edge Transport server, run these commands to verify the property values:

  ```powershell
  Get-TransportService | Format-List Name,DelayNotificationTimeout
  ```

  ```powershell
  Get-TransportConfig | Format-List *DelayDSNEnabled
  ```

## Configure the message expiration timeout interval

The message expiration timeout interval specifies how long to wait before the message expires and is returned to the sender in a non-delivery report (also known as an NDR or bounce message). This setting applies to the Transport service on a Mailbox server or an Edge Transport server.

### Use the EAC to configure the message expiration timeout interval on Mailbox servers

1. In the EAC, click **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. In the server properties window that opens, click **Transport limits**.

3. In the **Message expiration** section, enter a value for **Maximum time since submission (days)**, and then click **Save**.

### Use the Exchange Management Shell to configure the message expiration timeout interval on Mailbox servers or Edge Transport servers

To configure the message expiration timeout interval, use the following syntax.

```powershell
Set-TransportService -Identity <ServerIdentity> -MessageExpirationTimeout <dd.hh:mm:ss>
```

This example changes the message expiration timeout interval to 4 days on the Exchange server named Mailbox01.

```powershell
Set-TransportService -Identity Mailbox01 -MessageExpirationTimeout 4.00:00:00
```

### How do you know this worked?

To verify that you've configured the message expiration timeout interval, do any of these steps:

- On a Mailbox server, open the EAC and go to **Servers** \> **Servers**, select the server, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png). In the server properties window that opens, click **Transport limits**, and verify the **Maximum time since submission (days)** value in the **Message expiration** section.

- In the Exchange Management Shell on a Mailbox server or Edge Transport server, run this command to verify the **MessageExpirationTimeout** property value:

  ```powershell
  Get-TransportService | Format-List Name,MessageExpirationTimeout
  ```
