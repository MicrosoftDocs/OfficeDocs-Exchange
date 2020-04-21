---
localization_priority: Normal
description: 'Summary: Learn how to configure the Managed Folder Assistant in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 9fcfb9b6-bd24-4218-a163-bc599cd5476a
ms.reviewer:
title: Configure and run the Managed Folder Assistant in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Configure and run the Managed Folder Assistant in Exchange Server

The *Managed Folder Assistant* (MFA) is an Exchange Mailbox Assistant that applies and processes the message retention settings that are configured in retention policies.

As in Exchange 2013, the Managed Folder Assistant in Exchange 2016 and Exchange 2019 is a throttle-based assistant that's always running. The MFA doesn't need to be scheduled, and the system resources that are consumed by the MFA can be throttled. You can configure the Managed Folder Assistant to process all mailboxes on a Mailbox server within a certain time period that's known as a *work cycle*. By default, the work cycle for the MFA is one day (all mailboxes on the server are processed by the MFA every day).

You can also force the MFA to immediately process a specified mailbox.

## What do you need to know before you begin?

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/exchange-server/open-the-exchange-management-shell).

- Although the _ManagedFolderAssistantSchedule_ parameter is available in Exchange Server, it doesn't work on Exchange 2016 or Exchange 2019 servers. It's only used for coexistence with previous versions of Exchange.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions in Exchange Server](../../permissions/feature-permissions/policy-and-compliance-permissions.md) topic.

## Configure the Managed Folder Assistant

Configuring the interval for when the MFA processes mailboxes is a two-step process:

1. Configure the work cycle for the MFA.

2. Apply the new work cycle value for the MFA.

### Step 1: Use the Exchange Management Shell to configure the work cycle for the Managed Folder Assistant

To configure the work cycle for the MFA, use this syntax:

```PowerShell
New-SettingOverride -Name "<UniqueOverrideName>" -Component TimeBasedAssistants -Section ELCAssistant -Parameters @("WorkCycle=<Timespan>") -Reason "<DescriptiveReason>" [-Server <ServerName>]
```

 **Notes:**

- To specify a _\<TimeSpan\>_ value, use the syntax `d.hh:mm:ss`, where _d_ = days, _hh_ = hours, _mm_ = minutes, and _ss_ = seconds.

- To configure the same work cycle for the MFA on all Exchange 2016 and Exchange 2019 Mailbox servers in the Active Directory forest, don't use the _Server_ parameter.

- To configure the work cycle for the MFA on a specific Exchange 2016 and Exchange 2019 Mailbox server, use the _Server_ parameter and the name (not the fully qualified domain name or FQDN) of the server. This method is useful when you need to specify different work cycle values for the MFA on different Exchange servers.

This example configures the work cycle for the MFA to two days (the MFA processes mailboxes every two days). Because we aren't using the _Server_ parameter, the setting is applied to all Exchange 2016 and Exchange 2019 Mailbox servers in the organization.

- **Setting override name**: "MFA WorkCycle Override" (must be unique)

- **WorkCycle**: `2.00:00:00` (2 days; note the value `2`also works)

- **Override reason**: Process mailboxes every 2 days

```PowerShell
New-SettingOverride -Name "MFA WorkCycle Override" -Component TimeBasedAssistants -Section ELCAssistant -Parameters @("WorkCycle=2.00:00:00") -Reason "Process mailboxes every 2 days"
```

This example specifies the same 2 day work cycle for the MFA, but only on the server named Mailbox01.

```PowerShell
New-SettingOverride -Name "Mailbox01 MFA WorkCycle Override" -Component TimeBasedAssistants -Section ELCAssistant -Parameters @("WorkCycle=2.00:00:00") -Reason "Process mailboxes every 2 days" -Server Mailbox01
```

### Step 2: Use the Exchange Management Shell to apply the new the work cycle value for the Managed Folder Assistant

To apply the new the work cycle value for the MFA, use this syntax:

```PowerShell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh [-Server <ServerName>]
```

 **Notes:**

- If you didn't use the _Server_ parameter in Step 1, don't use it here. If you used the _Server_ parameter in Step 1, use the same server name here.

- If you delete the custom work cycle value for the MFA by using the **Remove-SettingOverride** cmdlet, you still need to run this command to change the work cycle back to the default value of one day.

This example applies the new work cycle value for the MFA on all Exchange 2016 and Exchange 2019 Mailbox servers in the organization.

```PowerShell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh
```

This example applies the new work cycle value for the MFA on the server named Mailbox01.

```PowerShell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh -Server Mailbox01
```

### How do you know this worked?

To verify that you've successfully configured the work cycle for the Managed Folder Assistant on one or more servers, replace _\<ServerName\>_ with the name of the server (not the FQDN), and run the following command to verify the value of the **WorkCycle** property:

```PowerShell
[xml]$diag=Get-ExchangeDiagnosticInfo -Server <ServerName> -Process MSExchangeMailboxAssistants -Component VariantConfiguration -Argument "Config,Component=TimeBasedAssistants"
$diag.Diagnostics.Components.VariantConfiguration.Configuration.TimeBasedAssistants.ElcAssistant
```

## Use the Exchange Management Shell to start the Managed Folder Assistant on a specific mailbox

To trigger the MFA to immediately process a mailbox, use this syntax:

```PowerShell
Start-ManagedFolderAssistant -Identity <MailboxIdentity>
```

This example triggers the Managed Folder Assistant to immediately process Morris Cornejo's mailbox.

```PowerShell
Start-ManagedFolderAssistant -Identity morris.cornejo@contoso.com
```

For detailed syntax and parameter information, see [Start-ManagedFolderAssistant](https://docs.microsoft.com/powershell/module/exchange/policy-and-compliance-retention/start-managedfolderassistant).
