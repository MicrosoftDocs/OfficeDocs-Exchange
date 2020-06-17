---
title: 'Enable or disable Play on Phone for Outlook Voice Access users: Exchange 2013 Help'
TOCTitle: Enable or disable Play on Phone for Outlook Voice Access users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: d3281a97-6fc6-42a3-855f-1af1184a644a
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable Play on Phone for Outlook Voice Access users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable or disable the Play on Phone feature for users associated with a Unified Messaging (UM) mailbox policy. This option is enabled by default and allows users to play their voice mail messages over any phone. This option isn't available to UM-enabled users who have a mailbox on a Microsoft Exchange Server 2007 server.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable Play on Phone

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, select or clear the check box next to **Allow Play on Phone for voice mail**.

4. Click **Save**.

## Use the Shell to enable or disable Play on Phone

This example enables the Play on Phone feature for users who are associated with the UM mailbox policy `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowPlayOnPhone $true
```

This example disables the Play on Phone feature for users who are associated with the UM mailbox policy `MyUMMailboxPolicy`.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowPlayOnPhone $false
```
