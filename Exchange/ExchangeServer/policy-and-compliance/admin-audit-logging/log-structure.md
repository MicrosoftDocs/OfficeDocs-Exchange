---
localization_priority: Normal
description: 'Summary: Learn about the contents of the administrator audit log in Exchange Server 2016 and Exchange Server 2019.'
ms.topic: article
author: chrisda
ms.author: chrisda
ms.assetid: 87e259c9-c884-4d53-bd78-d13f2300d73e
ms.date: 7/8/2018
ms.reviewer: 
title: Administrator audit log structure
ms.collection: exchange-server
audience: ITPro
ms.prod: exchange-server-it-pro
manager: dansimp

---

# Administrator audit log structure

Administrator audit logs contain a record of all the cmdlets and parameters that have been run in the Exchange Management Shell and by the Exchange admin center (EAC). They're created on-demand when you run the admin audit log report in the EAC, or when you run the **New-AdminAuditLogSearch** cmdlet in the Exchange Management Shell. For more information about audit logs, see [Administrator audit logging in Exchange Server](admin-audit-logging.md).

## Audit log XML tags and attributes

The audit logs are XML files and can contain multiple audit log entries. The following table describes each XML tag and its associated attributes.

|**Element**|**Attribute**|**Description**|
|:-----|:-----|:-----|
| `<?xml version="1.0" encoding="utf-8"?>`|N/A|This is the XML document declaration tag. It's included in every audit log XML file and contains the XML version number and the character encoding value.|
| `SearchResults`|N/A|This tag contains all the audit log entries in the XML file. The `Event` tag is a child of this tag.  <br/> There is only one `SearchResults` tag per XML file.|
| `Event`||This tag contains the audit log entry for an individual cmdlet. This tag contains the `Caller`, `Cmdlet`, `ObjectModified`, `RunDate`, `Succeeded`, `Error`, and `OriginatingServer` attributes. The `CmdletParameters` and `ModifiedProperties` tags are children of this tag.  <br/> There is one `Event` tag per audit log entry.|
|| `Caller`|This attribute contains the user account of the user who ran the cmdlet in the `Cmdlet` attribute.|
|| `Cmdlet`|This attribute contains the name of the cmdlet that was run by the user in the `Caller` attribute.|
|| `ObjectModified`|This attribute contains the object that was modified by the cmdlet specified in the `Cmdlet` attribute. The `ModifiedProperties` tag shows which properties were modified on this object.|
|| `RunDate`|This attribute contains the date and time when the cmdlet in the `Cmdlet` attribute was run.|
|| `Succeeded`|This attribute specifies whether the cmdlet in the `Cmdlet` attribute ran successfully. The value is either `True` or `False`.|
|| `Error`|This attribute contains the error message generated if the cmdlet in the `Cmdlet` attribute failed to complete successfully. If no error was encountered, the value is set to `None`.|
|| `OriginatingServer`|This attribute contains the server on which the cmdlet specified in the `Cmdlet` attribute was run.|
| `CmdletParameters`|N/A|This tag contains all of the parameters specified when the cmdlet was run. The `Parameter` tag is a child of this tag.  <br/> There is one `CmdletParameters` tag per `Event` tag.|
| `Parameter`||This tag contains an individual parameter that was specified when the cmdlet was run. This tag contains the `Name` and `Value` attributes.  <br/> There can be multiple `Parameter` tags per `CmdletParameters` tag.|
|| `Name`|This attribute contains the name of the parameter that was specified on the cmdlet that was run.|
|| `Value`|This attribute contains the value that was provided on the parameter specified in the `Name` attribute.|
| `ModifiedProperties`|N/A|This tag contains all of the properties that were modified by the cmdlet that was run. The `Property` tag is a child of this tag.  <br/> There is one `ModifiedProperties` tag per `Event` tag.  <br/> **Important**: This tag is only populated if the _LogLevel_ parameter on the **Set-AdminAuditLogConfig** cmdlet is set to `Verbose`.|
| `Property`||This tag contains an individual property that was specified when the cmdlet was run. This tag contains the `Name`, `OldValue`, and `NewValue` attributes.  <br/> There can be multiple `Property` tags per `ModifiedProperties` tag.|
|| `Name`|This attribute contains the name of the property that was modified when the cmdlet was run.|
|| `OldValue`|This attribute contains the value that was contained in the property specified in the `Name` attribute before it was changed.|
|| `NewValue`|This attribute contains the value that the property in the `Name` attribute was changed to.|

## Example of an admin audit log entry

The following is an example of a typical log entry in the admin audit log.

```
<?xml version="1.0" encoding="utf-8"?>
<SearchResults>
  <Event Caller="corp.e16.contoso.com/Users/Administrator" Cmdlet="Set-Mailbox" ObjectModified="corp.e16.contoso.com/Users/david" RunDate="2015-10-18T15:48:15-07:00" Succeeded="true" Error="None" OriginatingServer="WIN8MBX (15.01.0396.030)">
    <CmdletParameters>
      <Parameter Name="Identity" Value="david" />
      <Parameter Name="ProhibitSendReceiveQuota" Value="10 GB (10,737,418,240 bytes)" />
    </CmdletParameters>
    <ModifiedProperties>
      <Property Name="ProhibitSendReceiveQuota" OldValue="35 GB (37,580,963,840 bytes)" NewValue="10 GB (10,737,418,240 bytes)" />
    </ModifiedProperties>
  </Event>
</SearchResults>
```

Based on the information in this log entry, we know the following occurred:

- On 10/18/2017 at 3:48 P.M. Pacific Daylight Time (UTC-7), the user `Administrator` ran the cmdlet **Set-Mailbox**.

- The two following parameters were provided when the **Set-Mailbox** cmdlet was run:

  - _Identity_ with a value of `david`

  - _ProhibitSendReceiveQuota_ with a value of `10GB`

- The _ProhibitSendReceiveQuota_ property on the object `david` was modified with a new value of `10GB`, which replaced the old value of `35GB`.

    > [!NOTE]
    > The modified properties are saved to the audit log because the _LogLevel_ parameter on the `Set-AdminAuditLogConfig` cmdlet was set to `Verbose` in this example.

- The operation completed successfully without any errors.
