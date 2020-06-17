---
title: 'Call answering rules in the same mailbox policy: Exchange 2013 Help'
TOCTitle: Call answering rules in the same mailbox policy
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: e44acaa6-d5a8-41e8-94aa-100be0bd6391
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Call answering rules in the same mailbox policy in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can allow users who are associated with a Unified Messaging (UM) mailbox policy to configure call answering rules, or prevent them from doing so. If the option to configure call answering rules is disabled on a UM dial plan, the Call Answering Rules feature won't be available to UM-enabled users associated with the UM mailbox policy. The default setting is enabled.

For additional management tasks related to allowing users to forward calls, see [Forwarding calls procedures](forwarding-calls-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable or disable call answering rules on a UM mailbox policy

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. Under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page, select or clear the check box next to **Allow users to configure call answering rules**.

4. Click **Save**.

## Use the Shell to enable or disable call answering rules on a UM mailbox policy

This example allows users who are associated with the UM mailbox policy `MyUMMailboxPolicy` to create call answering rules.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowCallAnsweringRules $true
```

This example prevents users who are associated with the UM mailbox policy `MyUMMailboxPolicy` from creating call answering rules.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowCallAnsweringRules $false
```
