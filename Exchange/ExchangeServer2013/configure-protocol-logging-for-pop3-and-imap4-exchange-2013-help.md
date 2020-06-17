---
title: 'Configure protocol logging for POP3 and IMAP4: Exchange 2013 Help'
TOCTitle: Configure protocol logging for POP3 and IMAP4
ms:assetid: 451b337b-cb6b-4460-8687-be0b19c469bc
ms:mtpsurl: https://technet.microsoft.com/library/Aa997690(v=EXCHG.150)
ms:contentKeyID: 50395397
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure protocol logging for POP3 and IMAP4

_**Applies to:** Exchange Server 2013_

You can use the Shell to enable, disable, or modify protocol logging settings for POP3 and IMAP4. By default, protocol logging isn't enabled.

Protocol logging lets you review the POP3 and IMAP4 connections in your Exchange environment. This can be useful if you're troubleshooting issues related to POP3 or IMAP4 performance. For more information, see [Protocol logging for POP3 and IMAP4](protocol-logging-for-pop3-and-imap4-exchange-2013-help.md). For more information related to POP3 and IMAP4, see [POP3 and IMAP4 in Exchange Server 2013](pop3-and-imap4-in-exchange-server-2013-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "POP3 settings" and "IMAP4 settings" entries in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable protocol logging for POP3 or IMAP4

This example enables protocol logging for IMAP4 or POP3 on the Client Access server CAS01.

```powershell
Set-ImapSettings -Server "CAS01" -ProtocolLogEnabled $true
Set-PopSettings -Server "CAS01" -ProtocolLogEnabled $true
```

> [!NOTE]
> After you've changed protocol logging settings for POP3 or IMAP4, you must restart whichever services you're using: POP3 or IMAP4. For information about how to restart the POP3 and IMAP4 services, see <A href="start-and-stop-the-pop3-services-exchange-2013-help.md">Start and stop the POP3 services</A> and <A href="start-and-stop-the-imap4-services-exchange-2013-help.md">Start and stop the IMAP4 services</A>.

For detailed syntax and parameter information, see [Set-ImapSettings](https://docs.microsoft.com/powershell/module/exchange/Set-ImapSettings) and [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/Set-PopSettings).

## Use the Shell to disable protocol logging for POP3 or IMAP4

This example disables protocol logging for IMAP4 or POP3 on the Client Access server CAS01.

```powershell
Set-ImapSettings -Server "CAS01" -protocolLogEnabled $false
Set-PopSettings -Server "CAS01" -protocolLogEnabled $false
```

> [!NOTE]
> After you've changed protocol logging settings for POP3 or IMAP4, you must restart whichever services you're using: POP3 or IMAP4. For information about how to restart the POP3 and IMAP4 services, see <A href="start-and-stop-the-pop3-services-exchange-2013-help.md">Start and stop the POP3 services</A> and <A href="start-and-stop-the-imap4-services-exchange-2013-help.md">Start and stop the IMAP4 services</A>.

For detailed syntax and parameter information, see [Set-ImapSettings](https://docs.microsoft.com/powershell/module/exchange/Set-ImapSettings) and [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/Set-PopSettings).

## Use the Shell to modify protocol logging for POP3 or IMAP4

To modify POP3 or IMAP4 logging settings, run the **Set-ImapSettings** or **Set-PopSettings** cmdlets with one or more of the following parameters.

- *LogFileLocation*: This parameter specifies the location for the POP3 or IMAP4 protocol log files. By default, POP3 protocol log files are located in the C:\\Program Files\\Microsoft\\Exchange Server\\V15\\Logging\\Pop3 directory. This example turns on POP3 protocol logging on the Client Access server CAS01. It also changes the POP3 protocol logging directory to C:\\Pop3Logging.

  ```powershell
  Set-PopSettings -Server "CAS01" -ProtocolLogEnabled $true -LogFileLocation "C:\Pop3Logging"
  ```

- *LogFileRollOverSettings*: This parameter defines how frequently POP3 or IMAP4 protocol logging creates a new log file. By default, a new log file is created every day. The possible values are:

  Hourly

  Daily

  Weekly

  Monthly

  This setting applies only when the value for the parameter *LogPerFileSizeQuota* is set to zero. This example changes the POP3 protocol logging on the Client Access server CAS01 to create a new log file every hour.

  ```powershell
  Set-PopSettings -Server "CAS01" -LogPerFileSizeQuota 0 -LogFileRollOverSettings Hourly
  ```

- *LogPerFileSizeQuota*: This parameter defines the maximum size of a POP3 or IMAP4 protocol log file in bytes. By default, this value is set to zero. When this value is set to zero, a new protocol log file is created at the frequency specified by the *LogFileRollOverSettings* parameter.

  This example changes the POP3 protocol logging on the Client Access server CAS01 to create a new log file when a log file reaches 2 megabytes (MB).

  ```powershell
  Set-PopSettings -Server "CAS01" -LogPerFileSizeQuota 2000000
  ```

  This example changes the POP3 protocol logging on the Client Access server CAS01 to use the same log file regardless of its creation date and size.

  ```powershell
  Set-PopSettings -Server "CAS01" -LogPerFileSizeQuota unlimited
  ```

> [!NOTE]
> After you've changed protocol logging settings for POP3 or IMAP4, you must restart whichever services you're using: POP3 or IMAP4. For information about how to restart the POP3 and IMAP4 services, see <A href="start-and-stop-the-pop3-services-exchange-2013-help.md">Start and stop the POP3 services</A> and <A href="start-and-stop-the-imap4-services-exchange-2013-help.md">Start and stop the IMAP4 services</A>.

For detailed syntax and parameter information, see [Set-ImapSettings](https://docs.microsoft.com/powershell/module/exchange/Set-ImapSettings) and [Set-PopSettings](https://docs.microsoft.com/powershell/module/exchange/Set-PopSettings).

## How do you know this worked?

Run the following command in the Shell to verify POP3 protocol logging settings. If POP3 protocol logging is enabled, the value for the *ProtocolLogEnabled* parameter is `True`. If POP3 protocol logging is disabled, the value is `False`. You can also make sure the values for the *LogFileLocation*, *LogPerFileSizeQuota*, and *LogFileRollOverSettings* parameters are correct.

```powershell
Get-PopSettings | format-list
```

Run the following command in the Shell to verify IMAP4 protocol logging settings. If IMAP4 protocol logging is enabled, the value for the *ProtocolLogEnabled* parameter is `True`. If IMAP4 protocol logging is disabled, the value is `False`. You can also make sure the values for the *LogFileLocation*, *LogPerFileSizeQuota*, and *LogFileRollOverSettings* parameters are correct.

```powershell
Get-ImapSettings | format-list
```

## For more information

After you configure protocol logging settings for POP3 and IMAP4, you may also want to:

[Start and stop the IMAP4 services](start-and-stop-the-imap4-services-exchange-2013-help.md)

[Start and stop the POP3 services](start-and-stop-the-pop3-services-exchange-2013-help.md)
