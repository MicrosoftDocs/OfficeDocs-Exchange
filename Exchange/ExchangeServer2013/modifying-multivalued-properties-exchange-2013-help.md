---
title: 'Modifying multivalued properties: Exchange 2013 Help'
TOCTitle: Modifying multivalued properties
ms:assetid: dc2c1062-ad79-404b-8da3-5b5798dbb73b
ms:mtpsurl: https://technet.microsoft.com/library/Bb684908(v=EXCHG.150)
ms:contentKeyID: 49289431
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Modifying multivalued properties

_**Applies to:** Exchange Server 2013_

A multivalued property is a property that can contain more than one value. For example, the **BlockedRecipients** property on the **RecipientFilterConfig** object can accept multiple recipient addresses as in the following examples:

- john@contoso.com

- kim@northwindtraders.com

- david@adatum.com

Because the **BlockedRecipients** property can accept more than one value, it's called a multivalued property. This topic explains how to use the Exchange Management Shell to add values to and remove values from a multivalued property on an object.

For more information about objects, see [Structured data](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_objects). For more information about the Shell, see [Using PowerShell with Exchange 2013 (Exchange Management Shell)](https://docs.microsoft.com/powershell/exchange/exchange-management-shell).

## Modifying a multivalued property vs. modifying a property that accepts only a single value

How you modify a multivalued property is slightly different from how you modify a property that accepts only one value. When you modify a property that accepts only a single value, you can assign a value directly to it, as in the following command.

```powershell
Set-TransportConfig -MaxSendSize 12MB
```

When you use this command to provide a new value to the **MaxSendSize** property, the stored value is overwritten. This isn't a problem with properties that accept only one value. However, it becomes a problem with multivalued properties. For example, assume that the **BlockedRecipients** property on the **RecipientFilterConfig** object is configured to have the three values that are listed in the previous section. When you run the command `Get-RecipientFilterConfig | Format-List BlockedRecipients`, the following is displayed.

```powershell
BlockedRecipients : {david@adatum.com, kim@northwindtraders.com, john@contoso.com}
```

Now assume that you've received a request to add a new SMTP address to the blocked recipients list. You run the following command to add the new SMTP address.

```powershell
Set-RecipientFilterConfig -BlockedRecipients chris@contoso.com
```

When you run the `Get-RecipientFilterConfig | Format-List BlockedRecipients` command again, you will see the following.

```powershell
BlockedRecipients : {chris@contoso.com}
```

This isn't what you expected. You wanted to add the new SMTP address to the existing list of blocked recipients, but instead the existing list of blocked recipients was overwritten by the new SMTP address. This unintended result exemplifies how modifying a multivalued property differs from modifying a property that accepts only a single value. When you modify a multivalued property, you must make sure that you append or remove values instead of overwriting the whole list of values. The following sections show you how to do exactly that.

## Modifying multivalued properties

Modifying multivalued properties is similar to modifying single-valued properties. You just need to add some additional syntax to tell the Shell that you want to add or remove values to or from the multivalued property rather than replace everything that's stored in the property. The syntax is included, along with the value or values to add or remove to or from the property, as a value on a parameter when you run a cmdlet. The following table shows the syntax that you need to add to a parameter on a cmdlet to modify multivalued properties.

### Multivalue property syntax

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Action</th>
<th>Syntax</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Add one or more values to a multivalued property</p></td>
<td>

```powershell
@{Add="<value1>", "<value2>", "<value3>"}
```

</td>
</tr>
<tr class="even">
<td><p>Remove one or more values from a multivalued property</p></td>
<td>

```powershell
@{Remove="<value1>", "<value2>", "<value3>"}
```

</td>
</tr>
</tbody>
</table>

The syntax that you choose from the Multivalue property syntax table is specified as a parameter value on a cmdlet. For example, the following command adds multiple values to a multivalued property:

```powershell
Set-ExampleCmdlet -Parameter @{Add="Red", "Blue", "Green"}
```

When you use this syntax, the values that you specify are added or removed from the list of values already present on the property. Taking the **BlockedRecipients** example earlier in this topic, we can now add chris@contoso.com without overwriting the rest of the values on this property by using the following command:

```powershell
Set-RecipientFilterConfig -BlockedRecipients @{Add="chris@contoso.com"}
```

If you wanted to remove david@adatum.com from the list of values, you would use this command:

```powershell
Set-RecipientFilterConfig -BlockedRecipients @{Remove="david@adatum.com"}
```

More complex combinations can be used, such as adding or removing values to and from a property at the same time. To do so, insert a semicolon (`;` ) between `Add` and `Remove` actions. For example:

```powershell
Set-RecipientFilterConfig -BlockedRecipients @{Add="carter@contoso.com", "sam@northwindtraders.com", "brian@adatum.com"; Remove="john@contoso.com"}
```

If we use the `Get-RecipientFilterConfig | Format-List BlockedRecipients` command again, we can see that the email addresses for Carter, Sam, and Brian have been added while the address for John has been removed.

```powershell
BlockedRecipients : {brian@adatum.com, sam@northwindtraders.com, carter@contoso.com, chris@contoso.com, kim@northwindtraders.com}
```
