---
title: 'Change a SIP address: Exchange 2013 Help'
TOCTitle: Change a SIP address
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 33f4f464-9baa-48af-bf5e-a0d55bb45f60
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change a SIP address in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you enable a user for UM and link them to a SIP URI dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains a SIP address for the user. The extension number is used when the user calls in to an Outlook Voice Access number.

SIP URI dial plans and SIP addresses are used when you're integrating UM and Microsoft Office Communications Server 2007 R2 or Microsoft Lync Server. The SIP address is used by Communications Server or Lync Server to route incoming calls and send voice mail to the user. By default, the SIP address that's used by UM will be the SIP address that's used by Communications Server or Lync Server.

You can change the primary SIP address that was added when the user was enabled for UM or a secondary SIP address that was added later, along with the EUM proxy addresses for the user. The primary SIP address you added when the user was enabled for UM will be listed as the primary EUM proxy address. Any additional secondary SIP addresses you added will be listed as secondary EUM proxy addresses. When secondary SIP addresses are changed, callers can leave voice mail for the user at all SIP endpoints that the user is signed in to using the new SIP addresses. All the voice messages will be delivered to the same user's mailbox.

You can use the EAC or the Shell to change a primary or a secondary SIP address. You can use the **Email Address** page on the user's mailbox in the EAC to change a primary or a secondary SIP address. You can't use the **UM Mailbox** page in the EAC to change a primary or secondary SIP address.

You can view the primary and secondary SIP addresses for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in the Shell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a SIP URI UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the existing user is enabled for UM and linked to a SIP URI dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- Before you perform these procedures, confirm that the SIP address that will be assigned to the user is valid and formatted correctly.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to change the primary or a secondary SIP address

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to change a SIP address, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, select the SIP address you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif). The primary SIP address is listed in bold letters and numbers.

4. On the **Email address** page, in the **Address/Extension** box, enter the new SIP address for the user, and then click **OK**. If you need to select a new UM dial plan, you can click **Browse**.

5. Click **Save**.

## Use the Shell to change the primary or a secondary SIP address

This example changes a SIP address for Tony Smith.

> [!NOTE]
> Before you change a SIP address using the Shell, you need to determine the position of the EUM proxy address that you want to change. To determine the position, use the **$mbx.EmailAddresses** command. The first EUM proxy address is the default (primary) SIP address and it will be 0 in the list.

```powershell
$mbx=Get-Mailbox tony.smith
$mbx.EmailAddresses.Item(1)="eum:tsmith@contoso.com;phone-context=MySIPDialPlan.contoso.com"
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
