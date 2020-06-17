---
localization_priority: Normal
description: When you enable a user for Unified Messaging (UM), a default set of properties are applied to the user, and the user will be able to use the voice mail features included with Unified Messaging. After you enable a user for voice mail, you have the option of adding a Session Initiation Protocol (SIP) address for the user if they're assigned to a UM mailbox policy that's linked to a SIP URI dial plan. Or, you can add an E.164 number for the user if they're assigned to a UM mailbox policy that's linked to an E.164 dial plan. In both cases, the user must still have an extension number configured.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: ad027767-5e14-4cb1-9f8a-0791d9188db5
ms.reviewer: 
f1.keywords:
- CSH
ms.custom:
- Microsoft.Exchange.Management.SnapIn.Esm.Recipients.EnableUnifiedMessagingWizardForm.EnableUnifiedMessagingWizardPage
title: Enable a user for voice mail in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Enable a user for voice mail in Exchange Online

When you enable a user for Unified Messaging (UM), a default set of properties are applied to the user, and the user will be able to use the voice mail features included with Unified Messaging. After you enable a user for voice mail, you have the option of adding a Session Initiation Protocol (SIP) address for the user if they're assigned to a UM mailbox policy that's linked to a SIP URI dial plan. Or, you can add an E.164 number for the user if they're assigned to a UM mailbox policy that's linked to an E.164 dial plan. In both cases, the user must still have an extension number configured.

An extension number is required for each user that's associated with a telephone extension, SIP Uniform Resource Identifier (URI), or E.164 dial plan. The extension number must be the correct number of digits, as specified in the UM dial plan for the UM mailbox policy.

> [!NOTE]
> You must add, remove, or modify extension numbers for all UM-enabled users by using the EAC or Exchange Online PowerShell, even if they're linked to a SIP URI or E.164 dial plan. To add, remove or modify SIP address or E.164 numbers for users, you'll need to use Exchange Online PowerShell because those options aren't available in the EAC.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to enable a user for voice mail

1. In the EAC, click **Recipients**.

2. In the List view, select the user whose mailbox you want to enable for Unified Messaging.

3. In the Details pane, under **Phone and Voice Features**, click **Enable**.

4. On the **Enable UM mailbox** page, click the **Browse** button next to **UM mailbox policy**, locate the UM mailbox policy to assign the user from the list, and then click **OK**.

5. On the **Enable UM mailbox** page, complete the following boxes:

   - **SIP address** or **E.164 number**: In the **SIP address** or **E.164 number** text box, enter the SIP address or E.164 number for the user. These options are available if the user that you enable for Unified Messaging is assigned to a UM mailbox policy that's linked to either a SIP URI or an E.164 dial plan. You can't add a SIP address or E.164 number for a user if the user is associated with a telephone extension dial plan.

     When you assign a user to a UM mailbox policy that's linked to a SIP URI or E.164 dial plan, you must enter an extension number for the user. The user will use this extension number when accessing their mailbox via Outlook Voice Access. The number of digits that you configure in this box must match the number of digits configured on the SIP URI or E.164 dial plan.

   - **Extension number**: Use this text box to manually enter the extension number for the user you're enabling for UM.

     You must provide a valid extension number for the user and must match the number of digits specified on the dial plan. You can only enter digits from 1 through 20. The typical extension number is 3 to 7 digits long. The number of digits in the extension is set on the dial plan that's linked to the UM mailbox policy that's assigned to the user.

   - Under **PIN settings**, complete the following:

   - **Automatically generate PIN**: Click this button to automatically generate a PIN for the UM-enabled user to use for voice mail access via Outlook Voice Access. This is the default setting. The PIN is automatically generated based on the PIN policies configured on the UM mailbox policy assigned to the user. Using this setting will help protect the user's PIN. The PIN is sent to the user in the welcome message they receive after they're enabled for UM. By default, they'll have to change this PIN when they first sign in to their mailbox to get their voice mail.

   - **Type a PIN**: Click this button to enter a PIN that the user will use to access the voice mail system. The PIN must comply with the PIN policy settings configured on the UM mailbox policy associated with this UM-enabled user. For example, if the UM mailbox policy is configured to accept only PINs that contain seven or more digits, the PIN you enter in this box must be at least seven digits long.

   - **Require the user to reset their PIN the first time they sign in**: Select this check box to force the user to reset their voice mail PIN when they access the voice mail system from a telephone using Outlook Voice Access for the first time. They will be prompted to enter a PIN that's more familiar to them.It's a security best practice to force UM-enabled users to change their PIN when they first sign in to help protect against unauthorized access to their data and Inbox. This check box is selected by default.

6. On the **Enable UM mailbox** page, review your settings. Click **Finish** to enable the user for voice mail. Click **Back** to make configuration changes.

## Use Exchange Online PowerShell to enable a user for voice mail

This example enables Unified Messaging on the mailbox of tonysmith@contoso.com, sets the extension number to 51234, sets the PIN for the user to 5643892, and assigns the user to a UM mailbox policy named `MyUMMailboxPolicy`.

```PowerShell
Enable-UMMailbox -Identity tonysmith@contoso.com -UMMailboxPolicy MyUMMailboxPolicy -Extensions 51234 -PIN 5643892 -PINExpired $true
```

This example enables Unified Messaging on the mailbox of tonysmith@contoso.com, assigns the user to a UM mailbox policy named `MyUMMailboxPolicy`, and sets the extension number, SIP address, and PIN for the user.

```PowerShell
Enable-UMMailbox -Identity tonysmith@contoso.com -UMMailboxPolicy MyUMMailboxPolicy -Extensions 51234 -PIN 5643892 -SIPResourceIdentifier "tonysmith@contoso.com" -PINExpired $true
```
