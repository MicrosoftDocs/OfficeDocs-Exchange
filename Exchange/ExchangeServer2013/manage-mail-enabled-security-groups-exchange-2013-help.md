---
title: 'Manage mail-enabled security groups: Exchange 2013 Help'
TOCTitle: Manage mail-enabled security groups
ms.author: v-mapenn
author: mattpennathe3rd
manager: serdars
ms.reviewer:
ms.assetid: 80b3b537-4786-4d02-9202-44e373811a25
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Manage mail-enabled security groups in Exchange 2013

_**Applies to:** Exchange Server 2013_

A mail-enabled security group can be used to distribute messages as well as to grant access permissions to resources in Active Directory. For more information, see [Recipients](recipients-exchange-2013-help.md).

## What do you need to know before you begin?

- Estimated time to complete: 2 to 5 minutes.

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Distribution groups" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts for the Exchange admin center in Exchange 2013](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://go.microsoft.com/fwlink/p/?linkId=60612).

## Create a mail-enabled security group

### Use the EAC to create a security group

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. Click **New** ![Add Icon](images/ITPro_EAC_AddIcon.gif) \> **Security group**.

3. On the **New security group** page, complete the following fields:

   - **\* Display name** Use this box to type the display name. This name appears in the shared address book, on the To: line when email is sent to this group, and in the Groups list in the EAC. The display name is required and should be user-friendly so people recognize what it is. It also must be unique in the forest.

     > [!NOTE]
     > If a group naming policy is applied, you must follow the naming constraints enforced for your organization. For more information, see [Create a distribution group naming policy](create-group-naming-policy-exchange-2013-help.md). If you want to override your organization's group naming policy, see [Override the distribution group naming policy](override-group-naming-policy-exchange-2013-help.md).

   - **\* Alias** Use this box to type the alias for the security group. The alias can't exceed 64 characters and must be unique in the forest. When a user types the alias on the To: line of an email message, it resolves to the group's display name.

   - **Description** Use this box to describe the security group so people know what the purpose of the group is.

   - **Organizational unit** You can select an organizational unit (OU) other than the default (which is the recipient scope). If the recipient scope is set to the forest, the default value is set to the Users container in the Active Directory domain that contains the computer on which the EAC is running. If the recipient scope is set to a specific domain, the Users container in that domain is selected by default. If the recipient scope is set to a specific OU, that OU is selected by default.

     To select a different OU, click **Browse**. The dialog box displays all OUs in the forest that are within the specified scope. Select the desired OU, and then click **OK**.

   - **\* Owners** By default, the person who creates a group is the owner. All groups must have at least one owner. You can add owners by clicking **Add**.

   - **Members** Use this section to add members and to specify whether approval is required for people to join or leave the group.

     Group owners don't have to be members of the group. Use **Add group owners as members** to add or remove the owners as members.

     To add members to the group, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). When you've finished adding members, click **OK** to return to the **New security group** page.

     Select the **Owner approval is required** check box if you want the group owners to receive user requests to join the group. If you select this option, members can only be removed by the group owners.

4. When you've finished, click **Save** to create the security group.

> [!NOTE]
> By default, all new mail-enabled security groups require that all senders be authenticated. This prevents external senders from sending messages to mail-enabled security groups. To configure a mail-enabled security group to accept messages from all senders, you must modify the message delivery restriction settings for that group.

### Use the Shell to create a security group

This example creates a security group with an alias fsadmin and the name File Server Managers. The security group is created in the default OU, and anyone can join this group with approval by the group owners.

```powershell
New-DistributionGroup -Name "File Server Managers" -Alias fsadmin -Type security
```

For more information about using the Shell to create mail-enabled security groups, see [New-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/new-distributiongroup).

### How do you know this worked?

To verify that you've successfully created a mail-enabled security group, do one of the following:

- In the EAC, navigate to **Recipients** \> **Groups**. The new mail-enabled security group is displayed in the group list. Under **Group Type**, the type is **Security group**.

- In the Shell, run the following command to display information about the new mail-enabled security group.

  ```powershell
  Get-DistributionGroup <Name> | Format-List Name,RecipientTypeDetails,PrimarySmtpAddress
  ```

## Change mail-enabled security group properties

### Use the EAC to change mail-enabled security group properties

1. In the EAC, navigate to **Recipients** \> **Groups**.

2. In the list of groups, click the security group that you want to view or change, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

3. On the group properties page, click one of the following sections to view or change properties.

### General

Use this section to view or change basic information about the group.

- **\* Display name** This name appears in the address book, on the To: line when email is sent to this group, and in the Groups list. The display name is required and should be user-friendly so people recognize what it is. It also has to be unique in your domain.

- **\* Alias** This is the portion of the email address that appears to the left of the at (@) symbol. If you change the alias, the primary SMTP address for the group will also be changed, and contain the new alias. Also, the email address with the previous alias will be kept as a proxy address for the group.

- **Description** Use this box to describe the group so people know what the purpose of the group is. This description appears in the address book and in the Details pane in the EAC.

- **Hide this group from address lists** Select this check box if you don't want users to see this group in the address book. If this check box is selected, a sender has to type the group's alias or email address on the To: or Cc: lines to send mail to the group.

   > [!TIP]
   > Consider hiding security groups because they're typically used to assign permissions to group members and not to send email.

- **Organizational unit** This read-only box displays the organizational unit (OU) that contains the security group. You have to use Active Directory Users and Computers to move the group to a different OU.

### Ownership

Use this section to assign group owners. The group owner can add members to the group, and approve or reject requests to join the group. By default, the person who creates a group is the owner. All groups must have at least one owner.

You can add owners by clicking **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). You can remove an owner by selecting the owner and then clicking **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

### Membership

Use this section to add or remove members. Group owners don't have to be members of the group. Under **Members**, you can add members by clicking **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). You can remove a member by selecting a user in the member list and then clicking **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

### Membership approval

Use this section to specify whether owner approval is required for users to join the group. If you select the **Owner approval is required** check box, the group owner or owners receive an email requesting approval to join the group. As previously mentioned, only owners can remove members from the group.

> [!NOTE]
> This option will not work with mail-enabled security groups because of security-related limitations.

### Delivery management

Use this section to manage who can send email to this group.

- **Only senders inside my organization** Select this option to allow only senders in your organization to send messages to the group. This means that if someone outside of your organization sends an email message to this group, it will be rejected. This is the default setting.

- **Senders inside and outside of my organization** Select this option to allow anyone to send messages to the group.

   You can further limit who can send messages to the group by allowing only specific senders to send messages to this group. Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) and then select one or more recipients. If you add senders to this list, they are the only ones who can send mail to the group. Mail sent by anyone not in the list will be rejected.

   To remove a person or a group from the list, select them in the list and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

   > [!IMPORTANT]
   > If you've configured the group to allow only senders inside your organization to send messages to the group, email sent from a mail contact will be rejected, even if they're added to this list.

### Message approval

Use this section to set options for moderating the group. Moderators approve or reject messages sent to the group before they reach the group members.

- **Messages sent to this group have to be approved by a moderator** This check box isn't selected by default. If you select this check box, incoming messages will be reviewed by the group moderators before delivery. Group moderators can approve or reject incoming messages.

- **Group moderators** To add group moderators, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove a moderator, select the moderator, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif). If you've selected "Messages sent to this group have to be approved by a moderator" and you don't select a moderator, messages to the group will be sent to the group owners for approval.

- **Senders who don't require message approval** To add people or groups that can bypass moderation for this group, click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif). To remove a person or a group, select the item, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

- **Select moderation notifications** Use this section to set how users are notified about message approval.

  - **Notify all senders when their messages aren't approved** This is the default setting. Senders inside and outside your organization will be notified when their messages aren't approved.

  - **Notify senders in your organization when their messages aren't approved** When you select this option, only people or groups in your organization are notified when a message that they sent to the group isn't approved by a moderator.

  - **Don't notify anyone when a message isn't approved** When you select this option, notifications aren't sent to message senders whose messages aren't approved by the group moderators.

### Email options

Use this section to view or change the email addresses associated with the group. This includes the group's primary SMTP addresses and any associated proxy addresses. The primary SMTP address (also known as the reply address) is displayed in bold text in the address list, with the uppercase **SMTP** value in the **Type** column.

- **Add** Click **Add** ![Add Icon](images/ITPro_EAC_AddIcon.gif) to add a new email address for this mailbox. Select one of following address types:

  - **SMTP** This is the default address type. Click this button and then type the new SMTP address in the **\* Email address** box.

    > [!NOTE]
    > To make the new address the primary SMTP address for the group, select the **Make this the reply address** check box. This check box is displayed only when the **Automatically update email addresses based on the email address policy applied to this recipient** check box isn't selected.

  - **Custom address type** Click this button and type one of the supported non-SMTP email address types in the **\* Email address** box.

    > [!NOTE]
    > With the exception of X.400 addresses, Exchange doesn't validate custom addresses for correct formatting. You must make sure that the custom address you specify complies with the format requirements for that address type.

- **Edit** To change an email address associated with the group, select it in the list, and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif).

  > [!NOTE]
   > To make an existing address the primary SMTP address for the group, select the **Make this the reply address** check box. As previously mentioned, this check box is displayed only when the **Automatically update email addresses based on the email address policy applied to this recipient** check box isn't selected.

- **Remove** To delete an email address associated with the group, select it in the list, and then click **Remove** ![Remove icon](images/ITPro_EAC_RemoveIcon.gif).

- **Automatically update email addresses based on the email address policy applied to this recipient** Select this check box to have the recipient's email addresses automatically updated based on changes made to email address policies in your organization. By default, this box is selected.

### MailTip

Use this section to add a MailTip to alert users of potential issues before they send a message to this group. A MailTip is text that's displayed in the InfoBar when this group is added to the To, Cc, or Bcc lines of a new email message. For example, you could add a MailTip to large groups to warn potential senders that their message will be sent to lots of people.

> [!NOTE]
> MailTips can include HTML tags, but scripts aren't allowed. The length of a custom MailTip can't exceed 175 displayed characters. HTML tags aren't counted in the limit.

### Group delegation

Use this section to assign permissions to a user (called a delegate) to allow them to send messages as the group or send messages on behalf of the group. You can assign the following permissions:

- **Send As** This permission allows the delegate to send messages as the group. After this permission is assigned, the delegate has the option to add the group to the **From** line to indicate that the message was sent by the group.

- **Send on Behalf** This permission also allows a delegate to send messages on behalf of the group. After this permission is assigned, the delegate has the option to add the group in the **From** line. The message will appear to be sent by the group and will say that it was sent by the delegate on behalf of the group.

To assign permissions to delegates, click **Add** under the appropriate permission to display the **Select Recipient** page, which displays a list of all recipients in your Exchange organization that can be assigned the permission. Select the recipients you want, add them to the list, and then click **OK**. You can also search for a specific recipient by typing the recipient's name in the search box and then clicking **Search** ![Search icon](images/ITPro_EAC_.gif).

### Use the Shell to change security group properties

Use the **Get-DistributionGroup** and **Set-DistributionGroup** cmdlets to view and change properties for security groups. Advantages of using the Shell are the ability to change the properties that aren't available in the EAC and to change properties for multiple security groups. For information about which parameters correspond to which distribution group properties, see the following topics:

- [Get-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/get-distributiongroup)

- [Set-DistributionGroup](https://docs.microsoft.com/powershell/module/exchange/set-distributiongroup)

Here are some examples of using the Shell to change security group properties.

This example displays a list of all security groups in the organization.

```powershell
Get-DistributionGroup -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'MailUniversalSecurityGroup'"
```

This example changes the primary SMTP address (also called the reply address) for the Seattle Administrators security group from admins@contoso.com to seattle.admins@contoso.com. The previous reply address will be kept as a proxy address.

```powershell
Set-DistributionGroup "Seattle Employees" -EmailAddresses SMTP:sea.admins@contoso.com,smtp:admins@contoso.com
```

This example hides all security groups in the organization from the address book.

```powershell
Get-DistributionGroup -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'MailUniversalSecurityGroup'" | Set-DistributionGroup -HiddenFromAddressListsEnabled $true
```

### How do you know this worked?

To verify that you've successfully changed properties for a security group, do the following:

- In the EAC, select the group and then click **Edit** ![Edit icon](images/ITPro_EAC_EditIcon.gif) to view the property or feature that you changed. Depending on the property that you changed, it might be displayed in the Details pane for the selected group.

- In the Shell, use the **Get-DistributionGroup** cmdlet to verify the changes. One advantage of using the Shell is that you can view multiple properties for multiple groups. In the example above where all security groups were hidden from the address book, run the following command to verify the new value.

  ```powershell
  Get-DistributionGroup -ResultSize unlimited -Filter "RecipientTypeDetails -eq 'MailUniversalSecurityGroup'" | Format-List Name,HiddenFromAddressListsEnabled
  ```
