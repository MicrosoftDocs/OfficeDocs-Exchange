---
title: 'Remove an extension number: Exchange 2013 Help'
TOCTitle: Remove an extension number
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c2b896cf-21f7-4453-a4e6-b23d236a6dd3
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Remove an extension number in Exchange Server

_**Applies to:** Exchange Server 2013, Exchange Server 2016_

When you enable a user for UM and link them to a telephone extension dial plan, an EUM proxy address is created for the user that contains the user's extension number. You must define at least one extension number for UM to use so voice mail can be sent to the user's mailbox. The extension number is also used when the user calls in to an Outlook Voice Access number.

You can remove the primary extension number that was added when the user was enabled for UM or a secondary extension number that was added later, along with the related EUM proxy addresses for the user. The primary extension number you added when the user was enabled for UM will be listed as the primary EUM proxy address. Any additional extension numbers you added will be listed as secondary EUM proxy addresses. When an extension number is removed, callers can no longer leave voice mail for the user at the extension number that was removed.

If you remove the primary extension number, UM won't be able to send voice mail to the user's mailbox and call answering rules won't be processed. After the primary extension number has been removed, the EUM proxy address for the user will be listed as **Null** on the user's mailbox in the EAC and when you run the **Get-Mailbox** cmdlet in the Shell. Also, when you run the **Get-UMMailbox** cmdlet, the _Extensions_, _PhoneNumber_, and _CallAnsweringRulesExtensions_ parameters will be blank or null.

You can use the EAC or the Shell to remove a primary or a secondary extension number. You can use the **Email Address** page on the user's mailbox in the EAC to remove a primary or a secondary extension number. You can't use the **UM Mailbox** page in the EAC to remove a primary extension number, but you can use it to remove a secondary extension number.

You can view the primary and secondary extension numbers for a user by using the **Get-UMMailbox** cmdlet or the **Get-Mailbox** cmdlet in the Shell.

For additional management tasks related to users who are enabled for voice mail, see [Voice mail-enabled user procedures](voice-mail-enabled-user-procedures-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 3 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "UM mailboxes" entry in the [Unified Messaging permissions](unified-messaging-permissions-exchange-2013-help.md) topic.

- Before you perform this procedure, confirm that a UM dial plan has been created. For detailed steps, see [Create a UM dial plan](create-um-dial-plan-exchange-2013-help.md).

- Before you perform this procedure, confirm that a UM mailbox policy has been created. For detailed steps, see [Create a UM mailbox policy](create-um-mailbox-policy-exchange-2013-help.md).

- Before you perform these procedures, confirm that the user's mailbox has been enabled for UM and linked to a telephone extension dial plan. For detailed steps, see [Enable a user for voice mail](enable-a-user-for-voice-mail-exchange-2013-help.md).

- Before you perform these procedures, confirm that the primary and secondary extension numbers are configured for the user.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to remove the primary or secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the mailbox from which you want to remove an extension number, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the **User Mailbox** page, under **Email address**, select the extension number that you want to remove from the list, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif). The primary EUM proxy address or extension number is listed in bold letters and numbers.

4. Click **Save**.

## Use the EAC to remove a secondary extension number

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list view, select the user whose mailbox you want to remove an extension number from.

3. In the details pane, under **Phone and Voice Features** \> **Unified Messaging**, click **View details**.

4. On the **Other extensions** page, in the **Extension number** box, select the extension number you want to remove, and then click **Delete** ![Delete icon](images/ITPro_EAC_DeleteIcon.gif).

5. Click **Save**.

## Use the Shell to remove an extension number

This example removes the extension number 12345 from the mailbox of Tony Smith, a UM-enabled user.

> [!NOTE]
> Before you remove an extension number using the Shell, you need to determine the position of the EUM proxy address that you want to modify. To determine the position, use the **$mbx.EmailAddresses** command. The first EUM proxy address in the list will be 0.

```powershell
$mbx = Get-Mailbox tony.smith
$mbx.EmailAddresses.remove("eum:22222;phone-context=MyDialPlan.contoso.com")
Set-Mailbox tony.smith -EmailAddresses $mbx.EmailAddresses
```
