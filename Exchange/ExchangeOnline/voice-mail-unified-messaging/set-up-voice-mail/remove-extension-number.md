---
localization_priority: Normal
description: When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: c2b896cf-21f7-4453-a4e6-b23d236a6dd3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Remove an extension number in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Remove an extension number in Exchange Online

When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.

You can remove the primary extension number that was added when the user was enabled for UM or a secondary extension number that was added later, along with the related EUM proxy addresses for the user. The primary extension number you added when the user was enabled for UM will be listed as the primary EUM proxy address. Any additional extension numbers you added will be listed as secondary EUM proxy addresses. When an extension number is removed, callers can no longer leave voice mail for the user at the extension number that was removed.

If you remove the primary extension number, UM won't be able to send voice mail to the user's mailbox and call answering rules won't be processed. After the primary extension number has been removed, the EUM proxy address for the user will be listed as **Null** on the user's mailbox in the EAC and when you run the **Get-Mailbox** cmdlet in Exchange Online PowerShell. Also, when you run the **Get-UMMailbox** cmdlet, the _Extensions_, _PhoneNumber_, and _CallAnsweringRulesExtensions_ parameters will be blank or null.

You can use the EAC or Exchange Online PowerShell to remove a primary or a secondary extension number. You can use the **Email Address** page on the user's mailbox in the EAC to remove a primary or a secondary extension number. You can't use the **UM Mailbox** page in the EAC to remove a primary extension number, but you can use it to remove a secondary extension number.

You can view the primary and secondary extension numbers for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in Exchange Online PowerShell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to a telephone extension dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- Before you perform these procedures, confirm that the primary and secondary extension numbers are configured for the user.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to remove the primary or secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox from which you want to remove an extension number, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, select the extension number that you want to remove from the list, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif). The primary EUM proxy address or extension number is listed in bold letters and numbers.

4. Click **Save**.

## Use the EAC to remove a secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the user whose mailbox you want to remove an extension number from.

3. In the details pane, under **Phone and Voice Features** \> **Unified Messaging**, click **View details**.

4. On the **Other extensions** page, in the **Extension number** box, select the extension number you want to remove, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif).

5. Click **Save**.

## Use Exchange Online PowerShell to remove an extension number

This example removes the extension number 12345 from the mailbox of Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you remove an extension number using Exchange Online PowerShell, you need to determine the position of the EUM proxy address that you want to modify. To determine the position, use the **$mbx.EmailAddresses** command. The first EUM proxy address in the list will be 0.

```PowerShell
$mbx = Get-Mailbox tony.smith
$mbx.EmailAddresses.remove("eum:22222;phone-context=MyDialPlan.contoso.com")
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
