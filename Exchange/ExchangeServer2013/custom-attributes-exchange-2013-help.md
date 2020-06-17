---
title: 'Custom attributes: Exchange 2013 Help'
TOCTitle: Custom attributes
ms:assetid: 2b043878-0b34-4563-a9c2-28a9efa7447e
ms:mtpsurl: https://technet.microsoft.com/library/Ee423541(v=EXCHG.150)
ms:contentKeyID: 49352787
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Custom attributes

_**Applies to:** Exchange Server 2013_

Microsoft Exchange Server 2013 includes 15 extension attributes. You can use these attributes to add information about a recipient, such as an employee ID, organizational unit (OU), or some other custom value for which there isn't an existing attribute. These custom attributes are labeled in Active Directory as **ms-Exch-Extension-Attribute1** through **ms-Exch-Extension-Attribute15**. In the Exchange Management Shell, the corresponding parameters are *CustomAttribute1* through *CustomAttribute15*. These attributes aren't used by any Exchange components. They can be used to store Active Directory data without having to extend the Active Directory schema.

In Exchange Server 2003 and earlier, if you wanted to store this information in Active Directory, you had to create an attribute by extending the Active Directory schema. Schema extension requires planning, procuring object identifiers (OIDs) for new attributes, and testing the extension process in a test environment before you implement it in a production environment. In Exchange 2013, schema extensions can't be used in recipient filters used by address lists, e-mail address policies, and dynamic distribution groups.

## Advantages of custom attributes

Some of the advantages of using custom attributes include:

- You avoid extending the Active Directory schema.

- The attributes are created by Exchange Setup.

- You can use the Exchange admin center (EAC) or the Exchange Management Shell to manage the attributes. You don't need to build custom controls or write scripts to populate and display these attributes.

- The attributes are filterable properties that can be used in the *Filter* parameter with recipient cmdlets such as **Get-Mailbox**. They can also be used in the EAC and the Shell to create filters for e-mail address policies, address lists, and dynamic distribution groups.

## Multivalued custom attributes

In Exchange 2010 Service Pack 2 (SP2), five multivalued custom attributes were added to Exchange to allow you to store additional information for mail recipients if the traditional custom attributes didn't meet your needs. The *ExtensionCustomAttribute1* to *ExtensionCustomAttribute5* parameters can hold up to 1,300 values each. You can specify multiple values as a comma-delimited list. The following cmdlets support these new parameters:

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DistributionGroup)

- [Set-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/Set-DynamicDistributionGroup)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/Set-Mailbox)

- [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/Set-MailContact)

- [Set-MailPublicFolder](https://docs.microsoft.com/powershell/module/exchange/Set-MailPublicFolder)

- [Set-RemoteMailbox](https://docs.microsoft.com/powershell/module/exchange/Set-RemoteMailbox)

For more information about multivalued properties, see [Modifying multivalued properties](modifying-multivalued-properties-exchange-2013-help.md).

## Custom attribute examples

In many Exchange deployments, creating an e-mail address policy for all recipients in an OU is a common scenario. The OU isn't a filterable property that can be used in the *RecipientFilter* parameter of an e-mail address policy or an address list.

> [!NOTE]
> Dynamic distribution groups have an additional parameter that you can use to restrict it to recipients in a particular OU or container.

If the recipients in that OU don't share any common properties that you can filter by, such as department or location, you can populate one of the custom attributes with a common value, as shown in this example.

```powershell
Get-Mailbox -OrganizationalUnit Sales | Set-Mailbox CustomAttribute1 "SalesOU"
```

Now you can create an e-mail address policy for all recipients that have the *CustomAttribute1* property that equals SalesOU, as shown in this example.

```powershell
New-EmailAddressPolicy -Name "Sales" -RecipientFilter "CustomAttribute1 -eq 'SalesOU'" -EnabledEmailAddressTemplates "SMTP:%s%2g@sales.contoso.com"
```

## Custom attribute example using the ConditionalCustomAttributes parameter

When creating dynamic distribution groups, email address policies, or address lists, you don't need to use the *RecipeintFilter* parameter to specify custom attributes. You can use the *ConditionalCustomAttribute1* to *ConditionalCustomAttribute15* parameters instead.

This example creates a dynamic distribution group based on the recipients whose *CustomAttribute1* is set to SalesOU.

```powershell
New-DynamicDistributionGroup -Name "Sales Users and Contacts" -IncludedRecipients "MailboxUsers,MailContacts" -ConditionalCustomAttribute1 "SalesOU"
```

> [!NOTE]
> You must use the <EM>IncludedRecipients</EM> parameter if you use a <EM>Conditional</EM> parameter. In addition, you can't use <EM>Conditional</EM> parameters if you use the <EM>RecipientFilter</EM> parameter. If you want to include additional filters to create your dynamic distribution group, email address policies, or address lists, you should use the <EM>RecipientFilter</EM> parameter.

## Custom attribute example using ExtensionCustomAttributes parameter

In this example, the mailbox for Kweku will have *ExtensionCustomAttribute1* updated to reflect that he's enrolled in the following educational classes: MATH307, ECON202, and ENGL300.

```powershell
Set-Mailbox -Identity Kweku -ExtensionCustomAttribute1 MATH307,ECON202,ENGL300
```

Next, a dynamic distribution group for all students enrolled MATH307 is created by using the *RecipientFilter* parameter where *ExtensionCustomAttribute1* is equal to MATH307. When using the *ExtentionCustomAttributes* parameters, you can use the `-eq` operator instead of the `-like` operator.

```powershell
New-DynamicDistributionGroup -Name Students_MATH307 -RecipientFilter "ExtensionCustomAttribute1 -eq 'MATH307'"
```

In this example, Kweku's *ExtensionCustomAttribute1* values are updated to reflect that he's added the class ENGL210 and removed the class ECON202.

```powershell
Set-Mailbox -Identity Kweku -ExtensionCustomAttribute1 @{Add="ENGL210"; Remove="ECON202"}
```
