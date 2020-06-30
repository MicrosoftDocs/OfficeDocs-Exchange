---
localization_priority: Normal
description: "Summary: Why you should or shouldn't migrate your Exchange public folders to Microsoft 365 Groups."
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: d89e727b-675a-4623-b572-260f8b44b966
ms.reviewer: 
f1.keywords:
- NOCSH
title: Migrate your public folders to Microsoft 365 Groups
ms.collection:
- Strat_EX_EXOBlocker
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Migrate your public folders to Microsoft 365 Groups

 **Summary**: Why you should or shouldn't migrate your Exchange public folders to Microsoft 365 Groups.

This article provides a comparison of public folders and Microsoft 365 Groups, and how one or the other might be the best solution for your organization. Public folders have been around as long as Exchange, whereas Groups were introduced more recently. If you want to migrate some or all of your public folders to Groups, this article describes how the process works, and provides links to the articles that walk you through the process, step by step.

## What are public folders?

[Public Folders](public-folders.md) contain different kinds of data and are organized in a hierarchical structure.

Public folders are not recommended for the following situations:

- **Archiving data**. Users with mailbox limits sometimes use public folders instead of mailboxes to archive data. This practice isn't recommended because it affects storage in public folders and undermines the goal of mailbox limits.

- **Document sharing and collaboration**. Public folders don't provide document management features, such as versioning, controlled check-in and check-out functionality, and automatic notifications of content changes.

## What are Microsoft 365 Groups?

Microsoft 365 groups let you choose a set of people who you wish to collaborate with, and then easily set up a collection of resources for those people to share. You don't have to worry about manually assigning permissions to those resources, because adding members to your group automatically gives the members the permissions they need to access the tools and resources your group provides. Groups are also the new and improved experience for those tasks that were previously handled by distribution lists and shared mailboxes.

For the full Groups story, see [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2).

## Should you migrate your public folders to Microsoft 365 Groups?

Microsoft 365 Groups is the latest collaboration offering from Microsoft, which means there are many reasons why they would be a preferable solution over public folders, a much older technology. In Outlook, for example, Groups can replace mail-enabled public folders altogether. Compiling a list of every scenario in which Microsoft 365 Groups works better than public folders is impossible, but here are the highlights:

- **Collaboration over email**. Groups in Outlook has a dedicated **Conversations** space that stores all the emails and lets users collaborate over them. The group can even be set up to receive messages from people outside the group or from outside the organization. If you're currently using mail-enabled public folders to store project-related discussions, for example, or purchase orders that need to be viewed by a team of people, using groups would be an improvement. Groups are also better for situations when you simply want to broadcast information to a set of users.

- **Collaboration over documents**. In Outlook, Groups has a dedicated **Files** tab that displays all files from the group's SharePoint team site, as well as from mail attachments. You get one view of all the files, so you don't have to go searching for them like you would in public folders. Co-authoring also becomes easier. If you're using public folders for storing files meant to be consumed by multiple people, consider migrating to Groups.

- **Shared calendar**. Upon creation every group gets a shared calendar. Any member of the group can create events on that calendar. When you favorite a group, that group's calendar can be displayed alongside your personal calendar. You can also subscribe to a group's events, in which case events created in that group appear in your personal calendar. If you're using public folders to host calendars for your team, such as a schedule or a timetable, Groups would be an improved experience.

- **Simplified permissions**. When you assign users to a group, they immediately get the permissions they need, whereas with public folders you need to manually assign the proper permissions. Members can be added as "owners" or "members." Owners have full rights in the group, including the ability to perform group management tasks. Members can also create content and edit files like owners, but members cannot delete content that they have not created. If the public folders' permissions model is too overwhelming for you and you want something simple and quick, Microsoft 365 Groups is the way to go.

- **Mobile and Web presence**. Public folders can't be accessed through mobile devices and have a limited set of functionality on the Web. Microsoft 365 Groups, on the other hand, is accessible through Outlook mobile apps and has a richer set of features on the Web. If your team is on the move and requires mobile access, then you should be using Microsoft 365 Groups.

- **Access to a wide range of Microsoft 365 or Office 365 apps**. When you create a group, you unlock access to a wide range of apps from the Microsoft 365 or Office 365 suite. You get a SharePoint team site for storing files and a plan on Planner to track your tasks. Microsoft 365 Groups is the membership service that combines elements of the entire Microsoft 365 or Office 365 suite.

While Microsoft 365 Groups offers many advantages, you should be aware of a few major differences that you'll notice after leaving the public folders experience. These are primarily:

- **Folder hierarchy**. While public folders are often used to organize content in deep rooted hierarchy, Microsoft 365 Groups has a flat structure. All emails in the group reside in the Conversations space and all the documents go into the **Files** tab. Also, you can't create sub-folders in Microsoft 365 groups.

- **Granular permission roles**. While public folders have a variety of permission roles, Microsoft 365 Groups only provides two: owner and member.

Before you move to Groups, it's also a good idea to make note of the various limits that come with creating and maintaining groups. See *How do I manage my groups?* in [Learn about Microsoft 365 Groups](https://support.microsoft.com/office/b565caa1-5c40-40ef-9915-60fdb2d97fa2) for more information.

## Migrating public folders to Microsoft 365 Groups

If you decide to switch to Microsoft 365 Groups, you can use a process known as batch migration to move your email and calendar content from your existing public folders to Groups. The specific steps for running a batch migration depends on which version of Exchange currently hosts your public folder hierarchy. At the end of this article, you will find links to instructions that walk you through the batch migration process.

> [!NOTE]
> When you finish migrating a mail-enabled public folder to a particular Microsoft 365 group, all the emails addressed to the public folder will at that point be received by the group.

Key benefits of batch migrations are:

- **Mailbox Replication Service (MRS)-based migration**. The migration process uses migration batch cmdlets. Migration to multiple groups can be triggered together in a single migration batch. There are also scripts available to assist in the migration process.

- **Supports mail and calendar public folders**. Copied emails and posts will appear as in Groups as group conversations, and copied calendar items will be visible in group calendars. Other public folder types, such as tasks and contacts, are currently not supported for this migration.

- **On-premises public folders can be migrated directly to Microsoft 365 Groups**. This migration does not require you to first move your public folders to Microsoft 365 or Office 365 and then move to Groups. The MRS data copy cmdlets read the public folder data directly from your on-premises environment and then copy the data to Microsoft 365 Groups. Note that Exchange 2010 public folders will require an Outlook Anywhere endpoint. Exchange 2013 public folders will require an MRS Proxy-based endpoint.

- **Not an "all or nothing" migration**. You get to choose specific public folders to migrate to Groups, and only those chosen public folders get migrated.

- **One-shot data copy**. Batch migrations are designed to be a simple one-time data copy from source public folders to target groups, without the complexities of incremental synchronization and finalization.

- **Merges public folder data with existing data in a group**. The data copy will merge the public folder content with the existing group's content, if any. If there is a need for incremental data copy, you can simply run the data copy as many times as you need. This will copy incremental data over to the group.

### Overview of batch migrations

The following steps outline the overall process of migrating your public folder content to Microsoft 365 Groups in a batch migration. The specific details are contained in the articles listed below.

1. **Select source**: Choose the public folders that you want to migrate. You can choose any folder containing mail or calendar content.

2. **Create target**: Create corresponding groups for your folders, with the desired configurations, such as members, privacy settings, and data classification.

3. **Copy data**: Use the migration batch cmdlets to copy data from public folders to Groups.

4. **Lock source**: Lock the public folders once you have verified the data in Groups.

5. **Cutover**: Copy any new data that has been created between steps 3 and 4.

Note that your public folders and their corresponding groups will remain online for your users during steps 1 through 3 above. After step 3, you can evaluate whether or not to proceed with the rest of the migration, based on the Groups experience and whether or not it suits your users and your organization. You can roll back your migration and resume using public folders at that point. If you do proceed with the migration, after step 5 completes, you can delete the original public folders. Even post-migration it is possible to roll back to public folders, provided you have saved your backup files from the migration process and you have not deleted your original public folders.

### Batch migration prerequisites and step-by-step instructions

The following prerequisites are required in your Exchange environment before you can run a batch migration. The specific prerequisites depend on which version of Exchange you're currently running.

1. If your public folders are on-premises, your servers need to be running one of the following versions:

   - Exchange 2010 SP3 RU8 or later

   - Exchange 2013 CU15 or later

   - Exchange 2016 CU4 or later

2. If your public folders are on-premises, you must have an Exchange Hybrid environment set up. See [Exchange Server Hybrid Deployments](https://docs.microsoft.com/exchange/exchange-hybrid) for more information.

### Migration instructions

Select the appropriate link below for step-by-step instructions on running a batch migration.

- [Use batch migration to migrate Exchange Online public folders to Microsoft 365 Groups](batch-migration-of-exchange-online-public-folders.md)

- [Use batch migration to migrate Exchange 2010 public folders to Microsoft 365 Groups](https://docs.microsoft.com/exchange/use-batch-migration-to-migrate-exchange-2010-public-folders-to-microsoft-365-groups-exchange-2013-help)

- [Use batch migration to migrate Exchange 2013 public folders to Microsoft 365 Groups](https://docs.microsoft.com/exchange/use-batch-migration-to-migrate-exchange-2013-public-folders-to-microsoft-365-groups-exchange-2013-help)

- [Use batch migration to migrate Exchange 2016 public folders to Microsoft 365 Groups](https://docs.microsoft.com/Exchange/collaboration/public-folders/batch-migration-to-microsoft-365-groups)
