---
title: 'Enable faxing for a group of users: Exchange 2013 Help'
TOCTitle: Enable faxing for a group of users
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: b8d9f54d-ff06-4942-83e1-fc6c4ad02178
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Enable faxing for a group of users in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can enable inbound faxes for users linked with a Unified Messaging (UM) mailbox policy. By default, when you enable users for Unified Messaging, users can't receive fax messages until you specify the URI for the fax partner server, deploy a fax partner server for your organization, and enable faxing on a UM mailbox policy. If the option to allow incoming faxes is disabled on the UM dial plan, the users linked with the UM mailbox policy still won't be able to receive faxes. Similarly, if the option to allow incoming faxes is disabled on an individual user, that user won't be able to receive faxes.

For additional management tasks related to faxing, see [Faxing procedures](faxing-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailbox policies" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to enable inbound faxing

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the **UM dial plan** page, under **UM Mailbox Policies**, select the mailbox policy you want to modify, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **UM mailbox policy** page \> **General**, select the check box next to **Allow inbound faxes**.

4. Click **Save** to save your changes.

## Use the Shell to enable inbound faxing

This example allows users who are linked with the UM mailbox policy `MyUMMailboxPolicy` to use inbound faxing.

```powershell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -AllowFax $true
```
