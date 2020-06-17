---
title: 'Enable or Disable IRM for Internal Messages: Exchange 2013 Help'
TOCTitle: Enable or Disable IRM for Internal Messages
ms:assetid: a6a17f57-5304-41f1-954d-7301857d54a1
ms:mtpsurl: https://technet.microsoft.com/library/Bb124077(v=EXCHG.150)
ms:contentKeyID: 49319927
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or Disable IRM for Internal Messages

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, Information Rights Management (IRM) is enabled by default for internal messages. This allows you to create transport protection rules and Microsoft Outlook protection rules to IRM-protect messages in transport and on Microsoft Outlook 2010 and later clients. Enabling IRM for internal messages is a prerequisite for all other IRM features in Exchange Server 2013, such as transport decryption, journal rule decryption, IRM in Microsoft Office Outlook Web App, and IRM in Microsoft Exchange ActiveSync.

> [!WARNING]
> Disabling IRM for internal messages disables all IRM features in the Exchange organization. The client-side IRM features in Outlook (for example, the ability to read, reply to, forward, and create IRM-protected messages using an Active Directory Rights Management Services (AD&nbsp;RMS) server) aren't affected.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to enable or disable IRM for internal messages. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to enable IRM for internal messages

This example enables IRM for internal messages for the Exchange organization.

```powershell
Set-IRMConfiguration -InternalLicensingEnabled $true
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## Use the Shell to disable IRM for internal messages

This example disables IRM for internal messages for the Exchange organization.

```powershell
Set-IRMConfiguration -InternalLicensingEnabled $false
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## How do you know this worked?

To verify that you've enabled or disabled IRM for internal messages, use the [Get-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Get-IRMConfiguration) cmdlet to check the configuration.
