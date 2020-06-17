---
title: 'Remove an Outlook Web App mailbox policy from Exchange: Exchange 2013 Help'
TOCTitle: Remove an Outlook Web App mailbox policy from Exchange
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: edab7bac-b62c-4b82-8f21-dcac77cf0e8f
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an Outlook Web App mailbox policy from Exchange 2013

_**Applies to:** Exchange Server 2013_

You can remove a Microsoft Outlook Web App mailbox policy from an Exchange organization by using either the EAC or the Shell.

For additional management tasks related to Outlook Web App mailbox policies, see [Outlook Web App mailbox policies](outlook-web-app-mailbox-policies-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete each procedure: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Outlook Web App mailbox policies" entry in the [Clients and mobile devices permissions](clients-and-mobile-devices-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to remove an Outlook Web App mailbox policy

1. In the EAC, click **Permissions** \> **Outlook Web App policies**.

2. In the result pane, click to select the mailbox policy you want to remove.

3. Click the **Delete** button.

4. In the confirmation window, click **Yes** to remove the mailbox policy, or click **No** to cancel.

## Use the Shell to remove an Outlook Web App mailbox policy

This example removes an Outlook Web App mailbox policy named `Policy1`.

```powershell
Remove-OwaMailboxPolicy -Name Policy1
```

For more information about syntax and parameters, see [Remove-OwaMailboxPolicy](https://docs.microsoft.com/powershell/module/exchange/remove-owamailboxpolicy).

## How do you know this worked?

To verify that you've successfully removed an Outlook Web App mailbox policy:

- In the EAC, click **Permissions** \> **Outlook Web App policies**. The policy you removed should no longer appear in the list.
