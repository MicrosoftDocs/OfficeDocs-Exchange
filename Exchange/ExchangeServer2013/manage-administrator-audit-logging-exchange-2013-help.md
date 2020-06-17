---
title: 'Manage administrator audit logging: Exchange 2013 Help'
TOCTitle: Manage administrator audit logging
ms:assetid: 15c284c0-b8e6-42ca-9913-7c59fdb6885d
ms:mtpsurl: https://technet.microsoft.com/library/Dd335109(v=EXCHG.150)
ms:contentKeyID: 50117640
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage administrator audit logging

_**Applies to:** Exchange Server 2013_

Administrator audit logging in Microsoft Exchange Server 2013 enables you to create a log entry each time a specified cmdlet is run. Log entries provide you with information about what cmdlet was run, which parameters were used, who ran the cmdlet, and what objects were affected. For more information about administrator audit logging, see [Administrator audit logging](administrator-audit-logging-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: less than 5 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Administrator audit logging" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- Administrator audit logging relies on Active Directory replication to replicate the configuration settings you specify to the domain controllers in your organization. Depending on your replication settings, the changes you make may not be immediately applied to all Exchange 2013 servers in your organization.

- Changes to the audit log configuration are refreshed every 60 minutes on computers that have the Shell open at the time a configuration change is made. If you want to apply the changes immediately, close and then open the Shell again on each computer.

- A command may take up to 15 minutes after it's run to appear in audit log search results. This is because audit log entries must be indexed before they can be searched. If a command doesn't appear in the administrator audit log, wait a few minutes and run the search again.

- You must use the Shell to perform these procedures.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Specify the cmdlets to be audited

By default, audit logging creates a log entry for every cmdlet that's run. If you're enabling audit logging for the first time and want this behavior, you don't have to change the cmdlet audit list. If you've previously specified cmdlets to audit and now want to audit all cmdlets, you can audit all cmdlets by specifying the asterisk (\*) wildcard character with the *AdminAuditLogCmdlets* parameter on the **Set-AdminAuditLogConfig** cmdlet, as shown in the following command.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogCmdlets *
```

You can specify which cmdlets to audit by providing a list of cmdlets using the *AdminAuditLogCmdlets* parameter. When you provide the list of cmdlets to audit, you can provide single cmdlets, cmdlets with the asterisk (\*) wildcard characters, or a mix of both. Each entry in the list is separated by commas. The following values are all valid:

- `New-Mailbox`

- `*TransportRule`

- `*Management*`

- `Set-Transport*`

This example audits the cmdlets specified in the preceding list.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogCmdlets New-Mailbox, *TransportRule, *Management*, Set-Transport*
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AdminAuditLogConfig).

## Specify the parameters to be audited

By default, audit logging creates a log entry for every cmdlet that's run, regardless of the parameters specified. If you're enabling audit logging for the first time and want this behavior, you don't have to change the parameter audit list. If you've previously specified parameters to audit and now want to audit all parameters, you can do so by specifying the asterisk (\*) wildcard character with the *AdminAuditLogParameters* parameter on the **Set-AdminAuditLogConfig** cmdlet, as shown in the following command.

  ```powershell
  Set-AdminAuditLogConfig -AdminAuditLogParameters *
  ```

You can specify which parameters you want to audit by using the *AdminAuditLogParameters* parameter. When you provide the list of parameters to audit, you can provide single parameters, parameters with the asterisk (\*) wildcard characters, or a mix of both. Each entry in the list is separated by commas. The following values are all valid:

- `Database`

- `*Address*`

- `Custom*`

- `*Region`

> [!NOTE]
> For an audit log entry to be created when a command is run, the command must include at least one or more parameters that exist on at least one or more cmdlets specified with the <EM>AdminAuditLogCmdlets</EM> parameter.

This example audits the parameters specified in the preceding list.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogParameters Database, *Address*, Custom*, *Region
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AdminAuditLogConfig).

## Specify the audit log age limit

The audit log age limit determines how long audit log entries will be retained. When a log entry exceeds the age limit, it's deleted. The default is 90 days.

You can specify the number of days, hours, minutes, and seconds that audit log entries should be kept. To specify a value, use the format dd.hh.mm:ss where the following applies:

- **dd**: Number of days to keep the audit log entry

- **hh**: Number of hours to keep the audit log entry

- **mm**: Number of minutes to keep the audit log entry

- **ss**: Number of seconds to keep the audit log entry

> [!WARNING]
> You can set the audit log age limit to a value that's less than the current age limit. If you do this, any audit log entry whose age exceeds the new age limit will be deleted.<BR>If you set the age limit to 0, Exchange deletes all the entries in the audit log.<BR>We recommend that you grant permissions to configure the audit log age limit only to highly trusted users.

This example specifies an age limit of two years and six months.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogAgeLimit 913.00:00:00
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AdminAuditLogConfig).

## Enable or disable logging of Test cmdlets

Cmdlets that start with the verb **Test** aren't logged by default. This is because **Test** cmdlets can generate a significant amount of data in a short time. Only enable the logging of **Test** cmdlets for short periods of time.

This command enables the logging of **Test** cmdlets.

```powershell
Set-AdminAuditLogConfig -TestCmdletLoggingEnabled $True
```

This command disables the logging of **Test** cmdlets.

```powershell
Set-AdminAuditLogConfig -TestCmdletLoggingEnabled $False
```

For detailed syntax and parameter information, see [Set-AdminAuditLogConfig](https://docs.microsoft.com/powershell/module/exchange/Set-AdminAuditLogConfig).

## Disable administrator audit logging

To disable administrator audit logging, use the following command.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogEnabled $False
```

## Enable administrator audit logging

To enable administrator audit logging, use the following command.

```powershell
Set-AdminAuditLogConfig -AdminAuditLogEnabled $True
```

## View administrator audit logging settings

To view the administrator audit logging settings that you've configured for your organization, use the following command.

```powershell
Get-AdminAuditLogConfig
```
