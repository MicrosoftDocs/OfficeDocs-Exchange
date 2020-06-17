---
title: 'Change an extension number: Exchange 2013 Help'
TOCTitle: Change an extension number
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: ff22b366-3bfb-4bf7-9f11-62fba48f1caf
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Change an extension number in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.

You can change the primary extension number that was added when the user was enabled for UM or a secondary extension number that was added later, along with the related EUM proxy addresses for the user. The primary extension number you added when the user was enabled for UM will be listed as the primary EUM proxy address. Any additional secondary extension numbers you added will be listed as secondary EUM proxy addresses. When extension numbers have been changed, callers can leave voice mail for the user at all the new extension numbers that have been set. All the voice messages will be delivered to the same user's mailbox.

You can use the EAC or the Shell to change a primary or a secondary extension number for a user. You can use the **Email Address** page on the user's mailbox in the EAC to change a primary or secondary extension number. You can't use the **UM Mailbox** page in the EAC to change a primary extension number, but you can use it to change a secondary extension number. If you want to change a secondary extension number, you must first remove the existing secondary extension number and then add the correct secondary extension number for the user.

You can view the primary and secondary extension numbers for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in the Shell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform these procedures, confirm that a telephone extension UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform these procedures, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to a telephone extension dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- Before you perform these procedures, confirm that the extension number that will be assigned to the user contains the correct number of digits set on the UM dial plan.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to change the primary or secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox for which you want to change an extension number, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, select the extension number you want to change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif). The primary extension number is listed in bold letters and numbers.

4. On the **Email address** page, in the **Address/Extension** box, enter the new extension number for the user. If you need to select a new UM dial plan, you can click **Browse**.

5. Click **Save**.

## Use the Shell to change the primary or secondary extension number

This example changes the extension number to 22222 for Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you change an extension number using the Shell, you need to determine the position of the EUM proxy address that you want to change. To determine the position, use the **$mbx.EmailAddresses** command. The first EUM proxy address is the default (primary) extension number and it will be 0 in the list.

```powershell
$mbx=Get-Mailbox tony.smith
$mbx.EmailAddresses.Item(0)="eum:22222;phone-context=MyDialPlan.contoso.com"
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
