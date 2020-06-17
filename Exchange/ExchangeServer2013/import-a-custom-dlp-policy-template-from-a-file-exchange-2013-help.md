---
title: 'Import a custom DLP policy template from a file: Exchange 2013 Help'
TOCTitle: Import a custom DLP policy template from a file
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 83f49dbd-f9b1-498e-b548-1529c5e1ccdb
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Import a custom DLP policy template from a file in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can manage sensitive information through DLP policies by importing a file that contains policy information settings. DLP policy templates can be developed independent of Exchange as XML files. However, they must meet specific format requirements in order to work correctly. Alternatively, policies that are exported from a previous version of Exchange can be imported into Microsoft Exchange Server 2013.

> [!CAUTION]
> You should enable your DLP policies in test mode before running them in your production environment. During such tests, it is recommended that you configure sample user mailboxes and send test messages that invoke your test policies in order to confirm the results.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Data loss prevention (DLP)" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to import a custom DLP policy template from a file

Use the following procedure to import a custom DLP policy template from a file. In order to avoid confusion, supply a unique name for each part of your policy or rule when you have the option to provide your own name.

1. In the EAC, navigate to **Compliance management** \> **Data loss prevention**.

2. Click the arrow that is next to the **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) icon, then click **Import policy**.

3. On the **Import policy** page, complete the following fields:

1. **Select the file to import** Add the name of the policy file you want to install.

2. **Name** Add a name that will distinguish this policy from others.

3. **Description** Optionally, add a description that summarizes this policy.

4. **More options** Select the mode or state for this policy. The new policy is not fully enabled until you specify that it should be. The default mode for a policy is test without notifications.

5. Click **Next** to validate and import the policy.

## Use the Shell to import a custom DLP policy template from a file

This example imports a custom DLP policy template file in the file C:\My Documents\DLP Backup.xml. Importing a DLP policy collection from an XML file removes or overwrites all pre-existing DLP policies that were defined in your organization. Make sure that you have a backup of your current DLP policy collection before you import and overwrite your current DLP policies.

```powershell
Import-DlpPolicyCollection -FileData ([Byte[]]$(Get-Content -Path " C:\My Documents\DLP Backup.xml " -Encoding Byte -ReadCount 0))
```

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)
