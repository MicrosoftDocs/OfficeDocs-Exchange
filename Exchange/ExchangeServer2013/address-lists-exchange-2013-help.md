---
title: 'Address lists: Exchange 2013 Help'
TOCTitle: Address lists
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 8ee2672a-3a45-4897-8cc0-fa23c374dbf9
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Address lists in Exchange 2013

_**Applies to:** Exchange Server 2013_

An address list is a collection of recipient and other Active Directory objects. Each address list can contain one or more types of objects (for example, users, contacts, groups, public folders, and room and equipment resources). You can use address lists to organize recipients and resources, making it easier to find the recipients and resources you want. Address lists are updated dynamically. Therefore, when new recipients are added to your organization, they're automatically added to the appropriate address lists.

As shown in the following figure, client applications, such as Microsoft Outlook, display the available address lists that Exchange provides.

Address lists reside in Active Directory. Therefore, mobile users who are disconnected from the network are also disconnected from these server-side address lists. However, you can create offline address books (OABs) for users who are disconnected from the network. These OABs can be downloaded to a user's hard disk. Frequently, to conserve resources, OABs are subsets of the information in the actual address lists that reside on your servers. For more information, see [Offline address books in Exchange Online](../ExchangeOnline/address-books/offline-address-books/offline-address-books.md).

## Default address lists

When users want to use their client application to find recipient information, they can select from available address lists. Several address lists, such as the global address list (GAL), are created by default. Exchange contains the following default address lists, which are then automatically populated with new users, contacts, groups, or rooms as they're added to your organization:

- **All Contacts**: This address list contains all mail-enabled contacts in your organization. Mail-enabled contacts are those recipients who have an external email address. If you want mail-enabled contact information to be available to all users in your organization, you must include the contact in the GAL. To learn more about mail contacts, see [Recipients](recipients-exchange-2013-help.md).

- **All Distribution Lists**: This address list contains all distribution groups and mail-enabled security groups in your organization. Mail-enabled groups are lists of recipients that are created to expedite the mass sending of email messages and other information. When an email message is sent to a mail-enabled group, all members of that list receive a copy of the message. To learn more about mail-enabled groups, see [Recipients](recipients-exchange-2013-help.md).

- **All Rooms**: This address list contains all resources that have been designated as a room in your organization. Rooms are resources in your organization that can be scheduled by sending a meeting request from a client application. The user account that's associated with a room is disabled. To learn more about resource mailboxes, see [Recipients](recipients-exchange-2013-help.md).

- **All Users**: This address list contains all mail-enabled users in your organization. A mail-enabled user represents a user outside your Exchange organization. Each mail-enabled user has an external email address. All messages sent to mail-enabled users are routed to this external email address. A mail-enabled user is similar to a mail contact, except that a mail-enabled user has Active Directory logon credentials and can access resources. To learn more about mail-enabled users, see [Recipients](recipients-exchange-2013-help.md).

- **Default Global Address List**: This address list contains all mail-enabled users, contacts, groups, or rooms in the organization. During setup, Exchange creates various default address lists. The most familiar address list is the GAL. By default, the GAL contains all recipients in an Exchange organization. In other words, any mailbox-enabled or mail-enabled object in an Active Directory forest that has Exchange installed is listed in the GAL. For ease of use, the GAL is organized by name, not by email address. For more information, see [Create a global address list](create-global-address-list-exchange-2013-help.md).

- **Public Folders** This address list contains all public folders in your organization. Access permissions determine who can view and use the folders. Public folders are stored on computers running Exchange.

## Custom Address Lists

An Exchange organization can contain thousands of recipients. If you compile all your recipients in the default address lists, those lists could become quite large. To prevent this, you can create custom address lists to help users in your organization find what they are looking for more easily.

For example, consider a company that has two large divisions and one Exchange organization. One division, named Fourth Coffee, imports and sells coffee beans. The other division, Contoso, Ltd, underwrites insurance policies. For most day-to-day activities, the employees at Fourth Coffee don't communicate with the employees at Contoso, Ltd. Therefore, to make it easier for employees to find recipients who exist only in their division, you can create two new custom address lists: one for Fourth Coffee and one for Contoso, Ltd. When searching for recipients in their division, these custom address lists allow employees to select only the address list that's specific to their division. However, if an employee is unsure about the division in which the recipient exists, the employee can search within the GAL, which contains all recipients in both divisions.

You can also create subcategories of address lists called hierarchical address lists. For example, you can create an address list that contains all recipients in Manchester and another that contains all recipients in Stuttgart.

## Best Practices for Creating Address Lists

Although address lists are useful tools for users, poorly planned address lists can cause frustration. To make sure that your address lists are practical for users, consider the following best practices:

- Avoid creating so many address lists that users won't be sure which list to search for recipients.

- Address lists should make it easier for users to find addresses in the GAL.

- Name your address lists in such a way that, when users glance at them, they will know immediately which recipient types are contained in the list. If you have difficulty naming your address lists, create fewer lists and remind users that they can find anyone in your organization by using the GAL.

For detailed instructions about creating an address list in Exchange Server, see [Create an address list](create-an-address-list-exchange-2013-help.md).
