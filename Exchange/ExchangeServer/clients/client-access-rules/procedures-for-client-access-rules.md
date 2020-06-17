---
localization_priority: Normal
description: 'Summary: Learn how to view, create, modify, delete, and test Client Access Rules in Exchange 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
monikerRange: exchserver-2019
title: Procedures for Client Access Rules in Exchange 2019
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.service: exchange-server-it-pro
ms.reviewer:
manager: serdars

---

# Procedures for Client Access Rules in Exchange 2019

Client Access Rules allow or block Exchange admin center (EAC) or remote PowerShell connections to your Exchange 2019 organization based on the properties of the connection. For more information about Client Access Rules, see [Client Access Rules in Exchange Server](client-access-rules.md).

> [!TIP]
> Verify that your rules work the way you expect. Be sure to thoroughly test each rule and the interactions between rules. For more information, see the [Use the Exchange Management Shell to test Client Access Rules](#use-the-exchange-management-shell-to-test-client-access-rules) section later in this topic.

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes.

- The procedures in this topic are only available in the Exchange Management Shell. For more information, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell) or [Connect to Exchange servers using remote PowerShell](https://docs.microsoft.com/powershell/exchange/connect-to-exchange-servers-using-remote-powershell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Mail flow" entry in [Mail flow permissions](../../permissions/feature-permissions/mail-flow-permissions.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Exchange Management Shell to view Client Access Rules

To return a summary list of all Client Access Rules, run this command:

```powershell
Get-ClientAccessRule
```

To return detailed information about a specific rule, use this syntax:

```powershell
Get-ClientAccessRule -Identity "<RuleName>" | Format-List [<Specific properties to view>]
```

This example returns all the property values for the rule named "Block Client Connections from 192.168.1.0/24".

```powershell
Get-ClientAccessRule -Identity "Block Client Connections from 192.168.1.0/24" | Format-List
```

This example returns only the specified properties for the same rule.

```powershell
Get-ClientAccessRule -Identity "Block Client Connections from 192.168.1.0/24" | Format-List Name,Priority,Enabled,Scope,Action
```

For detailed syntax and parameter information, see [Get-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/get-clutter).

## Use the Exchange Management Shell to create Client Access Rules

To create Client Access Rules in the Exchange Management Shell, use this syntax:

```powershell
New-ClientAccessRule -Name "<RuleName>" [-Priority <PriorityValue>] [-Enabled <$true | $false>] -Action <AllowAccess | DenyAccess> [<Conditions>] [<Exceptions>]
```

This example creates a new Client Access Rule named Block PowerShell that blocks remote PowerShell access, except for clients in the IP address range 192.168.10.1/24.

```powershell
New-ClientAccessRule -Name "Block PowerShell" -Action DenyAccess -AnyOfProtocols RemotePowerShell -ExceptAnyOfClientIPAddressesOrRanges 192.168.10.1/24
```

 **Notes**:

- As a best practice, create a Client Access Rule with the highest priority to preserve your administrator access to remote PowerShell. For example: `New-ClientAccessRule -Name "Always Allow Remote PowerShell" -Action Allow -AnyOfProtocols RemotePowerShell -Priority 1`.

- The rule has the default priority value, because we didn't use the _Priority_ parameter. For more information, see the [Use the Exchange Management Shell to set the priority of Client Access Rules](#use-the-exchange-management-shell-to-set-the-priority-of-client-access-rules) section later in this topic.

- The rule is enabled, because we didn't use the _Enabled_ parameter, and the default value is `$true`.

This example creates a new Client Access Rule named Restrict EAC Access that blocks access for the Exchange admin center, except if the client is coming from an IP address in the 192.168.10.1/24 range or if the user account name contains "tanyas".

```powershell
New-ClientAccessRule -Name "Restrict EAC Access" -Action DenyAccess -AnyOfProtocols ExchangeAdminCenter -ExceptAnyOfClientIPAddressesOrRanges 192.168.10.1/24 -ExceptUsernameMatchesAnyOfPatterns *tanyas*
```

For detailed syntax and parameter information, see [New-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/new-clientaccessrule).

### How do you know this worked?

To verify that you've successfully created a Client Access Rule, use any of these procedures:

- Run this command in the Exchange Management Shell to see the new rule in the list of rules:

  ```powershell
  Get-ClientAccessRule
  ```

- Replace _\<RuleName\>_ with the name of the rule, and run this command to see the details of the rule:

  ```powershell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List
  ```

- See which Client Access Rules would affect a specific client connection to Exchange by using the **Test-ClientAccessRule** cmdlet. For more information, see the [Use the Exchange Management Shell to test Client Access Rules](#use-the-exchange-management-shell-to-test-client-access-rules) section later in this topic.

## Use the Exchange Management Shell to modify Client Access Rules

No additional settings are available when you modify a Client Access Rule. They're the same settings that were available when you created the rule.

To modify a Client Access Rule in the Exchange Management Shell, use this syntax:

```powershell
Set-ClientAccessRule -Identity "<RuleName>" [-Name "<NewName>"] [-Priority <PriorityValue>] [-Enabled <$true | $false>] -Action <AllowAccess | DenyAccess> [<Conditions>] [<Exceptions>]
```

This example disables the existing Client Access Rule named Allow EAC.

```powershell
Set-ClientAccessRule -Identity "Allow EAC" -Enabled $false
```

An important consideration when you modify Client Access Rules is modifying conditions or exceptions that accept multiple values:

- The values that you specify will *replace* any existing values.

- To add or remove values without affecting other existing values, use this syntax: `@{Add="<Value1>","<Value2>"...; Remove="<Value1>","<Value2>"...}`

This example adds the IP address range 172.17.17.27/16 to the existing Client Access Rule named Allow EAC without affecting the existing IP address values.

```powershell
Set-ClientAccessRule -Identity "Allow EAC" -AnyOfClientIPAddressesOrRanges @{Add="172.17.17.27/16"}
```

For detailed syntax and parameter information, see [Set-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/set-clientaccessrule).

### How do you know this worked?

To verify that you've successfully modified a Client Access Rule, use any of these procedures:

- Replace _\<RuleName\>_ with the name of the rule, and run this command to see the details of the rule:

  ```powershell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List
  ```

- See which Client Access Rules would affect a specific client connection to Exchange by using the **Test-ClientAccessRule** cmdlet. For more information, see the [Use the Exchange Management Shell to test Client Access Rules](#use-the-exchange-management-shell-to-test-client-access-rules) section later in this topic.

## Use the Exchange Management Shell to set the priority of Client Access Rules

By default, Client Access Rules are given a priority that's based on the order they were created in (newer rules are lower priority than older rules). A lower priority number indicates a higher priority for the rule, and rules are processed in priority order (higher priority rules are processed before lower priority rules). No two rules can have the same priority.

The highest priority you can set on a rule is 1. The lowest value you can set depends on the number of rules. For example, if you have five rules, you can use the priority values 1 through 5. Changing the priority of an existing rule can have a cascading effect on other rules. For example, if you have five rules (priorities 1 through 5), and you change the priority of a rule from 5 to 2, the existing rule with priority 2 is changed to priority 3, the rule with priority 3 is changed to priority 4, and the rule with priority 4 is changed to priority 5.

To set the priority of a Client Access Rule in the Exchange Management Shell, use this syntax:

```powershell
Set-ClientAccessRule -Identity "<RuleName>" -Priority <Number>
```

This example sets the priority of the rule named Disable PowerShell to 3. All existing rules that have a priority less than or equal to 3 are decreased by 1 (their priority numbers are increased by 1).

```powershell
Set-ClientAccessRule -Identity "Disable PowerShell" -Priority 4
```

 **Note**: To set the priority of a new rule when you create it, use the _Priority_ parameter on the **New-ClientAccessRule** cmdlet.

### How do you know this worked?

To verify that you've successfully set the priority of a Client Access Rule, use either of these procedures:

- Run the this command in the Exchange Management Shell to see the list of rules and their **Priority** values:

  ```powershell
  Get-ClientAccessRule
  ```

- Replace _\<RuleName\>_ with the name of the rule, and run this command:

  ```powershell
  Get-ClientAccessRule -Identity "<RuleName>" | Format-List Name,Priority
  ```

## Use the Exchange Management Shell to remove Client Access Rules

To remove Client Access Rules in the Exchange Management Shell, use this syntax:

```powershell
Remove-ClientAccessRule -Identity "<RuleName>"
```

This example removes the Client Access Rule named Block EAC.

```powershell
Remove-ClientAccessRule -Identity "Block EAC"
```

 **Note**: To disable a Client Access Rule without deleting it, use the _Enabled_ parameter with the value `$false` on the **Set-ClientAccessRule** cmdlet.

For detailed syntax and parameter information, see [Remove-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/remove-clientaccessrule).

### How do you know this worked?

To verify that you've successfully removed a Client Access Rule, run this command in the Exchange Management Shell to verify that the rule is no longer listed:

```powershell
Get-ClientAccessRule
```

## Use the Exchange Management Shell to test Client Access Rules

To see which Client Access Rules would affect a specific client connection to Exchange, use this syntax:

```powershell
Test-ClientAccessRule -User <MailboxIdentity> -AuthenticationType <AuthenticationType> -Protocol <Protocol> -RemoteAddress <ClientIPAddress> -RemotePort <TCPPortNumber>
```

This example returns the Client Access Rules that would match a client connection to Exchange that has these properties:

- **Authentication type**: Basic

- **Protocol**: `ExchangeAdminCenter`

- **Remote address**: 172.17.17.26

- **Remote port**: 443

- **User**: julia@contoso.com

```powershell
Test-ClientAccessRule -User julia@contoso.com -AuthenticationType BasicAuthentication -Protocol ExchangeAdminCenter -RemoteAddress 172.17.17.26 -RemotePort 443
```

For detailed syntax and parameter information, see [Test-ClientAccessRule](https://docs.microsoft.com/powershell/module/exchange/test-clientaccessrule).
