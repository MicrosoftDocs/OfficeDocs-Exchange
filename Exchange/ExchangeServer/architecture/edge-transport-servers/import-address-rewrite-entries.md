---
localization_priority: Normal
description: 'Summary: Learn how to create or import email address rewriting in bulk in Exchange Server.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: bd0942c6-9c66-4b4c-b9bc-2f5f783def76
ms.date: 7/3/2018
ms.reviewer: 
title: Import address rewrite entries on Edge Transport servers
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Import address rewrite entries on Edge Transport servers

You can bulk-create or import address rewriting information into an Edge Transport server by using a comma-separated value (CSV) file. The following list describes common scenarios that require you to do this:

- You are replacing an address rewriting solution with an Edge Transport server.

- You enter into an agreement with a third-party solution provider that requires you to rewrite their email addresses.

- You acquire another organization, and you need to temporarily rewrite the email addresses in the acquired organization.

You can use a spreadsheet application like Microsoft Excel to create the CSV file. Format the file as described in this topic and save it as a .csv file.

The first row, or *header row*, of the CSV file lists the names of the parameters. Each parameter is separated by a comma. The required and optional parameters are described in the following table.

|**Parameter**|**Required or optional**|**Description**|
|:-----|:-----|:-----|
| _Name_|Required|A unique, descriptive name for the address rewrites entry.|
| _InternalAddress_|Required|The address you want to change. You can use the following values:  <br/> • A single email address (chris@contoso.com)  <br/> • A single domain or subdomain (contoso.com or sales.contoso.com)  <br/> • A domain and all subdomains (\*.contoso.com)|
| _ExternalAddress_|Required|The final email address you want. You can use the following values:  <br/> • A single email address if you specified a single email address for _InternalAddress_ <br/> • A single domain or subdomain for all other values of _InternalAddress_|
| _ExceptionList_|Optional|Available only when you're rewriting email addresses in a domain and all subdomains (\*.contoso.com). Specifies one or more subdomains you want to exclude from address rewriting. Enclose the value in double quotation marks, and separate multiple values by commas. For example, `"marketing.contoso.com"` or `"marketing.contoso.com,legal.contoso.com"`.|
| _OutboundOnly_|Optional| `False` means that addresses are written on inbound and outbound mail. `True` means that addresses are rewritten on outbound mail only, and you need to manually configure the rewritten email address as a proxy address on the affected recipients.  <br/> The default value is `False`, but you need to set it to `True` if _InternalAddress_ contains the wildcard character (\*.contoso.com).  <br/> The _OutboundOnly_ parameter value in the CSV file is `True` or `False`, not `$True` or `$False`.|

Each row under the header row represents an individual address rewrite entry. The values in each row need to be in the same order as the parameter names in the header row. Each value is separated by a comma.

## What do you need to know before you begin?

- Estimated time to complete this task: 15 minutes

- Make sure you understand the ramifications of address rewriting. For example, the rewritten email address need to be unique in your Exchange organization, and you might need to configure proxy addresses on the affected recipients. For more information, see [Address rewriting on Edge Transport servers](address-rewriting.md) and [Address rewriting procedures on Edge Transport servers](address-rewriting-procedures.md).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Address Rewriting agent" entry in the [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md) topic.

- If you have more than one Edge Transport server, we recommend that you use the procedures in this topic to import the address rewrite entries into a single Edge Transport server and then clone the configuration of that Edge Transport server to the other Edge Transport servers in your organization. For more information about how to clone an Edge Transport server, see [Using Edge Transport Server Cloned Configuration](http://technet.microsoft.com/library/683a6b8a-59bf-43ed-96c8-504945c2f665.aspx).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](../../about-documentation/exchange-admin-center-keyboard-shortcuts.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Step 1: Create the CSV file

When you create the CSV file, consider the following items:

- If you specify values for optional parameters in the CSV file, every row must include a value in that column. If you want to create multiple address rewrite entries where some entries have optional parameters and some entries do not, you need to separate those address rewrite entries into different CSV files, and then import each CSV file separately.

- If the CSV file contains non-ASCII characters, be sure to save the CSV file with UTF-8 encoding or other Unicode encoding. Saving the CSV file with UTF-8 encoding or other Unicode encoding might be easier when the system locale of the computer matches the language that's used in the CSV file.

The following example shows how a CSV file can be populated with the optional _ExceptionList_ and _OutboundOnly_ parameters included:

```
Name,InternalAddress,ExternalAddress,ExceptionList,OutboundOnly
"Wingtip UK",*.wingtiptoys.co.uk,tailspintoys.com,"legal.wingtiptoys.co.uk,finance.wingtiptoys.co.uk,support.wingtiptoys.co.uk",True
"Wingtip USA",*.wingtiptoys.com,tailspintoys.com,"legal.wingtiptoys.com,finance.wingtiptoys.com,support.wingtiptoys.com,corp.wingtiptoys.com",True
"Wingtip Canada",*.wingtiptoys.ca,tailspintoys.com,"legal.wingtiptoys.ca,finance.wingtiptoys.ca,support.wingtiptoys.ca",True
```

## Step 2: Import the CSV file

To import the CSV file, use the following syntax:

```
Import-Csv <FileNameAndPath> | ForEach {New-AddressRewriteEntry -Name $_.Name -InternalAddress $_.InternalAddress -ExternalAddress $_.ExternalAddress -OutboundOnly ([Bool]::Parse($_.OutboundOnly)) -ExceptionList $_.ExceptionList}
```

This example imports the address rewrite entries from C:\My Documents\ImportAddressRewriteEntries.csv.

```
Import-Csv "C:\My Documents\ImportAddressRewriteEntries.csv" | ForEach {New-AddressRewriteEntry -Name $_.Name -InternalAddress $_.InternalAddress -ExternalAddress $_.ExternalAddress -OutboundOnly ([Bool]::Parse($_.OutboundOnly)) -ExceptionList $_.ExceptionList}
```

### How do you know this step worked?

To verify that you have successfully imported address rewrite entries from a CSV file, use either of the following procedures:

- To see all address rewrite entries, run the following command:

  ```
  Get-AddressRewriteEntry
  ```

- To see details about a specific address rewrite entry, replace _\<AddressRewriteIdentity\>_ with the name of the address rewrite entry, and run the following command:

  ```
  Get-AddressRewriteEntry "<AddressRewriteIdentity>" | Format-List
  ```
