---
localization_priority: Normal
description: You can specify whether you want individual users to be able to create and manage their own call answering rules by configuring their mailbox properties. By default, they can create call answering rules.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 81863440-8b21-4523-bdab-6a2311889a0d
ms.reviewer: 
f1.keywords:
- NOCSH
title: Call answering rules in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Call answering rules in Exchange Online

You can specify whether you want individual users to be able to create and manage their own call answering rules by configuring their mailbox properties. By default, they can create call answering rules.

You can enable or disable Call Answering Rules for multiple users that are enabled for Unified Messaging (UM) by configuring Call Answering Rules on a UM dial plan or UM mailbox policy.

> [!NOTE]
> You can't use the EAC to configure this feature. You must use Exchange Online PowerShell to enable or disable Call Answering Rules for a voice mail user.

For additional management tasks related to allowing users to forward calls, see [Forwarding calls procedures](forwarding-calls-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- Before you perform this procedure, confirm that the user's mailbox has been UM-enabled. For detailed steps, see [Enable a user for voice mail](../../voice-mail-unified-messaging/set-up-voice-mail/enable-a-user-for-voice-mail.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use Exchange Online PowerShell to enable or disable call answering rules for a UM-enabled user

This example enables Call Answering Rules for the user tony@contoso.com.

```PowerShell
Set-UMMailbox -Identity tony@contoso.com -CallAnsweringRulesEnabled $true
```

This example disables Call Answering Rules for the user tony@contoso.com.

```PowerShell
Set-UMMailbox -Identity tony@contoso.com -CallAnsweringRulesEnabled $false
```
