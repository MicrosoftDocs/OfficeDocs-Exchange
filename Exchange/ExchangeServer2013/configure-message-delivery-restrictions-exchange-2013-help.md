---
title: 'Configure message delivery restrictions for a mailbox: Exchange 2013 Help'
TOCTitle: Configure message delivery restrictions for a mailbox
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: c4b8b89f-3dbe-4cb8-8839-9a4e8067e00c
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure message delivery restrictions for a mailbox in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to place restrictions on whether messages are delivered to individual recipients. Message delivery restrictions are useful to control who can send messages to users in your organization. For example, you can configure a mailbox to accept or reject messages sent by specific users or to accept messages only from users in your Exchange organization.

The message delivery restrictions covered in this topic apply to all recipient types. To learn more about the different recipient types, see [Recipients](recipients-exchange-2013-help.md).

For additional management tasks related to recipients, see the following topics:

- [Manage user mailboxes](manage-user-mailboxes-exchange-2013-help.md)

- [Create and manage distribution groups](manage-distribution-groups-exchange-2013-help.md)

- [Manage dynamic distribution groups](manage-dynamic-distribution-groups-exchange-2013-help.md)

- [Manage mail users](manage-mail-users-exchange-2013-help.md)

- [Manage mail contacts](manage-mail-contacts-exchange-2013-help.md)

## What do you need to know before you begin?

- Estimated time to complete: 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Use the EAC to configure message delivery restrictions

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to configure message delivery restrictions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Delivery Restrictions**, click **View details** to view and change the following delivery restrictions:

   - **Accept messages from** Use this section to specify who can send messages to this user.

   - **All senders** This option specifies that the user can accept messages from all senders. This includes both senders in your Exchange organization and external senders. This is the default option. It includes external users only if you clear the **Require that all senders are authenticated** check box. If you select this check box, messages from external users will be rejected.

   - **Only senders in the following list** This option specifies that the user can accept messages only from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to display a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

   - **Require that all senders are authenticated** This option prevents anonymous users from sending messages to the user. This includes external users that are outside of your Exchange organization.

   - **Reject messages from** Use this section to block people from sending messages to this user.

   - **No senders** This option specifies that the mailbox won't reject messages from any senders in the Exchange organization. This is the default option.

   - **Senders in the following list** This option specifies that the mailbox will reject messages from a specified set of senders in your Exchange organization. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to display a list of all recipients in your Exchange organization. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

5. Click **OK** to close the **Message Delivery Restrictions** page, and then click **Save** to save your changes.

## Use the Shell to configure message delivery restrictions

The following examples show how to use the Shell to configure message delivery restrictions for a mailbox. For other recipient types, use the corresponding **Set-** cmdlet with the same parameters.

This example configures the mailbox of Robin Wood to accept messages only from the users Lori Penor, Jeff Phillips, and members of the distribution group Legal Team 1.

```powershell
Set-Mailbox -Identity "Robin Wood" -AcceptMessagesOnlyFrom "Lori Penor","Jeff Phillips" -AcceptMessagesOnlyFromDLMembers "Legal Team 1"
```

> [!NOTE]
> If you're configuring a mailbox to accept messages only from individual senders, you have to use the _AcceptMessagesOnlyFrom_ parameter. If you're configuring a mailbox to accept messages only from senders that are members of a specific distribution group, use the _AcceptMessagesOnlyFromDLMembers_ parameter.

This example adds the user named David Pelton to the list of users whose messages will be accepted by the mailbox of Robin Wood.

```powershell
Set-Mailbox -Identity "Robin Wood" -AcceptMessagesOnlyFrom @{add="David Pelton"}
```

This example configures the mailbox of Robin Wood to require all senders to be authenticated. This means the mailbox will only accept messages sent by other users in your Exchange organization.

```powershell
Set-Mailbox -Identity "Robin Wood" -RequireSenderAuthenticationEnabled $true
```

This example configures the mailbox of Robin Wood to reject messages from the users Joe Healy, Terry Adams, and members of the distribution group Legal Team 2.

```powershell
Set-Mailbox -Identity "Robin Wood" -RejectMessagesFrom "Joe Healy","Terry Adams" -RejectMessagesFromDLMembers "Legal Team 2"
```

This example configures the mailbox of Robin Wood to also reject messages sent by members of the group Legal Team 3.

```powershell
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

To verify that you've successfully configured message delivery restrictions for a user mailbox, do one the following:

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of user mailboxes, click the mailbox that you want to verify the message delivery restrictions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Features**.

4. Under **Message Delivery Restrictions**, click **View details** to verify the delivery restrictions for the mailbox.

Or

Run the following command in the Shell.

```powershell
Get-Mailbox <identity> | Format-List AcceptMessagesOnlyFrom,AcceptMessagesOnlyFromDLMembers,RejectMessagesFrom,RejectMessagesFromDLMembers,RequireSenderAuthenticationEnabled
```
