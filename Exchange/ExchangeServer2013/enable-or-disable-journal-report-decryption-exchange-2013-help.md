---
title: 'Enable or Disable Journal Report Decryption: Exchange 2013 Help'
TOCTitle: Enable or Disable Journal Report Decryption
ms:assetid: 1dedbe73-2c1a-4b14-8799-5091aaec7965
ms:mtpsurl: https://technet.microsoft.com/library/Dd638092(v=EXCHG.150)
ms:contentKeyID: 49319900
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or Disable Journal Report Decryption

_**Applies to:** Exchange Server 2013_

Enabling journal report decryption allows the Journaling agent to attach a decrypted copy of a rights-protected message to the journal report. Before you enable journal report decryption, you must add the Federated Delivery mailbox to the super users group configured on your [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)) server.

For additional management tasks related to Information Rights Management (IRM), see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- Members of the super users group are granted an owner use license when they request a license from the AD RMS cluster. This allows them to decrypt all RMS-protected content created by that AD RMS cluster.

- An AD RMS cluster must be installed in the Active Directory forest.

- The Federated Delivery mailbox has been added to an AD RMS super users group. For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to enable journal report decryption. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable journal report decryption

This example enables journal report decryption for the Exchange organization.

```powershell
Set-IRMConfiguration -JournalReportDecryptionEnabled $true
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## Use the Shell to disable journal report decryption

This example disables journal report decryption for the Exchange organization.

```powershell
Set-IRMConfiguration -JournalReportDecryptionEnabled $false
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## How do you know this worked?

To verify that you have enabled or disabled journal report decryption, run the **Get-IRMConfiguration** cmdlet and check the value of the *JournalDecryptionEnabled* property.

For an example of how to check the IRM configuration, see [Examples](https://technet.microsoft.com/e1821219-fe18-4642-a9c2-58eb0aadd61a\(exchg.150\)#examples) in **Get-IRMConfiguration**.
