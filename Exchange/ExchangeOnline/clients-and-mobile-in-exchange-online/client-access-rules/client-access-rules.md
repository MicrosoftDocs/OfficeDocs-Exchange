---
localization_priority: Normal
description: 'Summary: Learn how administrators can use Client Access Rules to allow or block different types of client connections to Exchange Online.'
ms.topic: overview
author: msdmaguire
ms.author: dmaguire
ms.assetid: 3792312e-882c-40b5-add4-a7bc17af4c58
ms.reviewer: 
title: Client Access Rules in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Client Access Rules in Exchange Online

 **Summary**: Learn how administrators can use Client Access Rules to allow or block different types of client connections to Exchange Online.

Client Access Rules help you control access to your Exchange Online organization based on client properties or client access requests. Client Access Rules are like mail flow rules (also known as transport rules) for client connections to your Exchange Online organization. You can prevent clients from connecting to Exchange Online based on their IP address, authentication type, and user property values, and the protocol, application, service, or resource that they're using to connect. For example:

- Allow access to Exchange ActiveSync clients from specific IP addresses, and block all other ActiveSync clients.

- Block access to Exchange Web Services (EWS) for users in specific departments, cities, or countries.

- Block access to an offline address book (OAB) for specific users based on their usernames.

- Prevent client access using federated authentication.

- Prevent client access using Exchange Online PowerShell.

- Block access to the Exchange admin center (EAC) for users in a specific country or region.

For Client Access Rule procedures, see [Procedures for Client Access Rules in Exchange Online](procedures-for-client-access-rules.md).

## Client Access Rule components

A rule is made of conditions, exceptions, an action, and a priority value.

- **Conditions**: Identify the client connections to apply the action to. For a complete list of conditions, see the [Client Access Rule conditions and exceptions](#client-access-rule-conditions-and-exceptions) section later in this topic. When a client connection matches the conditions of a rule, the action is applied to the client connection, and rule evaluation stops (no more Rules are applied to the connection).

- **Exceptions**: Optionally identify the client connections that the action shouldn't apply to. Exceptions override conditions and prevent the rule action from being applied to a connection, even if the connection matches all of the configured conditions. Rule evaluation continues for client connections that are allowed by the exception, but a subsequent rule could still affect the connection.

- **Action**: Specifies what to do to client connections that match the conditions in the rule, and don't match any of the exceptions. Valid actions are:

  - Allow the connection (the `AllowAccess` value for the _Action_ parameter).

  - Block the connection (the `DenyAccess` value for the _Action_ parameter).

    **Note**: When you block connections for a specific protocol, other applications that rely on the same protocol might also be affected.

- **Priority**: Indicates the order that the rules are applied to client connections (a lower number indicates a higher priority). The default priority is based on when the rule is created (older rules have a higher priority than newer rules), and higher priority rules are processed before lower priority rules. Remember, rule processing stops once the client connection matches the conditions in the rule.

    For more information about setting the priority value on rules, see [Use Exchange Online PowerShell to set the priority of Client Access Rules](procedures-for-client-access-rules.md#use-exchange-online-powershell-to-set-the-priority-of-client-access-rules).

### How Client Access Rules are evaluated

How multiple rules with the same condition are evaluated, and how a rule with multiple conditions, condition values, and exceptions are evaluated are described in the following table.

****

|Component|Logic|Comments|
|---|---|---|
|Multiple rules that contain the same condition|The first rule is applied, and subsequent rules are ignored|For example, if your highest priority rule blocks Outlook on the web connections, and you create another rule that allows Outlook on the web connections for a specific IP address range, all Outlook on the web connections are still blocked by the first rule. Instead of creating another rule for Outlook on the web, you need to add an exception to the existing Outlook on the web rule to allow connections from the specified IP address range.|
|Multiple conditions in one rule|AND|A client connection must match all conditions in the rule. For example, EWS connections from users in the Accounting department.|
|One condition with multiple values in a rule|OR|For conditions that allow more than one value, the connection must match any one (not all) of the specified conditions. For example, EWS or IMAP4 connections.|
|Multiple exceptions in one rule|OR|If a client connection matches any one of the exceptions, the actions are not applied to the client connection. The connection doesn't have to match all the exceptions. For example, IP address 19.2.168.1.1 or Basic authentication.|
|

You can test how a specific client connection would be affected by Client Access Rules (which rules would match and therefore affect the connection). For more information, see [Use Exchange Online PowerShell to test Client Access Rules](procedures-for-client-access-rules.md#use-exchange-online-powershell-to-test-client-access-rules).

### Important notes

#### Client connections from your internal network

Connections from your local network aren't automatically allowed to bypass Client Access Rules. Therefore, when you create Client Access Rules that block client connections to Exchange Online, you need to consider how connections from your internal network might be affected. The preferred method to allow internal client connections to bypass Client Access Rules is to create a highest priority rule that allows client connections from your internal network (all or specific IP addresses). That way, the client connections are always allowed, regardless of any other blocking rules that you create in the future.

#### Client Access Rules and middle-tier applications

Many applications that access Exchange Online use a middle-tier architecture (clients talk to the middle-tier application, and the middle-tier application talks to Exchange Online). A Client Access Rule that only allows access from your local network might block middle-tier applications. So, your rules need to allow the IP addresses of middle-tier applications.

Middle-tier applications owned by Microsoft (for example, Outlook for iOS and Android) will bypass blocking by Client Access Rules, and will always be allowed. To provide additional control over these applications, you need to use the control capabilities that are available in the applications.

#### Timing for rule changes

To improve overall performance, Client Access Rules use a cache, which means changes to rules don't immediately take effect. The first rule that you create in your organization can take up to 24 hours to take effect. After that, modifying, adding, or removing rules can take up to one hour to take effect.

#### Administration

You can only use remote PowerShell to manage Client Access Rules, so you need to be careful about rules that block your access to remote PowerShell. If you create a rule that blocks your access to remote PowerShell, or if you create a rule that blocks all protocols for everyone, you'll lose the ability to fix the rules yourself. You'll need to call Microsoft Customer Service and Support, and they will create a rule that gives you remote PowerShell access from anywhere so you can fix your own rules. Note that it can take up to one hour for this new rule to take effect.

As a best practice, create a Client Access Rule with the highest priority to preserve your access to remote PowerShell. For example:

```PowerShell
New-ClientAccessRule -Name "Always Allow Remote PowerShell" -Action Allow -AnyOfProtocols RemotePowerShell -Priority 1
```

#### Authentication types and protocols in Client Access Rules

Not all authentication types are supported for all protocols in Client Access Rules. The supported authentication types per protocol are described in this table:

****

|Protocol|AdfsAuthentication|BasicAuthentication|CertificateBasedAuthentication|NonBasicAuthentication|OAuthAuthentication|
|---|:---:|:---:|:---:|:---:|:---:|
|`ExchangeActiveSync`|n/a|supported|supported|n/a|supported|
|`ExchangeAdminCenter`|supported|supported|n/a|n/a|n/a|
|`IMAP4`|n/a|supported|n/a|n/a|supported|
|`OutlookWebApp`|supported|supported|n/a|n/a|n/a|
|`POP3`|n/a|supported|n/a|n/a|supported|
|`RemotePowerShell`|n/a|supported|n/a|supported|n/a|
|

## Client Access Rule conditions and exceptions

Conditions and exceptions in Client Access Rules identify the client connections that the rule is applied to or not applied to. For example, if the rule blocks access by Exchange ActiveSync clients, you can configure the rule to allow Exchange ActiveSync connections from a specific range of IP addresses. The syntax is the same for a condition and the corresponding exception. The only difference is conditions specify client connections to include, while exceptions specify client connections to exclude.

This table describes the conditions and exceptions that are available in Client Access Rules:

> [!NOTE]
> Microsoft sync technology that's used in Outlook for iOS and Android and Windows Mail isn't available in Client Access Rules.

|Condition parameter in Exchange Online PowerShell|Exception parameter in Exchange Online PowerShell|Description|
|---|---|---|
|_AnyOfAuthenticationTypes_|_ExceptAnyOfAuthenticationTypes_|Valid values are: <br/>• `AdfsAuthentication` <br/>• `BasicAuthentication` <br/>• `CertificateBasedAuthentication` <br/>• `NonBasicAuthentication` <br/>• `OAuthAuthentication` <br/> You can specify multiple values separated by commas. You can use quotation marks around each individual value ("_value1_","_value2_"), but not around all values (don't use "_value1_,_value2_").|
|_AnyOfClientIPAddressesOrRanges_|_ExceptAnyOfClientIPAddressesOrRanges_|Valid values are: <br/>• **A single IP address**: For example, `192.168.1.1`. <br/>• **An IP address range**: For example, `192.168.0.1-192.168.0.254`. <br/>• **Classless Inter-Domain Routing (CIDR) IP**: For example, `192.168.3.1/24`. <br/> You can specify multiple values separated by commas.|
|_AnyOfProtocols_|_ExceptAnyOfProtocols_|Valid values are: <br/>• `ExchangeActiveSync` <br/>• `ExchangeAdminCenter` <br/>• `ExchangeWebServices` <br/>• `IMAP4` <br/>• `OfflineAddressBook` <br/>• `OutlookAnywhere` (includes MAPI over HTTP) <br/>• `OutlookWebApp` (Outlook on the web) <br/>• `POP3` <br/>• `PowerShellWebServices` <br/>• `RemotePowerShell` <br/>• `REST` <br/> You can specify multiple values separated by commas. You can use quotation marks around each individual value (" _value1_","_value2_"), but not around all values (don't use "_value1_,_value2_"). <br/> **Note**: If you don't use this condition in a rule, the rule is applied to *all* protocols.|
|_Scope_|n/a| Specifies the type of connections that the rule applies to. Valid values are: <br/>• `Users`: The rule only applies to end-user connections. <br/>• `All`: The rule applies to all types of connections (end-users and middle-tier apps).|
|_UsernameMatchesAnyOfPatterns_|_ExceptUsernameMatchesAnyOfPatterns_|Accepts text and the wildcard character (\*) to identify the user's account name in the format `<Domain>\<UserName>` (for example, `contoso.com\jeff` or `*jeff*`, but not `jeff*`). Non-alphanumeric characters don't require an escape character. <br/> You can specify multiple values separated by commas.|
|_UserRecipientFilter_|n/a| Uses OPath filter syntax to identify the user that the rule applies to. For example, `"City -eq 'Redmond'"`. The filterable attributes are: <br/>• `City` <br/>• `Company` <br/>• `CountryOrRegion` <br/>• `CustomAttribute1` to `CustomAttribute15` <br/>• `Department` <br/>• `Office` <br/>• `PostalCode` <br/>• `StateOrProvince` <br/>• `StreetAddress` <br/> The search criteria uses the syntax `"<Property> -<Comparison operator> '<Value>'"`. <br/>• `<Property>` is a filterable property. <br/>• `-<Comparison Operator>` is an OPATH comparison operator. For example `-eq` for exact matches (wildcards are not supported) and `-like` for string comparison (which requires at least one wildcard in the property value). For more information about comparison operators, see [about_Comparison_Operators](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_comparison_operators). <br/>• `<Value>` is the property value. Text values with or without spaces or values with wildcards (\*) need to be enclosed in quotation marks (for example, `'<Value>'` or `'*<Value>'`). Don't use quotation marks with the system value `$null` (for blank values). <br/> You can chain multiple search criteria together using the logical operators `-and` and `-or`. For example, `"<Criteria1> -and <Criteria2>"` or `"(<Criteria1> -and <Criteria2>) -or <Criteria3>"`. For more information about OPATH filter syntax, see [Additional OPATH syntax information](https://docs.microsoft.com/powershell/exchange/recipient-filters#additional-opath-syntax-information).|
