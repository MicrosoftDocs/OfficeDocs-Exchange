---
localization_priority: Normal
description: This topic describes the recipient filter options that admins can use in custom address lists and global address lists (GALs) in Exchange Online.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 8eabea64-97c6-40af-b61c-9b6a125cbdf1
ms.reviewer: 
title: Recipient filters for address lists in Exchange Online PowerShell
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Recipient filters for address lists in Exchange Online PowerShell

Recipient filters identify the recipients that are included in address lists and GALs. There are two basic options: **precanned recipient filters** and **custom recipient filters**. These are basically the same recipient filtering options that are used by dynamic distribution groups and email address policies.

- **Precanned recipient filters**

  - Uses the required _IncludedRecipient_ parameter with the `AllRecipients` value *or* one or more of the following values: `MailboxUsers`, `MailContacts`, `MailGroups`, `MailUsers`, or `Resources`. You can specify multiple values separated by commas.

  - You can also use any of the optional _Conditional_ filter parameters: _ConditionalCompany_, _ConditionalCustomAttribute[1to15]_, _ConditionalDepartment_, and _ConditionalStateOrProvince_.

   You specify multiple values for a _Conditional_ parameter by using the syntax `"<Value1>","<Value2>"...`. Multiple values of the same property implies the **or** operator. For example, "Department equals Sales or Marketing or Finance".

- **Custom recipient filters**: Uses the required _RecipientFilter_ parameter with an OPATH filter.

  - The basic OPATH filter syntax is `"<Property1> -<Operator> '<Value1>' <Property2> -<Operator> '<Value2>'..."`.

  - Double quotation marks `" "` are required around the whole OPATH filter. Although the filter is a string (not a system block), you can also use braces `{ }`, but only if the filter doesn't contain variables that require expansion.

  - Hyphens (`-`) are required before all operators. Here are some of the most frequently used operators:

  - `and`, `or`, and `not`.

  - `eq` and `ne` (equals and does not equal; not case-sensitive).

  - `lt` and `gt` (less than and greater than).

  - `like` and `notlike` (string contains and does not contain; requires at least one wildcard in the string. For example, `"Department -like 'Sales*'"`.

  - Use parentheses to group `<Property> -<Operator> '<Value>'` statements together in complex filters. For example, `"(Department -like 'Sales*' -or Department -like 'Marketing*') -and (Company -eq 'Contoso' -or Company -eq 'Fabrikam')"`. Exchange stores the filter in the **RecipientFilter** property with each individual statement enclosed in parentheses, but you don't need to enter them that way.

  - For more information, see [Additional OPATH syntax information](https://docs.microsoft.com/powershell/exchange/recipient-filters#additional-opath-syntax-information).

For more information about address lists, see [Address lists in Exchange Online](address-lists.md).

For address list procedures that use recipient filters, see [Address list procedures in Exchange Online](address-list-procedures.md).
