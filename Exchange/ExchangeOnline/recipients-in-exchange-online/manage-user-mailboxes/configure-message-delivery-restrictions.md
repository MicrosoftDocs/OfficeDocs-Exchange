---
localization_priority: Normal
description: You can use the EAC or Exchange Online PowerShell to place restrictions on whether messages are delivered to individual recipients. Message delivery restrictions are useful to control who can send messages to users in your organization. For example, you can configure a mailbox to accept or reject messages sent by specific users or to accept messages only from users in your Exchange organization.
ms.topic: article
author: msdmaguire
ms.author: dmaguire
ms.assetid: c4b8b89f-3dbe-4cb8-8839-9a4e8067e00c
ms.reviewer: 
f1.keywords:
- NOCSH
title: Configure message delivery restrictions for a mailbox
ms.collection: 
- exchange-online
- M365-email-calendar
audience: ITPro
ms.service: exchange-online
manager: serdars

---

# Configure message delivery restrictions for a mailbox

You can use the new EAC, the classic EAC or Exchange Online PowerShell to place restrictions on whether messages are delivered to individual recipients. Message delivery restrictions are useful to control who can send messages to users in your organization. For example, you can configure a mailbox to accept or reject messages sent by specific users or to accept messages only from users in your Exchange organization.

> [!IMPORTANT]
> Message delivery restrictions do not impact mailbox permissions. A user with Full Access permissions on a mailbox will still be able to update the contents in that mailbox, such as by copying messages into the mailbox, even if that user has been restricted.

The message delivery restrictions covered in this topic apply to all recipient types. To learn more about the different recipient types, see [Recipients in Exchange Online](../recipients-in-exchange-online.md).

For additional management tasks related to recipients, see the following topics:

- [Manage user mailboxes](manage-user-mailboxes.md)

- [Create and manage distribution groups](../../recipients-in-exchange-online/manage-distribution-groups/manage-distribution-groups.md)

- [Manage dynamic distribution groups](../../recipients-in-exchange-online/manage-dynamic-distribution-groups/manage-dynamic-distribution-groups.md)

- [Manage mail users](../../recipients-in-exchange-online/manage-mail-users.md)

- [Manage mail contacts](../../recipients-in-exchange-online/manage-mail-contacts.md)

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipients" entry in the [Feature permissions in Exchange Online](../../permissions-exo/feature-permissions.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center](../../accessibility/keyboard-shortcuts-in-admin-center.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Online](https://social.technet.microsoft.com/forums/msonline/home?forum=onlineservicesexchange) or [Exchange Online Protection](https://social.technet.microsoft.com/forums/forefront/home?forum=FOPE).

## Use the new EAC to configure message delivery restrictions

1. In the new EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to configure message delivery restrictions for. A display pane is shown for the selected user mailbox.

3. Under **Mailbox** settings \> **Mail flow settings**, click the **Manage mail flow settings** link.

4. In the **Manage mail flow settings** display pane, you will see the **Message Delivery Restrictions** option. Click the **Edit** button next to this option. The **Message delivery restrictions** display pane is shown. 

- **Accept messages from**: Use this section to specify who can send messages to this user.

   - **All senders**: This option specifies that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This is the default option. It includes external users only if you clear the **Check if all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.
   
   - **Selected senders**: This specifies that the user can choose from a list of senders. Click ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) **Add sender** to display the list of all recipients in your Exchange organization. You can also search for a specific recipient by typing the recipient's name in the search box. Select the desired recipients, and then click **Confirm**.

   - **Check if all senders are authenticated**: This option prevents anonymous users from sending messages to the user. This includes external users that are outside of your Exchange organization.

  
- **Block messages from**: Use this section to block people from sending messages to this user.

   - **None**: This option specifies that the mailbox won't reject messages from any senders in the Exchange organization. This is the default option.

   - **Selected senders**: This specifies that the user can choose from a list of senders. Click ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) **Add sender** to display the list of all recipients in your Exchange organization. You can also search for a specific recipient by typing the recipient's name in the search box. Select the desired recipients, and then click **Confirm**.

5. Click **Save** to save your changes. Click **Close** to exit from the **Manage mail flow settings** display pane.

## Use the Classic EAC to configure message delivery restrictions

1. In the Classic EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to configure message delivery restrictions for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Delivery Restrictions**, click **View details** to view and change the following delivery restrictions:

   - **Accept messages from**: Use this section to specify who can send messages to this user.

   - **All senders**: This option specifies that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This is the default option. It includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

   - **Only senders in the following list**: This option specifies that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to display a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif).

   - **Require that all senders are authenticated**: This option prevents anonymous users from sending messages to the user. This includes external users that are outside of your Exchange organization.

   - **Reject messages from**: Use this section to block people from sending messages to this user.

   - **No senders**: This option specifies that the mailbox won't reject messages from any senders in the Exchange organization. This is the default option.

   - **Senders in the following list**: This option specifies that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](../../media/ITPro_EAC_AddIcon.gif) to display a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](../../media/ITPro_EAC_.gif).

5. Click **OK** to close the **Message Delivery Restrictions** page, and then click **Save** to save your changes.

## How do you know this worked?

To verify that you've successfully configured message delivery restrictions for a user mailbox, do one the following:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to verify the message delivery restrictions for, and then click **Edit** ![Edit icon](../../media/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Delivery Restrictions**, click **View details** to verify the delivery restrictions for the mailbox.

## Use Exchange Online PowerShell to configure message delivery restrictions

The following examples show how to use Exchange Online PowerShell to configure message delivery restrictions for a mailbox. For other recipient types, use the corresponding **Set-** cmdlet with the same parameters.

This example configures the mailbox of Robin Wood to accept messages only from the users Lori Penor, Jeff Phillips, and members of the distribution group Legal Team 1.

```PowerShell
Set-Mailbox -Identity "Robin Wood" -AcceptMessagesOnlyFrom "Lori Penor","Jeff Phillips" -AcceptMessagesOnlyFromDLMembers "Legal Team 1"
```

> [!NOTE]
> If you're configuring a mailbox to accept messages only from individual senders, you have to use the _AcceptMessagesOnlyFrom_ parameter. If you're configuring a mailbox to accept messages only from senders that are members of a specific distribution group, use the _AcceptMessagesOnlyFromDLMembers_ parameter.

This example adds the user named David Pelton to the list of users whose messages will be accepted by the mailbox of Robin Wood.

```PowerShell
Set-Mailbox -Identity "Robin Wood" -AcceptMessagesOnlyFrom @{add="David Pelton"}
```

This example configures the mailbox of Robin Wood to require all senders to be authenticated. This means the mailbox will only accept messages sent by other users in your Exchange organization.

```PowerShell
Set-Mailbox -Identity "Robin Wood" -RequireSenderAuthenticationEnabled $true
```

This example configures the mailbox of Robin Wood to reject messages from the users Joe Healy, Terry Adams, and members of the distribution group Legal Team 2.

```PowerShell
Set-Mailbox -Identity "Robin Wood" -RejectMessagesFrom "Joe Healy","Terry Adams" -RejectMessagesFromDLMembers "Legal Team 2"
```

This example configures the mailbox of Robin Wood to also reject messages sent by members of the group Legal Team 3.

```PowerShell
Set-Mailbox -Identity "Robin Wood" -RejectMessagesFromDLMembers @{add="Legal Team 3"}
```

> [!NOTE]
> If you're configuring a mailbox to reject messages from individual senders, you have to use the _RejectMessagesFrom_ parameter. If you're configuring a mailbox to reject messages from senders that are members of a specific distribution group, use the _RejectMessagesFromDLMembers_ parameter.

For detailed syntax and parameter information related to configuring delivery restrictions for different types of recipients, see the following topics:

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

- [Set-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-dynamicdistributiongroup)

- [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox)

- [Set-MailContact](https://docs.microsoft.com/powershell/module/exchange/set-mailcontact)

- [Set-MailUser](https://docs.microsoft.com/powershell/module/exchange/set-mailuser)

## How do you know this worked?

To verify that you've successfully configured message delivery restrictions for a user mailbox using powershell, do one the following:

Run the following command in Exchange Online PowerShell.

```PowerShell
Get-Mailbox <identity> | Format-List AcceptMessagesOnlyFrom,AcceptMessagesOnlyFromDLMembers,RejectMessagesFrom,RejectMessagesFromDLMembers,RequireSenderAuthenticationEnabled
```
