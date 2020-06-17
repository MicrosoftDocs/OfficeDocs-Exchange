---
description: 'Summary: Learn about address lists and global address lists (GALs) how administrators can use them to organize recipients in Exchange Server 2016 and Exchange Server 2019.'
localization_priority: Normal
ms.author: v-mapenn
ms.topic: article
author: mattpennathe3rd
ms.prod: exchange-server-it-pro
ms.assetid: 8ee2672a-3a45-4897-8cc0-fa23c374dbf9
ms.collection: exchange-server
ms.reviewer:
manager: serdars
f1.keywords:
- NOCSH
audience: ITPro
title: Address lists in Exchange Server

---

# Address lists in Exchange Server

An *address list* is a collection of mail-enabled recipient objects from Active Directory. Address lists are based on recipient filters, and are basically unchanged from Exchange 2010. You can filter by recipient type (for example, mailboxes and mail contacts), recipient properties (for example, Company or State or Province), or both. Address lists aren't static; they're updated dynamically. When you create or modify recipients in your organization, they're automatically added to the appropriate address lists. These are the different types of address lists that are available:

- **Global address lists (GALs)**: The built-in GAL that's automatically created by Exchange includes every mail-enabled object in the Active Directory forest. You can create additional GALs to separate users by organization or location, but a user can only see and use one GAL.

- **Address lists**: Address lists are subsets of recipients that are grouped together in one list, which makes them easier to find by users. Exchange comes with several built-in address lists, and you can create more based on you organization's needs.

- **Offline address books (OABs)**: OABs contain address lists and GALs. OABs are used by Outlook clients in cached Exchange mode to provide local access to address lists and GALs for recipient look-ups. For more information, see [Offline address books in Exchange Server](../../email-addresses-and-address-books/offline-address-books/offline-address-books.md).

Users in your organization use address lists and the GAL to find recipients for email messages. Here's an example of what address lists look like in Outlook 2016:

![Global Address List (GAL)](../../media/54c5aa4b-fbd3-4b37-8642-9a52b9558641.png)

For procedures related to address lists, see [Procedures for address lists in Exchange Server](address-list-procedures.md).

## Recipient filters for address lists

Recipient filters identify the recipients that are included in address lists and GALs. There are two basic options: **precanned recipient filters** and **custom recipient filters**. These are basically the same recipient filtering options that are used by dynamic distribution groups and email address policies. The following table summarizes the differences between the two filtering methods.

****

|**Recipient filtering method**|**User interface**|**Filterable recipient properties**|**Filter operators**|
|:-----|:-----|:-----|:-----|
|Precanned recipient filters|**Address lists**: Exchange admin center (EAC) and the Exchange Management Shell <br/><br/> **GALs**: Exchange Management Shell only|Limited to: <br/>• Recipient type (All recipient types or any combination of user mailboxes, resource mailboxes, mail contacts, mail users, and groups) <br/>• Company <br/>• Custom Attribute 1 to 15 <br/>• Department <br/>• State or Province|Property values require an exact match. Wildcards and partial matches aren't supported. For example, "Sales" doesn't match the value "Sales and Marketing". <br/><br/> Multiple values of the same property always use the **or** operator. For example, "Department equals Sales or Department equals Marketing". <br/><br/> Multiple properties always use the **and** operator. For example, "Department equals Sales and Company equals Contoso".|
|Custom recipient filters|Exchange Management Shell only|You can use virtually any available recipient attributes. For more information, see [Filterable Properties for the -RecipientFilter Parameter](https://docs.microsoft.com/powershell/exchange/recipientfilter-properties).|You use OPATH filter syntax to specify any available Windows PowerShell filter operators. Wildcards and partial matches are supported.|

 **Notes**:

- You can't used precanned filters and customized filters at the same time.

- The recipient's location in Active Directory (the organizational unit or container) is available in both precanned and custom recipient filters.

- If an address list uses custom recipient filters instead of precanned filters, you can see the address list in the EAC, but you can't modify or remove it by using the EAC.

- You can hide recipients from all address lists and GALs. For more information, see [Hide recipients from address lists](address-list-procedures.md#hide-recipients-from-address-lists).

## Global address lists

By default, a new installation of Exchange Server creates an GAL named Default Global Address List that's the primary repository of all recipients in the Exchange organization. Typically, most organizations have only one GAL, because users can only see and use one GAL in Outlook and Outlook on the web (formerly known as Outlook Web App). You might need to create multiple GALs if you want to prevent groups of recipients from seeing each other (for example, you single Exchange organization contains two separate companies). If you plan on creating additional GALs, consider the following issues:

- You can only use the Exchange Management Shell to create, modify, remove, and update GALs.

- The GAL that users see in Outlook and Outlook on the web is named Global Address List, even though the default GAL is named Default Global Address List, and any new GALs that you create will require a unique name (users can't tell which GAL that they're using by name).

- Users can only see a GAL that they belong to (the recipient filter of the GAL includes them). If a user belongs to multiple GALs, they'll still see only one GAL based on the following conditions:

  - The user needs permissions to view the GAL. You assign user permissions to GALs by using address book policies (ABPs). For more information, see [Address book policies in Exchange Server](../../email-addresses-and-address-books/address-book-policies/address-book-policies.md).

  - If a user is still eligible to see multiple GALs, only the largest GAL is used (the GAL that contains the most recipients).

  - Each GAL needs a corresponding offline address book (OAB) that includes the GAL. To create OABs, see [Use the Exchange Management Shell to create offline address books](../offline-address-books/oab-procedures.md#use-the-exchange-management-shell-to-create-offline-address-books).

## Default address lists

By default, Exchange comes with five built-in address lists and one GAL. These address lists are described in the following table. Note that by default, system-related mailboxes like arbitration mailboxes and public folder mailboxes are hidden from address lists.

|**Name**|**Type**|**Description**|**Recipient filter used**|
|:-----|:-----|:-----|:-----|
|All Contacts|Address list|Includes all mail contacts in the organization. To learn more about mail contacts, see [Recipients](../../recipients/recipients.md).|`"Alias -ne $null -and (ObjectCategory -like 'person' -and ObjectClass -eq 'contact')"`|
|All Distribution Lists|Address list|Includes all distribution groups and mail-enabled security groups in the organization. To learn more about mail-enabled groups, see [Recipients](../../recipients/recipients.md).|`"Alias -ne $null -and ObjectCategory -like 'group'"`|
|All Rooms|Address list|Includes all room mailboxes. Equipment mailboxes aren't included. To learn more about room and equipment (resource) mailboxes, see [Recipients](../../recipients/recipients.md).|`"Alias -ne $null -and (RecipientDisplayType -eq 'ConferenceRoomMailbox' -or RecipientDisplayType -eq 'SyncedConferenceRoomMailbox')"`|
|All Users|Address list|Includes all user mailboxes, linked mailboxes, remote mailboxes (Office 365 mailboxes), shared mailboxes, room mailboxes, equipment mailboxes, and mail users in the organization. To learn more about these recipient types, see [Recipients](../../recipients/recipients.md).|`"((Alias -ne $null) -and (((((((ObjectCategory -like 'person') -and (ObjectClass -eq 'user') -and (-not(Database -ne $null)) -and (-not(ServerLegacyDN -ne $null)))) -or (((ObjectCategory -like 'person') -and (ObjectClass -eq 'user') -and (((Database -ne $null) -or (ServerLegacyDN -ne $null))))))) -and (-not(RecipientTypeDetailsValue -eq 'GroupMailbox')))))"`|
|Default Global Address List|GAL|Includes all mail-enabled recipient objects in the organization (users, contacts, groups, dynamic distribution groups, and public folders.|`"((Alias -ne $null) -and (((ObjectClass -eq 'user') -or (ObjectClass -eq 'contact') -or (ObjectClass -eq 'msExchSystemMailbox') -or (ObjectClass -eq 'msExchDynamicDistributionList') -or (ObjectClass -eq 'group') -or (ObjectClass -eq 'publicFolder'))))"`|
|Public Folders|Address list|Includes all mail-enabled public folders in your organization. Access permissions determine who can view and use public folders. For more information about public folders, see [Public folders](../../collaboration/public-folders/public-folders.md).|`"Alias -ne $null -and ObjectCategory -like 'publicFolder'"`|

## Custom address lists

An Exchange organization might contain thousands of recipients, so the built-in address lists could become quite large. To prevent this, you can create custom address lists to help users find what they're looking for.

For example, consider a company that has two large divisions in one Exchange organization:

- Fourth Coffee, which imports and sells coffee beans.

- Contoso, Ltd, which underwrites insurance policies.

For most day-to-day activities, employees at Fourth Coffee don't communicate with employees at Contoso, Ltd. Therefore, to make it easier for employees to find recipients who exist only in their division, you can create two new custom address lists: one for Fourth Coffee and one for Contoso, Ltd. However, if an employee is unsure about where recipient exists, they can search in the GAL, which contains all recipients from both divisions.

You can also create address lists under other address lists. For example, you can create an address list that contains all recipients in Manchester, and you can create another address list under Manchester named Sales that contains only sales people in the Manchester office. You can also move address lists back to the root, or under other address lists after you've created them. For more information, see [Use the Exchange Management Shell to move address lists](address-list-procedures.md#use-the-exchange-management-shell-to-move-address-lists).

## Best practices for creating additional address lists

Although address lists are useful tools for users, poorly planned address lists can cause frustration. To make sure that your address lists are practical for users, consider the following best practices:

- Address lists should make it easier for users to find recipients.

- Avoid creating so many address lists that users can't tell which list to use.

- Use a naming convention and location hierarchy for your address lists so users can immediately tell what the list is for (which recipients are included in the list). If you have difficulty naming your address lists, create fewer lists and remind users that they can find anyone in your organization by using the GAL.

For detailed instructions about creating address lists in Exchange Server, see [Create address lists](address-list-procedures.md#create-address-lists).

## Update address lists

After you create or modify an address list, you need to update the membership.

If the address list contains a large number of recipients (our recommendation is more than 3000), you should use the Exchange Management Shell to update the address list (not the EAC). For more information, see [Update address lists](address-list-procedures.md#update-address-lists).

To update a GAL, you always need to use the Exchange Management Shell. For more information, see [Use the Exchange Management Shell to update global address lists](address-list-procedures.md#use-the-exchange-management-shell-to-update-global-address-lists).
