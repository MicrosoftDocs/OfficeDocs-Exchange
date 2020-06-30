---
title: 'Configure an Outlook Voice Access number: Exchange 2013 Help'
TOCTitle: Configure an Outlook Voice Access number
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 443c838e-f266-4893-b6b2-e5fc96579b55
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure an Outlook Voice Access number in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

An Outlook Voice Access number lets a user who is enabled for Unified Messaging (UM) and voice mail access their mailbox using Outlook Voice Access. When you configure an Outlook Voice Access or subscriber access number on a dial plan, UM-enabled users can call in to the number, sign in to their mailbox, and access their email, voice mail, calendar, and personal contact information.

By default, when you create a UM dial plan, an Outlook Voice Access number isn't configured. To configure an Outlook Voice Access number, you first need to create the dial plan, and then configure an Outlook Voice Access number under the dial plan's **Outlook Voice Access** option. Although an Outlook Voice Access number isn't required, you need to configure at least one Outlook Voice Access number to enable a UM-enabled user to use Outlook Voice Access to access their mailbox. You can configure multiple Outlook Voice Access numbers for a single dial plan.

Outlook Voice Access numbers can contain alphabetical, numeric, and special characters, separators, and spaces. For example:

- +14255551010

- +1-425-555-1010

- 4255551010

- +1 425 555 1010

- 1-800-555-CALL

For more information about the menu options available for Outlook Voice Access users, see the Quick Reference Guide for Outlook Voice Access, which is available from the [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=17369).

For additional management tasks related to UM dial plans, see [UM dial plan procedures in Exchange Server](um-dial-plan-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM dial plans" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure an Outlook Voice Access number

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**.

2. In the list view, select the UM dial plan you want to modify and on the toolbar, click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM dial plan** page, click **Configure**.

4. In **Outlook Voice Access**, under **Outlook Voice Access numbers**, use the box to enter the number you want to use, and then click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

5. Click **Save**.

## Use the Shell to configure an Outlook Voice Access number

This example sets the Outlook Voice Access number to 4255550100 for a UM dial plan named `MyUMDialPlan`.

```powershell
Set-UMDialPlan -identity MyUMDialPlan -AccessTelephoneNumbers 4255550100
```
