---
localization_priority: Normal
description: 'Summary: Learn about administrator audit logging in Exchange Server, and how you use the audit log to track changes to objects in your Exchange organization.'
ms.topic: overview
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 22b17eb8-d8ee-4599-b202-d6a7928c20d9
ms.reviewer:
title: Administrator audit logging in Exchange Server
ms.collection: exchange-server
f1.keywords:
- NOCSH
audience: ITPro
ms.prod: exchange-server-it-pro
manager: serdars

---

# Administrator audit logging in Exchange Server

You can use administrator audit logging in Exchange Server to log when a user or administrator makes a change in your organization. By keeping a log of the changes, you can trace changes to the person who made the change, augment your change logs with detailed records of the change as it was implemented, comply with regulatory requirements and requests for discovery, and more.

By default, administrator audit logging is enabled in new installations of Exchange Server.

## What gets audited

Cmdlets that are run directly in the Exchange Management Shell are audited. In addition, operations performed using the Exchange admin center (EAC) are also logged because those operations run cmdlets in the background.

Cmdlets, regardless of where they're run, are audited if a cmdlet is on the cmdlet auditing list and one or more parameters on that cmdlet are on the parameter auditing list. Audit logging is intended to show what actions have been taken to modify objects in an Exchange organization rather than what objects have been viewed.

 **Notes**:

- A cmdlet might not be logged if an error occurs before the cmdlet calls the Admin Audit Log cmdlet extension agent. If an error occurs after the Admin Audit Log agent is called, the cmdlet is logged along with the associated error. For more information, see the [Admin Audit Log agent](#admin-audit-log-agent) section later in this topic.

- Changes to the audit log configuration are refreshed every 60 minutes on computers that have the Exchange Management Shell open at the time a configuration change is made. If you want to apply the changes immediately, close and then open the Exchange Management Shell again on each computer.

- A command may take up to 15 minutes after it's run to appear in audit log search results. This is because audit log entries must be indexed before they can be searched. If a command doesn't appear in the administrator audit log, wait a few minutes and run the search again.

## Admin audit logging configuration

By default, when admin audit logging is enabled, a log entry is created every time any cmdlet is run. If you don't want to audit every cmdlet that's run, you can configure audit logging to audit only the cmdlets and parameters you're interested in. You configure audit logging with the **Set-AdminAuditLogConfig** cmdlet. The parameters referenced in the following sections are used with this cmdlet.

> [!IMPORTANT]
> Changes to the administrator audit log configuration are always logged, regardless of whether the **Set-AdminAuditLogConfig** cmdlet is included in the list of cmdlets being audited or whether audit logging is enabled or disabled.

When a command is run, Exchange inspects the cmdlet that was used. If the cmdlet that was run matches any of the cmdlets provided with the _AdminAuditLogCmdlets_ parameter, Exchange then checks the parameters specified in the _AdminAuditLogParameters_ parameter. If at least one or more parameters from the parameters list are matched, Exchange logs the cmdlet that was run. The following sections contain more information about each aspect of the audit logging configuration.

For more information about managing audit logging configuration, see [Manage administrator audit logging](manage-admin-audit-logging.md).

### Cmdlets

You can control which cmdlets are audited by providing a list of cmdlets, and their parameters, that you want to log. When you configure audit logging, you can specify to audit every cmdlet, or you can specify the cmdlets you want to audit by using the _AdminAuditLogCmdlets_ parameter. You can specify full cmdlet names, such as **New-Mailbox**, or you can specify partial cmdlet names and enclose those names in wildcard characters, such as an asterisk (`*`). For example, if you want to log when any cmdlet that contains the string `Transport` runs, you can specify a value of `*Transport*`. You can use a mix of full cmdlet names and partial cmdlet names at the same time to tailor the audit logging configuration to your needs.

To audit all cmdlets, specify only the wildcard character (\*). This is the default setting.

### Parameters

In addition to specifying which cmdlets you want to log, you can also indicate that cmdlets should only be logged if certain parameters on those cmdlets are used. Use the _AdminAuditLogParameters_ parameter to specify which parameters should be logged. As with cmdlets, you can specify full parameter names, such as `Database`, or partial parameter names enclosed in wildcard characters (`*`), such as `*Address*`, or a combination of both.

To audit all parameters, specify only the wildcard character (\*). This is the default setting.

### Admin audit log age limit

By default, admin audit logging is configured to store audit log entries for 90 days. After 90 days, the audit log entry is deleted. You can change the audit log age limit using the _AdminAuditLogAgeLimit_ parameter. For example, to change the age limit to 180 days, use the command `Set-AdminAuditLogConfig -AdminAuditLogAgeLimit 180`. You can also specify the number of days, hours, minutes, and seconds that audit log entries should be kept. To specify a value, use the format `dd.hh:mm:ss` where the following applies:

- **dd**: The number of days to keep the audit log entry.

- **hh**: The number of hours to keep the audit log entry.

- **mm**: The number of minutes to keep the audit log entry.

- **ss**: The number of seconds to keep the audit log entry.

You need to specify multiple years by using the `dd` field. For example, 365 days equals one year; 730 days equals two years; 913 days equals two years and six months. For example, to set the audit log age limit to two years and six months, use the value `913`.

 **Notes**:

- You can set the admin audit log age limit to a value that's less than the current age limit. If you do this, any audit log entry whose age exceeds the new age limit is deleted.

- If you set the age limit to 0, Exchange deletes all the entries in the audit log.

- We recommend that you assign permissions to configure the audit log age limit only to highly trusted users.

### Verbose logging

By default, the admin audit log records only the cmdlet name, cmdlet parameters (and values specified), the object that was modified, who ran the cmdlet, when the cmdlet was run, and on what server the cmdlet was run. The admin audit log doesn't log what properties were modified on the object. If you want the admin audit log to also include the properties of the object that were modified, you can enable verbose logging by setting the _LogLevel_ parameter to `Verbose`. When you enable verbose logging, in addition to the information logged by default, the properties modified on an object, including their old and new values, are included in the admin audit log.

### Test cmdlets

Cmdlets that begin with the verb **Test** aren't logged by default. You can indicate that **Test** cmdlets should be logged by setting the _TestCmdletLoggingEnabled_ parameter to `$true`. Although you can enable logging of test cmdlets, we recommend that you do this only for short periods of time because test cmdlets can produce a large number of audit log entries.

## Admin audit log

Each time a cmdlet is logged, an admin audit log entry is created. The audit log entries are stored in the admin audit log, which is stored in a hidden, dedicated arbitration mailbox that can only be accessed by using the EAC, the **Search-AdminAuditLog** cmdlet, or the **New-AdminAuditLogSearch** cmdlet. The following sections provide information about:

- What's included in the admin audit log.

- Reports available on the EAC **Auditing** page.

- Admin audit log search cmdlets.

### Audit log contents

Each audit log entry contains the information described in the following table. The audit log contains one or more audit log entries. The number of audit log entries is controlled by the audit log age limit specified using the `Set-AdminAuditLogConfig -AdminAuditLogAgeLimit` command. Any audit log entry that exceeds the age limit is deleted.

**Audit log entry fields**

|**Field**|**Description**|
|:-----|:-----|
|`RunspaceId`|This field is used internally by Exchange.|
|`ObjectModified`|This field contains the object that was modified by the cmdlet specified in the `CmdletName` field.|
|`CmdletName`|This field contains the name of the cmdlet that was run by the user in the `Caller` field.|
|`CmdletParameters`|This field contains the parameters that were specified when the cmdlet in the `CmdletName` field was run. Also stored in this field, but not visible in the default output, is the value specified with the parameter, if any.|
|`ModifiedProperties`|This field contains the properties that were modified on the object in the `ObjectModified` field. Also stored in this field, but not visible in the default output, are the old value of the property and the new value that was stored.  <br/> **Important**: This field is only populated if the _LogLevel_ parameter on the **Set-AdminAuditLogConfig** cmdlet is set to `verbose`.|
|`Caller`|This field contains the user account of the user who ran the cmdlet in the `CmdletName` field.|
|`Succeeded`|This field specifies whether the cmdlet in the `CmdletName` field ran successfully. The value is either `True` or `False`.|
|`Error`|This field contains the error message generated if the cmdlet in the `CmdletName` field failed to complete successfully.|
|`RunDate`|This field contains the date and time when the cmdlet in the `CmdletName` field was run. The date and time are stored in Coordinated Universal Time (UTC) format.|
|`OriginatingServer`|This field indicates the server on which the cmdlet specified in the `CmdletName` field was run.|
|`Identity`|This field is used internally by Exchange.|
|`IsValid`|This field is used internally by Exchange.|
|`ObjectState`|This field is used internally by Exchange.|

### EAC auditing reports

The **Auditing** page in the EAC has several reports that provide information about various types of compliance and administrative configuration changes. The following reports provide information about configuration changes in your organization:

- **Administrator role group report**: This report enables you to search for changes to management role groups that you specify within a specified timeframe. The results that are returned include the role groups that have been changed, who changed them and when, and what changes were made. A maximum of 3,000 entries can be returned. If your search might return more than 3,000 entries, use the **Administrator audit log** report or the **Search-AdminAuditLog** cmdlet.

- **Admin audit log report**: This report enables you to view entries in the admin audit log recorded within a specified time frame. You can also export admin audit log entries to a XML file and then send the file via email to a recipient you specify. For more information about the contents of the XML file, see [Administrator audit log structure](log-structure.md).

For information about how to use these reports, see [Search the role group changes or administrator audit logs](https://docs.microsoft.com/Exchange/search-role-group-changes-exchange-2013-help).

### Search-AdminAuditLog cmdlet

When you run the **Search-AdminAuditLog** cmdlet, all the audit log entries that match your search criteria are returned. You can specify the following search criteria:

- **Cmdlets**: Specifies the cmdlets you want to search for in the admin audit log.

- **Parameters**: Specifies the parameters, separated by commas, you want to search for in the admin audit log. You can only search for parameters if you specify a cmdlet to search for.

- **End date**: Scopes the admin audit log results to log entries that occurred on or before the specified date.

- **Start date**: Scopes the admin audit log results to log entries that occurred on or after the specified date.

- **Object IDs**: Specifies that only admin audit log entries that contain the specified changed objects should be returned

- **User IDs**: Specifies that only the admin audit log entries that contain the specified IDs of the user who ran the cmdlet should be returned.

- **Successful completion**: Specifies whether only admin audit log entries that indicated a success or failure should be returned.

Each audit log entry contains the information described in the table in [Audit log contents](#audit-log-contents). By default, only the first 1,000 log entries that match the search criteria are returned. However, you can override this default and return more or fewer entries using the _ResultSize_ parameter. You can specify a value of `Unlimited` with the _ResultSize_ parameter to return all log entries that match the specified criteria.

For information about how to use the **Search-AdminAuditLog** cmdlet, see [Search the role group changes or administrator audit logs](https://docs.microsoft.com/Exchange/search-role-group-changes-exchange-2013-help).

### New-AdminAuditLogSearch cmdlet

The **New-AdminAuditLogSearch** cmdlet searches the admin audit log just like the **Search-AdminAuditLog** cmdlet. However, instead of displaying the results of the search in the Exchange Management Shell, the **New-AdminAuditLogSearch** cmdlet performs the search and then sends the results to a recipient you specify via an email message. The results are included as an XML attachment to the email message.

You can use the same search criteria with the **New-AdminAuditLogSearch** cmdlet that's used on the **Search-AdminAuditLog** cmdlet. For a list of the search criteria, see [Search-AdminAuditLog cmdlet](#search-adminauditlog-cmdlet).

After you run the **New-AdminAuditLogSearch** cmdlet, Exchange may take up to 15 minutes to deliver the report to the specified recipient. The XML file attached report can be a maximum of 10 MB. The XML file contains the same information described in the table in [Audit log contents](#audit-log-contents). For more information about the structure of the XML file, see [Administrator audit log structure](log-structure.md).

> [!NOTE]
> Outlook Web App doesn't allow you to open XML attachments by default. You can either configure Exchange to allow XML attachments to be viewed using Outlook Web App, or you can use another email client, such as Microsoft Outlook, to view the attachment. For information about how to configure Outlook Web App to allow you to view an XML attachment, see [View or configure Outlook on the web virtual directories in Exchange Server](../../clients/outlook-on-the-web/virtual-directories.md).

For information about how to use the **New-AdminAuditLogSearch** cmdlet, see [Search the role group changes or administrator audit logs](https://docs.microsoft.com/Exchange/search-role-group-changes-exchange-2013-help).

## Manual admin audit log entries

In addition to logging Exchange cmdlets when they're run, Exchange Server enables you to manually write log entries to the audit log. Exchange Server supports this using the **Write-AdminAuditLog** cmdlet. Situations where you might want to add a manual log entry include the following:

- Custom script entry and exit

- Change control information

- Maintenance start and end times

With the **Write-AdminAuditLog** cmdlet, you specify a string of text to include in the audit log using the _Comment_ parameter. The _Comment_ parameter accepts an alphanumeric string up to 500 characters. Included in the manual audit log entry along with the comment string is all of the same information captured when an Exchange cmdlet is logged. For a description of each field included in the audit log, see the table in [Audit log contents](#audit-log-contents).

You can retrieve manual audit log entries the same way as any other log entry, using the EAC **Auditing** page or using the **Search-AdminAuditLog** or **New-AdminAuditLogSearch** cmdlets.

To view the contents of the _Comment_ parameter on the **Write-AdminAuditLog** cmdlet in a manual audit log entry, see [Search the role group changes or administrator audit logs](https://docs.microsoft.com/Exchange/search-role-group-changes-exchange-2013-help).

## Active Directory replication

Administrator audit logging relies on Active Directory replication to replicate the configuration settings you specify to the domain controllers in your organization. Depending on your replication settings, the changes you make may not be immediately applied to all servers running Exchange in your organization.

## Admin Audit Log agent

The Admin Audit Log built-in cmdlet extension agent performs admin audit logging of cmdlet operations in Exchange Server. This agent reads the audit log configuration and then performs an evaluation of each cmdlet run in your organization. If the criteria you've specified in the admin audit log configuration matches the cmdlet that's being run, the agent generates an audit log entry.

The Admin Audit Log agent is enabled by default, which is required for admin audit logging to function. It can't be disabled, and its priority can't be changed. For more information about cmdlet extension agents, see [Cmdlet Extension Agents](https://docs.microsoft.com/exchange/cmdlet-extension-agents-exchange-2013-help).
