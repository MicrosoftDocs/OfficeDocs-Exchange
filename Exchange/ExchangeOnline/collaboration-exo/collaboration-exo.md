---
localization_priority: Normal
description: Office 365 and Exchange Online provides several features that can help your end users easily collaborate in email.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 7d433daa-c68f-4441-b2f5-1280991185c2
ms.reviewer: 
title: Collaboration in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Collaboration in Exchange Online

Office 365 and Exchange Online provides several features that can help your end users easily collaborate in email.

Each of these features, described in the following sections, has a different user experience and feature set and should be used based on what your users need to accomplish and what your organization can provide.

This topic compares these collaboration features to help you decide which features to offer your users.

## Public folders
<a name="PFs"> </a>

Public folders are designed for shared access and provide an easy and effective way to collect, organize, and share information with other people in your workgroup or organization.

Public folders organize content in a deep hierarchy that's easy to browse. Users discover interesting and relevant content by browsing through branches of the hierarchy that are relevant to them. Users always see the full hierarchy in their Outlook folder view. Public folders are a great technology for distribution group archiving. A public folder can be mail-enabled and added as a member of the distribution group. Email sent to the distribution group is automatically added to the public folder for later reference. Public folders also provide simple document sharing and don't require SharePoint to be installed in your organization. Finally, end users can use public folders with the following supported Outlook clients: Outlook 2010 or later and Outlook on the web (formerly known as Outlook Web App), but with some limitations.

To learn more, see [Public folders in Office 365 and Exchange Online](public-folders/public-folders.md).

## Shared mailboxes
<a name="Shared"> </a>

A shared mailbox is a mailbox that multiple designated users can access to read and send email messages and to share a common calendar. Shared mailboxes can provide a generic email address (such as info@contoso.com or sales@contoso.com) that customers can use to inquire about your company. If the shared mailbox has the Send As permission assigned when a delegated user responds to the email message, it can appear as though the mailbox (for example, sales@contoso.com) is responding, not the actual user.

To learn more, see [Shared mailboxes in Exchange Online](shared-mailboxes.md).

## Groups
<a name="Groups"> </a>

Groups (also called distribution groups) are a collection of two or more recipients that appears in the shared address book. When an email message is sent to a group, it's received by all members of the group. Distribution groups can be organized by a particular discussion subject (such as "Dog Lovers") or by users who share a common work structure that requires them to communicate frequently.

To learn more, see [Recipients in Exchange Online](../recipients-in-exchange-online/recipients-in-exchange-online.md).

## Which one to use?
<a name="Groups"> </a>

The following table gives you a quick glance at each of the collaboration features to help you decide which one to use.

||**Public folders**|**Shared mailboxes**|**Groups**|
|:-----|:-----|:-----|:-----|
|**Type of group**|With the proper permissions, everyone in your organization can access and search public folders. Public folders are ideal for maintaining history or distribution group conversations.|Delegates working on behalf of a virtual identity, and they can respond to email as that shared mailbox identity. Example: support@tailspintoys.com|Users who need to send email to a group of recipients with a common interest or characteristic.|
|**Ideal group size**|Large|Small<sup>1</sup>|Large|
|**Access**|Accessible by anyone in your organization.|Users can be granted Full Access and/or Send As permissions. If granted Full Access permissions, users must also add the shared mailbox to their Outlook profile to access the shared mailbox.|For distribution groups, members must be manually added. For dynamic distribution groups, members are added based on filtering criteria.|
|**Shared calendar?**|Yes|Yes|No|
|**Email arrives in user's personal Inbox?**|No. Email arrives in the public folder.|No. Email arrives in the Inbox of the shared mailbox.|Yes. Email arrives in the Inbox of a distribution group member.|
|**Supported clients**|Outlook 2010 or later <br/> Outlook on the web <br/>|Outlook 2010 or later <br/> Outlook on the web|Outlook 2010 or later <br/> Outlook on the web|

> [!NOTE]
> <sup>1</sup> Depending on workload, the ideal group size may be very small. If more than a few users need to access a Shared mailbox at the same time, consider one of the other options.

