---
title: 'Export DLP sensitive information types from Exchange: Exchange 2013 Help'
TOCTitle: Export DLP sensitive information types from Exchange
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 8f02fbc2-dd1c-4276-be1a-517a43fe39b2
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Export DLP sensitive information types from Exchange 2013

_**Applies to:** Exchange Server 2013_

You can view or change the details within your DLP policies without using the Exchange admin center (EAC) or Exchange Management Shell cmdlets by exporting the policies, saving them as an XML file, and modifying that XML file. Typically you would then import the XML file back into Exchange. In this way, policies can be edited independent of Exchange. However, they must meet specific format requirements, also referred to as XML schema, in order to work correctly.

For additional management tasks related to DLP, see [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- The EAC doesn't provide a way to export DLP policies or templates to an external file. To use the Shell, see [Open the Exchange Management Shell](https://docs.microsoft.com/powershell/exchange/open-the-exchange-management-shell).

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Data loss prevention (DLP)" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

### Use the Exchange Management Shell to export the DLP sensitive information types

This example exports all DLP sensitive information types along with their attributes to an XML file in the path C:\My Documents\exportedInformationTypes.xml. We recommend making a backup copy of your current DLP sensitive information types collection. One way to achieve this is to export and then immediately copy and rename the same XML file.

1. Open the Exchange Management Shell.

2. Type Get-ClassificationRuleCollection, and your organization's sensitive information types should display on screen. If you haven't created any sensitive information types of your own, you'll only see the default, built-in sensitive information types collection, labeled "Microsoft Rule Package."

3. Store the sensitive information types in a variable by typing $ruleCollections = Get-ClassificationRuleCollection.

4. Now make a formatted XML file with all that data by typing Set-Content -path "C:\My Documents\exportedRules.xml" -Encoding Byte -Value $ruleCollections.SerializedClassificationRuleCollection..

You can now edit the XML file to adjust the policies as needed. To learn how to customize the built-in sensitive information types, see [Customize the built-in DLP sensitive information types](customize-the-built-in-dlp-sensitive-information-types-exchange-2013-help.md). For details on importing policies back into Exchange, see [Import a custom DLP policy template from a file](import-a-custom-dlp-policy-template-from-a-file-exchange-2013-help.md).

## For more information

[Customize the built-in DLP sensitive information types](customize-the-built-in-dlp-sensitive-information-types-exchange-2013-help.md)

[Import a custom DLP policy template from a file](import-a-custom-dlp-policy-template-from-a-file-exchange-2013-help.md)
