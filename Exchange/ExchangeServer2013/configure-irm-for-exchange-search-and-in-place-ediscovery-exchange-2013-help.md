---
title: 'Configure IRM for Exchange Search and In-Place eDiscovery: Exchange 2013 Help'
TOCTitle: Configure IRM for Exchange Search and In-Place eDiscovery
ms:assetid: d96790e9-93ad-4a56-b90f-2dbfa2f2073c
ms:mtpsurl: https://technet.microsoft.com/library/Gg588319(v=EXCHG.150)
ms:contentKeyID: 49319934
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure IRM for Exchange Search and In-Place eDiscovery

_**Applies to:** Exchange Server 2013_

In Microsoft Exchange Server 2013, you can configure Information Rights Management (IRM) so that Exchange Search can index IRM-protected messages.

When members of the Discovery Management role group perform an [In-Place eDiscovery](https://docs.microsoft.com/exchange/security-and-compliance/in-place-ediscovery/in-place-ediscovery) search, IRM-protected messages are returned in the search results and copied to the Discovery mailbox specified in the search. Furthermore, members of the Discovery Management role group can use Outlook Web App to access the IRM-protected messages that were copied to the Discovery mailbox as a result of the discovery search.

> [!NOTE]
> Members of the Discovery Management role group can't access IRM-protected messages exported from a Discovery mailbox to another mailbox or to a .pst file. IRM-protected messages in a Discovery mailbox can be accessed only by using Outlook Web App.

For additional management tasks related to IRM, see [Information Rights Management procedures](information-rights-management-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to completion: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Rights protection" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- IRM must be configured in your Exchange 2013 organization. To learn more, see [Enable or Disable IRM for Internal Messages](enable-or-disable-irm-for-internal-messages-exchange-2013-help.md).

- The Federation mailbox must be added to the Active Directory Rights Management Services (AD RMS) super users group. To learn more, see [Add the Federation Mailbox to the AD RMS Super Users Group](add-the-federation-mailbox-to-the-ad-rms-super-users-group-exchange-2013-help.md).

- You can't use the Exchange admin center (EAC) to configure IRM for Exchange Search and In-Place eDiscovery. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure IRM for Exchange Search

This example configures IRM to allow Exchange Search to index IRM-protected messages.

> [!NOTE]
> By default, the <EM>SearchEnabled</EM> parameter is set to <CODE>$true</CODE>. To disable indexing of IRM-protected messages, set it to <CODE>$false</CODE>. Disabling indexing of IRM-protected messages prevents them from being returned in search results when users search their mailbox or when discovery managers use In-Place eDiscovery.

```powershell
Set-IRMConfiguration -SearchEnabled $true
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## Use the Shell to configure IRM for In-Place eDiscovery

This example enables members of the Discovery Management role group to access IRM-protected messages that reside in the Discovery mailbox.

> [!NOTE]
> By default, the <EM>EDiscoverySuperUserEnabled</EM> parameter is set to <CODE>$true</CODE>. To disable access to IRM-protected messages for members of the Discovery Management role group, set it to <CODE>$false</CODE>.

```powershell
Set-IRMConfiguration -EDiscoverySuperUserEnabled $true
```

For detailed syntax and parameter information, see [Set-IRMConfiguration](https://docs.microsoft.com/powershell/module/exchange/Set-IRMConfiguration).

## How do you know this worked?

To verify that you have successfully configured IRM for Exchange Search and In-Place eDiscovery, use the **Get-IRMConfigurtaion** cmdlet to retrieve the IRM configuration information. For an example of how to retrieve the IRM configuration, see [Examples](https://technet.microsoft.com/e1821219-fe18-4642-a9c2-58eb0aadd61a\(exchg.150\)#examples) in **Get-IRMConfiguration**.
