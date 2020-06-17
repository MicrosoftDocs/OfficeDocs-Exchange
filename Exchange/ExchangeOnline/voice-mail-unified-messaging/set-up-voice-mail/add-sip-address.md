---
localization_priority: Normal
description: When you enable a user for UM and link them to a SIP URI dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains a SIP address for the user. The extension number is used when the user calls in to an Outlook Voice Access number.
ms.topic: article
author: mattpennathe3rd
ms.author: v-mapenn
ms.assetid: 40295bcf-c62b-4f26-95ca-a8c4bd210fb3
ms.reviewer: 
f1.keywords:
- NOCSH
title: Add a SIP address in Exchange Online
ms.collection: exchange-online
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Add a SIP address in Exchange Online

When you enable a user for UM and link them to a SIP URI dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains a SIP address for the user. The extension number is used when the user calls in to an Outlook Voice Access number.

SIP URI dial plans and SIP addresses are used when you're integrating UM and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server. The SIP address is used by Communications Server or Lync Server to route incoming calls and send voice mail to the user. By default, the SIP address that's used by UM will be the SIP address that's used by Communications Server or Lync Server.

The primary SIP address you added when the user was enabled for UM will be listed as the primary EUM proxy address. If the primary SIP address was removed, the first EUM proxy address you add that contains the user's SIP address will be listed as the primary EUM proxy address. Any additional SIP addresses you add will be listed as secondary EUM proxy addresses. When secondary SIP addresses are added, callers can leave voice mail for the user at SIP endpoints that the user is signed in to using the SIP addresses. All the voice messages will be delivered to the same user's mailbox.

You can use the EAC or Exchange Online PowerShell to add a primary or a secondary SIP address for a user. You can use the **Email Address** page on the user's mailbox in the EAC to add a primary or secondary SIP address. You can't use the **UM Mailbox** page in the EAC to add a primary or secondary SIP address.

You can view the primary and secondary SIP addresses for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in Exchange Online PowerShell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Unified Messaging" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- Before you perform these procedures, confirm that a SIP URI UM dial plan has been created. For detailed steps, see [Create a UM dial plan](../../voice-mail-unified-messaging/connect-voice-mail-system/create-um-dial-plan.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy.md).

- Before you perform these procedures, confirm that the existing user is enabled for UM and linked to a SIP URI dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail.md).

- Before you perform these procedures, confirm that the SIP address that will be assigned to the user is valid and formatted correctly.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://go.microsoft.com/fwlink/p/?linkId=267542) or [Exchange Online Protection](https://go.microsoft.com/fwlink/p/?linkId=285351).

## Use the EAC to add a primary or secondary SIP address

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to add a SIP address, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif).

4. On the **New email address** page, select **EUM** and, in the **Address/Extension** box, enter the new SIP address for the user.

5. On the **New email address** page, under **Dial plan**, click **Browse** to select the SIP URI dial plan, and then click **OK**.

6. Click **Save**.

## Use Exchange Online PowerShell to add a SIP address

This example adds a SIP address for Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you add a SIP address using Exchange Online PowerShell, you need to determine the position of the EUM proxy address that you want to add. To determine the position, use the **$mbx.EmailAddresses** command. The first proxy address in the list will be 0.

```PowerShell
$mbx=Get-Mailbox tony.smith
$mbx.EmailAddresses +="eum:tsmit@contoso.com;phone-context=MyDialPlan.contoso.com"
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
