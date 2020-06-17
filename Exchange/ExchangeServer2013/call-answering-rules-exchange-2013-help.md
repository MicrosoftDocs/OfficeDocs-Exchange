---
title: 'Call answering rules: Exchange 2013 Help'
TOCTitle: Call answering rules
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 81863440-8b21-4523-bdab-6a2311889a0d
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Call answering rules in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

You can specify whether you want individual users to be able to create and manage their own call answering rules by configuring their mailbox properties. By default, they can create call answering rules.

You can enable or disable Call Answering Rules for multiple users that are enabled for Unified Messaging (UM) by configuring Call Answering Rules on a UM dial plan or UM mailbox policy.

> [!NOTE]
> You can't use the EAC to configure this feature. You must use the Shell to enable or disable Call Answering Rules for a voice mail user.

For additional management tasks related to allowing users to forward calls, see [Forwarding calls procedures](forwarding-calls-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the Shell to enable or disable call answering rules for a UM-enabled user

This example enables Call Answering Rules for the user tony@contoso.com.

```powershell
Set-UMMailbox -Identity tony@contoso.com -CallAnsweringRulesEnabled $true
```

This example disables Call Answering Rules for the user tony@contoso.com.

```powershell
Set-UMMailbox -Identity tony@contoso.com -CallAnsweringRulesEnabled $false
```
