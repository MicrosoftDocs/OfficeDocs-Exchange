---
localization_priority: Normal
description: 'Summary: Learn how to configure address rewriting on an Edge Transport server in Exchange Server, and how to verify the address rewriting configuration.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 323a0b55-f921-425d-b1b0-18ad0fac315c
ms.reviewer:
title: Address rewriting procedures on Edge Transport servers
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Address rewriting procedures on Edge Transport servers

You can create address rewrite entries on Edge Transport servers that apply to a single recipient, a specific domain or subdomain, or multiple subdomains. Address rewriting can be outbound only, or inbound and outbound (bidirectional). When you create address rewrite entries, remember the following:

- Verify that the resulting email addresses are unique in your organization.

- Only literal strings are supported in the email address values.

- The wildcard character (\*) is supported only in the internal address (the addresses you want to change). Valid syntax for using the wildcard character is **\*.contoso.com**. The values **\*contoso.com** or **sales.\*.com** are not allowed.

- When you use the wildcard character, you need to configure the address rewriting as outbound only (you need to set the _OutboundOnly_ parameter to the value `$true`), and outbound only address rewriting requires that you configure the rewritten email address as a proxy address on the affected recipients.

- By default, address rewriting is bidirectional for a single recipient, or for a specific domain or subdomain (the default value for the _OutboundOnly_ parameter is `$false`).

For more information about address rewriting, see [Address rewriting on Edge Transport servers](address-rewriting.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Edge Transport servers" section in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- Be careful when you configure address rewriting. Any changes that you make are immediately applied when you run the command. Consider running the command with the _WhatIf_ parameter. For more information about the _WhatIf_ parameter, see [WhatIf and Confirm](https://docs.microsoft.com/exchange/whatif-confirm-and-validateonly-switches-exchange-2013-help).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the Exchange Management Shell to enable or disable address rewriting

To completely enable or disable address rewriting, you enable or disable the address rewriting agents. By default, the address rewriting agents on an Edge Transport server are enabled.

To disable address rewriting, run the following command:

```PowerShell
Disable-TransportAgent "Address Rewriting Inbound Agent"; Disable-TransportAgent "Address Rewriting Outbound Agent"
```

To enable address rewriting, run the following command:

```PowerShell
Enable-TransportAgent "Address Rewriting Inbound Agent"; Enable-TransportAgent "Address Rewriting Outbound Agent"
```

### How do you know this worked?

To verify that you have successfully enabled or disabled address rewriting, run the following command to verify the **Enabled** property value:

```PowerShell
Get-TransportAgent "Address Rewriting *"
```

## Use the Exchange Management Shell to view address rewrite entries

To view a summary list of all address rewrite entries, run the following command.

```PowerShell
Get-AddressRewriteEntry
```

To view details of an address rewrite entry, use the following syntax.

```PowerShell
Get-AddressRewriteEntry <AddressRewriteEntryIdentity> | Format-List
```

The following example displays the details of the address rewrite entry named Rewrite Contoso.com to Northwindtraders.com:

```PowerShell
Get-AddressRewriteEntry "Rewrite Contoso.com to Northwindtraders.com" | Format-List
```

For more information, see [Get-AddressRewriteEntry](https://docs.microsoft.com/powershell/module/exchange/get-addressrewriteentry).

## Use the Exchange Management Shell to create address rewrite entries

### Rewrite the email address for a single recipient

To rewrite the email address for a single recipient, use the following syntax:

```PowerShell
New-AddressRewriteEntry -Name "<Descriptive Name>" -InternalAddress <internal email address> -ExternalAddress <external email address> [-OutboundOnly <$true | $false>]
```

This example rewrites the email address of all messages entering and leaving the Exchange organization for joe@contoso.com. Outbound messages are rewritten so they appear to come from support@nortwindtraders.com. Inbound messages sent to support@northwindtraders.com are rewritten to joe@contoso.com for delivery to the recipient (the _OutboundOnly_ parameter is `$false` by default).

```PowerShell
New-AddressRewriteEntry -Name "joe@contoso.com to support@northwindtraders.com" -InternalAddress joe@contoso.com -ExternalAddress support@northwindtraders.com
```

### Rewrite email addresses in a single domain or subdomain

To rewrite the email addresses in a single domain or subdomain, use the following syntax:

```PowerShell
New-AddressRewriteEntry -Name "<Descriptive Name>" -InternalAddress <domain or subdomain> -ExternalAddress <domain> [-OutboundOnly <$true | $false>]
```

This example rewrites the email addresses of all messages entering and leaving the Exchange organization for the contoso.com domain. Outbound messages are rewritten so they appear to come from the fabrikam.com domain. Inbound messages sent to fabrikam.com email addresses are rewritten to contoso.com for delivery to the recipients (the _OutboundOnly_ parameter is `$false` by default).

```PowerShell
New-AddressRewriteEntry -Name "Contoso to Fabrikam" -InternalAddress contoso.com -ExternalAddress fabrikam.com
```

This example rewrites the email addresses of all messages leaving the Exchange organization for the sales.contoso.com subdomain. Outbound messages are rewritten so they appear to come from the contoso.com domain. Inbound messages sent to contoso.com email addresses aren't rewritten.

```PowerShell
New-AddressRewriteEntry -Name "sales.contoso.com to contoso.com" -InternalAddress sales.contoso.com -ExternalAddress contoso.com -OutboundOnly $true
```

### Rewrite email addresses in multiple subdomains

To rewrite the email addresses in a domain and all subdomains, use the following syntax.

```PowerShell
New-AddressRewriteEntry -Name "<Descriptive Name>" -InternalAddress *.<domain> -ExternalAddress <domain> -OutboundOnly $true [-ExceptionList <domain1,domain2...>]
```

This example rewrites the email addresses of all messages leaving the Exchange organization for the contoso.com domain and all subdomains. Outbound messages are rewritten so they appear to come from the contoso.com domain. Inbound messages sent to contoso.com recipients can't be rewritten, because a wildcard is used in the _InternalAddress_ parameter.

```PowerShell
New-AddressRewriteEntry -Name "Rewrite all contoso.com subdomains" -InternalAddress *.contoso.com -ExternalAddress contoso.com -OutboundOnly $true
```

This example is just like the previous example, except now messages sent from the legal.contoso.com and corp.contoso.com subdomains are never rewritten:

```PowerShell
New-AddressRewriteEntry -Name "Rewrite all contoso.com subdomains except legal.contoso.com and corp.contoso.com" -InternalAddress *.contoso.com -ExternalAddress contoso.com -OutboundOnly $true -ExceptionList legal.contoso.com,corp.contoso.com
```

For more information, see [New-AddressRewriteEntry](https://docs.microsoft.com/powershell/module/exchange/new-addressrewriteentry).

### How do you know this worked?

To verify that you have successfully created address rewrite entries, do the following:

1. Replace _\<AddressRewriteEntryIdentity\>_ with the name of the address rewrite entry, and run the following command to verify the property values:

   ```PowerShell
   Get-AddressRewriteEntry <AddressRewriteEntryIdentity> | Format-List
   ```

2. From a mailbox that's affected by the address rewrite entry, send a test message to an external mailbox. Verify the test message appears to originate from the rewritten email address.

3. Reply to the test message from the external mailbox. Verify the original mailbox receives the reply.

## Use the Exchange Management Shell to modify address rewrite entries

The configuration options that are available when you modify an existing address rewrite entry are identical to the configuration options when you create a new address rewrite entry.

### Modify an address rewrite entry for a single recipient

To modify an address rewrite entry that rewrites the email address of a single recipient, use the following syntax:

```PowerShell
Set-AddressRewriteEntry <AddressRewriteEntryIdentity> [-Name "<Descriptive Name>"] [-InternalAddress <internal email address>] [-ExternalAddress <external email address>] [-OutboundOnly <$true | $false>]
```

This example modifies the following properties of the address rewrite entry named "joe@contoso.com to support@nortwindtraders.com":

- Changes the external address to support@northwindtraders.net.

- Changes the name of the address rewrite entry to "joe@contoso.com to support@northwindtraders.net".

- Changes the value of _OutboundOnly_ to `$true`. Note that this change requires you to configure support@northwindtraders.net as a proxy address on Joe's mailbox.

```PowerShell
Set-AddressRewriteEntry "joe@contoso.com to support@nortwindtraders.com" -Name "joe@contoso.com to support@northwindtraders.net" -ExternalAddress support@northwindtraders.net -OutboundOnly $true
```

### Modify an address rewrite entry for a single domain or subdomain

To modify an address rewrite entry that rewrites the email addresses from a single domain or subdomain, use the following syntax.

```PowerShell
Set-AddressRewriteEntry <AddressRewriteEntryIdentity> [-Name "<Descriptive Name>"] [-InternalAddress <domain or subdomain>] [-ExternalAddress <domain>] [-OutboundOnly <$true | $false>]
```

This example changes the internal address value of the address rewrite entry named "Northwind Traders to Contoso".

```PowerShell
Set-AddressRewriteEntry "Northwindtraders to Contoso" -InternalAddress northwindtraders.net
```

### Modify an address rewrite entry for multiple subdomains

To modify an address rewrite entry that rewrites the email addresses in a domain and all subdomains, use the following syntax.

```PowerShell
Set-AddressRewriteEntry <AddressRewriteEntryIdentity> [-Name "<Descriptive Name>"] [-InternalAddress *.<domain>] [-ExternalAddress <domain>] [-ExceptionList <list of domains>]
```

To replace the existing exception list values of an address rewrite entry, use the following syntax:

```PowerShell
Set-AddressRewriteEntry <AddressRewriteEntryIdentity> -ExceptionList <domain1,domain2,...>
```

This example replaces the existing exception list for the address rewrite entry named Contoso to Northwind Traders with the values marketing.contoso.com and legal.contoso.com:

```PowerShell
Set-AddressRewriteEntry "Contoso to Northwind Traders" -ExceptionList sales.contoso.com,legal.contoso.com
```

To add or remove exception list values without affecting other exception list entries, use the following syntax:

```PowerShell
Set-AddressRewriteEntry <AddressRewriteEntryIdentity> -ExceptionList @{Add="<domain1>","<domain2>"...; Remove="<domain3>","<domain4>"...}
```

This example adds finanace.contoso.com and removes marketing.contoso.com from the exception list of the address rewrite entry named Contoso to Northwind Traders:

```PowerShell
Set-AddressRewriteEntry "Contoso to Northwind Traders" -ExceptionList @{Add="finanace.contoso.com"; Remove="marketing.contoso.com"}
```

For more information, see [Set-AddressRewriteEntry](https://docs.microsoft.com/powershell/module/exchange/set-addressrewriteentry).

### How do you know this worked?

To verify that you have successfully modified an address rewrite entry, do the following:

1. Replace _\<AddressRewriteEntryIdentity\>_ with the name of the address rewrite entry, and run the following command to verify the property values:

   ```PowerShell
   Get-AddressRewriteEntry <AddressRewriteEntryIdentity> | Format-List
   ```

2. From a mailbox that's affected by the address rewrite entry, send a test message to an external mailbox. Verify the test message appears to originate from the rewritten email address.

3. From the external mailbox, reply to the test message. Verify the original mailbox receives the reply.

## Use the Exchange Management Shell to remove address rewrite entries

To remove a single address rewrite entry, use the following syntax:

```PowerShell
Remove-AddressRewriteEntry <AddressRewriteEntryIdentity>
```

This example removes the address rewrite entry named "Contoso.com to Northwindtraders.com":

```PowerShell
Remove-AddressRewriteEntry "Contoso.com to Northwindtraders.com"
```

To remove multiple address rewrite entries, use the following syntax:

```PowerShell
Get-AddressRewriteEntry [<search criteria>] | Remove-AddressRewriteEntry [-WhatIf]
```

This example removes all address rewrite entries:

```PowerShell
Get-AddressRewriteEntry | Remove-AddressRewriteEntry
```

This example simulates the removal of address rewrite entries that contain the text "to contoso.com" in the name. The _WhatIf_ switch allows you to preview the result without committing any changes.

```PowerShell
Get-AddressRewriteEntry "*to contoso.com" | Remove-AddressRewriteEntry -WhatIf
```

If you're satisfied with the result, run the command again without the _WhatIf_ switch to remove the address rewrite entries.

```PowerShell
Get-AddressRewriteEntry "*to contoso.com" | Remove-AddressRewriteEntry
```

For more information, see [Remove-AddressRewriteEntry](https://docs.microsoft.com/powershell/module/exchange/remove-addressrewriteentry).

### How do you know this worked?

To verify that you have successfully removed an address rewrite entry, do the following:

1. Run the command `Get-AddressRewriteEntry`, and verify that the address rewrite entries you removed aren't listed.

2. From a mailbox that was affected by the address rewrite entry, send a test message to an external mailbox. Verify the test message is no longer affected by the removed address rewrite entry.

3. From the external mailbox, reply to the test message. Verify the original mailbox receives the reply and that the message is unaffected by the removed address rewrite entry.
