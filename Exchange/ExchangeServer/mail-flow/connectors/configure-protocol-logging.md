---
localization_priority: Normal
description: 'Summary: Learn how to configure protocol logging for Send connectors and Receive connectors in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c81cac9c-b990-492a-b899-5be8d08a6068
ms.reviewer: 
title: Configure protocol logging
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure protocol logging

Protocol logging records the SMTP conversations that occur between messaging servers and between Exchange services in the transport pipeline as part of message delivery.

The following options are available for the protocol logs of all Send connectors and Receive connectors on the Exchange server:

- Specify the location of the protocol log files. The default locations are:

  - Front End Transport service on Mailbox servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\FrontEnd\ProtocolLog\SmtpSend`

  - Transport service on Mailbox servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Hub\ProtocolLog\SmtpSend`

  - Mailbox Transport Delivery service on Mailbox servers (**Receive connectors**): `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpReceive\Delivery`

  - Mailbox Transport Submission service on Mailbox servers (**Send connectors**): `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Submission`

    **Note**: Protocol logging for side effect messages that are submitted after messages are delivered to mailboxes occurs in `%ExchangeInstallPath%TransportRoles\Logs\Mailbox\ProtocolLog\SmtpSend\Delivery`. For example, a message that's delivered to a mailbox triggers an Inbox rule that redirects the message to another recipient.

  - Transport service on Edge Transport servers:

  - **Receive connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpReceive`

  - **Send connectors**: `%ExchangeInstallPath%TransportRoles\Logs\Edge\ProtocolLog\SmtpSend`

- Specify a maximum size for the protocol log files. The default size is 10 megabytes (MB).

- Specify a maximum size for the protocol log files. The default size is 250 MB.

- Specify a maximum age for the protocol log files. The default age is 30 days.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport Service", "Front End Transport service", "Mailbox Transport service", "Receive connectors" and "Send connectors" entries in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- You can use the Exchange admin center (EAC) to enable or disable protocol logging for Receive connectors and Send connectors on Mailbox servers. You can also use the EAC to configure the protocol log paths for the Transport service only. For all other protocol logging options, you need to use the Exchange Management Shell. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You enable or disable protocol logging on each individual connector. You configure other protocol logging options for all Receive connectors or all Send connectors that affect each individual transport service on the Exchange server. All Receive connectors in a transport service share the same protocol log files and protocol log options. These files and options are separate from the Send connector protocol log files and protocol log options in the same transport service.

  > [!CAUTION]
  > Don't perform this procedure on an Edge Transport server that has been subscribed to the Exchange organization by using EdgeSync. Instead, make the changes in the Transport service on the Mailbox server. The changes are then replicated to the Edge Transport server the next time EdgeSync synchronization occurs.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure protocol logging

### Use the EAC to enable or disable protocol logging on a connector

Use this procedure to enable or disable protocol logging on a Send connector or a Receive connector in the Transport service on Mailbox servers, or a Receive connector in the Front End Transport service on Mailbox servers.

1. Open the EAC and navigate to one of the following locations:

   - **Mail flow** \> **Send connectors**.

   - **Mail flow** \> **Receive connectors**.

2. Select the connector you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the **General** tab in the **Protocol logging level** section, select one of the following options:

   - **None**: Protocol logging disabled on the connector.

   - **Verbose**: Protocol logging is enabled on the connector.

4. When you're finished, click **Save**.

### Use the EAC to configure the location of the protocol logs on an Exchange server

Use this procedure to configure the location of the protocol logs for all Send connectors or all Receive connectors in the Transport service on Mailbox servers.

1. Open the EAC and navigate to **Servers** \> **Servers**.

2. Select the Mailbox server you want to configure, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

3. On the server properties page, click **Transport logs**. In the **Protocol log** section, change the following settings:

   - **Send protocol log path**

   - **Receive protocol log path**

   Specify a location on the local Exchange server. If the folder doesn't exist, it's created when you click **Save**.

4. When you're finished, click **Save**.

### How do you know this worked?

To verify that you have successfully used the EAC to configure protocol logging, browse to the location that you specified for the Send connector or the Receive connector protocol logs. If you enabled protocol logging, verify that a log file exists, and that the file is being updated for the connector. If you disabled protocol logging, verify that the latest log file is no longer being updated for the connector.

## Use the Exchange Management Shell to enable or disable protocol logging on a connector

### Use the Exchange Management Shell to enable or disable protocol logging on a Send connector or a Receive connector

Use this procedure to enable or disable protocol logging on:

- A Send connector or a Receive connector in the Transport service on Mailbox servers.

- A Receive connector in the Front End Transport service on Mailbox servers.

- A Send connector or a Receive connector in the Transport service on Edge Transport servers.

To enable or disable protocol logging on a Send connector or a Receive connector, use the following syntax in the Exchange Management Shell:

```PowerShell
<Set-SendConnector | Set-ReceiveConnector> <ConnectorIdentity> -ProtocolLoggingLevel <Verbose | None>
```

This example enables protocol logging for the Receive connector named Connection from Contoso.com on the server named Mailbox01.

```PowerShell
Set-ReceiveConnector "Mailbox01\Connection from Contoso.com" -ProtocolLoggingLevel Verbose
```

This example disables protocol logging for the Send connector named Connection to Internet.

```PowerShell
Set-ReceiveConnector "Connection to Internet" -ProtocolLoggingLevel None
```

### Use the Exchange Management Shell to enable or disable protocol logging on the intra-organization Send connector

Use this procedure to enable or disable protocol logging on the implicit and invisible intra-organization Send connector that exists in the Transport service, the Front End Transport service, and the Mailbox Transport Submission service on Mailbox servers. For more information about these connectors, see [Implicit Send connectors](send-connectors.md#implicit-send-connectors).

Protocol logging for the intra-organization Send connector occurs in the Send connector protocol logs for the specified transport service. Note that the Transport service setting controls protocol logging for the intra-organization Send connector in the Transport service and in the Mailbox Transport Submission service.

To enable or disable protocol logging on the intra-organization Send connector, use the following syntax in the Exchange Management Shell:

```PowerShell
<Set-TransportService | Set-FrontEndTransportService> <ServerIdentity> -IntraOrgConnectorProtocolLoggingLevel <Verbose | None>
```

This example enables protocol logging on the intra-organization Send connector in the Transport service and in the Mailbox Transport Submission service on the server named Mailbox01.

```PowerShell
Set-TransportService Mailbox01 -IntraOrgConnectorProtocolLoggingLevel Verbose
```

This example disables protocol logging on the intra-organization Send connector in the Front End Transport service on the same server.

```PowerShell
Set-FrontEndTransportService Mailbox01 -IntraOrgConnectorProtocolLoggingLevel None
```

### Use the Exchange Management Shell to enable or disable protocol logging on the mailbox delivery Receive connector

Use this procedure to enable or disable protocol logging on the implicit and invisible mailbox delivery Receive connector that exists in the Mailbox Transport Delivery service. Protocol logging for this connector occurs in the Receive connector protocol logs for the Mailbox Transport Delivery service. For more information about this connector, see [Implicit Receive connectors in the Mailbox Transport Delivery service on Mailbox servers](receive-connectors.md#implicit-receive-connectors-in-the-mailbox-transport-delivery-service-on-mailbox-servers).

To enable or disable protocol logging on the mailbox delivery Receive connector, use the following syntax in the Exchange Management Shell:

```PowerShell
Set-MailboxTransportService <ServerIdentity> -MailboxDeliveryConnectorProtocolLoggingLevel <Verbose | None>
```

This example enables protocol logging on the mailbox delivery Receive connector on the server named Mailbox01.

```PowerShell
Set-MailboxTransportService Mailbox01 -MailboxDeliveryConnectorProtocolLoggingLevel Verbose
```

This example disables protocol logging on the mailbox delivery Receive connector on the same server.

```PowerShell
Set-MailboxTransportService Mailbox01 -MailboxDeliveryConnectorProtocolLoggingLevel None
```

### How do you know this worked?

To verify that you have successfully used the Exchange Management Shell to enable or disable protocol logging on a connector, perform the following steps:

1. Run the following command in the Exchange Management Shell to verify whether protocol logging is enabled or disabled for all connectors on the Exchange server:

   ```PowerShell
   Write-Host "Send Connectors:" -ForegroundColor yellow; Get-SendConnector | Format-List Name,ProtocolLoggingLevel; Write-Host "Receive Connectors:" -ForegroundColor yellow; Get-ReceiveConnector | Format-List Name,TransportRole,ProtocolLoggingLevel; Write-Host "Mailbox Transport Delivery service:" -ForegroundColor yellow; Get-MailboxTransportService | Format-List *ProtocolLoggingLevel; Write-Host "Front End Transport service:" -ForegroundColor yellow; Get-FrontEndTransportService | Format-List *ProtocolLoggingLevel; Write-Host "Transport service and Mailbox Transport Submission service:" -ForegroundColor yellow; Get-TransportService | Format-List *ProtocolLoggingLevel
   ```

2. Browse to the location of the protocol log. If you enabled protocol logging, verify that a log file exists, and that the file is being updated for the connector. If you disabled protocol logging, verify that the latest log file is no longer being updated for the connector.

## Use the Exchange Management Shell to configure the protocol log settings on an Exchange server

Use this procedure to configure the protocol log settings for all Send connectors or Receive connectors in a transport service on a Mailbox server, and in the Transport service on an Edge Transport server.

To configure the protocol log settings in the Exchange Management Shell, use the following syntax:

```PowerShell
<Set-FrontEndTransportService | Set-MailboxTransportService | Set-TransportService> <ServerIdentity> -ReceiveProtocolLogPath <LocalFilePath> -ReceiveProtocolLogMaxFileSize <Size> -ReceiveProtocolLogMaxDirectorySize <Size> -ReceiveProtocolLogMaxAge <dd.hh:mm:ss> -SendProtocolLogPath <LocalFilePath> -SendProtocolLogMaxFileSize <Size> -SendProtocolLogMaxDirectorySize <Size> -SendProtocolLogMaxAge <dd.hh:mm:ss>
```

This example sets the following protocol log settings in the Transport service on the server named Mailbox01:

- Sets the location of protocol log for all Receive connectors to D:\Hub SMTP Receive Log and the location for all Send connectors to D:\Hub SMTP Send Log. If the folder doesn't exist, it's created for you.

- Sets the maximum size of a connector protocol log file for Receive connectors and Send connectors to 20 MB.

- Sets the maximum size of the connector protocol log folder for Receive connectors and Send connectors to 400 MB.

- Sets the maximum age of a protocol log file for Receive connectors and Send connectors to 45 days.

```PowerShell
Set-TransportService Mailbox01 -ReceiveProtocolLogPath "D:\Hub SMTP Receive Log" -ReceiveProtocolLogMaxFileSize 20MB -ReceiveProtocolLogMaxDirectorySize 400MB -ReceiveProtocolLogMaxAge 45.00:00:00 -SendProtocolLogPath "D:\Hub SMTP Send Log" -SendProtocolLogMaxFileSize 20MB -SendProtocolLogMaxDirectorySize 400MB -SendProtocolLogMaxAge 45.00:00:00
```

 **Notes**:

- Setting the _SendProtocolLogPath_ or _ReceiveProtocolLogPath_ parameters to the value `$null` effectively disables protocol logging for all Send connectors or Receive connectors on the server. However, setting the value to `$null` generates event log errors when protocol logging is enabled for any Send connector or Receive connector on the server, including the intra-organization Send connector or the mailbox delivery Receive connector.

- Setting the _ReceiveProtocolLogMaxAge_ or _SendProtocolLogMaxAge_ parameters to the value `00:00:00` prevents the automatic removal of protocol log files because of their age.

### How do you know this worked?

To verify that you have successfully used the Exchange Management Shell to configure the protocol logging settings on an Exchange server, perform the following steps:

1. Run the following command in the Exchange Management Shell and verify the protocol log settings on the Exchange server:

   ```PowerShell
   Write-Host "Front End Transport service:" -ForegroundColor yellow; Get-FrontEndTransportService | Format-List ReceiveProtocolLog*,SendProtocolLog*; Write-Host "Mailbox Transport Submission and Mailbox Transport Delivery services:" -ForegroundColor yellow; Get-MailboxTransportService | Format-List ReceiveProtocolLog*,SendProtocolLog*; Write-Host "Transport service:" -ForegroundColor yellow; Get-TransportService | Format-List ReceiveProtocolLog*,SendProtocolLog*
   ```

2. Open the location of the protocol log in Windows Explorer or File Explorer to verify that the log files exist, that data is being written to the files, and that the files are being recycled based on the maximum file size and maximum directory size values that you configured.
