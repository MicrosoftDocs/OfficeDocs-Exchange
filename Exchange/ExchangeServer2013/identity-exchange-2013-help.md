---
title: 'Identity: Exchange 2013 Help'
TOCTitle: Identity
ms:assetid: e90fae91-37e7-4fdc-9170-44f0dc965c66
ms:mtpsurl: https://technet.microsoft.com/library/Bb125042(v=EXCHG.150)
ms:contentKeyID: 49352392
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Identity

_**Applies to:** Exchange Server 2013_

The *Identity* parameter is a special parameter that you can use with most cmdlets. The *Identity* parameter gives you access to the unique identifiers that refer to a specific object in Microsoft Exchange Server 2013. This capability lets you perform actions on a specific Exchange 2013 object.

The following sections describe the Identity parameter and provide examples of how you can use it effectively:

Characteristics of the Identity parameter

Wildcard characters in Identity

Examples of the Identity parameter

## Characteristics of the Identity parameter

The primary unique identifier of an object in Exchange 2013 is always a GUID. A GUID is a 128-bit identifier, such as `63d64005-42c5-4f8f-b310-14f6cb125bf3`. This GUID never repeats and is therefore always unique. However, you don't want to type such GUIDs regularly. Therefore the *Identity* parameter typically also consists of the values of other parameters, or combined set of values from multiple parameters on a single object. These values are also guaranteed to be unique across that set of objects. You can specify the values of these other parameters, such as *Name* and *DistriguishedName*, or they can be system-generated. The additional parameters that are used, if any, and how they are populated, depend on the object you refer to.

The *Identity* parameter is also considered a positional parameter. The first argument on a cmdlet is assumed to be the *Identity* parameter when no parameter label is specified. This reduces the number of keystrokes when you type commands. For more information about positional parameters, see [Parameters](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_parameters).

The following example shows the use of the *Identity* parameter by using the Receive connector's unique *Name* parameter value. This example also shows how you can omit the *Identity* parameter name because *Identity* is a positional parameter.

```powershell
Get-ReceiveConnector -Identity "From the Internet"
Get-ReceiveConnector "From the Internet"
```

Like all objects in Exchange 2013, this Receive connector can also be referred to by its unique GUID. For example, if the Receive connector named `"From the Internet"` is also assigned the GUID `63d64005-42c5-4f8f-b310-14f6cb125bf3`, you can also retrieve the Receive connector by using the following command:

```powershell
Get-ReceiveConnector 63d64005-42c5-4f8f-b310-14f6cb125bf3
```

## Wildcard characters in Identity

Some **Get** cmdlets can accept a wildcard character (`*`) as part of the value you submit to *Identity* when you run the cmdlet. By using a wildcard with the *Identity* parameter, you can specify a partial name and retrieve a list of objects that match that partial name. You can place a wildcard character at the beginning or the end of the *Identity* value, but you can't place the character in the middle of a string. For example, the commands `Get-Mailbox David*` and `Get-Mailbox *anders*` are valid, but `Get-Mailbox Reb*ca` isn't a valid command.

Some **Get** cmdlets retrieve objects in Exchange 2013 that are organized in a hierarchical or parent and children relationship. That is, there may be a collection of parent objects that also contain their own child objects. Objects that have a parent and child relationship may have an *Identity* with the syntax of `<parent>\<child>`.

When an *Identity* parameter has a syntax of `<parent>\<child>`, some cmdlets enable you to use a wildcard character (\*) to replace all or some of the parent or child names. For example, if you want to find all of the child objects named "Contoso" in all parent objects, you could use the syntax `"*\Contoso"`. Likewise, if you want to find all of the child objects with a partial name of "Auth" that exist under the `"ServerA"` parent object, you could use the syntax `"ServerA\Auth*"`.

Some, but not all, cmdlets allow you to specify just the child portion of the Identity parameter when you run a command. When you do this, the cmdlets default to the current parent object being accessed. For example, two receive connectors named "Contoso Receive Connector" exist on both MBX1 and MBX2. If you run the command `Get-ReceiveConnector "Contoso Receive Connector"` on MBX2, only the receive connector on the server MBX2 is returned.

The specific behavior of the Identity parameter and wildcard characters is dependent on the cmdlet that's being run. For more information about the cmdlet you're running, see the feature-specific content for that cmdlet.

## Examples of the Identity parameter

The examples described in this topic illustrate how the *Identity* parameter can accept different unique values to refer to specific objects in the Exchange 2013 organization. These examples also illustrate how the *Identity* parameter label can be omitted to reduce the number of keystrokes when you type commands.

## DSN messages

The examples in this section refer to the delivery status notification (DSN) messages that can be configured in an Exchange 2013 organization. The first example shows how to retrieve DSN 5.4.1 by using the **Get-SystemMessage** cmdlet. In the **Get-SystemMessage** cmdlet, the *Identity* parameter consists of several pieces of data that are configured on each DSN message object. These pieces of data include the language that the DSN is written in, whether the DSN is internal or external in scope, and the DSN message code as in the following example:

```powershell
Get-SystemMessage en\internal\5.4.1
```

You can also retrieve this DSN message by using its GUID as in the following example, because all objects in Exchange 2013 have a GUID:

```powershell
Get-SystemMessage 82ca7bde-1c2d-4aa1-97e1-f298a6f10222
```

For more information about the makeup of the *Identity* parameter when it's used with the **SystemMessage** cmdlets, see [DSN message identity](dsn-message-identity-exchange-2013-help.md).

## Management role entries

The examples in this section refer to management role entries that make up management roles in Exchange 2013. Management roles are used to control the permissions that are granted to administrators and end users. Management role entries are made up of two parts: the management role they're associated with and a cmdlet. The Identity parameter is likewise made up of both the management role name and the cmdlet name. For example, the following is the role entry for the **Set-Mailbox** cmdlet on the `Mail Recipients` role:

```powershell
Mail Recipients\Set-Mailbox
```

The `Mail Recipients\Set-Mailbox` role entry is one of several entries on the `Mail Recipients` role. To view all the role entries on the `Mail Recipients` role, you can use the following command:

```powershell
Get-ManagementRoleEntry "Mail Recipients\*"
```

To view all the role entries on the `Mail Recipients` role that contain the string "`Mailbox`", use the following command:

```powershell
Get-ManagementRoleEntry "Mail Recipients\*Mailbox*"
```

To view all the management roles where **Set-Mailbox** is one of the role entries, use the following command:

```powershell
Get-ManagementRoleEntry *\Set-Mailbox
```

With role entries you can use the wildcard character in a variety of ways to query Exchange 2013 for the information you're interested in.

For more information about management roles, see [Understanding management roles](understanding-management-roles-exchange-2013-help.md).
