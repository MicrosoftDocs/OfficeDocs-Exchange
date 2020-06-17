---
localization_priority: Normal
description: 'Summary: Learn how to configure message tracking in Exchange Server 2016 or Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 50eb5213-cf27-4179-b427-38d751ee4a70
ms.reviewer: 
title: Configure message tracking
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure message tracking

Message tracking records the message activity as mail flows through the transport pipeline on Mailbox servers and Edge Transport servers. You can use message tracking logs for message forensics, mail flow analysis, reporting, and troubleshooting.

You use the **Set-TransportService** cmdlet in the Exchange Management Shell on Mailbox servers and Edge Transport servers for all message tracking configuration tasks. For example:

- Enable or disable message tracking. The default is enabled.

- Specify the location of the message tracking log files. The default location is `%ExchangeInstallPath%TransportRoles\Logs\MessageTracking`.

- Specify a maximum size for the individual message tracking log files. The default is 10 MB.

- Specify a maximum size for the directory that contains the message tracking log files: The default is 1000 MB.

- Specify maximum age for the message tracking log files: The default is 30 days.

- Enable or disable message subject logging in the message tracking logs. The default is enabled.

> [!NOTE]
> On Mailbox servers, you can also use the Exchange admin center (EAC) to enable or disable message tracking, and to specify the location of the message tracking log files.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes

- To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" entries in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to configure message tracking on Mailbox servers

1. Open the EAC and navigate to **Servers** \> **Servers** \> select the Mailbox server that you want to configure \> and click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.png).

2. On the server properties page, click **Transport Logs**. In the **Message tracking log** section, change any of the following settings:

   - **Enable message tracking log**: To disable message tracking on the server, clear the check box. To enable message tracking on the server, select the check box.

   - **Message tracking log path**: The value you specify must be on the local Exchange server. If the folder doesn't exist, it's created for you when you click **Save**.

3. When you're finished, click **Save**.

## Use the Exchange Management Shell to configure message tracking

As previously explained, you can use the **Set-TransportService** cmdlet to perform all message tracking configuration tasks on Mailbox servers and Edge Transport servers. To configure message tracking in the Exchange Management Shell, use the following syntax:

```powershell
Set-TransportService [<ServerIdentity>] -MessageTrackingLogEnabled <$true | $false> -MessageTrackingLogMaxAge <dd.hh:mm:ss> -MessageTrackingLogMaxDirectorySize <Size> -MessageTrackingLogMaxFileSize <Size> -MessageTrackingLogPath <LocalFilePath> -MessageTrackingLogSubjectLoggingEnabled <$true | $false>
```

Note that you don't need to specify the Exchange server when you run the command on the server that you want to configure.

This example configures the following message tracking log settings on the server named Mailbox01:

- Sets the location of the message tracking log files to D:\Message Tracking Log. Note that if the folder doesn't exist, it's created for you.

- Sets the maximum size of a message tracking log file to 20 MB.

- Sets the maximum size of the message tracking log directory to 1.5 GB.

- Sets the maximum age of a message tracking log file to 45 days.

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogPath "D:\Message Tracking Log" -MessageTrackingLogMaxFileSize 20MB -MessageTrackingLogMaxDirectorySize 1.5GB -MessageTrackingLogMaxAge 45.00:00:00
```

> [!NOTE]
> • Setting the _MessageTrackingLogPath_ parameter to the value `$null`, effectively disables message tracking. However, if the value of the _MessageTrackingLogEnabled_ parameter is `$true`, event log errors are generated. <br/><br/>• Setting the _MessageTrackingLogMaxAge_ parameter to the value `00:00:00` prevents the automatic removal of message tracking log files because of their age. <br/><br/>• The maximum size of the message tracking log directory is three times the value of the _MessageTrackingLogMaxDirectorySize_ parameter. Although the message tracking log files that are generated by the four different services have four different name prefixes, the amount and frequency of data written to the moderated transport log (**MSGTRKMA**) is negligible compared to the other three logs. For more information, see [Structure of the message tracking log files](message-tracking.md#structure-of-the-message-tracking-log-files).

This example disables message subject logging in the message tracking log on the server named Mailbox01:

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogSubjectLoggingEnabled $false
```

This example disables message tracking on the Mailbox server named Mailbox01:

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogEnabled $false
```

## How do you know this worked?

To verify that you have successfully configured message tracking, run the following command in the Exchange Management Shell:

```powershell
Get-TransportService [<ServerIdentity>] | Format-List MessageTrackingLog*
```

You can also open the location of the message tracking log in Windows Explorer or File Explorer to verify that the log files exist, that data is being written to the files, and that they're being recycled based on the maximum file size and maximum directory size values that you configured.
