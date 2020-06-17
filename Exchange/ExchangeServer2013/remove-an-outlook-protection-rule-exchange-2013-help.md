---
title: 'Remove an Outlook Protection Rule: Exchange 2013 Help'
TOCTitle: Remove an Outlook Protection Rule
ms:assetid: 569fc3be-b269-43f5-8797-73ab0691e685
ms:mtpsurl: https://technet.microsoft.com/library/Ee633467(v=EXCHG.150)
ms:contentKeyID: 49319914
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an Outlook Protection Rule

_**Applies to:** Exchange Server 2013_

Using Microsoft Outlook protection rules, you can protect messages with Information Rights Management (IRM) by applying an [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)x) template in Outlook 2010 before the messages are sent. To prevent an Outlook protection rule from being applied, you can disable the rule. Removing an Outlook protection rule removes the rule definition from Active Directory.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to remove Outlook protection rules. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to remove an Outlook protection rule

This example removes the Outlook protection rule OPR-DG-Finance.

```powershell
Remove-OutlookProtectionRule -Identity "OPR-DG-Finance"
```

For detailed syntax and parameter information, see [Remove-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/Remove-OutlookProtectionRule).

## Use the Shell to remove all Outlook protection rules

This example removes all Outlook protection rules in the Exchange organization.

```powershell
Get-OutlookProtectionRule | Remove-OutlookProtectionRule
```

For detailed syntax and parameter information, see [Get-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/Get-OutlookProtectionRule) and [Remove-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/Remove-OutlookProtectionRule).

## How do you know this worked?

To verify that you have successfully removed an Outlook protection rule, use the [Get-OutlookProtectionRule](https://docs.microsoft.com/powershell/module/exchange/Get-OutlookProtectionRule) cmdlet to retrieve Outlook protection rules. For an example of how to retrieve Outlook protection rules, see [Examples](https://docs.microsoft.com/powershell/module/exchange/Get-OutlookProtectionRule#examples).
