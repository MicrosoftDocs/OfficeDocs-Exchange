---
title: 'Create an offline address book virtual directory: Exchange 2013 Help'
TOCTitle: Create an offline address book virtual directory
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 2c70e21f-2b12-414a-9e8c-65634a767c72
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Create an offline address book virtual directory in Exchange 2013

_**Applies to:** Exchange Server 2013_

The OAB virtual directory is the distribution for the OAB. By default, when Microsoft Exchange Server 2013 is installed, a new virtual directory named OAB is created in the default internal website in Internet Information Services (IIS). If you have client-side users that connect to Microsoft Outlook from outside your organization's firewall, you can add an external website. Alternatively, when you run the **New-OABVirtualDirectory** cmdlet in the Shell, a new virtual directory named OAB is created in the default IIS website on the local Exchange server.

Creating an OAB virtual directory isn't a common task. Exchange allows for one OAB virtual directory named OAB, and you should create an OAB virtual directory only if there is a problem with the existing OAB virtual directory, and the previous OAB virtual directory was removed.

For additional management tasks related to OABs, see [Offline address book procedures](offline-address-book-procedures-exchange-2013-help.md).

> [!IMPORTANT]
> Before you create an OAB virtual directory, make sure that your users are aware of the changes you are making. This procedure may interrupt the OAB downloading process for your users.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Offline address books" entry in the [Email address and address book permissions](email-address-and-address-book-permissions-exchange-2013-help.md) topic.

- The local Exchange server must have the Client Access server role installed.

- A default IIS website must exist (for example, /w3svc/1/root).

- A virtual directory named OAB doesn't already exist.

- Although Web-based distribution is enabled by default and doesn't require further configuration, we recommend that you enable Secure Sockets Layer (SSL) for the OAB distribution point.

- You can't use the Exchange admin center (EAC) to perform this procedure. You must use the Shell.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to create an OAB virtual directory

To create an OAB virtual directory with all of the default settings, you can run the **New-OABVirtualDirectory** cmdlet without any parameters. Use the following procedure to create an OAB virtual directory with custom settings.

> [!NOTE]
> When creating an OAB virtual directory, we recommend that you have SSL enabled.

This example creates an OAB virtual directory on the Client Access server named CASServer01 that has SSL enabled and an external URL.

```powershell
New-OABVirtualDirectory -Server CASServer01 -RequireSSL $true -ExternalURL "https://www.contoso.com/OAB"
```

After you create a new OAB virtual directory, you must edit the settings on each OAB that uses Web-based distribution to reconnect to the OAB virtual directory. For more information, see [Change the offline address book generation schedule](change-address-book-generation-schedule-exchange-2013-help.md).

For detailed syntax and parameter information, see [New-OABVirtualDirectory](https://docs.microsoft.com/powershell/module/exchange/new-oabvirtualdirectory).
