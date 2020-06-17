---
localization_priority: Normal
description: When you enable a user for UM and link them to a SIP URI dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains a SIP address for the user. The extension number is used when the user calls in to an Outlook Voice Access number.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: eaaff0b0-7d85-4845-a7b8-ac22b42bc415
ms.reviewer: 
f1.keywords:
- NOCSH
title: Remove a SIP address in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Remove a SIP address in Exchange Online

When you enable a user for UM and link them to a SIP URI dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains a SIP address for the user. The extension number is used when the user calls in to an Outlook Voice Access number.

SIP URI dial plans and SIP addresses are used when you're integrating UM and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server. The SIP address is used by Communications Server or Lync Server to route incoming calls and send voice mail to the user. By default, the SIP address that's used by UM will be the SIP address that's used by Communications Server or Lync Server.

You can remove the primary SIP address that was added when the user was enabled for UM or a secondary SIP address that was added later, along with the EUM proxy address for the user. The primary SIP address you added when the user was enabled for UM will be listed as the primary EUM proxy address. Any additional SIP addresses you added will be listed as secondary EUM proxy addresses. When a SIP address is removed, callers can no longer leave voice mail for the user at the SIP address that was removed even if the user is signed in with the SIP address assigned to the user in Communications Server or Lync Server.

If you remove the primary SIP address, UM won't be able to send voice mail to the user's mailbox and call answering rules won't be processed. After the primary SIP address has been removed, the EUM proxy address for the user will be listed as **Null** on the user's mailbox in the EAC and when you run the **Get-Mailbox** cmdlet in Exchange Online PowerShell. Also, when you run the **Get-UMMailbox** cmdlet, the _Extensions_, _PhoneNumber_, and _CallAnsweringRulesExtensions_ parameters will be blank or null.

You can use the EAC or Exchange Online PowerShell to remove a primary or a secondary SIP address. You can use the **Email Address** page on the user's mailbox in the EAC to remove a primary or a secondary SIP address. You can't use the **UM Mailbox** page in the EAC to remove a primary or secondary SIP address.

You can view the primary and secondary SIP addresses for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in Exchange Online PowerShell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to a SIP URI dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- Before you perform these procedures, confirm that the primary and secondary SIP addresses are configured for the user.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to remove the primary or a secondary SIP address

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox from which you want to remove a SIP address, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, select the SIP address that you want to remove from the list, and then click **Delete** ![Delete icon](../../media/ITPro_EAC_DeleteIcon.gif). The primary EUM proxy address or SIP address is listed in bold letters and numbers.

4. Click **Save**.

## Use Exchange Online PowerShell to remove the primary or a secondary SIP address

This example removes the SIP address which is second in the list of available addresses from the mailbox of Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you remove a SIP address using Exchange Online PowerShell, you need to determine the position of the EUM proxy address that you want to modify. To determine the position, use the **$mbx.EmailAddresses** command. The first EUM proxy address in the list will be 0.

```PowerShell
$mbx = Get-Mailbox tony.smith
$mbx.EmailAddresses.Remove($mbx.EmailAddresses.Item(1))
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
