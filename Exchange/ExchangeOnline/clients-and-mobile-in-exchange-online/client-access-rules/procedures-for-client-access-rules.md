---
localization_priority: Normal
description: 'Summary: Learn how to view, create, modify, delete, and test Client Access Rules in Exchange Online.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: a0e11307-3577-499f-b407-6fcfae576844
ms.reviewer: 
title: Procedures for Client Access Rules in Exchange Online
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
f1.keywords:
- NOCSH
manager: serdars

---

# Procedures for Client Access Rules in Exchange Online

 **Summary**: Learn how to view, create, modify, delete, and test Client Access Rules in Exchange Online.

Client Access Rules allow or block client connections to your Exchange Online organization based on the properties of the connection. For more information about Client Access Rules, see [Client Access Rules in Exchange Online](client-access-rules.md).

> [!TIP]
> Verify that your rules work the way you expect. Be sure to thoroughly test each rule and the interactions between rules. For more information, see the [Use Exchange Online PowerShell to test Client Access Rules](#use-exchange-online-powershell-to-test-client-access-rules) section later in this topic.

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes.

- The procedures in this topic are only available in Exchange Online PowerShell. To learn how to use Windows PowerShell to connect to Exchange Online, see [Connect to Exchange Online PowerShell](https://go.microsoft.com/fwlink/p/?linkid=396554).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to view Client Access Rules

To return a summary list of all Client Access Rules, run this command:

```PowerShell
Get-ClientAccessRule
```

To return detailed information about a specific rule, use this syntax:

```PowerShell
Get-ClientAccessRule -Identity "<RuleName>" | Format-List [<Specific properties to view>]
```

This example returns all the property values for the rule named "Block Client Connections from 192.168.1.0/24".

```PowerShell
Get-ClientAccessRule -Identity "Block Client Connections from 192.168.1.0/24" | Format-List
```

This example returns only the specified properties for the same rule.

```PowerShell
Get-ClientAccessRule -Identity "Block Client Connections from 192.168.1.0/24" | Format-List Name,Priority,Enabled,Scope,Action
```

For detailed syntax and parameter information, see [Get-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/get-clutter).

## Use Exchange Online PowerShell to create Client Access Rules

To create Client Access Rules in Exchange Online PowerShell, use this syntax:

```PowerShell
New-ClientAccessRule -Name "<RuleName>" [-Priority <PriorityValue>] [-Enabled <$true | $false>] -Action <AllowAccess | DenyAccess> [<Conditions>] [<Exceptions>]
```

This example creates a new Client Access Rule named Block ActiveSync that blocks access for Exchange ActiveSync clients, except for clients in the IP address range 192.168.10.1/24.

```PowerShell
New-ClientAccessRule -Name "Block ActiveSync" -Action DenyAccess -AnyOfProtocols ExchangeActiveSync -ExceptAnyOfClientIPAddressesOrRanges 192.168.10.1/24
```

 **Notes**:

- As a best practice, create a Client Access Rule with the highest priority to preserve your administrator access to remote PowerShell. For example: `New-ClientAccessRule -Name "Always Allow Remote PowerShell" -Action Allow -AnyOfProtocols RemotePowerShell -Priority 1`.

- The rule has the default priority value, because we didn't use the _Priority_ parameter. For more information, see the [Use Exchange Online PowerShell to set the priority of Client Access Rules](#use-exchange-online-powershell-to-set-the-priority-of-client-access-rules)  section later in this topic.

- The rule is enabled, because we didn't use the _Enabled_ parameter, and the default value is `$true`.

This example creates a new Client Access Rule named Restrict EAC Access that blocks access for the Exchange admin center, except if the client is coming from an IP address in the 192.168.10.1/24 range or if the user account name contains "tanyas".

```PowerShell
New-ClientAccessRule -Name "Restrict EAC Access" -Action DenyAccess -AnyOfProtocols ExchangeAdminCenter -ExceptAnyOfClientIPAddressesOrRanges 192.168.10.1/24 -ExceptUsernameMatchesAnyOfPatterns *tanyas*
```

For detailed syntax and parameter information, see [New-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/new-clientaccessrule).

### How do you know this worked?

To verify that you've successfully created a Client Access Rule, use any of these procedures:

- Run this command in Exchange Online PowerShell to see the new rule in the list of rules:

  ```PowerShell
  Get-ClientAccessRule
  ```

- Replace _\<RuleName\>_ with the name of the rule, and run this command to see the details of the rule:

  ```PowerShell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List
  ```

- See which Client Access Rules would affect a specific client connection to Exchange Online by using the **Test-ClientAccessRule** cmdlet. For more information, see the [Use Exchange Online PowerShell to test Client Access Rules](#use-exchange-online-powershell-to-test-client-access-rules) section later in this topic.

## Use Exchange Online PowerShell to modify Client Access Rules

No additional settings are available when you modify a Client Access Rule. They're the same settings that were available when you created the rule.

To modify a Client Access Rule in Exchange Online PowerShell, use this syntax:

```PowerShell
Set-ClientAccessRule -Identity "<RuleName>" [-Name "<NewName>"] [-Priority <PriorityValue>] [-Enabled <$true | $false>] -Action <AllowAccess | DenyAccess> [<Conditions>] [<Exceptions>]
```

This example disables the existing Client Access Rule named Allow IMAP4.

```PowerShell
Set-ClientAccessRule -Identity "Allow IMAP4" -Enabled $false
```

An important consideration when you modify Client Access Rules is modifying conditions or exceptions that accept multiple values:

- The values that you specify will *replace* any existing values.

- To add or remove values without affecting other existing values, use this syntax: `@{Add="<Value1>","<Value2>"...; Remove="<Value1>","<Value2>"...}`

This example adds the IP address range 172.17.17.27/16 to the existing Client Access Rule named Allow IMAP4 without affecting the existing IP address values.

```PowerShell
Set-ClientAccessRule -Identity "Allow IMAP4" -AnyOfClientIPAddressesOrRanges @{Add="172.17.17.27/16"}
```

For detailed syntax and parameter information, see [Set-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/set-clutter).

### How do you know this worked?

To verify that you've successfully modified a Client Access Rule, use any of these procedures:

- Replace _\<RuleName\>_ with the name of the rule, and run this command to see the details of the rule:

  ```PowerShell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List
  ```

- See which Client Access Rules would affect a specific client connection to Exchange Online by using the **Test-ClientAccessRule** cmdlet. For more information, see the [Use Exchange Online PowerShell to test Client Access Rules](#use-exchange-online-powershell-to-test-client-access-rules) section later in this topic.

## Use Exchange Online PowerShell to set the priority of Client Access Rules

By default, Client Access Rules are given a priority that's based on the order they were created in (newer rules are lower priority than older rules). A lower priority number indicates a higher priority for the rule, and rules are processed in priority order (higher priority rules are processed before lower priority rules). No two rules can have the same priority.

The highest priority you can set on a rule is 1. The lowest value you can set depends on the number of rules. For example, if you have five rules, you can use the priority values 1 through 5. Changing the priority of an existing rule can have a cascading effect on other rules. For example, if you have five rules (priorities 1 through 5), and you change the priority of a rule from 5 to 2, the existing rule with priority 2 is changed to priority 3, the rule with priority 3 is changed to priority 4, and the rule with priority 4 is changed to priority 5.

To set the priority of a Client Access Rule in Exchange Online PowerShell, use this syntax:

```PowerShell
Set-ClientAccessRule -Identity "<RuleName>" -Priority <Number>
```

This example sets the priority of the rule named Disable IMAP4 to 2. All existing rules that have a priority less than or equal to 2 are decreased by 1 (their priority numbers are increased by 1).

```PowerShell
Set-ClientAccessRule -Identity "Disable IMAP" -Priority 2
```

 **Note**: To set the priority of a new rule when you create it, use the _Priority_ parameter on the **New-ClientAccessRule** cmdlet.

### How do you know this worked?

To verify that you've successfully set the priority of a Client Access Rule, use either of these procedures:

- Run the this command in Exchange Online PowerShell to see the list of rules and their **Priority** values:

  ```PowerShell
  Get-ClientAccessRule
  ```

- Replace _\<RuleName\>_ with the name of the rule, and run this command:

  ```PowerShell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List Name,Priority
  ```

## Use Exchange Online PowerShell to remove Client Access Rules

To remove Client Access Rules in Exchange Online PowerShell, use this syntax:

```PowerShell
Remove-ClientAccessRule -Identity "<RuleName>"
```

This example removes the Client Access Rule named Block POP3.

```PowerShell
Remove-ClientAccessRule -Identity "Block POP3"
```

 **Note**: To disable a Client Access Rule without deleting it, use the _Enabled_ parameter with the value `$false` on the **Set-ClientAccessRule** cmdlet.

For detailed syntax and parameter information, see [Remove-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/remove-clientaccessrule).

### How do you know this worked?

To verify that you've successfully removed a Client Access Rule, run this command in Exchange Online PowerShell to verify that the rule is no longer listed:

```PowerShell
Get-ClientAccessRule
```

## Use Exchange Online PowerShell to test Client Access Rules

To see which Client Access Rules would affect a specific client connection to Exchange Online, use this syntax:

```PowerShell
Test-ClientAccessRule -User <MailboxIdentity> -AuthenticationType <AuthenticationType> -Protocol <Protocol> -RemoteAddress <ClientIPAddress> -RemotePort <TCPPortNumber>
```

This example returns the Client Access Rules that would match a client connection to Exchange Online that has these properties:

- **Authentication type**: Basic

- **Protocol**: `OutlookWebApp`

- **Remote address**: 172.17.17.26

- **Remote port**: 443

- **User**: julia@contoso.com

```PowerShell
Test-ClientAccessRule -User julia@contoso.com -AuthenticationType BasicAuthentication -Protocol OutlookWebApp -RemoteAddress 172.17.17.26 -RemotePort 443
```

For detailed syntax and parameter information, see [Test-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/test-clientaccessrule).
