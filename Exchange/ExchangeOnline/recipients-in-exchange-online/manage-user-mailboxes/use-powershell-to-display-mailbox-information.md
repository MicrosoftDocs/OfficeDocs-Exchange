---
localization_priority: Normal
description: Admins can learn how to use Exchange Online PowerShell to display information about mailboxes in their Microsoft 365 or Office 365 organization.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: e09b354c-1e3e-4bbf-a865-035d28d1a388
ms.reviewer: 
f1.keywords:
- NOCSH
title: Use Exchange Online PowerShell to display Office 365 mailbox information
search.appverid: MET150
ms.collection:
- Ent_O365
- exchange-online
audience: Admin
ms.service: exchange-online
manager: serdars

---

# Use Exchange Online PowerShell to display Office 365 mailbox information

Admins can learn how to use Exchange Online PowerShell to display information about mailboxes in their Microsoft 365 or Office 365 organization.

To give you an idea of some of the things you can do with PowerShell in Office 365, let's take a look at user mailboxes in Exchange Online PowerShell

## Before you begin

To learn how to use remote PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-online-powershell).

## Display mailbox information with Exchange Online PowerShell

You can easily get information about a single user mailbox. For example, here's a command that returns some information about Ken Myer's mailbox:

```PowerShell
Get-Mailbox -Identity "Ken Myer"
```

This command will return something similar to this:

```PowerShell
Name      Alias       ServerName      ProhibitSendQuota
----      -----       ----------      -----------------
kenmyer   kenmyer     bn1pr02mb038    49.5 GB (53,150,220,288 bytes)
```

You can see things like Ken's alias and his mailbox size quota. But there's a lot more information that's associated with an Exchange Online mailbox than just the four properties returned by the **Get-Mailbox** cmdlet.

Here's an example command that displays all the information for a specific mailbox:

```PowerShell
Get-Mailbox -Identity "Ken Myer" | Format-List
```

The command instructs Exchange Online PowerShell to return all of the available properties for the mailbox in a list. There are about 200 different properties and property values. You can also use the **Format-List** and **Format-Table** cmdlets to return only specific property values. For example, you can also view litigation hold-related properties for Ken Myer with this command:

```PowerShell
Get-Mailbox -Identity "Ken Myer" | Format-List DisplayName, LitigationHoldEnabled, LitigationHoldDate, LitigationHoldOwner, LitigationHoldDuration
```

You can also use wildcard characters when working with the **Format-List** cmdlet. For example, all the litigation hold properties start with the letters `lit`. You can retrieve this same information by using this command:

```PowerShell
Get-Mailbox -Identity "Ken Myer" | Format-List DisplayName, Lit*
```

This command tells **Get-Mailbox** to retrieve the value of Ken's **DisplayName** property along with the values of any properties that have names that begin with the letters `lit`. Here's an example of what we get back:

```PowerShell
DisplayName            : Ken Myer
LitigationHoldEnabled  : False
LitigationHoldDate     :
LitigationHoldOwner    :
LitigationHoldDuration : Unlimited
```

You can return information about multiple mailboxes by leaving out the _Identity_ parameter. This example returns the **DisplayName** and **LitigationHoldEnabled** properties for all mailboxes:

```PowerShell
Get-Mailbox -ResultSize unlimited | Format-Table DisplayName, LitigationHoldEnabled -Auto
```

In many cases, you only want to look at a subset of your mailboxes. For example, suppose you are asked to come up with a list of all the mailboxes that have been assigned a litigation hold. You can use the **Where-Object** cmdlet in conjunction with the **Get-Mailbox** cmdlet. The **Where-Object** cmdlet needs a filter phrase to tell Exchange Online PowerShell what set of mailboxes you are interested in.

In their simplest form, filter phrases use the syntax `"<PropertyName> -<ComparisonOperator> <PropertyValue>"`.

Some commonly used comparison operators are:

- `eq` (equals; not case-sensitive)

- `ne` (does not equal; not case-sensitive)

- `gt` (greater than)

- `lt` (less than)

For a complete list of comparison operators, see [Where-Object](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/where-object).

Values for `<PropertyValue>` depend on the property, and can be values like strings, numbers, Boolean values ( `$True` or `$False`), or no value ( `$Null`). Text values with spaces require quotation marks around the value. Numerical values, Boolean values and `$Null` don't require quotation marks around the value.

Returning to our example of all the mailboxes that have been assigned a litigation hold, the filter phrase is `"LitigationHoldEnabled -eq $True"`:

- The property name is `LitigationHoldEnabled`.

- The comparison operator is `eq`.

- The property value we're looking for is `$True`.

Once you have the filter phrase, you can construct the **Where-Object** portion of the command using this syntax:

```PowerShell
Get-Mailbox -ResultSize unlimited | Where-Object {$_.<Filter Phrase>}
```

Here's the command for our example:

```PowerShell
Get-Mailbox -ResultSize unlimited | Where-Object {$_.LitigationHoldEnabled -eq $True}
```

For another example, suppose you'd like to make sure that all of your users have the junk email rule enabled. Here's a quick command to find any users who don't have that rule enabled:

```PowerShell
Get-Mailbox -ResultSize unlimited | Get-MailboxJunkEmailConfiguration | Where-Object {$_.Enabled -eq $False}
```

This is just one example. If you want to display a set of mailboxes based on a setting and can't filter on that setting in the Microsoft 365 admin center, do these steps:

1. Find the mailbox property that corresponds to the setting you're interested in by running the command `Get-Mailbox -Identity "<MailboxIdentity" | Select-Object *` to list all the properties of a mailbox. `<MailboxIdentity>` is any unique identifier for the mailbox (name, email address, alias, etc.)

2. Construct your Office 365 PowerShell command like this: `Get-Mailbox -ResultSize unlimited | Where-Object {$_.<PropertyName> -<ComparisonOperator> <PropertyValue>}`
