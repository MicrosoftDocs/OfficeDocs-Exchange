---
title: 'Manage sender reputation: Exchange 2013 Help'
TOCTitle: Manage sender reputation
ms:assetid: f2716bd9-e3ac-46d9-9264-4e3dabfa0f38
ms:mtpsurl: https://technet.microsoft.com/en-us/library/Bb125186(v=EXCHG.150)
ms:contentKeyID: 49248691
ms.date: 12/09/2016
ms.reviewer: 
manager: dansimp
ms.author: dmaguire
author: msdmaguire
mtps_version: v=EXCHG.150
---

# Manage sender reputation

_**Applies to:** Exchange Server 2013_

Sender reputation is provided by the Protocol Analysis agent. Sender reputation blocks messages according to various characteristics of the sender. Sender reputation relies on persisted data about the sender to determine what action, if any, to take on an inbound message.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-spam features" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- By default, anti-spam features aren't enabled in the Transport service on a Mailbox server. Typically, you only enable the anti-spam features on a Mailbox server if your Exchange organization doesn't do any prior anti-spam filtering before accepting incoming messages. For more information, see [Enable anti-spam functionality on Mailbox servers](enable-anti-spam-functionality-on-mailbox-servers-exchange-2013-help.md).

- The Protocol Analysis agent is the underlying agent for sender reputation functionality. When you disable sender reputation, the Protocol Analysis agent is still enabled.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable sender reputation

This example disables sender reputation.

```powershell
Set-SenderReputationConfig -Enabled $false
```

This example enables sender reputation.

```powershell
Set-SenderReputationConfig -Enabled $true
```

## How do you know this worked?

To verify that you have successfully enabled or disabled sender reputation, do the following:

1. Verify the Protocol Analysis agent is installed and enabled by running the following command:

    ```powershell
    Get-TransportAgent
    ```

2. Verify the sender reputation values you configured by running the following command:

    ```powershell
    Get-SenderReputationConfig | Format-List Enabled,*MailEnabled
    ```

## Use the Shell to enable or disable sender reputation for internal or external messages

By default, sender reputation is enabled for external messages, and disabled for internal messages. A message is considered external if it comes from an unauthenticated connection that's external to your Exchange organization. A message is considered internal if it comes from authenticated connection, and the sender's domain is configured as an authoritative domain in your Exchange organization.

To disable sender reputation for external messages, run the following command:

```powershell
Set-SenderReputationConfig -ExternalMailEnabled $false
```

To enable sender reputation for external messages, run the following command:

```powershell
Set-SenderReputationConfig -ExternalMailEnabled $true
```

To disable sender reputation for internal messages, run the following command:

```powershell
Set-SenderReputationConfig -InternalMailEnabled $false
```

To enable sender reputation for internal messages, run the following command:

```powershell
Set-SenderReputationConfig -InternalMailEnabled $true
```

## How do you know this worked?

To verify that you have successfully enabled or disabled sender reputation for internal and external messages, do the following:

1. Run the following command:

    ```powershell
    Get-SenderReputationConfig | Format-List Enabled,*MailEnabled
    ```

2. Verify the values displayed match the values you configured.

## Use the Shell to configure sender reputation properties

To configure the sender reputation properties, run the following command:

```powershell
Set-SenderReputationConfig -SrlBlockThreshold <Value> -SenderBlockingPeriod <Hours>
```

This example sets the sender reputation level (SRL) block threshold to 6 and configures sender reputation to add offending senders to the IP Block List for 36 hours:

```powershell
Set-SenderReputationConfig -SrlBlockThreshold 6 -SenderBlockingPeriod 36
```

## How do you know this worked?

To verify that you have successfully configured the sender reputation properties, do the following:

1. Run the following command:

    ```powershell
    Get-SenderReputationConfig
    ```

2. Verify the values displayed match the values you configured.

## Use the Shell to configure outbound access for the detection of open proxy servers

You may need to perform additional steps to allow sender reputation to traverse any firewalls that are between the Internet and the Exchange server that's running the Protocol Analysis agent. The following table lists the outbound ports that are required for sender reputation.

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Protocols</th>
<th>Ports</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>SOCKS4, SOCKS5</p></td>
<td><p>1081, 1080</p></td>
</tr>
<tr class="even">
<td><p>Wingate, Telnet, Cisco</p></td>
<td><p>23</p></td>
</tr>
<tr class="odd">
<td><p>HTTP CONNECT, HTTP POST</p></td>
<td><p>6588, 3128, 80</p></td>
</tr>
</tbody>
</table>

To configure outbound access for the detection of open proxy servers, run the following command:

```powershell
Set-SenderReputationConfig -ProxyServerName <String> -ProxyServerPort <Port> -ProxyServerType <String>
```

This example configures sender reputation to use the open proxy server named SERVER01 that uses the HTTP CONNECT protocol on port 80.

```powershell
Set-SenderReputationConfig - ProxyServerName SERVER01 -ProxyServerPort 80 -ProxyServerType HttpConnect
```

## How do you know this worked?

To verify that you have successfully configured outbound access for detection of open proxy servers, do the following:

1. Run the following command:

    ```powershell
    Get-SenderReputationConfig | Format-List ProxyServer*
    ```

2. Verify the values displayed are the values you configured.
