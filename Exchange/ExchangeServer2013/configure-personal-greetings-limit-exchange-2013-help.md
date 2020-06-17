---
title: 'Configure the limit on personal greetings for Outlook Voice Access users: Exchange 2013 Help'
TOCTitle: Configure the limit on personal greetings for Outlook Voice Access users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: d400f250-0f55-45f5-9918-5f1d7819fbdf
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure the limit on personal greetings for Outlook Voice Access users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

The **Limit on personal greetings (minutes)** setting enables you to enter the maximum number of minutes that users associated with the Unified Messaging (UM) mailbox policy can use to record their voice mail greetings. This setting applies to both their standard voice mail and their Out of Office voice mail greetings. By default, the maximum greeting duration is set to 5 minutes. However, you can configure the maximum greeting duration to any setting between 1 and 10 minutes.

For additional management tasks related to UM mailbox policies, see [UM mailbox policy procedures](um-mailbox-policy-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to change the maximum greeting duration

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM mailbox policy** page \> **General**, under **Limit on personal greetings (minutes)**, enter the length of time, in minutes, allowed for personal greetings for voice mail users.

4. Click **Save**.

## Use the Shell to change the maximum greeting duration

This example configures the maximum greeting duration on the UM mailbox policy `MyUMMailboxPolicy` to 3 minutes.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy MaxGreetingDuration 3
```
