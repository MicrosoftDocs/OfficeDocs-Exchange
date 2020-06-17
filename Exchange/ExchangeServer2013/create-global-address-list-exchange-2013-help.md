---
title: 'Create a global address list: Exchange 2013 Help'
TOCTitle: Create a global address list
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 59e4955a-8999-4d17-be9f-23a41a23b929
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create a global address list in Exchange 2013

_**Applies to:** Exchange Server 2013_

The global address list (GAL) is a directory that contains entries for every group, user, and contact within an organization's implementation of Microsoft Exchange. If your organization uses address book policies, you may want to create additional GALs. To learn more, see [Address book policies](address-book-policies-exchange-2013-help.md).

For additional management tasks related to address lists, see [Address list procedures](address-list-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address lists" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to create a GAL using conditional filter properties

This example creates a GAL named GAL_Contoso that includes recipients who are mailbox users and have their company listed as Contoso.

```powershell
New-GlobalAddressList -Name "GAL_Contoso" -IncludedRecipients MailboxUsers -ConditionalCompany Contoso
```

> [!NOTE]
> If you're using precanned conditional filter properties, the _IncludedRecipients_ parameter can't be blank.

For detailed syntax and parameter information, see [New-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/new-globaladdresslist).

## Use the Shell create a GAL using a recipient filter

This example creates a GAL named GAL_AgencyA that includes recipients for which the _CustomAttribute15_ parameter has a value of `AgencyA`.

```powershell
New-GlobalAddressList -Name "GAL_AgencyA" -RecipientFilter "CustomAttribute15 -like 'AgencyA'"
```

For detailed syntax and parameter information, see [New-GlobalAddressList](https://docs.microsoft.com/powershell/module/exchange/new-globaladdresslist).
