---
title: 'Manage attachment filtering on Edge Transport servers: Exchange 2013 Help'
TOCTitle: Manage attachment filtering on Edge Transport servers
ms:assetid: 2ec91cc6-6ade-48ee-88bb-66153874393d
ms:mtpsurl: https://technet.microsoft.com/library/Aa997139(v=EXCHG.150)
ms:contentKeyID: 61200281
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage attachment filtering on Edge Transport servers

_**Applies to:** Exchange Server 2013_

Attachment filtering is provided by the Attachment Filter agent that's available only on Edge Transport servers. Attachment filtering can help prevent files that are attached in email messages from entering your organization. You can configure one or more attachment filter entries to filter attachments either by content type or by file name.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-spam features" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) and the "Transport agents" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- Configuration changes that you make to attachment filtering on an Edge Transport server are made only to the local computer. If you have multiple Edge Transport servers in your perimeter network, you need to configure attachment filtering on each Edge Transport server separately.

- You can only use the Shell to perform this procedure.

- When you disable attachment filtering and restart the Microsoft Exchange Transport service, all attachment filtering features stop working.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable attachment filtering

When you enable or disable the Attachment Filtering agent, the change takes effect after you restart the Microsoft Exchange Transport service. When you restart the Microsoft Exchange Transport service on an Edge Transport server, mail flow on the server is temporarily interrupted.

To disable attachment filtering, run the following command:

```powershell
Disable-TransportAgent "Attachment Filtering Agent"
```

To enable attachment filtering, run the following command:

```powershell
Enable-TransportAgent "Attachment Filtering Agent"
```

After you enable or disable attachment filtering, restart the Microsoft Exchange Transport service by running the following command:

```powershell
Restart-Service MSExchangeTransport
```

## How do you know this worked?

To verify that you successfully enabled or disabled attachment filtering, do the following:

1. Run the following command:

    ```powershell
    Get-TransportAgent "Attachment Filtering Agent"
    ```

2. If the value of **Enabled** is `True`, attachment filtering is enabled. If the value is `False`, attachment filtering is disabled.

## Use the Shell to view attachment filtering entries

Attachment filtering entries define the message attachments that you want to keep out of your organization. To view the attachment filtering entries that are used by the Attachment Filtering agent, run the following command:

```powershell
Get-AttachmentFilterEntry | Format-Table
```

To view a specific MIME content type entry, use the following syntax:

```powershell
Get-AttachmentFilteringEntry ContentType:<MIMEContentType>
```

For example, to view the content type entry for JPEG images, run the following command:

```powershell
Get-AttachmentFilteringEntry ContentType:image/jpeg
```

To view a specific file name or file name extension entry, use the following syntax:

```powershell
Get-AttachmentFilteringEntry FileName:<FileName or FileNameExtension>
```

For example, to view the file name extension entry for JPEG attachments, run the following command:

```powershell
Get-AttachmentFilteringEntry FileName:*.jpg
```

## Use the Shell to add attachment filtering entries

To add an attachment filtering entry that filters attachments by MIME content type, use the following syntax:

```powershell
Add-AttachmentFilterEntry -Name <MIMEContentType> -Type ContentType
```

The following example adds a MIME content type entry that filters JPEG images.

```powershell
Add-AttachmentFilterEntry -Name image/jpeg -Type ContentType
```

To add an attachment filtering entry that filters attachments by file name or file name extension, use the following syntax:

```powershell
Add-AttachmentFilterEntry -Name <FileName or FileNameExtension> -Type FileName
```

The following example filters attachments that have the .jpg file name extension.

```powershell
Add-AttachmentFilterEntry -Name *.jpg -Type FileName
```

## How do you know this worked?

To verify that you successfully added an attachment filtering entry, do the following:

1. Run the following command to verify that the filtering entry exists.

    ```powershell
    Get-AttachmentFilterEntry | Format-Table
    ```

2. Send a test message that contains a prohibited attachment from an external mailbox to an internal recipient and verify that the message is rejected, stripped, or deleted.

## Use the Shell to remove attachment filtering entries

To remove an attachment filtering entry that filters attachments by MIME content type, use the following syntax:

```powershell
Remove-AttachmentFilterEntry ContentType:<ContentType>
```

The following example removes the MIME content type entry for JPEG images.

```powershell
Remove-AttachmentFilterEntry ContentType:image/jpeg
```

To remove an attachment filtering entry that filters attachments by file name or file name extension, use the following syntax:

```powershell
Remove-AttachmentFilterEntry FileName:<FileName or FileNameExtension>
```

The following example removes the file name entry for the .jpg file name extension.

```powershell
Remove-AttachmentFilterEntry FileName:*.jpg
```

## How do you know this worked?

To verify that you successfully removed an attachment filtering entry, do the following:

1. Run the following command to verify that the filtering entry was removed.

    ```powershell
    Get-AttachmentFilterEntry | Format-Table
    ```

2. Send a test message that contains an allowed attachment from an external mailbox to an internal recipient and verify that the message was successfully delivered with the attachment.

## Use the Shell to view the attachment filtering action

To view the attachment filtering action that's used when a prohibited attachment is detected in a message, run the following command:

```powershell
Get-AttachmentFilterListConfig
```

## Use the Shell to configure the attachment filtering action

To configure the attachment filtering action that will be used when a prohibited attachment is detected in a message, use the following syntax:

```powershell
Set-AttachmentFilterListConfig [-Action <Reject | Strip | SilentDelete>] [-RejectResponse "<Message text>"] [-AdminMessage "<Replacement file text>"] [-ExceptionConnectors <ConnectorGUID>]
```

This example makes the following changes to the attachment filtering configuration:

- Reject (block) messages that have prohibited attachments.

- Use a custom response for rejected messages.

```powershell
Set-AttachmentFilterListConfig -Action Reject -RejectResponse "This message contains a prohibited attachment. Your message can't be delivered. Please resend the message without the attachment."
```

For more information, see [Set-AttachmentFilterListConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AttachmentFilterListConfig).

## How do you know this worked?

To verify that you successfully configured the attachment filtering action, send a test message that contains a prohibited attachment from an external mailbox to an internal recipient and verify that the message and the attachment are processed as you expect.
