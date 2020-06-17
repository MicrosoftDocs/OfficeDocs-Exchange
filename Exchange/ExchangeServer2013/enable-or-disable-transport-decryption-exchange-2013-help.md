---
title: 'Enable or Disable Transport Decryption: Exchange 2013 Help'
TOCTitle: Enable or Disable Transport Decryption
ms:assetid: 4663f54e-dd0a-4a42-983e-8765e2adc412
ms:mtpsurl: https://technet.microsoft.com/library/Dd638126(v=EXCHG.150)
ms:contentKeyID: 49319910
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or Disable Transport Decryption

_**Applies to:** Exchange Server 2013_

Enabling transport decryption allows the Transport Rules agent on Microsoft Exchange Server 2013 Mailbox servers to access content in messages protected by Information Rights Management (IRM). As a result, other transport agents can access message content and possibly make changes to it. For example, the Transport Rules agent may need to inspect message content and apply transport rules (such as rules that apply a disclaimer to the message). To successfully decrypt IRM-protected messages, you must add the Federated Delivery mailbox to the super users group configured on your [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)) server.

> [!IMPORTANT]
> Members of the super users group are granted an owner use license when they request a license from the AD&nbsp;RMS cluster. This allows them to decrypt all RMS-protected content created by that AD&nbsp;RMS cluster.

When enabling transport decryption, you can specify the following settings:

- **Mandatory**: Rejects messages that can't be decrypted and returns a non-delivery report (NDR) to the sender.

- **Optional**: Uses a best-effort approach to decryption. If possible, messages are decrypted, but they're delivered even if decryption fails. This is the default setting.

To learn more about transport decryption, see [Transport decryption](transport-decryption-exchange-2013-help.md).

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- An AD RMS server exists in the Active Directory forest and is accessible.

- The Federated Delivery mailbox has been added to the AD RMS super users group. For details, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to enable transport decryption. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable transport decryption

This example enables transport decryption for the Exchange 2013 organization. Messages that can't be decrypted are rejected and an NDR is returned to the sender.

```powershell
Set-IRMConfiguration -TransportDecryptionSetting Mandatory
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## Use the Shell to disable transport decryption

This example disables transport decryption for the Exchange 2013 organization.

```powershell
Set-IRMConfiguration -TransportDecryptionSetting Disabled
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## How do I know this worked?

To verify that you have enabled or disabled transport decryption, use the **Get-IRMConfiguration** cmdlet and check the value of the *JournalDecryptionEnabled* property.

For an example of how to check the IRM configuration, see [Examples](https://technet.microsoft.com/e1821219-fe18-4642-a9c2-58eb0aadd61a\(exchg.150\)#examples) in **Get-IRMConfiguration**.
