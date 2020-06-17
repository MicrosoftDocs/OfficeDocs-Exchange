---
title: 'Manage permissions for recipients: Exchange 2013 Help'
TOCTitle: Manage permissions for recipients
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 749cdfe3-496b-453f-96eb-20a0bf28fd52
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage permissions for recipients in Exchange 2013

_**Applies to:** Exchange Server 2013_

You can use the EAC or the Shell to assign permissions to users or groups (called _delegates_) that allow them to open or send messages from other mailboxes. Permissions can be assigned to user mailboxes, linked mailboxes, resource mailboxes, and shared mailboxes. You can also assign permissions to distribution groups, dynamic distribution groups, and mail-enabled security groups to allow delegates to send messages on behalf of the group. You can assign delegates the following permissions to access mailboxes or send messages on behalf of mailboxes or groups:

- **Full Access**: This permission allows a delegate to open a user's mailbox and access the contents of the mailbox. However, assigning the Full Access permission doesn't allow the delegate to send mail from the mailbox. You have to assign the delegate the Send As or the Send on Behalf permission to send mail.

    The Full Access permission isn't available when configuring permissions for groups.

- **Send As**: This permission allows delegates to use the mailbox to send messages. After this permission is assigned to a delegate, any message that the delegate sends from the mailbox will appear to have been sent by the mailbox owner. However, this permission doesn't allow a delegate to sign in to the user's mailbox. It only allows users to open the mailbox. If this permission is assigned to a group, a message sent by the delegate will appear to have been sent by the group.

- **Send on Behalf**: This permission also allows a delegate to use the mailbox to send messages. After this permission is assigned to a delegate, the **From** address in any message sent by the delegate indicates that the message was sent by the delegate on behalf of the mailbox owner.

> [!NOTE]
> If you assign the Full Access, Send As, or Send on Behalf permission to access a mailbox that is hidden from address lists, the delegate won't be able to open the mailbox or send messages.

## What do you need to know before you begin?

- Estimated time to complete each procedure: 2 minutes.

- If permissions to access a mailbox are granted to a user through a group, the mailbox won't be automatically added to the user's profile. The user will need to manually add the mailbox to the profile manually.

- When a mailbox is added to Outlook using Advanced Settings, only the primary mailbox will be added; the archive mailbox won't be added. If a user needs to also access the archive mailbox, the mailbox should be added to Outlook as a second account in the same Outlook profile.

- Procedures in this topic require specific permissions. See each procedure for its permissions information.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Assign permissions to a mailbox

As previously stated, you can assign delegates permissions to user mailboxes, linked mailboxes, resource mailboxes, and shared mailboxes. You can also use the Shell to assign delegates permissions to access a discovery mailbox.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Permissions and delegation" entry in the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

### Use the EAC to assign permissions

The following procedure shows how to assign permissions to a user mailbox. You follow a similar procedure to assign permissions to resource or shared mailboxes by navigating to the **Resources** or **Shared** page in the EAC and selecting the mailbox to assign the permissions to.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, click the mailbox that you want to assign permissions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Delegation**.

4. To assign permissions to delegates, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under the appropriate permission to display a page that lists all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

    To remove a permission for a recipient, under the appropriate permission, select the recipient and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

5. Click **Save** to save your changes.

### Use the EAC to bulk assign permissions

Use the following steps to bulk assign permissions.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. Select the mailboxes that you wish to assign permissions to.

3. Click or tap **More options** in the right pane, and under **Mailbox Delegation** choose, **Add**.

4. On the **bulk add delegation** page, click or tap **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under the appropriate permission to display a page that lists all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**.

   To remove a permission for recipients, under the appropriate permission, select the recipients and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

### Use the EAC to assign a user permission to send email from another user's mailbox

The following procedure shows how to assign a user permission to send email from another user's mailbox.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, click the mailbox that you want to assign send as permissions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Delegation**.

4. To assign permissions to delegates, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under **Send As** or **Send on Behalf** to display a page that lists all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

   The **Send As** permission allows the delegate to send email from this mailbox.

   The **Send on Behalf** permission allows the delegate to send email on behalf of this mailbox. The **From** line in any message sent by a delegate indicates that the message was sent by the delegate on behalf of the mailbox owner.

    > [!NOTE]
    > If the user also needs to be able to open and view the content of that mailbox, you must also assign the user the **Full Access** permission.

5. Click **Save** to save your changes.

### Use the EAC to assign a user permission to send email from a group

The following procedure shows how to assign a user permission to send email from a group.

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. In the list of groups, click the group that you want to assign send as permissions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the group properties page, click **Group Delegation**.

4. To assign permissions to delegates, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under **Send As** or **Send on Behalf** to display a page that lists all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

   The **Send As** permission allows the delegate to send email from this group.

   The **Send on Behalf** permission allows the delegate to send email on behalf of this group. The **From** line in any message sent by a delegate indicates that the message was sent by the delegate on behalf of the group.

5. Click **Save** to save your changes.

### Use the EAC to assign full access permissions

The following procedure shows how to assign full access permissions to a user mailbox.

1. In the EAC, navigate to **Recipients** \> **Mailboxes**.

2. In the list of mailboxes, click the mailbox that you want to assign full access permissions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the mailbox properties page, click **Mailbox Delegation**.

4. To assign permissions to delegates, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under **Full Access** to display a page that lists all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

   The **Full Access** permission allows a delegate to open a user's mailbox and access the contents of the mailbox.

   > [!NOTE]
   > Assigning the **Full Access** permission doesn't allow the delegate to send mail from the mailbox. You have to assign the delegate the **Send As** or the **Send on Behalf** permission to send mail.

5. Click **Save** to save your changes.

### Use the Shell to assign permissions to mailboxes

The following sections show how to use the Shell to manage Full Access, Send As, and Send on Behalf permissions for mailboxes.

### Manage the Full Access permission for mailboxes

The following examples show how to use the **Add-MailboxPermission** and **Remove-MailboxPermission** cmdlets to manage Full Access permissions.

This example assigns the delegate Raymond Sam the Full Access permission to the mailbox of Terry Adams.

```powershell
Add-MailboxPermission -Identity "Terry Adams" -User raymonds -AccessRights FullAccess -InheritanceType all
```

This example assigns Esther Valle the Full Access permission to the organization's default discovery search mailbox.

```powershell
Add-MailboxPermission -Identity "DiscoverySearchMailbox{D919BA05-46A6-415f-80AD-7E09334BB852}" -User estherv -AccessRights FullAccess -InheritanceType all
```

This example assigns members of the Helpdesk distribution group the Full Access permission to the Helpdesk Tickets shared mailbox.

```powershell
Add-MailboxPermission "HelpdeskTickets" -User helpdesk -AccessRights FullAccess -InheritanceType all
```

This example removes Jim Hance's Full Access permission to Ayla Kol's mailbox.

```powershell
Remove-MailboxPermission -Identity ayla -User "Jim Hance" -AccessRights FullAccess -Inheritance
```

For detailed syntax and parameter information, see the following topics:

- [Add-MailboxPermission](https://docs.microsoft.com/powershell/module/exchange/add-mailboxpermission)

- [Remove-MailboxPermission](https://docs.microsoft.com/powershell/module/exchange/remove-mailboxpermission)

### Manage the Send As permission for mailboxes

The following examples show how to manage Send As permissions in Exchange Server 2013. In Exchange 2013, you use the **Add-ADPermission** and **Remove-ADPermission** cmdlets. The _Identity_ parameter specifies the name of the mailbox on which the Send As permission should be added or removed and the _User_ parameter specifies the delegate (for example, a user or group) that will be assigned or unassigned the Send As permission.

> [!TIP]
> Use the **Get-Recipient** cmdlet to retrieve the _Name_ property for the mailbox and the delegate. Use these values to assign the Send As permission.

This example assigns the Send As permission to the Helpdesk group on the shared mailbox Helpdesk Support Team.

```powershell
Add-ADPermission -Identity helpdesksupport -User helpdeskgroup -ExtendedRights "Send As"
```

This example removes the Send As permission for the user Pilar Pinilla on the mailbox of James Alvord.

```powershell
Remove-ADPermission -Identity "James Alvord" -User pilarp -ExtendedRights "Send As"
```

For detailed syntax and parameter information, see:

- [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/add-adpermission)

- [Remove-ADPermission](https://docs.microsoft.com/powershell/module/exchange/remove-adpermission)

### Manage the Send on Behalf permission for mailboxes

The following examples show how to use the **Set-Mailbox** cmdlet to manage Send on Behalf permissions.

This example assigns the delegate Holly Holt the Send on Behalf permission to the mailbox of Sean Chai.

```powershell
Set-Mailbox -Identity seanc@contoso.com -GrantSendOnBehalfTo hollyh
```

This example removes the Send on Behalf permission on the Contoso Executives shared mailbox that was assigned to the Temporary Executive Assistants group.

```powershell
Set-Mailbox "Contoso Executives" -GrantSendOnBehalfTo @{remove="tempassistants@contoso.com"}
```

For detailed syntax and parameter information, see [Set-Mailbox](https://docs.microsoft.com/powershell/module/exchange/set-mailbox).

### How do you know this worked?

To verify that you've successfully assigned permissions to a mailbox or a shared mailbox, do one of the following:

- In the EAC:

1. Navigate to **Recipients** \> **Mailbox** or **Shared**, click the mailbox, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the mailbox properties page, click **Mailbox Delegation**.

3. If you assigned permissions to a recipient, verify that the user or group is listed under the appropriate permission. If you removed permissions, verify that the recipient isn't listed under the appropriate permission.

Or

- In the Shell, run one of the following commands, depending on the permission you managed.

  - **Full Access**

  ```powershell
  Get-MailboxPermission -Identity <mailbox>
  ```

    To verify whether a specific delegate is assigned the Full Access permission to a mailbox, run the following command.

  ```powershell
  Get-MailboxPermission -Identity <mailbox> -User <delegate>
  ```

  - **Send As**

  ```powershell
  Get-ADPermission -Identity <name of mailbox> -User <delegate>
  ```

  - **Send on Behalf**

  ```powershell
  Get-Mailbox -Identity <mailbox> | Format-List GrantSendOnBehalfTo
  ```

## Assign permissions to a group

As previously stated, you can assign the Send As and Send on Behalf permissions to distribution groups, dynamic distribution groups, and mail-enabled security groups to allow delegates to send messages as the group or on behalf of the group.

You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution groups" and "Dynamic distribution groups" entries in the "Recipient Provisioning Permissions" section in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

### Use the EAC to assign permissions to groups

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. In the list of groups, click the group that you want to assign permissions for, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the group properties page, click **Group Delegation**.

4. To assign permissions to delegates, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) under the appropriate permission to display a page that displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

    To remove permission for a recipient, under the appropriate permission, select the recipient and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

5. Click **Save** to save your changes.

### Use the Shell to assign permissions to groups

The following sections show how to use the Shell to manage Send As and Send on Behalf permissions for groups.

### Manage the Send As permission for groups

The following examples show how to manage Send As permissions for groups in Exchange Server 2013. In Exchange 2013, you use the **Add-ADPermission** and **Remove-ADPermission** cmdlets. The _Identity_ parameter specifies the name of the group on which the Send As permission should be added or removed and the _User_ or _Trustee_ parameter specifies the delegate (for example, a user or group) that will be assigned or unassigned the Send As permission.

> [!TIP]
> Use the **Get-Recipient** cmdlet to retrieve the _Name_ property for the group and the delegate. Use these values to assign the Send As permission.

This example assigns the Send As permission to the Sales Admins group for the group named Contoso Sales Info. This allows members of the sales admin group to send messages as the Contoso Sales Information group.

```powershell
Add-ADPermission -Identity "Contoso Sales Info" -User "Sales Admins" -ExtendedRights "Send As"
```

This example removes the Send As permission for the user Alan Shen on the group Corporate IT Admins.

```powershell
Remove-ADPermission -Identity "Corporate IT Admins" -User contoso\alans -ExtendedRights "Send As"
```

For detailed syntax and parameter information, see:

- [Add-ADPermission](https://docs.microsoft.com/powershell/module/exchange/add-adpermission)

- [Remove-ADPermission](https://docs.microsoft.com/powershell/module/exchange/remove-adpermission)

### Manage the Send on Behalf permission

The following examples show how to use the **Set-DistributionGroup** and **Set-DynamicDistributionGroup** cmdlets to manage Send on Behalf permissions for groups.

This example assigns the delegate Sara Davis the Send on Behalf permission to the Printer Support distribution group.

```powershell
Set-DistributionGroup -Identity printersupport@contoso.com -GrantSendOnBehalfTo sarad
```

This example assigns the delegate Administrator the Send on Behalf permission to the All Employees dynamic distribution group.

```powershell
Set-DynamicDistributionGroup -Identity "All Employees" -GrantSendOnBehalfTo administrator
```

This example removes the Send on Behalf permission on the All Employees dynamic distribution group that was assigned to the administrator.

```powershell
Set-DynamicDistributionGroup "All Employees" -GrantSendOnBehalfTo @{remove="administrator"}
```

For detailed syntax and parameter information, see:

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

- [Set-DynamicDistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-dynamicdistributiongroup)

### How do you know this worked?

To verify that you've successfully assigned permissions to a group, do one of the following:

- In the EAC:

1. Navigate to **Recipients** \> **Groups**, click the group, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

2. On the group properties page, click **Group Delegation**.

3. If you assigned permissions to a recipient, verify that the user or group is listed under the appropriate permission. If you removed permissions, verify that the recipient isn't listed under the appropriate permission.

Or

- In the Shell, run one of the following commands depending on the permission you managed.

  - **Send As**

  ```powershell
  Get-ADPermission -Identity <name of group> -User <delegate>
  ```

  - **Send on Behalf**

  ```powershell
  Get-DistributionGroup -Identity <group> | Format-List GrantSendOnBehalfTo
  ```

    Or

  ```powershell
  Get-DynamicDistributionGroup -Identity <group> | Format-List GrantSendOnBehalfTo
  ```
