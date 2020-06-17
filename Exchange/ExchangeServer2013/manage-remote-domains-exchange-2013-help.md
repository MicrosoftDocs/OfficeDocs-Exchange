---
title: 'Manage remote domains: Exchange 2013 Help'
TOCTitle: Manage remote domains
ms:assetid: 41a86907-bd9e-40d0-94d3-6deb95a0bffa
ms:mtpsurl: https://technet.microsoft.com/library/Aa997639(v=EXCHG.150)
ms:contentKeyID: 51438504
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- CSH
mtps_version: v=EXCHG.150
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.OrganizationConfiguration.NewRemoteDomainWizardForm.NewRemoteDomainWizardPage
---

# Manage remote domains

_**Applies to:** Exchange Server 2013_

Remote domains are SMTP domains that are external to your Microsoft Exchange organization. You can create remote domain entries to define the settings for message transferred between your Exchange organization and specific external domains. The settings in the remote domain entry for a specific external domain override the settings in the default remote domain that normally apply to all external recipients. The remote domain settings are global for the Exchange organization.

If you remove a remote domain entry, the settings for message transfer no longer apply to messages sent to the remote domain. Removing a remote domain entry doesn't disable mail flow to the remote domain. After a remote domain entry is removed, the configuration settings of the default remote domain apply to new messages sent to that domain. You can't remove the default remote domain.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 10 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Remote domains" entry in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic.

- You can only use the Shell to perform this procedure.

- You can't create a remote domain for an address space that's configured as an accepted domain in your organization. For example, if your organization accepts mail for fabrikam.com, you can't create a remote domain for fabrikam.com.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## What Do You Want to Do?

## Use the Shell to create a remote domain

To create a new remote domain entry, use the following syntax.

```powershell
New-RemoteDomain -Name <Descriptive Name> -DomainName <SMTP address space>
```

This example creates a remote domain entry for messages sent to the contoso.com domain.

```powershell
New-RemoteDomain -Name Contoso -DomainName contoso.com
```

This example creates a remote domain entry for messages sent to the fabrikam.com domain and all subdomains.

```powershell
New-RemoteDomain -Name Fabrikam -DomainName *.fabrikam.com
```

## How do you know this worked?

To verify that you have successfully created a remote domain, do the following:

1. Run the command **Get-RemoteDomain** and verify that the remote domain is listed.

2. Run the command `Get-RemoteDomain <Remote Domain Name> | Format-List` to verify the settings on the new remote domain. Send a test message to a recipient in the address space that's specified in the new remote domain entry, and verify that the message settings match those specified by the new remote domain entry.

## Use the Shell to configure a remote domain

You configure the settings in the remote domain entry using the **Set-RemoteDomain** cmdlet. There are many different settings that relate to automatic replies, message format and encoding, and other message settings. For more information, see [Set-RemoteDomain](https://docs.microsoft.com/powershell/module/exchange/Set-RemoteDomain).

To configure remote domains for specific scenarios, see the following topics:

- [Configure remote domain out of office replies](configure-remote-domain-out-of-office-replies-exchange-2013-help.md)

- [Configure remote domain automatic replies](configure-remote-domain-automatic-replies-exchange-2013-help.md)

- [Configure remote domain message reporting](configure-remote-domain-message-reporting-exchange-2013-help.md)

## Use the Shell to remove a remote domain

To remove a remote domain entry, use the following syntax.

```powershell
Remove-RemoteDomain <RemoteDomainName>
```

This example removes the remote domain entry named Contoso

```powershell
Remove-RemoteDomain Contoso
```

## How do you know this worked?

To verify that you have successfully removed the remote domain, do the following:

1. Run the command **Get-RemoteDomain** and verify that the remote domain is isn't listed.

2. Run the command `Get-RemoteDomain Default | Format-List` to verify the settings on the default remote domain. Send a test message to a recipient in the address space that was specified in the removed remote domain, and verify that the message settings match those specified by the default remote domain.
