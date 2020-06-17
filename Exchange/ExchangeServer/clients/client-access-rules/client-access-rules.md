---
localization_priority: Normal
description: 'Summary: Learn how administrators can use Client Access Rules to allow or block access to the Exchange admin center (EAC) and remote PowerShell in Exchange 2019.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
monikerRange: exchserver-2019
title: Client Access Rules in Exchange 2019
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-server-it-pro
ms.reviewer: 
manager: serdars

---

# Client Access Rules in Exchange 2019

Client Access Rules help you control access to your Exchange 2019 organization in the Exchange admin center (EAC) and remote PowerShell based on client properties or client access requests. Client Access Rules are like mail flow rules (also known as transport rules) for EAC and remote PowerShell connections to your Exchange organization. You can prevent EAC and remote PowerShell clients from connecting to Exchange based on their IP address, authentication type, and user property values. For example:

- Prevent client access using remote PowerShell (which also includes the Exchange Management Shell).

- Block access to the EAC for users in a specific country or region.

For Client Access Rule procedures, see [Procedures for Client Access Rules in Exchange Server](procedures-for-client-access-rules.md).

## Client Access Rule components

A rule is made of conditions, exceptions, an action, and a priority value.

- **Conditions**: Identify the client connections to apply the action to. For a complete list of conditions, see the [Client Access Rule conditions and exceptions](#client-access-rule-conditions-and-exceptions) section later in this topic. When a client connection matches the conditions of a rule, the action is applied to the client connection, and rule evaluation stops (no more rules are applied to the connection).

- **Exceptions**: Optionally identify the client connections that the action shouldn't apply to. Exceptions override conditions and prevent the rule action from being applied to a connection, even if the connection matches all of the configured conditions. Rule evaluation continues for client connections that are allowed by the exception, but a subsequent rule could still affect the connection.

- **Action**: Specifies what to do to client connections that match the conditions in the rule, and don't match any of the exceptions. Valid actions are:

  - Allow the connection (the `AllowAccess` value for the _Action_ parameter).

  - Block the connection (the `DenyAccess` value for the _Action_ parameter).

    **Note**: When you block connections for a specific protocol, other applications that rely on the same protocol might also be affected.

- **Priority**: Indicates the order that the rules are applied to client connections (a lower number indicates a higher priority). The default priority is based on when the rule is created (older rules have a higher priority than newer rules), and higher priority rules are processed before lower priority rules. Remember, rule processing stops once the client connection matches the conditions in the rule.

    For more information about setting the priority value on rules, see [Use the Exchange Management Shell to set the priority of Client Access Rules](procedures-for-client-access-rules.md#use-the-exchange-management-shell-to-set-the-priority-of-client-access-rules).

### How Client Access Rules are evaluated

How multiple rules with the same condition are evaluated, and how a rule with multiple conditions, condition values, and exceptions are evaluated are described in the following table.

|**Component**|**Logic**|**Comments**|
|:-----|:-----|:-----|
|Multiple rules that contain the same condition|The first rule is applied, and subsequent rules are ignored|For example, if your highest priority rule blocks remote PowerShell connections, and you create another rule that allows remote PowerShell connections for a specific IP address range, all remote PowerShell connections are still blocked by the first rule. Instead of creating another rule for remote PowerShell, you need to add an exception to the existing remote PowerShell rule to allow connections from the specified IP address range.|
|Multiple conditions in one rule|AND|A client connection must match all conditions in the rule. For example, EAC connections from users in the Accounting department.|
|One condition with multiple values in a rule|OR|For conditions that allow more than one value, the connection must match any one (not all) of the specified conditions. For example, EAC or remote PowerShell connections.|
|Multiple exceptions in one rule|OR|If a client connection matches any one of the exceptions, the actions are not applied to the client connection. The connection doesn't have to match all the exceptions. For example, IP address 19.2.168.1.1 or Basic authentication.|

You can test how a specific client connection would be affected by Client Access Rules (which rules would match and therefore affect the connection). For more information, see [Use the Exchange Management Shell to test Client Access Rules](procedures-for-client-access-rules.md#use-the-exchange-management-shell-to-test-client-access-rules).

### Important notes

#### Client connections from your internal network

Connections from your local network aren't automatically allowed to bypass Client Access Rules. Therefore, when you create Client Access Rules that block client connections to Exchange, you need to consider how connections from your internal network might be affected. The preferred method to allow internal client connections to bypass Client Access Rules is to create a highest priority rule that allows client connections from your internal network (all or specific IP addresses). That way, the client connections are always allowed, regardless of any other blocking rules that you create in the future.

#### Client Access Rules and middle-tier applications

Many applications that access Exchange use a middle-tier architecture (clients talk to the middle-tier application and the middle-tier application talks to Exchange). A Client Access Rule that only allows access from your local network might block middle-tier applications. So, your rules need to allow the IP addresses of middle-tier applications.

Middle-tier applications owned by Microsoft (for example, Outlook for iOS and Android) will bypass blocking by Client Access Rules, and will always be allowed. To provide additional control over these applications, you need to use the control capabilities that are available in the applications.

#### Timing for rule changes

To improve overall performance, Client Access Rules use a cache, which means changes to rules don't immediately take effect. The first rule that you create in your organization can take up to 24 hours to take effect. After that, modifying, adding, or removing rules can take up to one hour to take effect.

#### Administration

You can only use the Exchange Management Shell (remote PowerShell) to manage Client Access Rules, so you need to be careful about rules that block your access to remote PowerShell.

As a best practice, create a Client Access Rule with the highest priority to preserve your access to remote PowerShell. For example:

```powershell
New-ClientAccessRule -Name "Always Allow Remote PowerShell" -Action Allow -AnyOfProtocols RemotePowerShell -Priority 1
```

#### Authentication types and protocols

Not all authentication types are supported for all protocols. The supported authentication types per protocol in Exchange Server are described in this table:

||**AdfsAuthentication**|**BasicAuthentication**|**CertificateBasedAuthentication**|**NonBasicAuthentication**|**OAuthAuthentication**|
|:-----|:-----|:-----|:-----|:-----|:-----|
|`ExchangeAdminCenter`|supported|supported|n/a|n/a|n/a|
|`RemotePowerShell`|n/a|supported|n/a|supported|n/a|

## Client Access Rule conditions and exceptions

Conditions and exceptions in Client Access Rules identify the client connections that the rule is applied to or not applied to. For example, if the rule blocks access by remote PowerShell clients, you can configure the rule to allow remote PowerShell connections from a specific range of IP addresses. The syntax is the same for a condition and the corresponding exception. The only difference is conditions specify client connections to include, while exceptions specify client connections to exclude.

This table describes the conditions and exceptions that are available in Client Access Rules:

|**Condition parameter in the Exchange Management Shell**|**Exception parameter in the Exchange Management Shell**|**Description**|
|:-----|:-----|:-----|
|_AnyOfAuthenticationTypes_|_ExceptAnyOfAuthenticationTypes_|Valid values in Exchange Server are: <br/>• For the EAC: `AdfsAuthentication` and `BasicAuthentication` <br/>• For remote PowerShell: `BasicAuthentication` and `NonBasicAuthentication` <br/> You can specify multiple values separated by commas. You can use quotation marks around each individual value ("_value1_","_value2_"), but not around all values (don't use "_value1_,_value2_").|
|_AnyOfClientIPAddressesOrRanges_|_ExceptAnyOfClientIPAddressesOrRanges_|Valid values are: <br/>• **A single IP address**: For example, `192.168.1.1`. <br/>• **An IP address range**: For example, `192.168.0.1-192.168.0.254`. <br/>• **Classless Inter-Domain Routing (CIDR) IP**: For example, `192.168.3.1/24`. <br/> You can specify multiple values separated by commas.|
|_AnyOfProtocols_|_ExceptAnyOfProtocols_|Valid values in Exchange Server are: <br/>• `ExchangeAdminCenter` <br/>• `RemotePowerShell` <br/> You can specify multiple values separated by commas. You can use quotation marks around each individual value (" _value1_","_value2_"), but not around all values (don't use "_value1_,_value2_"). <br/> **Note**: If you don't use this condition in a rule, the rule is applied to both protocols.|
|_Scope_|n/a|Specifies the type of connections that the rule applies to. Valid values are: <br/>• `Users`: The rule only applies to end-user connections. <br/>• `All`: The rule applies to all types of connections (end-users and middle-tier apps).|
|_UsernameMatchesAnyOfPatterns_|_ExceptUsernameMatchesAnyOfPatterns_|Accepts text and the wildcard character (\*) to identify the user's account name in the format `<Domain>\<UserName>` (for example, `contoso.com\jeff` or `*jeff*`, but not `jeff*`). Non-alphanumeric characters don't require an escape character. <br/> You can specify multiple values separated by commas.|
|_UserRecipientFilter_|n/a| Uses OPath filter syntax to identify the user that the rule applies to. For example, `"City -eq 'Redmond'"`. The filterable attributes are: <br/>• `City` <br/>• `Company` <br/>• `CountryOrRegion` <br/>• `CustomAttribute1` to `CustomAttribute15` <br/>• `Department` <br/>• `Office` <br/>• `PostalCode` <br/>• `StateOrProvince` <br/>• `StreetAddress` <br/> The search criteria uses the syntax `"<Property> -<Comparison operator> '<Value>'"`. <br/>• `<Property>` is a filterable property. <br/>• `-<Comparison Operator>` is an OPATH comparison operator. For example `-eq` for exact matches (wildcards are not supported) and `-like` for string comparison (which requires at least one wildcard in the property value). For more information about comparison operators, see [about_Comparison_Operators](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comparison_operators). <br/>• `<Value>` is the property value. Text values with or without spaces or values with wildcards (\*) need to be enclosed in quotation marks (for example, `'<Value>'` or `'*<Value>'`). Don't use quotation marks with the system value `$null` (for blank values). <br/> You can chain multiple search criteria together using the logical operators `-and` and `-or`. For example, `"<Criteria1> -and <Criteria2>"` or `"(<Criteria1> -and <Criteria2>) -or <Criteria3>"`. For more information about OPATH filter syntax, see [Additional OPATH syntax information](https://docs.microsoft.com/powershell/exchange/recipient-filters#additional-opath-syntax-information).|
