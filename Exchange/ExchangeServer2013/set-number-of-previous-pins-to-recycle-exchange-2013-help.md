---
title: 'Set the number of previous voice mail PINs to recycle: Exchange 2013 Help'
TOCTitle: Set the number of previous voice mail PINs to recycle
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: b094e68e-c493-4576-a6b1-4c780e635405
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Set the number of previous voice mail PINs to recycle in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When Outlook Voice Access users dial in to an Outlook Voice Access number, they're prompted to enter their PIN so that the voice mail system can authenticate them. After they're authenticated, they can access the voice mail, email, calendaring, and personal contact information in their mailbox from any telephone.

Several PIN-related settings can be configured on a Unified Messaging (UM) mailbox policy. The **PIN recycle count** setting specifies the number of unique PINs users must use before they can reuse an old PIN. You can set the value of this setting between 1 and 20. For most organizations, this value should be set to 5 PINs, which is the default. Setting this value too high can frustrate users because it can be difficult for users to create and memorize many PINs. Setting it too low may introduce a security threat to your network.

> [!IMPORTANT]
> The PIN recycle count can't be disabled.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to change the PIN recycle count

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the dial plan you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

4. Click **PIN policies**, and next to **PIN recycle count**, enter a value between 1 and 20.

5. Click **Save**.

## Use the Shell to change the PIN recycle count

This example sets the PIN recycle count on the UM mailbox policy `MyUMMailboxPolicy` to 10.

```powershell
Set-UMMailboxPolicy -Identity MyUMMailboxPolicy -PINHistoryCount 10
```
