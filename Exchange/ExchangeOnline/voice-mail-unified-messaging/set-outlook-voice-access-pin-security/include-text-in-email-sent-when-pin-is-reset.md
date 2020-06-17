---
localization_priority: Normal
description: You can include additional text in the email message that's sent to users when their Unified Messaging (UM) or voice mail PIN is reset. You do this by entering custom text in the When a user's Outlook Voice Access PIN is reset box on a UM mailbox policy. The customized text can include, for example, security-related information for UM-enabled users.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: f7a4d775-a588-412f-ac2c-11ab1a5c67eb
ms.reviewer: 
f1.keywords:
- NOCSH
title: Include text with the email message sent when a PIN Is reset in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Include text with the email message sent when a PIN Is reset in Exchange Online

You can include additional text in the email message that's sent to users when their Unified Messaging (UM) or voice mail PIN is reset. You do this by entering custom text in the **When a user's Outlook Voice Access PIN is reset** box on a UM mailbox policy. The customized text can include, for example, security-related information for UM-enabled users.

By default, a PIN used for Outlook Voice Access is reset by the Unified Messaging or voice mail system if the number of failed sign-in attempts exceeds 5. Users can also reset their PINs using the UM features included with Outlook on the web (formerly known as Outlook Web App) or Outlook 2010 or later, or by using Outlook Voice Access from a telephone.

> [!NOTE]
> The text you enter in this box is limited to 512 characters, and can include simple HTML text.

For additional tasks related to Outlook Voice Access PIN security, see [PIN security procedures](pin-security-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: Less than 1 minute.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](../../voice-mail-unified-messaging/set-up-voice-mail/create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to add text to the email message sent to users when their PIN is reset

1. In the EAC, navigate to **Unified Messaging** \> **UM dial plans**. In the list view, select the UM dial plan you want to change, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

2. On the **UM Dial Plan** page, under **UM Mailbox Policies**, select the UM mailbox policy you want to manage, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **UM Mailbox Policy** page \> **Message text**, in the text box for **When a user's Outlook Voice Access PIN is reset**, enter the text you want to include in the email message that's sent when a user's PIN is reset.

4. Click **Save**.

## Use Exchange Online PowerShell to add text to the email message sent to users when their PIN is reset

This example includes the additional text, "Do not share your PIN with other users. Doing so may result in disciplinary action", in the email message sent to users who are associated with the UM mailbox policy `MyUMMailboxPolicy` when their PIN is reset.

```PowerShell
Set-UMMailboxPolicy -identity MyUMMailboxPolicy -ResetPINText "Do not share your PIN with other users. Doing so may result in disciplinary action."
```
