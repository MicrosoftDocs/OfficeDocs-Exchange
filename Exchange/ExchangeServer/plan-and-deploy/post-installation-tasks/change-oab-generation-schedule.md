---
localization_priority: Normal
ms.topic: get-started-article
author: mattpennathe3rd
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.Mailbox.OfflineAddressBookGeneralPage
ms.author: v-mapenn
ms.assetid: d2b4d527-311e-442d-9f1f-54fac8371b80
ms.reviewer: 
description: 'Summary: Learn how to configure the offline address book (OAB) update interval in Exchange Server 2016 or Exchange Server 2019.'
title: Change the offline address book generation schedule in Exchange
ms.collection:
- Strat_EX_Admin
- exchange-server
f1.keywords:
- CSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Change the offline address book generation schedule in Exchange

An offline address book (OAB) is a copy of an address book that's been downloaded so that an Outlook user can access the information it contains while disconnected from the server. By default, a new OAB is generated every 8 hours in Exchange Server 2016 and Exchange Server 2019, but you can change the interval by using the Exchange Management Shell.

For additional management tasks related to OABs, see [Procedures for offline address books in Exchange Server](../../email-addresses-and-address-books/offline-address-books/oab-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete this procedure: 5 minutes.

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Email address and address book permissions](../../permissions/feature-permissions/address-book-permissions.md) topic.

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at: [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612), [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542), or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Change the offline address book generation schedule

Changing the OAB generation schedule is a two-step process:

1. Change the OAB generation schedule.

2. Apply the new OAB generation schedule.

### Step 1: Use the Exchange Management Shell to change the OAB generation schedule

To change the OAB generation schedule, use this syntax:

```powershell
New-SettingOverride -Name "<UniqueOverrideName>" -Component TimeBasedAssistants -Section OABGeneratorAssistant -Parameters @("WorkCycle=<Timespan>") -Reason "<DescriptiveReason>" [-Server <ServerName>]
```

 **Notes:**

- To specify a _\<TimeSpan\>_ value, use the syntax `d.hh:mm:ss`, where _d_ = days, _hh_ = hours, _mm_ = minutes, and _ss_ = seconds.

- To configure the OAB generation schedule on all Exchange 2016 and Exchange 2019 Mailbox servers in the Active Directory forest, don't use the _Server_ parameter.

- To configure the OAB generation schedule on a specific Exchange 2016 or Exchange 2019 Mailbox server, use the _Server_ parameter and the name (not the fully qualified domain name or FQDN) of the server. This method is useful when you need to specify different OAB generation schedules on different Exchange servers.

- In Exchange 2016 Cumulative Update 3 (CU3) or earlier, the _Component_ parameter value is `MailboxAssistants`.

This example specifies that the OAB is generated every two hours on all Exchange 2016 and Exchange 2019 servers in the organization that are responsible for generating OABs.

- **Setting override name**: "OAB Generation Override" (must be unique)

- **WorkCycle**: `02:00:00` (2 hours)

- **Override reason**: Generate OAB every 2 hours

```powershell
New-SettingOverride -Name "OAB Generation Override" -Component TimeBasedAssistants -Section OABGeneratorAssistant -Parameters @("WorkCycle=02:00:00") -Reason "Generate OAB every 2 hours"
```

This example specifies the same OAB generation schedule, but only on the server named Mailbox01.

```powershell
New-SettingOverride -Name "Mailbox01 OAB Generation Override" -Component TimeBasedAssistants -Section OABGeneratorAssistant -Parameters @("WorkCycle=02:00:00") -Reason "Generate OAB every 2 hours" -Server Mailbox01
```

### Step 2: Use the Exchange Management Shell to apply the new OAB generation schedule

To apply the new OAB generation schedule, use this syntax:

```powershell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh [-Server <ServerName>]
```

 **Notes:**

- If you didn't use the _Server_ parameter in Step 1, don't use it here. If you used the _Server_ parameter in Step 1, use the same server name here.

- If you delete the custom OAB generation schedule by using the **Remove-SettingOverride** cmdlet, you still need to run this command to change the generation schedule back to the default value of 8 hours.

This example applies the new OAB generation schedule on all Exchange 2016 and Exchange 2019 Mailbox servers in the organization.

```powershell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh
```

This example applies the new OAB generation schedule on the server named Mailbox01.

```powershell
Get-ExchangeDiagnosticInfo -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Refresh -Server Mailbox01
```

### How do you know this worked?

To verify that you've configured the OAB generation schedule on one or more Exchange servers, replace _\<ServerName\>_ with the name of the server (not the FQDN), and run the following command to verify the value of the **WorkCycle** property:

```powershell
[xml]$diag=Get-ExchangeDiagnosticInfo -Server <ServerName> -Process MSExchangeMailboxAssistants -Component VariantConfiguration -Argument "Config,Component=TimeBasedAssistants"; $diag.Diagnostics.Components.VariantConfiguration.Configuration.TimeBasedAssistants.OABGeneratorAssistant
```

 **Note**: In Exchange 2016 CU3 or earlier, you need to run this command instead: `[xml]$diag=Get-ExchangeDiagnosticInfo -Server <ServerName> -Process Microsoft.Exchange.Directory.TopologyService -Component VariantConfiguration -Argument Config; $diag.Diagnostics.Components.VariantConfiguration.Configuration.MailboxAssistants.OABGeneratorAssistant`.

## See also

[Procedures for offline address books in Exchange Server](../../email-addresses-and-address-books/offline-address-books/oab-procedures.md)
