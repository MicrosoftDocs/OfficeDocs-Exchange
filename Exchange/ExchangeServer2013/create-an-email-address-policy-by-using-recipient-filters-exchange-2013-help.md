---
title: 'Create an email address policy by using recipient filters: Exchange 2013 Help'
TOCTitle: Create an email address policy by using recipient filters
ms:assetid: e3f446bd-1511-479c-8d87-2dfce5547c90
ms:mtpsurl: https://technet.microsoft.com/library/Bb232194(v=EXCHG.150)
ms:contentKeyID: 49289441
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an email address policy by using recipient filters

_**Applies to:** Exchange Server 2013_

You can use the Shell to create an email address policy by using recipient filters. To learn more about email address policies, see [Email address policies](email-address-policies-exchange-2013-help.md).

For additional management tasks related to email address policies, see [Email address policy procedures](email-address-policy-procedures-exchange-2013-help.md).

## What do you need to know before begin?

- Estimated time to complete: 5 minutes.

- To use the *RecipientFilter* parameter to create a custom filter, you must specify a string for the filter. The Shell uses OPath for the filtering syntax. OPath is a querying language designed to query object data sources.

    > [!IMPORTANT]
    > If you use a recipient filter to create or edit an email address policy, you can't use the Exchange admin center (EAC) to edit the email address policy. You must use the Shell. For detailed syntax and parameter information, see <A href="https://docs.microsoft.com/powershell/module/exchange/Set-EmailAddressPolicy">Set-EmailAddressPolicy</A>.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Email address policies" entry in the [Email addresses and address books](email-addresses-and-address-books-exchange-2013-help.md) topic.

- Before an SMTP address domain can be used in an email address policy, you must configure an accepted domain. For more, see [Accepted domains](accepted-domains-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!WARNING]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to create an email address policy by using recipient filters

To create an email address policy by using recipient filters, use the following syntax.

```powershell
New-EmailAddressPolicy -Name <String> -RecipientFilter <String>
```

This example creates an email address policy that applies to all executives and for which the local part of the email address consists of the first two letters of their first name and their entire last name.

```powershell
New-EmailAddressPolicy -Name 'Execs' -EnabledEmailAddressTemplates 'SMTP:%2g%s@contoso.com' -RecipientFilter "((RecipientType -eq 'UserMailbox') -and (Title -like 'executive'))"
```

For detailed syntax and parameter information, see [New-EmailAddressPolicy](https://docs.microsoft.com/powershell/module/exchange/New-EmailAddressPolicy).
