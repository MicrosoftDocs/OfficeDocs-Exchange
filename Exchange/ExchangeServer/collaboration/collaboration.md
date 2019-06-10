---
localization_priority: Normal
description: 'Summary: An overview of the different collaboration methods available in Exchange 2016 and 2019: groups, site mailboxes, shared mailboxes, and public folders.'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: 3311562b-8fd3-4d66-a5f7-8e65a38c6b39
ms.date: 7/6/2018
ms.reviewer: 
title: Collaboration
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Collaboration

Exchange Server provides the following rich features that can help your end users collaborate in email:

- Site mailboxes (deprecated in SharePoint 2019)

- Public folders

- Shared mailboxes

- Distribution groups

Each of these features has a different user experience and feature set and should be used based on what the user needs to accomplish and what your organization can provide. For example, site mailboxes provide great documentation collaboration features. However site mailboxes rely on SharePoint Server, so if you aren't planning on deploying SharePoint, you should use public folders to share documents.

This topic compares these collaboration features to help you decide which features to offer your users.

## Site mailboxes

A site mailbox is functionally comprised of a SharePoint 2013 or later site membership (owners and members), shared storage through an Exchange 2016 or later mailbox for email messages, and a SharePoint site to store and share information. Essentially, site mailboxes bring Exchange email and SharePoint documents together. For users, a site mailbox serves as a central filing cabinet for the project, providing a place to file project email and documents that can be accessed and edited only by site members. In addition, site mailboxes have a specified lifecycle and are optimized to be used for projects that have set start and end dates. To fully implement site mailboxes, end users must use Outlook 2013 or later.

To learn more, see [Site mailboxes](site-mailboxes.md).

## Public folders

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization.

Public folders organize content in a deep hierarchy that's easy to browse. Users discover interesting and relevant content by browsing through branches of the hierarchy that are relevant to them. Users always see the full hierarchy in their Outlook folder view. Public folders are a great technology for distribution group archiving. A public folder can be mail-enabled and added as a member of the distribution group. Email sent to the distribution group is automatically added to the public folder for later reference. Public folders also provide simple document sharing and don't require SharePoint Server to be installed in your organization. Finally, end users can use public folders with Outlook 2007 or later.

To learn more, see [Public folders](public-folders/public-folders.md).

## Shared mailboxes

A shared mailbox is a mailbox that multiple designated users can access to read and send email messages and to share a common calendar. Shared mailboxes can provide a generic email address (such as info@contoso.com or sales@contoso.com) that customers can use to inquire about your company. If the shared mailbox has the Send As permission assigned when a delegated user responds to the email message, it can appear as though the mailbox (for example, sales@contoso.com) is responding, not the actual user.

To learn more, see [Shared mailboxes](shared-mailboxes/shared-mailboxes.md).

## Groups

Groups (also called distribution groups) are a collection of two or more recipients that appears in the shared address book. When an email message is sent to a group, it's received by all members of the group. Distribution groups can be organized by a particular discussion subject (such as "Dog Lovers") or by users who share a common work structure that requires them to communicate frequently.

To learn more, see [Recipients](../recipients/recipients.md).

## Which one to use?

The following table gives you a quick glance at each of the collaboration features to help you decide which one to use.

||**Site mailboxes**|**Public folders**|**Shared mailboxes**|**Groups**|
|:-----|:-----|:-----|:-----|:-----|
|**Type of group**|Users who work together as a team on a specific project with definitive start and end dates.|With the proper permissions, everyone in your organization can access and search public folders. Public folders are ideal for maintaining history or distribution group conversations.|Delegates working on behalf of a virtual identity, and they can respond to email as that shared mailbox identity. Example: support@tailspintoys.com|Users who need to send email to a group of recipients with a common interest or characteristic.|
|**Ideal group size**|Small|Large|Small|Large|
|**Access**|Site mailbox owners and members.|Accessible by anyone in your organization.|Users can be granted Full Access and/or Send As permissions. If granted Full Access permissions, users must also add the shared mailbox to their Outlook profile to access the shared mailbox.|For distribution groups, members, must be manually added. For dynamic distribution groups, members are added based on filtering criteria.|
|**Shared calendar?**|No|Yes|Yes|No|
|**Email arrives in user's personal Inbox?**|No. Email arrives in the site mailbox.|No. Email arrives in the public folder.|No. Email arrives in the Inbox of the shared mailbox.|Yes. Email arrives in the Inbox of a distribution group member.|
|**Supported clients**|Outlook 2013 or later  <br/> SharePoint 2013|Outlook 2007 or later|Outlook 2007 or later  <br/> Outlook Web App|Outlook 2007 or later  <br/> Outlook Web App|
