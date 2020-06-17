---
title: 'Configure the Managed Folder Assistant: Exchange 2013 Help'
TOCTitle: Configure the Managed Folder Assistant
ms:assetid: 9fcfb9b6-bd24-4218-a163-bc599cd5476a
ms:mtpsurl: https://technet.microsoft.com/library/Bb123958(v=EXCHG.150)
ms:contentKeyID: 49318583
ms.reviewer: 
manager: serdars
ms.author: v-mapenn
author: mattpennathe3rd
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the Managed Folder Assistant

_**Applies to:** Exchange Server 2013_

The *Managed Folder Assistant* is a Microsoft Exchange Mailbox Assistant that applies message retention settings configured in retention policies.

For additional management tasks related to messaging records management (MRM), see [Messaging Records Management Procedures](https://docs.microsoft.com/office365/securitycompliance/inactive-mailboxes-in-office-365).

## What do you need to know before you begin?

- Time to complete: 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Messaging records management" entry in the [Messaging policy and compliance permissions](messaging-policy-and-compliance-permissions-exchange-2013-help.md) topic.

- You can't use the Exchange admin center (EAC) to configure the Managed Folder Assistant. You must use the Shell

- In Exchange 2013, the Managed Folder Assistant is a throttle-based assistant. Throttle-based assistants are always running and don't need to be scheduled. The system resources they can consume are throttled. You can configure the Managed Folder Assistant to process all mailboxes on a Mailbox server within a certain period (known as a *work cycle)*. The work cycle is set to one day by default.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkid=60612).

## Use the Shell to configure the Managed Folder Assistant

This example configures the Managed Folder Assistant to process all mailboxes within one day.

```powershell
Set-MailboxServer MyMailboxServer -ManagedFolderWorkCycle 1
```

For detailed syntax and parameter information, see [Set-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Set-MailboxServer).

## How do I know this worked?

To verify that you have successfully configured the Managed Folder Assistant, use the [Get-MailboxServer](https://docs.microsoft.com/powershell/module/exchange/Get-MailboxServer) cmdlet to check the *ManagedFolderWorkCycle* parameter.

This command retrieves all Mailbox servers in the organization and outputs the Managed Folder Assistant's workcycle properties from each server in a table format. The *Auto* switch is used to automatically fit column width.

```powershell
Get-MailboxServer | Format-Table Name,ManagedFolderWorkCycle* -Auto
```

## Use the Shell to start the Managed Folder Assistant

This example triggers the Managed Folder Assistant to immediately process Morris Cornejo's mailbox.

```powershell
Start-ManagedFolderAssistant -Identity morris.cornejo@contoso.com
```

For detailed syntax and parameter information, see [Start-ManagedFolderAssistant](https://docs.microsoft.com/powershell/module/exchange/Start-ManagedFolderAssistant).
