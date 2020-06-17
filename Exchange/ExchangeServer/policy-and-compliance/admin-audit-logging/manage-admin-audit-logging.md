---
localization_priority: Normal
description: 'Summary: Learn how to configure, enable, and disable administrator audit logging in Exchange Server, and how to view the admit audit log settings.'
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 15c284c0-b8e6-42ca-9913-7c59fdb6885d
ms.reviewer:
title: Manage administrator audit logging
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Manage administrator audit logging

Administrator audit logging in Exchange Server enables you to create a log entry each time a specified cmdlet is run. Log entries provide you with information about what cmdlet was run, which parameters were used, who ran the cmdlet, and what objects were affected. For more information about administrator audit logging, see [Administrator audit logging in Exchange Server](admin-audit-logging.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes

- You can only use PowerShell to perform this procedure. To learn how to open the Exchange Management Shell in your on-premises Exchange organization, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Administrator audit logging" entry in the [Exchange infrastructure and PowerShell permissions](../../permissions/feature-permissions/infrastructure-permissions.md) topic.

- Admin audit logging relies on Active Directory replication to replicate the configuration settings you specify to the domain controllers in your organization. Depending on your replication settings, the changes you make may not be immediately applied to all Exchange 2016 and Exchange 2019 servers in your organization.

- Changes to the audit log configuration are refreshed every 60 minutes on computers that have the Exchange Management Shell open at the time a configuration change is made. If you want to apply the changes immediately, close and then open the Exchange Management Shell again on each computer.

- A command may take up to 15 minutes after it's run to appear in audit log search results. This is because audit log entries must be indexed before they can be searched. If a command doesn't appear in the administrator audit log, wait a few minutes and run the search again.

## Specify the cmdlets to be audited
<a name="cmdlets"> </a>

By default, audit logging creates a log entry for every cmdlet that's run. If you're enabling audit logging for the first time and want this behavior, you don't have to change the cmdlet audit list. If you've previously specified cmdlets to audit and now want to audit all cmdlets, you can audit all cmdlets by specifying the asterisk (\*) wildcard character with the _AdminAuditLogCmdlets_ parameter on the **Set-AdminAuditLogConfig** cmdlet, as shown in the following command.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogCmdlets *
```

You can specify which cmdlets to audit by providing a list of cmdlets using the _AdminAuditLogCmdlets_ parameter. When you provide the list of cmdlets to audit, you can provide single cmdlets, cmdlets with the asterisk (\*) wildcard characters, or a mix of both. Each entry in the list is separated by commas. The following values are all valid:

- `New-Mailbox`

- `*TransportRule`

- `*Management*`

- `Set-Transport*`

This example audits the cmdlets specified in the preceding list.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogCmdlets New-Mailbox, *TransportRule, *Management*, Set-Transport*
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/set-adminauditlogconfig).

## Specify the parameters to be audited
<a name="parameters"> </a>

By default, audit logging creates a log entry for every cmdlet that's run, regardless of the parameters specified. If you're enabling audit logging for the first time and want this behavior, you don't have to change the parameter audit list. If you've previously specified parameters to audit and now want to audit all parameters, you can do so by specifying the asterisk (\*) wildcard character with the _AdminAuditLogParameters_ parameter on the **Set-AdminAuditLogConfig** cmdlet, as shown in the following command.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogParameters *
```

You can specify which parameters you want to audit by using the _AdminAuditLogParameters_ parameter. When you provide the list of parameters to audit, you can provide single parameters, parameters with the asterisk (\*) wildcard characters, or a mix of both. Each entry in the list is separated by commas. The following values are all valid:

- `Database`

- `*Address*`

- `Custom*`

- `*Region`

> [!NOTE]
> For an audit log entry to be created when a command is run, the command must include at least one or more parameters that exist on at least one or more cmdlets specified with the _AdminAuditLogCmdlets_ parameter.

This example audits the parameters specified in the preceding list.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogParameters Database, *Address*, Custom*, *Region
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/set-adminauditlogconfig).

## Specify the admin audit log age limit
<a name="agelimit"> </a>

The audit log age limit determines how long audit log entries will be retained. When a log entry exceeds the age limit, it's deleted. The default is 90 days.

You can specifiy the age limit in days. Or you can specify the number of days, hours, minutes, and seconds that audit log entries should be kept. To specify a value more specific than days, use the format dd.hh.mm:ss where the following applies:

- **dd**: Number of days to keep the audit log entry

- **hh**: Number of hours to keep the audit log entry

- **mm**: Number of minutes to keep the audit log entry

- **ss**: Number of seconds to keep the audit log entry

> [!CAUTION]
> You can set the audit log age limit to a value that's less than the current age limit. If you do this, any audit log entry whose age exceeds the new age limit will be deleted. > If you set the age limit to 0, Exchange deletes all the entries in the audit log. > We recommend that you assign permissions to configure the audit log age limit only to highly trusted users.

This example specifies an age limit of two years and six months.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogAgeLimit 913
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/set-adminauditlogconfig).

## Enable or disable logging of Test cmdlets
<a name="testcmdlets"> </a>

Cmdlets that start with the verb **Test** aren't logged by default. This is because **Test** cmdlets can generate a significant amount of data in a short time. Only enable the logging of **Test** cmdlets for short periods of time.

This command enables the logging of **Test** cmdlets.

```PowerShell
Set-AdminAuditLogConfig -TestCmdletLoggingEnabled $true
```

This command disables the logging of **Test** cmdlets.

```PowerShell
Set-AdminAuditLogConfig -TestCmdletLoggingEnabled $false
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/set-adminauditlogconfig).

## Disable admin audit logging
<a name="disable"> </a>

To disable admin audit logging, use the following command.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogEnabled $false
```

## Enable admin audit logging
<a name="enable"> </a>

To enable admin audit logging, use the following command.

```PowerShell
Set-AdminAuditLogConfig -AdminAuditLogEnabled $true
```

## View admin audit logging settings
<a name="viewauditlog"> </a>

To view the admin audit logging settings that you've configured for your organization, use the following command.

```PowerShell
Get-AdminAuditLogConfig
```
