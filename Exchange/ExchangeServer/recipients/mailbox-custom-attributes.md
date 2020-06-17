---
localization_priority: Normal
description: 'Summary: Learn to use the custom attributes in Exchange Server 2016 or Exchange Server 2019 to add information about a mail recipient.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 2b043878-0b34-4563-a9c2-28a9efa7447e
ms.reviewer:
title: Custom attributes
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Custom attributes

Exchange Server includes 15 extension attributes that you can use to add information about a recipient, such as an employee ID, organizational unit (OU), or some other custom value for which there isn't an existing attribute.

In earlier versions of Exchange, if you wanted to store this information in Active Directory, you had to create an attribute by extending the Active Directory schema. Schema extension requires planning, procuring object identifiers (OIDs) for new attributes, and testing the extension process in a test environment before you implement it in a production environment. Exchange Server doesn't let you use schema extensions in recipient filters that are used by address lists, e-mail address policies, and dynamic distribution groups.

The custom attributes available to Exchange Server are labeled in Active Directory as **ms-Exch-Extension-Attribute1** through **ms-Exch-Extension-Attribute15**. In the Exchange Management Shell, the corresponding parameters are _CustomAttribute1_ through _CustomAttribute15_. These attributes aren't used by any Exchange components. They can be used to store Active Directory data without having to extend the Active Directory schema.

> [!NOTE]
> **ms-Exch-Extension-Attribute-16** to **ms-Exch-Extension-Attribute-45** are present in Active Directory, but aren't available in the Exchange admin center (EAC) or the Exchange Management Shell. Don't use non-Exchange tools to edit these attributes because they might be used for future Exchange features.

## Advantages of custom attributes
<a name="AO"> </a>

There are several advantages to using custom attributes:

- You avoid extending the Active Directory schema.

- You don't have to do the work, because the attributes are created by Exchange Setup.

- You can use the EAC or the Exchange Management Shell to manage the attributes. You don't need to build custom controls or write scripts to populate and display these attributes.

- You can filter and reuse the attributes, as attributes are filterable properties that can be used in the _Filter_ parameter with recipient cmdlets such as **Get-Mailbox**. They can also be used in the EAC and the Exchange Management Shell to create filters for e-mail address policies, address lists, and dynamic distribution groups.

### Multivalued custom attributes

Starting with Exchange 2010 Service Pack 2 (SP2), five multivalued custom attributes were added to Exchange to allow you to store additional information for mail recipients if the traditional custom attributes didn't meet your needs. The _ExtensionCustomAttribute1_ to _ExtensionCustomAttribute5_ parameters can hold up to 1,300 values each. You can specify multiple values as a comma-delimited list. The following cmdlets support these new parameters:

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

- [Set-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-dynamicdistributiongroup)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/set-mailcontact)

- [Set-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/set-mailpublicfolder)

- [Set-RemoteMailbox](https://docs.microsoft.com/powershell/module/exchange/set-remotemailbox)

For more information about multivalued properties, see [Modifying multivalued properties](https://docs.microsoft.com/exchange/modifying-multivalued-properties-exchange-2013-help).

## Custom attribute examples
<a name="CA"> </a>

A common scenario in many Exchange deployments is that of creating an e-mail address policy for all recipients in an OU. The OU isn't a filterable property that can be used in the _RecipientFilter_ parameter of an e-mail address policy or an address list.

> [!NOTE]
> Dynamic distribution groups have an additional parameter that you can use to restrict it to recipients in a particular OU or container.

If the recipients in a particular OU don't share any common properties that you can filter by, such as department or location, you can populate one of the custom attributes with a common value, as shown in this example.

```PowerShell
Get-Mailbox -OrganizationalUnit Sales | Set-Mailbox CustomAttribute1 "SalesOU"
```

With that done, now you can create an e-mail address policy for all recipients that have the _CustomAttribute1_ property that equals SalesOU, as shown in this example.

```PowerShell
New-EmailAddressPolicy -Name "Sales" -RecipientFilter "CustomAttribute1 -eq 'SalesOU'" -EnabledEmailAddressTemplates "SMTP:%s%2g@sales.contoso.com"
```

## Custom attribute example using the ConditionalCustomAttributes parameter
<a name="CAE"> </a>

When creating dynamic distribution groups, email address policies, or address lists, you don't need to use the _RecipeintFilter_ parameter to specify custom attributes. You can use the _ConditionalCustomAttribute1_ to _ConditionalCustomAttribute15_ parameters instead.

This example creates a dynamic distribution group based on the recipients whose _CustomAttribute1_ is set to SalesOU.

```PowerShell
New-DynamicDistributionGroup -Name "Sales Users and Contacts" -IncludedRecipients "MailboxUsers,MailContacts" -ConditionalCustomAttribute1 "SalesOU"
```

> [!NOTE]
> You need to use the _IncludedRecipients_ parameter if you use a _Conditional_ parameter. In addition, you can't use _Conditional_ parameters if you use the _RecipientFilter_ parameter. If you want to include additional filters to create your dynamic distribution group, email address policies, or address lists, you should use the _RecipientFilter_ parameter.

## Custom attribute example using ExtensionCustomAttributes parameter
<a name="extcusparam"> </a>

In this example, the mailbox for Kweku will have _ExtensionCustomAttribute1_ updated to reflect that he's enrolled in the following educational classes: MATH307, ECON202, and ENGL300.

```PowerShell
Set-Mailbox -Identity Kweku -ExtensionCustomAttribute1 MATH307,ECON202,ENGL300
```

Next, a dynamic distribution group for all students enrolled MATH307 is created by using the _RecipientFilter_ parameter where _ExtensionCustomAttribute1_ is equal to MATH307. When using the _ExtentionCustomAttributes_ parameters, you can use the `-eq` operator instead of the `-like` operator.

```PowerShell
New-DynamicDistributionGroup -Name Students_MATH307 -RecipientFilter "ExtensionCustomAttribute1 -eq 'MATH307'"
```

In this example, Kweku's _ExtensionCustomAttribute1_ values are updated to reflect that he's added the class ENGL210 and removed the class ECON202.

```PowerShell
Set-Mailbox -Identity Kweku -ExtensionCustomAttribute1 @{Add="ENGL210"; Remove="ECON202"}
```
