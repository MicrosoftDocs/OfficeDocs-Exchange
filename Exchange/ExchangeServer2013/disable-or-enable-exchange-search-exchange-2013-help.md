---
title: 'Disable or enable Exchange Search: Exchange 2013 Help'
TOCTitle: Disable or enable Exchange Search
ms:assetid: 195b25be-53fb-4215-90a5-04340d640bcc
ms:mtpsurl: https://technet.microsoft.com/library/Aa996416(v=EXCHG.150)
ms:contentKeyID: 51407260
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Disable or enable Exchange Search

_**Applies to:** Exchange Server 2013_

By default, Exchange Search is enabled for all new mailbox databases and doesn't require additional configuration. However, if you want to stop Exchange Search from indexing mailbox content, you can disable it for individual mailbox databases or for an entire Mailbox server.

> [!WARNING]
> Disabling Exchange Search impacts the functionality and performance of the full-text searches that are performed by your users using Outlook in online mode or on Windows mobile devices.<BR><A href="https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery">In-Place eDiscovery</A> also relies on Exchange Search. If you disable Exchange Search for a mailbox database or for a Mailbox server, In-Place eDiscovery searches won't return any messages from the database or server.

For additional management tasks related to Exchange Search, see [Exchange Search procedures](exchange-search-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 1 minute

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- You can enable or disable Exchange Search for servers or mailbox databases but not for individual mailbox users.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Disable or enable Exchange Search for a mailbox database

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange Search" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

> [!NOTE]
> You can't use the EAC to disable or enable Exchange Search for a mailbox database.

This command disables Exchange Search for a mailbox database named EXCH01.

```powershell
Set-MailboxDatabase "Mailbox Database (EXCH01)" -IndexEnabled $false
```

This command enables Exchange Search for a mailbox database named EXCH01.

```powershell
Set-MailboxDatabase "Mailbox Database (EXCH01)" -IndexEnabled $true
```

For detailed syntax and parameter information, see [Set-MailboxDatabase](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxDatabase).

## Disable or enable Exchange Search for a Mailbox server

To disable Exchange Search for a Mailbox server, you have to disable and stop the Microsoft Exchange Search service. Similarly, to enable Exchange Search for a Mailbox server, you have to enable and start the Microsoft Exchange Search service. You can use either the Services console or the Shell to do this.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Manage Exchange Search service on a Mailbox server" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

### Use the Services console

1. Go to **Start** \> **Administrative Tools** \> **Services**.

2. In the **Services** details pane, right-click the **Microsoft Exchange Search** service, and then select **Properties**.

3. On the **General** tab, in the **Startup type** list, select **Disabled** to disable the service or **Automatic** to start it automatically.

    > [!NOTE]
    > The startup type impacts the service the next time an attempt is made to start it, either automatically after the server is restarted or by manually starting the service. In the next step, the service is stopped or started manually.

4. Click **Stop** to stop the service or **Start** to start the service.

5. Click **OK** to save the changes.

### Use the Shell

Run the following commands to stop and disable the Microsoft Exchange Search service.

```powershell
Stop-Service MSExchangeFastSearch
```

```powershell
Set-Service MSExchangeFastSearch -StartupType Disabled
```

Run the following commands to configure the Exchange Search service to start automatically and then start the service.

```powershell
Set-Service MSExchangeFastSearch -StartupType Automatic
```

```powershell
Start-Service MSExchangeFastSearch
```
