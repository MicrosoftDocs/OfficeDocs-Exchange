---
title: 'Enable or disable Information Rights Management on Client Access servers'
TOCTitle: Enable or Disable Information Rights Management on Client Access Servers
ms:assetid: c7ce069b-a572-4755-90a3-7105472e4c83
ms:mtpsurl: https://technet.microsoft.com/library/Dd876938(v=EXCHG.150)
ms:contentKeyID: 49319932
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or Disable Information Rights Management on Client Access Servers

_**Applies to:** Exchange Server 2013_

Enabling Information Rights Management (IRM) on Client Access servers enables the following features:

- Microsoft Office Outlook Web App

- IRM in Microsoft Exchange ActiveSync

When IRM is enabled on Client Access servers, Outlook Web App users can IRM-protect messages by applying an [Active Directory Rights Management Services (AD RMS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831364(v=ws.11)) template created on your AD RMS cluster. Outlook Web App users can also view IRM-protected messages and supported attachments. Before you enable IRM on Client Access servers, you must add the Federation mailbox to the super users group on the AD RMS cluster.

> [!IMPORTANT]
> Members of the super users group are granted an owner use license when they request a license from the AD&nbsp;RMS cluster. This allows them to decrypt all RMS-protected content by that cluster.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Information Rights Management (IRM) configuration" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- An AD RMS cluster must be installed in the Active Directory forest.

- The Federation mailbox has been added to the AD RMS super users group. For detailed instructions, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

- IRM features must be enabled for the organization. For detailed instructions, see [Enable or Disable IRM for Internal Messages](enable-or-disable-irm-for-internal-messages-exchange-2013-help.md).

- You can use the **Set-IRMConfiguration** cmdlet to enable or disable IRM in Outlook Web App and IRM in Exchange ActiveSync for the entire Exchange organization or at specific levels.

  You can control IRM in Outlook Web App at the following levels:

  - **Per-Outlook Web App virtual directory**: To enable or disable IRM in Outlook Web App for an Outlook Web App virtual directory, use the **Set-OWAVirtualDirectory** cmdlet and set the *IRMEnabled* parameter to `$false` or `$true` (default). This allows you to disable IRM in Outlook Web App for one virtual directory on an Exchange 2013 Client Access server, while keeping it enabled on another virtual directory on a different Client Access server.

  - **Per-Outlook Web App mailbox policy**: To enable or disable IRM in Outlook Web App for an Outlook Web App mailbox policy, use the **Set-OWAMailboxPolicy** cmdlet and set the *IRMEnabled* parameter to `$false` or `$true` (default). This allows you to enable IRM in Outlook Web App for one set of users and disable it for another set of users by assigning them a different Outlook Web App mailbox policy.

    You can control IRM in Exchange ActiveSync per Exchange ActiveSync mailbox policy. To disable or enable IRM in Exchange ActiveSync for an Exchange ActiveSync mailbox policy, use the **Set-ActiveSyncMailboxPolicy** cmdlet and set the *IRMEnabled* parameter to `$false` or `$true` (default). This allows you to enable IRM in Exchange ActiveSync for one set of users and disable it for another set of users by assigning them a different Exchange ActiveSync mailbox policy.

- You can't use the Exchange admin center (EAC) to enable or disable IRM on Client Access servers. You must use the Shell.

## Use the Shell to enable IRM on Client Access servers

This example enables IRM on a Client Access server for an Exchange organization.

```powershell
Set-IRMConfiguration -ClientAccessServerEnabled $true
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## Use the Shell to disable IRM on Client Access servers

This example disables IRM on a Client Access server for an Exchange organization.

```powershell
Set-IRMConfiguration -ClientAccessServerEnabled $false
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## How do you know this worked?

To verify that you have successfully enabled or disabled IRM on Client Access servers, do the following:

- Run the **Get-IRMConfiguration** cmdlet and check the value of the *ClientAccessServerEnabled* property.

    For an example of how to retrieve the IRM configuration, see [Examples](https://technet.microsoft.com/e1821219-fe18-4642-a9c2-58eb0aadd61a\(exchg.150\)#examples) in **Get-IRMConfiguration**.

- Use Outlook Web App to create or read an IRM-protected message.
