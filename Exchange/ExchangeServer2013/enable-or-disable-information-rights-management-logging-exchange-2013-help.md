---
title: 'Enable or Disable Information Rights Management Logging: Exchange 2013 Help'
TOCTitle: Enable or Disable Information Rights Management Logging
ms:assetid: 6933bc65-4d98-4878-9167-0e9eaac68b6b
ms:mtpsurl: https://technet.microsoft.com/library/Ff686962(v=EXCHG.150)
ms:contentKeyID: 49319919
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or Disable Information Rights Management Logging

_**Applies to:** Exchange Server 2013_

In Exchange Server 2013, you can use Information Rights Management (IRM) logs to monitor and troubleshoot IRM operations. IRM logging is enabled by default.

IRM logs use the following common set of parameters:

- *IrmLogEnabled*: Enables or disables IRM logging. Default: `$true`.

- *IrmLogMaxAge*: Specifies the maximum age of IRM log files. Files older than the specified age are deleted. Default: 30 days.

- *IrmLogMaxDirectorySize*: Specifies the maximum size of the directory that contains IRM logs. When a directory reaches its maximum file size, the server deletes the oldest log files first. Default: 250 MB.

- *IrmLogMaxFileSize*: Specifies the maximum size of each IRM log file. When a log file reaches the specified size, a new log file is created. Default: 10 MB.

- *IrmLogPath*: Specifies the location of the IRM log directory. Default: `%ExchangeInstallPath%Logging\IRMLogs`.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2-5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Configure IRM logging" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to enable or disable IRM logging on a server. You must use the Shell

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable IRM logging on a server

This example enables IRM log on a Mailbox server.

```powershell
Set-TransportService -Identity EXCH01 -IRMLogEnabled $true
```

For detailed syntax and parameter information, see [Set-TransportService](https://docs.microsoft.com/powershell/module/exchange/Set-TransportService).

## Use the Shell to disable IRM logging on a server

This example disables IRM logging on a Mailbox server.

```powershell
Set-TransportService -Identity EXCH01 -IRMLogEnabled $false
```

For detailed syntax and parameter information, see [Set-TransportService](https://docs.microsoft.com/powershell/module/exchange/Set-TransportService).

## How do you know this worked?

To verify that you have successfully enabled or disbled IRM logging on a server, run the [Get-TransportService](https://docs.microsoft.com/powershell/module/exchange/Get-TransportService) cmdlet to retrieve IRM settings.

This example retrieves all IRM logging properties on the server EXCH01.

```powershell
Get-TransportService -Identity EXCH01 | Format-List IRMLog*
```
