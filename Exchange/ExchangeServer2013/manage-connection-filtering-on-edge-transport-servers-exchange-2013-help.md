---
title: 'Manage Connection Filtering on Edge Transport Servers: Exchange 2013 Help'
TOCTitle: Manage Connection Filtering on Edge Transport Servers
ms:assetid: baebc865-ec3e-48ca-ac48-7aac8b34c003
ms:mtpsurl: https://technet.microsoft.com/library/Bb124376(v=EXCHG.150)
ms:contentKeyID: 61200297
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage Connection Filtering on Edge Transport Servers

_**Applies to:** Exchange Server 2013_

Connection filtering is an anti-spam feature that's provided by the Connection Filtering agent, which is available only on Edge Transport servers in Microsoft Exchange 2013. Connection filtering enables the following features:

- IP Block list

- IP Block List providers

- IP Allow list

- IP Allow List providers

Each of these features can be enabled or disabled separately.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Anti-spam features" entry in the [Anti-spam and anti-malware permissions](anti-spam-and-anti-malware-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable or disable connection filtering

To completely enable or disable connection filtering, you enable or disable the Connection Filtering agent. The change takes effect after you restart the Microsoft Exchange Transport service. When you restart the Microsoft Exchange Transport service on an Edge Transport server, mail flow on the server is temporarily interrupted.

To disable connection filtering, run the following command:

```powershell
Disable-TransportAgent "Connection Filtering Agent"
```

To enable connection filtering, run the following command:

```powershell
Enable-TransportAgent "Connection Filtering Agent"
```

To make the change take effect, restart the Microsoft Exchange Transport service by running the following command:

```powershell
Restart-Service MSExchangeTransport
```

## How do you know this worked?

To verify that you successfully enabled or disabled connection filtering, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-TransportAgent "Connection Filtering Agent" | Format-List Enabled
```

## IP Block list procedures

These procedures apply to the IP Block list that you manually configure. They don't apply to IP Block List providers.

Use the **IPBlockListConfig** cmdlets to view and configure how connection filtering uses the IP Block list. Use the **IPBlockListEntry** cmdlets to view and configure the IP addresses in the IP Block list.

## Use the Shell to view the configuration of the IP Block list

To view the configuration of the IP Block list, run the following command:

```powershell
Get-IPBlockListConfig | Format-List *Enabled,*Response
```

## Use the Shell to enable or disable the IP Block list

To disable the IP Block list, run the following command:

```powershell
Set-IPBlockListConfig -Enabled $false
```

To enable the IP Block list, run the following command:

```powershell
Set-IPBlockListConfig -Enabled $true
```

## How do you know this worked?

To verify that you successfully enabled or disabled the IP Block list, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-IPBlockListConfig | Format-List Enabled
```

## Use the Shell to configure the IP Block list

To configure the IP Block list, use the following syntax:

```powershell
Set-IPBlockListConfig [-ExternalMailEnabled <$true | $false>] [-InternalMailEnabled <$true | $false> -MachineEntryRejectionResponse "<Custom response text>"] [-StaticEntryRejectionResponse "<Custom response text>"]
```

This example configures the IP Block list with the settings as follows:

- The IP Block list filters incoming connections from internal and external mail servers. By default, connections are filtered from external mail servers only (*ExternalMailEnabled* is set to `$true`, and *InternalMailEnabled* is set to `$false`). Non-authenticated connections and authenticated connections from external partners are considered external.

- The custom response text for connections that were filtered by IP addresses that were automatically added to the IP Block list by the sender reputation feature of the Protocol Analysis agent is set to the value "Connection from IP address {0} was rejected by sender reputation."

- The custom response text for connections that were filtered by IP addresses that were manually added to the IP Block list is set to the value "Connection from IP address {0} was rejected by connection filtering."

```powershell
Set-IPBlockListConfig -InternalMailEnabled $true -MachineEntryRejectionResponse "Connection from IP address {0} was rejected by sender reputation." -StaticEntryRejectionResponse "Connection from IP address {0} was rejected by connection filtering."
```

## How do you know this worked?

To verify that you successfully configured the IP Block list, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPBlockListConfig | Format-List *MailEnabled,*Response
```

## Use the Shell to view IP Block list entries

To view all IP Block list entries, run the following command:

```powershell
Get-IPBlockListEntry
```

Note that each IP Block list entry is identified by an integer value. The identity integer is assigned in ascending order when you add entries to the IP Block list and the IP Allow list.

To view a specific IP Block list entry, use the following syntax:

```powershell
Get-IPBlockListEntry <-Identity IdentityInteger | -IPAddress IPAddress>
```

For example, to view the IP Block list entry that contains the IP address 192.168.1.13, run the following command:

```powershell
Get-IPBlockListEntry -IPAddress 192.168.1.13
```

> [!NOTE]
> When you use the <EM>IPAddress</EM> parameter, the resulting IP Block list entry can be an individual IP address, an IP address range, or a Classless InterDomain Routing (CIDR) IP. To use the <EM>Identity</EM> parameter, you specify the integer value that's assigned to the IP Block list entry.

## Use the Shell to add IP Block list entries

To add IP Block list entries, use the following syntax:

```powershell
Add-IPBlockListEntry <-IPAddress IPAddress | -IPRange IP range or CIDR IP> [-ExpirationTime <DateTime>] [-comment "<Descriptive Comment>"]
```

The following example adds the IP Block list entry for the IP address range 192.168.1.10 through 192.168.1.15 and configures the IP Block list entry to expire on July 4, 2014 at 15:00.

```powershell
Add-IPBlockListEntry -IPRange 192.168.1.10-192.168.1.15 -ExpirationTime "7/4/2014 15:00"
```

## How do you know this worked?

To verify that you successfully added an IP Block list entry, run the following command and verify that the new IP Block list entry is displayed.

```powershell
Get-IPBlockListEntry
```

## Use the Shell to remove IP Block list entries

To remove IP Block list entries, use the following syntax:

```powershell
Remove-IPBlockListEntry <IdentityInteger>
```

The following example removes the IP Block list entry that has the *Identity* value 3.

```powershell
Remove-IPBlockListEntry 3
```

The following example removes the IP Block list entry that contains the IP address 192.168.1.12 without using the *Identity* integer value. Note that the IP Block list entry can be an individual IP address or an IP address range.

```powershell
Get-IPBlockListEntry -IPAddress 192.168.1.12 | Remove-IPBlockListEntry
```

## How do you know this worked?

To verify that you successfully removed an IP Block list entry, run the following command and verify that the IP Block list entry you removed is gone.

```powershell
Get-IPBlockListEntry
```

## IP Block List provider procedures

These procedures apply to IP Block List providers. They don't apply to the IP Block list.

Use the **IPBlockListProvidersConfig** cmdlets to view and configure how connection filtering uses all IP Block List providers. Use the **IPBlockListProvider** cmdlets to view, configure, and test IP Block List providers.

## Use the Shell to view the configuration of all IP Block List providers

To view how connection filtering uses all IP Block List providers, run the following command:

```powershell
Get-IPBlockListProvidersConfig | Format-List *Enabled,Bypassed*
```

## Use the Shell to enable or disable all IP Block List providers

To disable all IP Block List providers, run the following command:

```powershell
Set-IPBlockListProvidersConfig -Enabled $false
```

To enable all IP Block List providers, run the following command:

```powershell
Set-IPBlockListProvidersConfig -Enabled $true
```

## How do you know this worked?

To verify that you enabled or disabled all IP Block List providers, run the following command an verify that the value displayed is the value you configured.

```powershell
Get-IPBlockListProvidersConfig | Format-List Enabled
```

## Use the Shell to configure all IP Block List providers

To configure how connection filtering uses all IP Block List providers, use the following syntax:

```powershell
Set-IPBlockListProvidersConfig [-BypassedRecipients <recipient1,recipient2...>] [-ExternalMailEnabled <$true | $false>] [-InternalMailEnabled <$true | $false>]
```

The following example configures all IP Block List providers with the following settings:

- IP Block List providers filter incoming connections from internal and external mail servers. By default, connections are filtered from external mail servers only (*ExternalMailEnabled* is set to `$true`, and *InternalMailEnabled* is set to `$false`). Non-authenticated connections and authenticated connections from external partners are considered external.

- Messages sent to the internal recipients chris@fabrikam.com and michelle@fabrikam.com are excluded from filtering by IP Block List providers. Note that if you want to add recipients to the list without affecting existing recipients, use the syntax, `@{Add="<recipient1>","<recipient2>"...}`.

```powershell
Set-IPBlockListProvidersConfig -BypassedRecipients chris@fabrikam.com,michelle@fabrikam.com -InternalMailEnabled $true
```

For more information, see [Set-IPBlockListProvidersConfig](https://docs.microsoft.com/powershell/module/exchange/Set-IPBlockListProvidersConfig).

## How do you know this worked?

To verify that you successfully configured all IP Block List providers, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPBlockListProvidersConfig | Format-List *MailEnabled,Bypassed*
```

## Use the Shell to view IP Block List providers

To view the summary list of all the IP Block List providers, run the following command:

```powershell
Get-IPBlockListProvider
```

To view the details of a specific provider, use the following syntax:

```powershell
Get-IPBlockListProvider <IPBlockListProviderIdentity>
```

The following example show the details of the provider named Contoso IP Block List Provider.

```powershell
Get-IPBlockListProvider "Contoso IP Block List Provider" | Format-List Name,Enabled,Priority,LookupDomain,*Match,*Response
```

## Use the Shell to add an IP Block List provider

To add an IP Block List provider, use the following syntax:

```powershell
Add-IPBlockListProvider -Name "<Descriptive Name>" -LookupDomain <FQDN> [-Priority <Integer>] [-Enabled <$true | $false>] [-AnyMatch <$true | $false>] [-BitmaskMatch <IPAddress>] [-IPAddressesMatch <IPAddressStatusCode1,IPAddressStatusCode2...>] [-RejectionResponse "<Custom Text>"]
```

This example creates an IP Block List provider named "Contoso IP Block List Provider" with the following options:

- **FQDN to use the provider**: rbl.contoso.com

- **Bitmask code to use from the provider**: 127.0.0.1

```powershell
Add-IPBlockListProvider -Name "Contoso IP Block List Provider" -LookupDomain rbl.contoso.com -BitmaskMatch 127.0.0.1
```

> [!NOTE]
> When you add a new IP Block List provider, it's enabled by default (the value of <EM>Enabled</EM> is <CODE>$true</CODE>), and the priority value is incremented (the first entry has the <EM>Priority</EM> value 1).

For more information, see [Add-IPBlockListProvider](https://docs.microsoft.com/powershell/module/exchange/Add-IPBlockListProvider).

## How do you know this worked?

To verify that you successfully added an IP Block List provider, run the following command and verify that the new IP Block List provider is displayed.

```powershell
Get-IPBlockListProvider
```

## Use the Shell to enable or disable an IP Block List provider

To enable or disable a specific IP Block List provider, use the following syntax:

```powershell
Set-IPBlockListProvider <IPBlockListProviderIdentity> -Enabled <$true | $false>
```

The following example disables the provider named Contoso IP Block List Provider.

```powershell
Set-IPBlockListProvider "Contoso IP Block List Provider" -Enabled $false
```

The following example enables the provider named Contoso IP Block List Provider.

```powershell
Set-IPBlockListProvider "Contoso IP Block List Provider" -Enabled $true
```

## How do you know this worked?

To verify that you successfully enabled or disabled an IP Block List provider, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-IPBlockListProvider <IPBlockListProviderIdentity> | Format-List Enabled
```

## Use the Shell to configure an IP Block List provider

The configuration options that are available on the **Set-IPBlockListProvider** cmdlet are identical to those on the **New-IPBlockListProvider** cmdlet.

To configure an existing IP Block List provider, use the following syntax:

```powershell
Set-IPBlockListProvider <IPBlockListProviderIdentity> -Name "<Descriptive Name>" -LookupDomain <FQDN> [-Priority <Integer>] [-AnyMatch <$true | $false>] [-BitmaskMatch <IPAddress>] [-IPAddressesMatch <IPAddressStatusCode1,IPAddressStatusCode2...>] [-RejectionResponse "<Custom Text>"]
```

For example, to add the IP address status code 127.0.0.1 to the list of existing status codes for the provider named Contoso IP Block List Provider, run the following command:

```powershell
Set-IPBlockListProvider "Contoso IP Block List Provider" -IPAddressesMatch @{Add="127.0.0.1"}
```

For more information, see [Set-IPBlockListProvider](https://docs.microsoft.com/powershell/module/exchange/Set-IPBlockListProvider).

## How do you know this worked?

To verify that you successfully configured an IP Block List provider, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPBlockListProvider <IPBlockListProviderIdentity> | Format-List
```

## Use the Shell to test an IP Block List provider

To test an IP Block List provider, use the following syntax.

```powershell
Test-IPBlockListProvider <IPBlockListProviderIdentity> -IPAddress <IPAddressToTest>
```

The following example tests the provider named Contoso IP Block List Provider by looking up the IP address 192.168.1.1.

```powershell
Test-IPBlockListProvider "Contoso IP Block List Provider" -IPAddress 192.168.1.1
```

## Use the Shell to remove an IP Block List provider

To remove an IP Block List provider, use the following syntax:

```powershell
Remove-IPBlockListProvider <IPBlockListProviderIdentity>
```

The following example removes the IP Block List provider named Contoso IP Block List Provider.

```powershell
Remove-IPBlockListProvider "Contoso IP Block list Provider"
```

## How do you know this worked?

To verify that you successfully removed an IP Block List provider, run the following command and verify that the IP Block List provider you removed is gone.

```powershell
Get-IPBlockListProvider
```

## IP Allow list procedures

These procedures apply to the IP Allow list that you manually configure. They don't apply to IP Allow List providers.

Use the **IPAllowListConfig** cmdlets to view and configure how connection filtering uses the IP Allow list. Use the **IPAllowListEntry** cmdlets to view and configure the IP addresses in the IP Allow list.

## Use the Shell to view the configuration of the IP Allow list

To view the configuration of the IP Allow list, run the following command.

```powershell
Get-IPAllowListConfig | Format-List *Enabled
```

## Use the Shell to enable or disable the IP Allow list

To disable the IP Allow list, run the following command:

```powershell
Set-IPAllowListConfig -Enabled $false
```

To enable the IP Allow list, run the following command:

```powershell
Set-IPAllowListConfig -Enabled $true
```

## How do you know this worked?

To verify that you successfully enabled or disabled the IP Allow list, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-IPAllowListConfig | Format-List *Enabled
```

## Use the Shell to configure the IP Allow list

To configure the IP Allow list, use the following syntax:

```powershell
Set-IPAllowListConfig [-ExternalMailEnabled <$true | $false>] [-InternalMailEnabled <$true | $false>
```

This example configures the IP Allow list to filter incoming connections from internal and external mail servers. By default, connections are filtered from external mail servers only (*ExternalMailEnabled* is set to `$true`, and *InternalMailEnabled* is set to `$false`). Non-authenticated connections and authenticated connections from external partners are considered external.

```powershell
Set-IPAllowListConfig -InternalMailEnabled $true
```

## How do you know this worked?

To verify that you successfully configured the IP Allow list, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPAllowListConfig | Format-List *MailEnabled
```

## Use the Shell to view IP Allow list entries

To view all IP Allow list entries, run the following command:

```powershell
Get-IPAllowListEntry
```

Note that each IP Allow list entry is identified by an integer value. The identity integer is assigned in ascending order when you add entries to the IP Block list and the IP Allow list.

To view a specific IP Allow list entry, use the following syntax:

```powershell
Get-IPAllowListEntry <-Identity IdentityInteger | -IPAddress IPAddress>
```

For example, to view the IP Allow list entry that contains the IP address 192.168.1.13, run the following command:

```powershell
Get-IPAllowListEntry -IPAddress 192.168.1.13
```

> [!NOTE]
> When you use the <EM>IPAddress</EM> parameter, the resulting IP Allow list entry can be an individual IP address, an IP address range, or a Classless InterDomain Routing (CIDR) IP. To use the <EM>Identity</EM> parameter, you specify the integer value that's assigned to the IP Allow list entry.

## Use the Shell to add IP Allow list entries

To add IP Allow list entries, use the following syntax:

```powershell
Add-IPAllowListEntry <-IPAddress IPAddress | -IPRange IP range or CIDR IP> [-ExpirationTime <DateTime>] [-Comment "<Descriptive Comment>"]
```

This example adds the IP Allow list entry for the IP address range 192.168.1.10 through 192.168.1.15 and configures the IP Allow list entry to expire on July 4, 2014 at 15:00.

```powershell
Add-IPAllowListEntry -IPRange 192.168.1.10-192.168.1.15 -ExpirationTime "7/4/2014 15:00"
```

## How do you know this worked?

To verify that you successfully added an IP Allow list entry, run the following command and verify that the new IP Allow list entry is displayed.

```powershell
Get-IPAllowListEntry
```

## Use the Shell to remove IP Allow list entries

To remove IP Allow list entries, use the following syntax:

```powershell
Remove-IPAllowListEntry <IdentityInteger>
```

The following example removes the IP Allow list entry that has the *Identity* value 3.

```powershell
Remove-IPAllowListEntry 3
```

This example removes the IP Allow list entry that contains the IP address 192.168.1.12 without using the *Identity* integer value. Note that the IP Allow list entry can be an individual IP address or an IP address range.

```powershell
Get-IPAllowListEntry -IPAddress 192.168.1.12 | Remove-IPAllowListEntry
```

## How do you know this worked?

To verify that you successfully removed an IP Allow list entry, run the following command and verify that the IP Allow list entry you removed is gone.

```powershell
Get-IPAllowListEntry
```

## IP Allow List provider procedures

These procedures apply to IP Allow List providers. They don't apply to the IP Allow list.

Use the **IPAllowListProvidersConfig** cmdlets to view and configure how connection filtering uses all IP Allow List providers. Use the **IPAllowListProvider** cmdlets to view, configure, and test IP Allow List providers.

## Use the Shell to view the configuration of all IP Allow List providers

To view how connection filtering uses all IP Allow List providers, run the following command:

```powershell
Get-IPAllowListProvidersConfig | Format-List *Enabled
```

## Use the Shell to enable or disable all IP Allow List providers

To disable all IP Allow List providers, run the following command:

```powershell
Set-IPAllowListProvidersConfig -Enabled $false
```

To enable all IP Allow List providers, run the following command:

```powershell
Set-IPAllowListProvidersConfig -Enabled $true
```

## How do you know this worked?

To verify that you enabled or disabled all IP Allow List providers, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-IPAllowListProvidersConfig | Format-List Enabled
```

## Use the Shell to configure all IP Allow List providers

To configure how connection filtering uses all IP Allow List providers, use the following syntax:

```powershell
Set-IPAllowListProvidersConfig [-ExternalMailEnabled <$true | $false>] [-InternalMailEnabled <$true | $false>]
```

This example configures all IP Allow List providers to filter incoming connections from internal and external mail servers. By default, connections are filtered from external mail servers only (*ExternalMailEnabled* is set to `$true`, and *InternalMailEnabled* is set to `$false`). Non-authenticated connections and authenticated connections from external partners are considered external.

```powershell
Set-IPAllowListProvidersConfig -InternalMailEnabled $true
```

For more information, see [Set-IPBlockListProvidersConfig](https://docs.microsoft.com/powershell/module/exchange/Set-IPBlockListProvidersConfig).

## How do you know this worked?

To verify that you successfully configured all IP Allow List providers, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPAllowListProvidersConfig | Format-List *MailEnabled
```

## Use the Shell to view IP Allow List providers

To view the summary list of all the IP Allow List providers, run the following command.

```powershell
Get-IPAllowListProvider
```

To view the details of a specific provider, use the following syntax.

```powershell
Get-IPAllowListProvider <IPAllowListProviderIdentity>
```

This example show the details of the provider named Contoso IP Allow List Provider.

```powershell
Get-IPAllowListProvider "Contoso IP Allow List Provider" | Format-List Name,Enabled,Priority,LookupDomain,*Match
```

## Use the Shell to add an IP Allow List provider

To add an IP Allow List provider, use the following syntax:

```powershell
Add-IPAllowListProvider -Name "<Descriptive Name>" -LookupDomain <FQDN> [-Priority <Integer>] [-Enabled <$true | $false>] [-AnyMatch <$true | $false>] [-BitmaskMatch <IPAddress>] [-IPAddressesMatch <IPAddressStatusCode1,IPAddressStatusCode2...>]
```

This example creates an IP Allow List provider named "Contoso IP Allow List Provider" with the following options:

- **FQDN to use the provider**: allow.contoso.com

- **Bitmask code to use from the provider**: 127.0.0.1

```powershell
Add-IPAllowListProvider -Name "Contoso IP Allow List Provider" -LookupDomain allow.contoso.com -BitmaskMatch 127.0.0.1
```

> [!NOTE]
> When you add a new IP Allow List provider, it's enabled by default (the value of <EM>Enabled</EM> is <CODE>$true</CODE>), and the priority value is incremented (the first entry has the <EM>Priority</EM> value 1).

For more information, see [Add-IPBlockListProvider](https://docs.microsoft.com/powershell/module/exchange/Add-IPBlockListProvider).

## How do you know this worked?

To verify that you successfully added an IP Allow List provider, run the following command and verify that the new IP Allow List provider is displayed.

```powershell
Get-IPAllowListProvider
```

## Use the Shell to enable or disable an IP Allow List provider

To enable or disable a specific IP Allow List provider, use the following syntax.

```powershell
Set-IPAllowListProvider <IPAllowListProviderIdentity> -Enabled <$true | $false>
```

This example disables the provider named Contoso IP Allow List Provider.

```powershell
Set-IPAllowListProvider "Contoso IP Allow List Provider" -Enabled $false
```

This example enables the provider named Contoso IP Allow List Provider.

```powershell
Set-IPAllowListProvider "Contoso IP Allow List Provider" -Enabled $true
```

## How do you know this worked?

To verify that you successfully enabled or disabled an IP Allow List provider, run the following command and verify that the value displayed is the value you configured.

```powershell
Get-IPAllowListProvider <IPAllowListProviderIdentity> | Format-List Enabled
```

## Use the Shell to configure an IP Allow List provider

The configuration options that are available on the **Set-IPAllowListProvider** cmdlet are identical to those on the **New-IPAllowListProvider** cmdlet.

To configure an existing IP Allow List provider, use the following syntax:

```powershell
Set-IPAllowListProvider <IPAllowListProviderIdentity> -Name "<Descriptive Name>" -LookupDomain <FQDN> [-Priority <Integer>] [-AnyMatch <$true | $false>] [-BitmaskMatch <IPAddress>] [-IPAddressesMatch <IPAddressStatusCode1,IPAddressStatusCode2...>]
```

For example, to add the IP address status code 127.0.0.1 to the list of existing status codes for the provider named Contoso IP Allow List Provider, run the following command:

```powershell
Set-IPAllowListProvider "Contoso IP Allow List Provider" -IPAddressesMatch @{Add="127.0.0.1"}
```

For more information, see [Set-IPBlockListProvider](https://docs.microsoft.com/powershell/module/exchange/Set-IPBlockListProvider).

## How do you know this worked?

To verify that you successfully configured an IP Allow List provider, run the following command and verify that the values displayed are the values you configured.

```powershell
Get-IPAllowListProvider <IPAllowListProviderIdentity> | Format-List
```

## Use the Shell to test an IP Allow List provider

To test an IP Allow List provider, use the following syntax:

```powershell
Test-IPAllowListProvider <IPAllowListProviderIdentity> -IPAddress <IPAddressToTest>
```

The following example tests the provider named Contoso IP Allow List Provider by looking up the IP address 192.168.1.1.

```powershell
Test-IPAllowListProvider "Contoso IP Allow List Provider" -IPAddress 192.168.1.1
```

## Use the Shell to remove an IP Allow List provider

To remove an IP Allow List provider, use the following syntax:

```powershell
Remove-IPAllowListProvider <IPAllowListProviderIdentity>
```

This example removes the IP Allow List provider named Contoso IP Allow List Provider.

```powershell
Remove-IPAllowListProvider "Contoso IP Allow List Provider"
```

## How do you know this worked?

To verify that you successfully removed an IP Allow List provider, run the following command and verify that the IP Allow List provider you removed is gone.

```powershell
Get-IPAllowListProvider
```
