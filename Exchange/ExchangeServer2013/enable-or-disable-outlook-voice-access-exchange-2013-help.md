---
title: 'Enable or disable Outlook Voice Access for users: Exchange 2013 Help'
TOCTitle: Enable or disable Outlook Voice Access for users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c0c244a0-ad2f-4adf-bc1f-1d55fd7ea2d5
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable or disable Outlook Voice Access for users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable or disable access to Outlook Voice Access for UM-enabled users who are associated with a Unified Messaging (UM) mailbox policy. Outlook Voice Access is a feature used by UM-enabled users to access their mailbox over a phone. By default, this setting is enabled.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable Outlook Voice Access

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. Under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, select or clear the check box next to **Allow Outlook Voice Access**.

4. Click **Save**.

## Use the Shell to enable or disable Outlook Voice Access

This example allows users who are associated with the UM mailbox policy `MyUMMailboxPolicy` to use Outlook Voice Access.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowSubscriberAccess $true
```

This example prevents users who are associated with the UM mailbox policy `MyUMMailboxPolicy` from using Outlook Voice Access.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowSubscriberAccess $false
```
