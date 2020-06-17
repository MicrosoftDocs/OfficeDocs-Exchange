---
localization_priority: Normal
description: Learn how to modify the connection response that messaging servers receive after connecting to an Exchange server 2016 or 2019.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d667704e-fd69-4aca-9c35-eef7006944b2
ms.reviewer: 
title: Modify the SMTP banner on Receive connectors
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Modify the SMTP banner on Receive connectors

The *SMTP banner* is the initial SMTP connection response that a messaging server receives after it connects to an Exchange server. Specifically, the messaging server connects to a Receive connector that's configured on the Exchange server. For Exchange Mailbox servers, external messaging servers connect through Receive connectors that are configured in the Front End Transport service. The default Receive connector that's configured to accept anonymous SMTP connections is named Default Frontend _\<ServerName\>_. For Edge Transport servers, the default Receive connector in the Transport service named Default internal receive connector _\<ServerName\>_\> is configured to accept anonymous SMTP connections. For more information, see [How messages from external senders enter the transport pipeline](../mail-flow.md#how-messages-from-external-senders-enter-the-transport-pipeline) and [Default Receive connectors created during setup](receive-connectors.md#default-receive-connectors-created-during-setup).

By default, the connection response looks like this:

 `220 <ServerName> Microsoft ESMTP MAIL service ready at <RegionalDay-Date-24HourTimeFormat><RegionalTimeZoneOffset>`

Here are some reasons that you might want to modify the default SMTP banner:

- You don't want Exchange or the internal Exchange server name disclosed in the connection response to external messaging servers.

- You want the connection response to include your domain name to satisfy antispam or reverse DNS to SMTP banner checks.

- You want the connection response to include the name of the Receive connector to make it easier to troubleshoot connection problems.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- The replacement SMTP banner text string must always start with `220` (the default "Service ready" SMTP response code is 220).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Receive connectors" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to modify the SMTP banner on a Receive connector

Use the following syntax:

```PowerShell
Set-ReceiveConnector -Identity <ConnectorIdentity> -Banner "220 <Banner Text>"
```

This example changes the SMTP banner on the Receive connector named Default Frontend Mailbox01 to the value 220 contoso.com.

```PowerShell
Set-ReceiveConnector -Identity "Default Frontend Mailbox01" -Banner "220 consoso.com"
```

This example removes the custom SMTP banner, which returns the SMTP banner to the default value.

```PowerShell
Set-ReceiveConnector -Identity "Default Frontend Mailbox01" -Banner $null
```

## How do you know this worked?

To verify that you have successfully modified the SMTP banner on a Receive connector, do these steps:

1. Open a Telnet client on a computer that can access the Receive connector, and run the following command:

   ```
   open <Connector FQDN or IP address><TCPPort>
   ```

2. Verify the that response contains the SMTP banner you configured.

Note that this procedure only works on Receive connectors that allow anonymous or Basic authentication. For more information, see [Use Telnet to test SMTP communication on Exchange servers](../../mail-flow/test-smtp-with-telnet.md).
