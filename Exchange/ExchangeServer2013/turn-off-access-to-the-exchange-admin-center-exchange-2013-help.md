---
title: 'Turn off access to the Exchange admin center: Exchange 2013 Help'
TOCTitle: Turn off access to the Exchange admin center
ms:assetid: 49f4fa77-1722-4703-81c9-8724ae0334fb
ms:mtpsurl: https://technet.microsoft.com/library/JJ218639(v=EXCHG.150)
ms:contentKeyID: 48385052
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Turn off access to the Exchange admin center

_**Applies to:** Exchange Server 2013_

For security purposes, some organizations may want to restrict access to the Exchange admin center (EAC) for users coming from the Internet. This procedure shows you how to turn off access to the EAC. This procedure doesn't prevent users from accessing the Options in Outlook Web App.

> [!NOTE]
> This procedure disables EAC administrator access entirely on the CAS server where the steps are applied. If you to enable EAC administrator for internal users, you should install a separate CAS server and configure it to only handle internal requests using the following command:<BR><CODE>Set-ECPVirtualDirectory -Identity "InternalCAS\ecp (default web site)" -AdminEnabled $True</CODE>

> [!WARNING]
> The procedure applies only to on-premises deployments of Exchange Server 2013.

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Exchange admin center connectivity" entry in the [Exchange and Shell infrastructure permissions](exchange-and-shell-infrastructure-permissions-exchange-2013-help.md) topic.

- You can't use the EAC to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to turn off Internet access to the EAC

This example turns off the access to the EAC on server CAS01.

```powershell
Set-ECPVirtualDirectory -Identity "CAS01\ecp (default web site)" -AdminEnabled $false
```

For detailed syntax and parameter information, see [Set-EcpVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/Set-EcpVirtualDirectory).

## How do you know this worked?

To verify that you have successfully turned off access to the EAC, do the following:

1. Using your Internet browser, type your organization's internal or external URL for accessing Outlook Web App but replace the **/owa** identifier with **/ecp**. For example, if your external URL for accessing Outlook Web App is https://primary.tailspintoys.com/owa, use https://primary.tailspintoys.com/ecp.

2. If access is turned off, you'll receive a **404 - website not found** error.
