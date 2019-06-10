---
localization_priority: Normal
description: 'Summary: Important considerations to read before you deploy public folders in your organization.'
ms.topic: reference
author: msdmaguire
ms.author: dmaguire
ms.assetid: 2e416eed-b88f-45db-a482-1232fd2610fa
ms.date: 7/6/2018
ms.reviewer: 
title: Considerations when deploying public folders
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Considerations when deploying public folders

Although there are many advantages to using Exchange public folders, there are some things to consider before implementing them in your organization.

## Deployment considerations for public folders

This article contains factors to consider before you deploy public folders in your organization, especially if you plan to have a large number of public folders. Exchange Server supports up to one million public folders.

- Activity in a public folder directly impacts the load that's placed on the public folder mailbox where the folder is located. To avoid client connectivity issues, such as high latency or the inability to access a public folder, we recommend you do the following:

  - Don't let public folder mailboxes exceed 50% of the mailbox size limit. If this happens consider using the `Split-PublicFolderMailbox.ps1` script located in C:\Program Files\Microsoft\Exchange Server\V15\Scripts folder on the Exchange server to move some public folders to a new public folder mailbox.

  - Consider moving heavily-used public folders to a dedicated public folder mailbox.

  - Exclude heavily-used public folders from serving public folder hierarchy. You can do this by setting the _IsExcludedFromServingHierarchy_ property on the public folder mailbox using the **Set-Mailbox** cmdlet.

  - For large organizations with many public folders, consider adding additional public folder mailboxes to distribute the load of servicing public folder hierarchy requests.

- Place the primary public folder mailbox in a DAG to improve availability of the mailbox. The primary public folder mailbox is the authoritative copy of the public folder hierarchy.

- Place secondary public folder mailboxes in a DAG or back up the mailboxes frequently.

- Place public folder mailboxes in the geographical location that's nearest the users that will access the public folder content in them.

- Improve public folder hierarchy access times by using the DefaultPublicFolderMailbox property on the users' mailboxes to specify a public folder mailbox close to them. This will prevent those users from retrieving the public folder hierarchy from a public folder mailbox in other geographical locations.

- In deployments with more than 50 secondary public folder mailboxes, we recommend that you don't store public folder content in the primary public folder mailbox. This dedicates the primary public folder mailbox to synchronizing the hierarchy with the secondary public folder mailboxes.

- Exchange 2016 doesn't support public folder databases. As a result, Outlook on the web users will not be able to access Exchange 2010 public folders. Exchange 2016 users can access Exchange 2010 public folders with Outlook or Outlook for Mac.

- Outlook on the web is supported, but with limitations. You can add and remove public folders from your Favorites and perform item-level operations such as creating, editing, deleting posts, and replying to posts. However, you can't create or delete public folders from Outlook on the web. Also, only Mail, Post, Calendar, and Contact public folders can be added to the Favorites list in Outlook on the web.

- Although a full text search of public folder content is available, public folder content isn't searchable across public folders and the content isn't indexed by Exchange Search.

- You must use Outlook 2010 or later to access public folders on Exchange servers.

- Retention policies aren't supported for public folder mailboxes.
