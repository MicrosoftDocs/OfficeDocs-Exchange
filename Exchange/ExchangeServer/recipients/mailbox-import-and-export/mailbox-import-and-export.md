---
description: 'Summary: Learn about how administrators can import and export mailboxes to .pst files, and .pst files to mailboxes in Exchange Server 2016 and Exchange Server 2019.'
localization_priority: Normal
ms.author: chrisda
ms.topic: article
author: chrisda
ms.prod: exchange-server-it-pro
ms.assetid: 157a7d88-d3aa-4056-9a50-df67451b14be
ms.collection: exchange-server
ms.date: 7/5/2018
ms.reviewer: 
manager: dansimp
audience: ITPro
title: Mailbox imports and exports in Exchange Server

---

# Mailbox imports and exports in Exchange Server

Exchange Server uses the Microsoft Exchange Mailbox Replication service (MRS) to import .pst files to mailboxes, and export mailboxes to .pst files. The advantages of using MRS instead of Outlook to import and export mailboxes are:

- Import and export requests are asynchronous (you can import and export multiple .pst files at the same time).

- Imports and exports take advantage of the queuing and throttling that's provided by the MRS.

- You can import a .pst file directly to a user's archive mailbox.

- The source or destination .pst files can reside on any network share that's accessible by your Exchange servers.

This feature was introduced in Exchange 2010 Service Pack 1 (SP1). In Exchange 2010, the MRS runs on Client Access servers. In Exchange 2013 or later, the MRS runs in the backend services on Mailbox servers (not in the frontend Client Access services).

> [!NOTE]
> Mailbox imports and exports are available only in the Mailbox Import Export role, and by default, that role isn't assigned to a role group. To use these features, you need to add the Mailbox Import Export role to a role group that you belong to (for example, the Organization Management role group). For more information, see [Add a role to a role group](../../permissions/role-groups.md#add-a-role-to-a-role-group).

## Reasons to import or export mailboxes
<a name="Reasons"> </a>

As an administrator, you might need to import .pst files to mailboxes or export mailboxes to .pst files. For example:

- **Compliance requirements**: You can export a mailbox to a .pst file for legal discovery purposes. After the export is complete, you can import the .pst file to a mailbox that's specifically used for compliance purposes.

- **Create a point-in-time snapshot of a mailbox**: Suppose you're keeping a backup of an entire mailbox database for just a few mailboxes. By exporting those mailboxes to .pst files, you can eliminate the mailbox database backup.

- **Get content out of .pst files and into mailboxes**: Typically, Outlook users can save their email messages locally in .pst files. You can import users' .pst files to their primary mailboxes or archive mailboxes. This is an easy method for transferring email from a user's local computer to an Exchange server.

## Considerations
<a name="Pre"> </a>

Before you import .pst files to mailboxes, or export mailboxes to .pst files, consider these issues:

- You need to use a UNC network share (\\ _\<Server\>_\ _\<Share\>_\ or \\ _\<LocalServerName\>_\c$\). The Exchange Trusted Subsystem security group requires permissions to the network share (Read for imports, Read/Write for exports). If the share doesn't have these permissions, you'll get errors when you try to import or export .pst files.

- We recommend that you don't try to import or export .pst files that are larger than 50 gigabytes (GB), because 50 GB is the maximum .pst file size that's supported by current versions of Outlook. You can export mailboxes that are larger than 50 GB to .pst files by using multiple export requests that include or exclude specific folders, or by using a content filter.

- The operations may take several hours depending on the size of the .pst files or mailboxes, the available network bandwidth, and MRS throttling.

- You can't import .pst files to public folders.

## Import .pst files to mailboxes
<a name="Imp"> </a>

Here are some things to consider when you import .pst files to mailboxes:

- You can create new mailbox import requests in the EAC or the Exchange Management Shell. To view, modify, suspend, resume, or remove mailbox import requests, you need to use the Exchange Management Shell.

- You can import the .pst file to a different mailbox. For example, you can export data from john@contoso.com and import it to legaldiscovery@contoso.com.

- You can import the .pst file directly to the user's personal archive instead of their primary mailbox.

- By default, associated messages are imported if they exist in the .pst file. Associated messages are special messages that contain hidden data with information about rules, views, and forms. You can change this setting in the Exchange Management Shell (the _AssociatedMessagesCopyOption_ parameter).

- By default, the Recoverable Items folder is imported if it exists in the .pst file. You can change this setting in the Exchange Management Shell (the _ExcludeDumpster_ switch).

- In the Exchange Management Shell, you can include or exclude specific folders to import (the _IncludeFolders_, _ExcludeFolders_, or _SourceRootFolder_ parameters).

- In the Exchange Management Shell, you can specify the destination folder for imported items in the target mailbox (the _TargetRootFolder_ parameter).

- In the Exchange Management Shell, you can increase or decrease the priority value for mailbox import requests (the _Priority_ parameter).

For mailbox import procedures, see [Procedures for mailbox imports from .pst files in Exchange Server](import-procedures.md).

## Export mailboxes to .pst files
<a name="Exp"> </a>

Here are some things to consider when you export mailboxes to .pst files:

- You can create new mailbox export requests in the EAC or the Exchange Management Shell. To view, modify, suspend, resume, or remove mailbox export requests, you need to use the Exchange Management Shell.

- You can export a mailbox or a user's archive mailbox to a .pst file.

- By default, associated messages are exported from the mailbox. Associated messages are special messages that contain hidden data with information about rules, views, and forms. You can change this setting in the Exchange Management Shell (the _AssociatedMessagesCopyOption_ parameter).

- By default, the Recoverable Items folder is exported from the mailbox. You can change this setting in the Exchange Management Shell (the _ExcludeDumpster_ switch).

- In the Exchange Management Shell, you can filter the messages to export from the mailbox (the _ContentFilter_ parameter). You can filter by message content, attachment, senders, recipients, Inbox category, importance, message type, message size, and when the message was sent, received, or expired.

- In the Exchange Management Shell, you can include or exclude specific folders to export (the _IncludeFolders_, _ExcludeFolders_, or _SourceRootFolder_ parameters).

- In the Exchange Management Shell, you can specify the destination folder for exported items in the target .pst file (the _TargetRootFolder_ parameter).

- In the Exchange Management Shell, you can increase or decrease the priority value for mailbox export requests (the _Priority_ parameter).

For mailbox export procedures, see [Procedures for mailbox exports to .pst files in Exchange Server](export-procedures.md).
