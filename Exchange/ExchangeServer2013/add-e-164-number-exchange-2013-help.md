---
title: 'Add an E.164 number: Exchange 2013 Help'
TOCTitle: Add an E.164 number
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: fab86207-be03-40ef-9fea-045a50f3d122
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Add an E.164 number in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you enable a user for UM and link them to an E.164 dial plan, two EUM proxy addresses are created. One contains the user's extension number and the other contains the E.164 number for the user. The extension number is used when the user calls in to an Outlook Voice Access number.

The primary E.164 number you added when the user was enabled for UM will be listed as the primary EUM proxy address. If the primary E.164 number was removed, the first EUM proxy address you add that contains the user's E.164 number will be listed as the primary EUM proxy address. Any additional E.164 numbers you add will be listed as secondary EUM proxy addresses. When additional E.164 numbers are added, callers can leave voice mail for the user at all E.164 numbers that have been set. All the voice messages will be delivered to the same user's mailbox.

You can use the EAC or the Shell to add a primary or a secondary E.164 number for a user. You can use the **Email Address** page on the user's mailbox in the EAC to add a primary or secondary E.164 number. You can't use the **UM Mailbox** page in the EAC to add a primary or secondary E.164 number.

You can view the primary and secondary E.164 numbers for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in the Shell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that an E.164 UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to an E.164 dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- Before you perform these procedures, confirm that the E.164 number that will be assigned to the user is valid and formatted correctly.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to add a primary or secondary E.164 number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to add an E.164 number, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif).

4. On the **New email address** page, select **EUM** and, in the **Address/Extension** box, enter the new E.164 number for the user.

5. On the **New email address** page, under **Dial plan**, click **Browse** to select the E.164 dial plan and then click **OK**.

6. Click **Save**.

## Use the Shell to add an E.164 number

This example adds an E.164 number for Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you add an E.164 number using the Shell, you need to determine the position of the EUM proxy address that you want to add. To determine the position, use the **$mbx.EmailAddresses** command. The first proxy address in the list will be 0.

```powershell
$mbx=Get-Mailbox tony.smith
$mbx.EmailAddresses.Item(2)="eum:+14255550123;phone-context=MyDialPlan.contoso.com"
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
